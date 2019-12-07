# How to add support for distributed caching to a microservice

This guide describes how a distributed cache can be added to a microservice and how it should be used in custom code.

Caching is a very important feature for most applications as it can improve performance dramatically and favor the overall user experience.

This is particularly true for microservices that may be potentially invoked hundreds or thousands of times per second by multiple clients performing the same or similar requests.

Since microservices are often published on a Web server farm, if caching is distributed (meaning that cache items are shared between all servers) the performance gains can be even greater. In fact, depending on the microservice behavior/implementation, sharing the cache between servers may be required to ensure the correct implementation of the service's business logic.

REDIS is the "de facto" standard when implementing distributed caching on the Web today. That is why the Lithium Framework includes support to configure a REDIS cache easily on any microservice.

## Adding the Distributed Cache

To add the distributed cache dependency to a microservice follow these steps:

1. Open the service model.
2. Using the context menu, add a Dependency.
3. Select `DistributedCache` as the dependency kind.
4. Save the model.
5. Transform all projects.

### Generated Code

Notice the following pieces of code that are generated in `Startup.gen.cs`.

`StartupBase.AddDependencies`:

```csharp
/// <summary>
/// Called to add dependencies to the service collection.
/// </summary>
/// <param name="services">The service collection.</param>
/// <param name="hostConfiguration">The host configuration.</param>
/// <remarks>
/// The method is called from <see cref="ConfigureServices(IServiceCollection)"/>.
/// </remarks>
protected virtual void AddDependencies(IServiceCollection services, HostConfiguration hostConfiguration)
{
    // Add dependencies

    // Distributed cache

    this.AddDistributedCache(services, hostConfiguration);
}
```

> This adds the new dependency.

`StartupBase.AddDistributedCache`:

```csharp
/// <summary>
/// Called when configuring services to configure the distributed cache service.
/// </summary>
/// <param name="services">The service collection.</param>
/// <param name="hostConfiguration">The host configuration.</param>
protected virtual void AddDistributedCache(IServiceCollection services, HostConfiguration hostConfiguration)
{
    // Validation

    SmartGuard.NotNull(() => services, services);

    // REDIS cache (resilient)

    services.AddRedisCache();
    services.AddResilientDistributedCache();
}
```

> This configures a REDIS cache with resilience (see below) to the service collection.

`StartupBase.AddConfiguration`:

```csharp
/// <summary>
/// Called to add configuration to the service collection.
/// </summary>
/// <param name="services">The service collection.</param>
/// <returns>
/// An instance of <see cref="HostConfiguration"/> that can be used in the application configuration process.
/// </returns>
/// <remarks>
/// The method is called from <see cref="ConfigureServices(IServiceCollection)"/>.
/// </remarks>
protected virtual HostConfiguration AddConfiguration(IServiceCollection services)
{
    // Common options

    services
        .AddOptions()
        .Configure<HostConfiguration>(
            this.Configuration.GetSection(nameof(HostConfiguration)))
        .Configure<AzureInsightsTelemetryOptions>(
            this.Configuration.GetSection(nameof(AzureInsightsTelemetryOptions)))
        .Configure<ThrottlingOptions>(
            this.Configuration.GetSection(nameof(ThrottlingOptions)))
        .Configure<ClientRateThrottlingOptions>(
            this.Configuration.GetSection(nameof(ClientRateThrottlingOptions)));

    // REDIS cache options

    services
        .Configure<RedisCacheOptions>(
            this.Configuration.GetSection(nameof(RedisCacheOptions)));

    // Host configuration snapshot

    services
        .AddOptionsSnapshot<HostConfiguration>();

    // Resolve the host configuration instance

    IServiceProvider provider = services.BuildServiceProvider();
    return provider.GetRequiredService<HostConfiguration>();
}
```

> This sets up configuration to recognize the options required by the REDIS cache service (`RedisCacheOptions`).

## Configuration

Next you only need to set the connection string for the REDIS cache instance you wish to use in the application configuration, by setting `ConnectionString` in a section called `RedisCacheOptions`:

```json
{
    "RedisCacheOptions": {
        "ConnectionString": "(...)"
    }
}
```

> For more configuration options, check the Hydrogen documentation on `RedisCacheOptions`.

## Using the Distributed Cache

Now that the caching service is fully configured, using it is as simple as resolving the `IDistributedCache` service anywhere you need it.

### `IDistributedCache`

This service should be resolved through the service provider:

```csharp
private IDistributedCache DistributedCache
{
    get
    {
        return this.ServiceProvider.GetRequiredService<IDistributedCache>();
    }
}
```

Then it can be used to set and retrieve items from the cache:

```csharp
string lockExists = await this.DistributedCache.GetStringAsync(
    LockCacheKey,
    false)
    .ConfigureAwait(false);

await this.DistributedCache.SetStringAsync(
    LockCacheKey,
    "true",
    GetCacheOptions(), 
    false)
    .ConfigureAwait(false);
```

### Resilience

REDIS cache instances are somehow unstable and prone to transient errors.

That is the reason why REDIS caches in Lithium are set up with a "resilience extension" provided by a custom service. This service includes a retry policy to detect transient errors and retry requests to the cache instance.

This behavior is added by the following line in `AddDistributedCache()` (see above):

```csharp
services.AddResilientDistributedCache();
```

There is another aspect of resilience that is provided by Hydrogen in the form of extension methods for the `IDistributedCache` type.

On most caching scenarios, if an item cannot be retrieved from the cache (although it exists there), treating them as if the cache item does not exist is a better choice than raising errors (off course the benefit of the cache is lost).

To achieve that you just need to add the following using statement in the code file where the cache is being used:

```csharp
using Primavera.Hydrogen.Caching.Distributed.Resilience;
```

Now `IDistributedCache` has extension methods that allow specifying an additional parameter - called `throwErrors` - to specify whether the calls should raise errors or not when invoking the REDIS cache instance. When false, a failing cache will simply behave like an empty cache.

```csharp
await this.DistributedCache.SetStringAsync(
    key: LockCacheKey,
    value: "true",
    options: GetCacheOptions(), 
    throwErrors: false)
    .ConfigureAwait(false);
```