# Capability - Configuration

> Tags: Service Collection, Dependency Injection, IOptions, Options Snapshot

Configuration is a crucial aspect of .NET Core applications and Lithium microservices which is tightly related with the dependency injection mechanism.

## Configuration Files

Microservices developed with the Lithium Framework rely on the configuration system built-in in .NET Core. This means that app settings can be defined via environment variables, app settings files, secrets storage, etc.

Most of the configuration options will be specified in app settings files, part automatically generated, part defined in custom files, which allows the microservice behavior to be customized also at "runtime" (in the platform).

The following app settings file are considered when loading configuration (by this order):

```
\GeneratedCode\appsettings.gen.json
\CustomCode\appsettings.json
\GeneratedCode\appsettings-{environment}.gen.json
\CustomCode\appsettings-{environment}.json
```

> Notice that files in the `GeneratedCode` are automatically generated and files in the `CustomCode` must be set by the developer. {environment} depends on the microservice runtime environment (`Development`, `Staging`, `Production`).

For more information on how configuration works see [Configuration in ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration).

## Externalized Application Settings using Azure AppConfiguration

As the number of microservices grows over time manually managing their application settings becomes an overwhelming and error-prone task. Also, different microservices may share one or more settings and if ever there were a need to change one of these shared settings doing so would be extremely time consuming and fallible.

To address this matter the Lithium Framework uses the [Azure AppConfiguration](https://docs.microsoft.com/en-us/azure/azure-app-configuration/overview) service as way of managing application settings.

For more information see: [How to manage application settings externally using Azure AppConfiguration](../howto/howto-use-azure-appconfiguration.md).

## Configuration in Generated Code

The microservice will include a set of common configuration options and a set of configuration options for the dependencies it includes. This affects the generated code and the application startup.

For example:

`StartupBase.AddConfiguration()`:

```csharp
protected virtual HostConfiguration AddConfiguration(IServiceCollection services)
{
    // Validation

    SmartGuard.NotNull(() => services, services);

    // Common options

    services
        .AddOptions()
        .Configure<HostConfiguration>(
            this.Configuration.GetSection(nameof(HostConfiguration)))
        .Configure<AzureInsightsTelemetryOptions>(
            this.Configuration.GetSection(nameof(AzureInsightsTelemetryOptions)));

    // Table storage options

    services
        .Configure<AzureTableStorageOptions>(
            this.Configuration.GetSection(nameof(AzureTableStorageOptions)));

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

## Custom Configuration

Each microservice also includes a custom configuration type called `HostConfiguration` that is automatically generated. This class can be customized and the new options can be set in custom app settings files.

For more information see: [How to add custom application settings](..//howto/howto-add-custom-appsettings.md).

## Configuration Options

There are also scenarios where you might need to customize configuration in code. This requires overriding `AddConfiguration()` (see above). Here's an example:

```csharp
/// <inheritdoc />
protected override HostConfiguration AddConfiguration(IServiceCollection services)
{
    // Validation

    SmartGuard.NotNull(() => services, services);

    // Custom options

    services
        .Configure<MyCustomOptions>(
            this.Configuration.GetSection(nameof(MyCustomOptions)));

    // Default behavior

    return base.AddConfiguration(services);
}
```
