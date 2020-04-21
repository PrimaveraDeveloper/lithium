# How to externally manage application settings using Azure AppConfiguration

This guide describes how to externally manage any microservice application settings using the Azure AppConfiguration service. For more information see: [Azure AppConfiguration Overview](https://docs.microsoft.com/en-us/azure/azure-app-configuration/overview).

By default and because of deployment conventions you can configure up to 4 independent Azure AppConfiguration environments as settings providers for any microservice. These are:

- **PrimaryAppConfigurationService** - Contains **shared settings** between several microservices.

- **PrimaryAppConfigurationServiceFailover** - A replica of the
"PrimaryAppConfigurationService" and serves as a failover system.

- **SecondaryAppConfigurationService** - Contains **specific settings** for this microservice.

- **SecondaryAppConfigurationServiceFailover** - A replica of the
"SecondaryAppConfigurationService" and serves as a failover system.

## Environment Variables

To configure the Azure AppConfiguration settings provider you must set some enviroment variables:

| Name  | Required | Value Type | Description
| --- | --- | --- | --- |
| \<appConfigurationName>ConnectionString | Yes | string | The connection string used to authenticate with the Azure AppConfiguration service.
| \<appConfigurationName>KeyFilter  | No  | string | The filter to apply to all keys when querying Azure App Configuration for key-values.
| \<appConfigurationName>LabelFilter  | No  | string | The label filter to apply when querying Azure App Configuration for key-values.
| \<appConfigurationName>TrimKeyPrefix  | No  | string | Trims the provided prefix from the keys of all key-values retrieved from Azure.
| \<appConfigurationName>SentinelKey  | No  | string | This key indicates that all configuration values should be refreshed after this key is updated. For more information see [Sentinel Key](https://docs.microsoft.com/en-us/azure/azure-app-configuration/enable-dynamic-configuration-aspnet-core?tabs=core2x#add-a-sentinel-key)
| \<appConfigurationName>RefreshTimeout  | No  | int | Minimum time in seconds that must elapse before the cache is expired. Default is 30 seconds. This only works if the sentinel Key is defined.

The \<appConfigurationName> tag serves as a placeholder to identify the several configuration environments.
For the default environments the appConfigurationName values are:

| Environment | appConfigurationName | Sample Environment Keys
| --- | --- | --- |
| PrimaryAppConfigurationService | GeneralAppConfiguration | GeneralAppConfigurationConnectionString, GeneralAppConfigurationSentinelKey
| PrimaryAppConfigurationServiceFailover | GeneralAppConfigurationFailover | GeneralAppConfigurationFailoverConnectionString, GeneralAppConfigurationFailoverTrimKeyPrefix
| SecondaryAppConfigurationService | AppConfiguration | AppConfigurationConnectionString, AppConfigurationKeyFilter
| SecondaryAppConfigurationServiceFailover | AppConfigurationFailover | AppConfigurationFailoverConnectionString, AppConfigurationFailoverLabelFilter

## Externalized Configuration in Generated Code

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

    // Azure AppConfiguration

    builder.
        AddAzureConfiguration();
}
```
