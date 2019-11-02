# Primavera.Hydrogen.Security.Azure

**Class library that contains types that define generic security services that use Windows Azure services.**

## Secrets Storage (AzureKeyVaultSecretsStorageService)

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

### AzureKeyVaultSecretsStorageOptions

This options class provides the following configuration options:

- `AutomaticAuthenticationEnabled` (default is true) - indicates whether the automatic authentication with KeyVault is enabled.
- `ClientId` (optional) - the client application identifier (for authentication with KeyVault).
- `ClientSecret` (optional) - the client application identifier (for authentication with KeyVault).
- `DeveloperMode` (default is false) - indicates whether automatic authentication can use the Visual Studio credentials for the Azure Service Token.
- `RetryPolicy` (optional) (default is Exponential) - the retry policy that should be applied to all requests to KeyVault.
- `RetryPolicyMaximumAttempts` (optional) (default is 3) - the maximum number of retries for all requests to KeyVault.

### Authentication

Azure KeyVault is integrated with the Azure Active Directory and requires authentication of the client application that is trying to access it.

By default, `AzureKeyVaultSecretsStorageService` will try both automatic authentication and client credentials authentication (client id + client secret) by that order.

Automatic authentication does not require the client identifier and the client secret to be specified in the configuration options and works by using the Azure
Service Token (see `IAzureServiceTokenService`) to automatically identify the client application.

On a Azure WebApp that automatic authentication is configured in the Azure Management Portal and, when active, the WebApp will automatically be identified as the
client application so that access tokens can be retrieved (provided that WebApp is configured to have access to the specified KeyVault).

Visual Studio also allows configuring the credentials to access Azure resources and, when set, can be used by the Azure Service Token. That is why `AzureKeyVaultSecretsStorageOptions.DeveloperMode` exists.

By default `AzureKeyVaultSecretsStorageService` will have that option disabled, resulting in a behavior similar to an Azure WebApp. However, that option can be enabled for example
to execute unit tests on the automatic authentication behavior.

When automatic authentication is disabled or when it fails, the client identifier and the client secret provided in the configuration options will be used to retrieve
an access token from Azure AD to access the KeyVault.

## Azure Service Token Service (IAzureServiceTokenService)

`IAzureServiceTokenService` and `DefaultAzureServiceTokenService` (the default implementation) allows retrieving access tokens to access Azure resources using the Azure Service Token underneath.

This service is used internally by `AzureKeyVaultSecretsStorageService` to automatically authenticate client applications.

The following methods are available:

- `Task<string> GetKeyVaultAccessTokenAsync(string authority, string resource, string scope, bool developerMode = false)`
- `Task<string> TryGetKeyVaultAccessTokenAsync(string authority, string resource, string scope, bool developerMode = false)`

> None of the methods require any client identifier or client secret because the Azure Service Token is supposed to determine that automatically (or else fail). If developer mode
is on, then the credentials set in Visual Studio will be used.

## Configuration Secrets (AzureKeyVaultSecretsStorageConfigurationExtensions)

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
`AzureKeyVaultSecretsStorageService` and `DefaultAzureServiceTokenService` directly.
