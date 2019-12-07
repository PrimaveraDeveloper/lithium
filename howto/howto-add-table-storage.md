# How to add support for Table Storage to a microservice

This guide describes how support for storing data in Azure Table Storage can be added to a microservice and how the Table Storage Service should be used in custom code.

Storage is an important factor in the development of microservices as is with any other kind of application. But using full-fledged relational databases is typically costly and contrary to the general idea that microservices should be as simple as possible.

NoSQL storage is much more convenient for microservices because they provide storage at a lower cost but also because it tends to support more flexible and fluid data models.

Azure provides a semi-structured NoSQL storage called Table Storage that is very useful to implement most scenarios where microservices need to store state in the form of tabular data (e.g. key/value settings, records of templates, etc.).

## Adding the Table Storage

To add the table storage dependency to a microservice follow these steps:

1. Open the service model.
2. Using the context menu, add a Dependency.
3. Select `TableStorage` as the dependency kind.
4. Save the model.
5. Transform all projects.

### Generated Code

The generated code will reflect the following changes to configure this storage.

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

    // Table storage

    this.AddAzureTableStorage(services, hostConfiguration);
}
```

`StartupBase.AddAzureTableStorage`:

```csharp
//// <summary>
/// Called when configuring services to configure the table storage service.
/// </summary>
/// <param name="services">The service collection.</param>
/// <param name="hostConfiguration">The host configuration.</param>
protected virtual void AddAzureTableStorage(IServiceCollection services, HostConfiguration hostConfiguration)
{
    // Validation

    SmartGuard.NotNull(() => services, services);

    // Azure table storage

    services.AddAzureTableStorage();
}
```

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

    // Table storage options

    services
        .Configure<AzureTableStorageOptions>(
            this.Configuration.GetSection(nameof(AzureTableStorageOptions)));

    // Host configuration snapshot

    services
        .AddOptionsSnapshot<HostConfiguration>();

    // Resolve the host configuration instance

    IServiceProvider provider = services.BuildServiceProvider();
    return provider.GetRequiredService<HostConfiguration>();
}
```

> This sets up configuration to recognize the options required by the Azure Table Storage service (`AzureTableStorageOptions`).

## Configuration

Next you only need to set the connection string for the Azure Storage Account (or CosmosDB Account) you wish to use in the application configuration, by setting `ConnectionString` in a section called `AzureTableStorageOptions`:

```json
{
    "AzureTableStorageOptions": {
        "ConnectionString": "(...)"
    }
}
```

> For more configuration options, check the Hydrogen documentation on `AzureTableStorageOptions`.

## Using the Table Storage

Now that the storage service is fully configured, using it is as simple as resolving the `ITableStorageService` service anywhere you need it.

### `ITableStorageService`

This service should be resolved through the service provider:

```csharp
private ITableStorageService TableStorage
{
    get
    {
        return this.ServiceProvider.GetRequiredService<ITableStorageService>();
    }
}
```

> See the documentation available for Hydrogen to understand all the features provided by the table storage service and how to use them.