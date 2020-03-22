# Capability - Secrets

> Tags: Configuration, KeyVault

A secret is any piece of information that needs to be protected from being viewed. It can be a password, a connection string, a certificate, a configuration option, etc.

## Secrets Storage

The secrets storage is provided by [ISecretsStorageService](../ref/hydrogen-2.0/Security.Abstractions.md).

The [Certificates Service](../dir/common/cs.md) is an example of a microservice that uses this service.

## Configuration

The .NET Core supports connecting the configuration system with a Azure KeyVault to protect configuration options. This is automatically supported by the Lithium Framework in the code generated for the microservices.

`HostBuilderBase:ConfigureAppConfiguration()`:

```csharp
protected virtual void ConfigureAppConfiguration(HostBuilderContext context, IConfigurationBuilder builder)
{
    // Environment

    IHostEnvironment env = context.HostingEnvironment;

    // Normalize environment name
    // NOTE: File names are case sensitive in Linux

    string environmentName = env.EnvironmentName.Transform().ToLowerCase();

    // JSON configuration

    builder
        .AddJsonFile($"GeneratedCode/appsettings.gen.json", optional: false, reloadOnChange: true)
        .AddJsonFile($"CustomCode/appsettings.json", optional: true, reloadOnChange: true)
        .AddJsonFile($"GeneratedCode/appsettings-{environmentName}.gen.json", optional: true, reloadOnChange: true)
        .AddJsonFile($"CustomCode/appsettings-{environmentName}.json", optional: true, reloadOnChange: true);

    // Environment variables configuration

    builder
        .AddEnvironmentVariables();

    // Secrets storage

    builder
        .AddAzureSecretsStorage();
}
```

> For more information, see [Primavera.Hydrogen.Security.Azure](../ref/hydrogen-2.0/Security.Azure.md).