# How to add support for Blob Storage to a microservice

This guide describes how support for storing data in Azure Blob Storage can be added to a microservice and how the Blob Storage Service should be used in custom code.

Azure provides a unstructured storage for objects called Blob Storage that allows storing objects, files, images, etc.

## Adding the Blob Storage

To add the blob storage dependency to a microservice follow these steps:

1. Open the service model.
2. Using the context menu, add a Dependency.
3. Select `BlobStorage` as the dependency kind.
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

    // Blob storage

    this.AddAzureBlobStorage(services, hostConfiguration);
}
```

`StartupBase.AddAzureBlobStorage`:

```csharp
//// <summary>
/// Called when configuring services to configure the blob storage service.
/// </summary>
/// <param name="services">The service collection.</param>
/// <param name="hostConfiguration">The host configuration.</param>
protected virtual void AddAzureBlobStorage(IServiceCollection services, HostConfiguration hostConfiguration)
{
    // Validation

    SmartGuard.NotNull(() => services, services);

    // Azure blob storage

    services.AddAzureBlobStorage();
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
            this.Configuration.GetSection(nameof(AzureInsightsTelemetryOptions)));

    // Blob storage options

    services
        .Configure<AzureBlobStorageOptions>(
            this.Configuration.GetSection(nameof(AzureBlobStorageOptions)));

    // Host configuration snapshot

    services
        .AddOptionsSnapshot<HostConfiguration>();

    // Resolve the host configuration instance

    IServiceProvider provider = services.BuildServiceProvider();
    return provider.GetRequiredService<HostConfiguration>();
}
```

> This sets up configuration to recognize the options required by the Azure Blob Storage service (`AzureBlobStorageOptions`).

## Configuration

Next you only need to set the connection string for the Azure Storage Account you wish to use in the application configuration, by setting `ConnectionString` in a section called `AzureBlobStorageOptions`:

```json
{
    "AzureBlobStorageOptions": {
        "ConnectionString": "(...)"
    }
}
```

> For more configuration options, check the Hydrogen documentation on `AzureBlobStorageOptions`.

## Using the Blob Storage

Now that the storage service is fully configured, using it is as simple as resolving the `IBlobStorageService` service anywhere you need it.

### `IBlobStorageService`

This service should be resolved through the service provider:

```csharp
private IBlobStorageService BlobStorage
{
    get
    {
        return this.ServiceProvider.GetRequiredService<IBlobStorageService>();
    }
}
```

> See the documentation available for Hydrogen to understand all the features provided by the blob storage service and how to use them.