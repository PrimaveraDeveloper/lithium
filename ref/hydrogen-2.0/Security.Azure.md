# Primavera.Hydrogen.Security.Azure

**Class library that contains types that define generic security services that use Windows Azure services.**

## Secrets Storage (`AzureKeyVaultSecretsStorageService`)

The `AzureKeyVaultSecretsStorageService` implements the `ISecretsStorageService` using the Microsoft KeyVault services.

The service implementation should be registered using one of the following extension methods for `IServiceCollection`:

```csharp
IServiceCollection services = (...);

services.AddAzureSecretsStorage();
```

This will register the service using default configuration options set my a configuration section named `AzureKeyVaultSecretsStorageOptions`.

```csharp
IServiceCollection services = (...);

services.AddAzureSecretsStorage(
    (options) =>
    {
    });
```

This will register the service using the specified configuration delete, after reading the default configuration section (if found).

### `AzureKeyVaultSecretsStorageOptions`

This options class provides the following configuration options:

- `Credentials` - configuration options to setup the credentials required to access the secrets storage service (see [`AzureCredentialsOptions`](./Azure.Client.md)).
- `RetryPolicy` (optional) (default is `Exponential`) - the retry policy that should be applied to all requests to KeyVault.
- `RetryPolicyMaximumAttempts` (optional) (default is 3) - the maximum number of retries for all requests to KeyVault.
- `RetryPolicyBackoffTime` (optional) (default value is 0.8 seconds) - the initial back-off time before the first retry.
- `RetryPolicyMaximumBackoffTime` (optional) (default value is 1 seconds) - the maximum back-off time between retries.
- `ConfigurationStorageBaseUrl` - the base URI of the secrets storage that should be used to load app settings.

### Authorization

Azure KeyVault is integrated with the Azure Active Directory and Azure Managed Identity and requires authorization of the client application that is trying to access it.

By default, `AzureKeyVaultSecretsStorageService` will apply a chain a credentials built according to the configuration options set in `AzureKeyVaultSecretsStorageOptions.Credentials`. This chain is setup using [`ChainedTokenCredentialBuilder`](./Azure.Client.md).

## Configuration Secrets (`AzureKeyVaultSecretsStorageConfigurationExtensions`)

You can use the Secrets Storage service to store application settings very much like you can store those settings in a JSON file.

When the application configuration is being setup you just need to add the configuration provider using one of the following alternatives:

```c#
IHost host = new HostBuilder()
    .ConfigureAppConfiguration(
        (config) =>
        {
            config.SetBasePath(Directory.GetCurrentDirectory());
            config.AddJsonFile("_Config\\appsettings.secretsstorageconfig3.json", optional: false);
            config.AddAzureSecretsStorage();
        })
    .Build();
```

```c#
IHost host = new HostBuilder()
    .ConfigureAppConfiguration(
        (config) =>
        {
            config.SetBasePath(Directory.GetCurrentDirectory());
            config.AddJsonFile("_Config\\appsettings.secretsstorageconfig.json", optional: false);
            config.AddAzureSecretsStorage(new Uri(AzureResources.KeyVaultUrl));
        })
    .Build();
```

The configuration provider will use the `AzureKeyVaultSecretsStorageOptions` configuration options to setup the secrets storage service.

If you provide the secrets storage base URL it will use it. Otherwise it will use the value set in `AzureKeyVaultSecretsStorageOptions.ConfigurationStorageBaseUrl`.

If no value is set in the options or provided when adding the configuration provider, this feature will not raise any error and will remain simply disabled.

> The configuration provider (`AzureKeyVaultSecretsStorageConfigurationProvider`) does not rely on dependency injection to resolve services and it will use
`AzureKeyVaultSecretsStorageService` directly.
