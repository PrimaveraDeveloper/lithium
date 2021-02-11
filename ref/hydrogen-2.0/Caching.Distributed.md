# Primavera.Hydrogen.Caching.Distributed

**Class library that contains types that provide distributed caching services.**

## REDIS Cache

A REDIS cache service can be added to the application services and configured using the extension methods provided by `RedisCacheServiceCollectionExtensions`. These extension methods should be preferred to using the extension methods provided in the ASP.NET Core framework (like `AddStackExchangeRedisCache()`).

Examples:

```
services.Configure<RedisCacheOptions>(
    configuration.GetSection(
        nameof(RedisCacheOptions)));

// (...)

services.AddRedisCache();
```

```
services.Configure<RedisCacheOptions>(
    configuration.GetSection(
        nameof(RedisCacheOptions)));

// (...)

services.AddRedisCache(
    (options) =>
    {
        // (...)
    }
);
```

`RedisCacheOptions` provides the following configuration settings:

- `ConnectionString`: the connection string that should be used to connect to the REDIS cache.
- `InstanceName`: the cache instance name.

## Resilient Distributed Cache

Distributed cache services - like REDIS - tend to be subject to transient errors (timeouts) which affect applications behavior (performance and stability).

`IResilientDistributeCache` and `ResilientDistributeCache` provide a mechanism to add resilience to `IDistributedCache` by means of a retry policy that detects transient exceptions (with `RetryPolicy`).

This service is not designed to be used directly. Instead it is designed to "replace" the configured implementation of the distributed cache using a dependency injection decorator.

The distributed cache should be configured in the application as follows:

```csharp
ServiceCollection services = new ServiceCollection();

// The concrete distributed cache service should be registered before adding the resilient cache service

services.AddStackExchangeRedisCache(
    (options) =>
    {
        // Configuration
    });

// Then the resilient cache should be added to decorate the concrete service

services.AddResilientDistributedCache();
```

This way any component that requires the distributed cache will not be affected, as it will be injected with an instance of `ResilientDistributedCache` (that uses the concrete service with a retry policy).

```csharp
public class MyComponent
{
    public MyComponent(IDistributeCache cache)
    {
        // The cache instance is of type ResilientDistributedCache
    }
}
```

### Retry Policy

By default the resilient cache is configured to use a retry policy set with an `ExponentialBackoffRetryStrategy` (with a number of retries and minimum back-off time set by `ResilientCacheOptions`).

This retry policy uses an error detection strategy that recognizes the following exceptions as transient:

- `StackExchange.Redis.RedisConnectionException`
- `StackExchange.Redis.RedisTimeoutException`

You can configure the behavior of the resilient cache retry policy by setting `ResilientCacheOptions` when adding the decorator:

```csharp
services.AddResilientDistributedCache(
    (options) =>
    {
        // Replace the retry policy entirely

        options.RetryPolicy = (...);
    });

services.AddResilientDistributedCache(
    (options) =>
    {
        // Keep the default retry policy and replace the error detection strategy predicate

        options.ErrorDetectionPredicate = (...);
    });

services.AddResilientDistributedCache(
    (options) =>
    {
        // Keep the default retry policy and configure the maximum number of retries and the minimum back-off time

        options.MaxRetries = 1;
        options.MinRetryBackoff = TimeSpan.FromSeconds(1);
    });

services.AddResilientDistributedCache(
    (options) =>
    {
        // Keep the default retry policy, replace the error detection strategy predicate, and configure the maximum number of retries and the minimum back-off time

        options.ErrorDetectionPredicate = (...);
        options.MaxRetries = 1;
        options.MinRetryBackoff = TimeSpan.FromSeconds(1);
    });
```

### IDistributedCache (Resilient Extension Methods)

`ResilentDistributedCacheExtensions` provides a set of extension methods that allow for resilience when using `IDistributedCache,` even when the resilient cache service (see above) is not used.

These methods allow controlling whether `IDistributedCache` should raise exceptions (parameter `throwErrors`) and even return default values when the cache items do not exist (parameter `defaultValue`).

- `byte[] Get(this IDistributedCache cache, string key, bool throwErrors)`
- `Task<byte[]> GetAsync(this IDistributedCache cache, string key, bool throwErrors, CancellationToken token = default)`
- `string GetString(this IDistributedCache cache, string key, bool throwErrors)`
- `string GetString(this IDistributedCache cache, string key, string defaultValue, bool throwErrors)`
- `Task<string> GetStringAsync(this IDistributedCache cache, string key, bool throwErrors, CancellationToken token = default)`
- `Task<string> GetStringAsync(this IDistributedCache cache, string key, string defaultValue, bool throwErrors, CancellationToken token = default)`
- `void Set(this IDistributedCache cache, string key, byte[] value, bool throwErrors)`
- `void Set(this IDistributedCache cache, string key, byte[] value, DistributedCacheEntryOptions options, bool throwErrors)`
- `Task SetAsync(this IDistributedCache cache, string key, byte[] value, bool throwErrors, CancellationToken token = default)`
- `Task SetAsync(this IDistributedCache cache, string key, byte[] value, DistributedCacheEntryOptions options, bool throwErrors, CancellationToken token = default)`
- `void SetString(this IDistributedCache cache, string key, string value, bool throwErrors)`
- `void SetString(this IDistributedCache cache, string key, string value, DistributedCacheEntryOptions options, bool throwErrors)`
- `Task SetStringAsync(this IDistributedCache cache, string key, string value, bool throwErrors, CancellationToken token = default)`
- `Task SetStringAsync(this IDistributedCache cache, string key, string value, DistributedCacheEntryOptions options, bool throwErrors, CancellationToken token = default)`
- `void Refresh(this IDistributedCache cache, string key, bool throwErrors)`
- `Task RefreshAsync(this IDistributedCache cache, string key, bool throwErrors, CancellationToken token = default)`
- `void Remove(this IDistributedCache cache, string key, bool throwErrors)`
- `Task RemoveAsync(this IDistributedCache cache, string key, bool throwErrors, CancellationToken token = default)`

> Only timeout exceptions raised by REDIS will be ignored. Connection exception will not be ignored.