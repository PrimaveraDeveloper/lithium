# Capability - Logging, Tracing and Telemetry

> Tags: Application Insights, Logger

Lithium microservices rely on [.NET Core Logging](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging) and [Azure Application Insights](https://docs.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview) to implement these features.

## Logging

Logging in a microservice is configured in the `HostBuilder`.

`HostBuilderBase.ConfigureLogging()`:

```csharp
protected virtual void ConfigureLogging(HostBuilderContext context, ILoggingBuilder builder)
{
    // Clear default providers

    builder
        .ClearProviders();

    // Configuration

    builder
        .AddConfiguration(context.Configuration.GetSection("Logging"));

    // Console and debug

    bool useDevelopmentSettings = this.UseDevelopmentSettings || context.HostingEnvironment.IsDevelopment();

    if (useDevelopmentSettings)
    {
        builder
            .AddConsole()
            .AddDebug();
    }
}
```

Notice that the configuration options are defined in a configuration section called `Logging`, which is automatically generated:

`GeneratedCode\appsettings.gen.json`:

```json
"Logging": {
    "ApplicationInsights": {
        "LogLevel": {
            "Default": "Warning"
        }
    },
    "Console": {
        "LogLevel": {
            "Default": "Warning"
        }
    },
    "Debug": {
        "LogLevel": {
            "Default": "Warning"
        }
    },
    "IncludeScopes": false
}
```

`GeneratedCode\appsettings-development.gen.json`:

```json
"Logging": {
    "ApplicationInsights": {
        "LogLevel": {
            "Default": "Trace"
        }
    },
    "Console": {
        "LogLevel": {
            "Default": "Trace"
        }
    },
    "Debug": {
        "LogLevel": {
            "Default": "WarniTraceng"
        }
    },
    "IncludeScopes": false
}
```

> You can override these settings in the custom application settings files.

This setup ensures that all logging requests are sent to Azure Application Insights.

`StartupBase.AddTelemetry()`:

```csharp
protected virtual void AddTelemetry(IServiceCollection services, HostConfiguration hostConfiguration)
{
    // Ignore Spelling: api

    // Validation

    SmartGuard.NotNull(() => services, services);
    SmartGuard.NotNull(() => hostConfiguration, hostConfiguration);

    // Add status code 404 telemetry initializer

    services
        .AddSingleton<Microsoft.ApplicationInsights.Extensibility.ITelemetryInitializer>(
            (provider) =>
            {
                return new StatusCodeTelemetryInitializer(provider)
                {
                    Requests = RequestTelemetryInitializerBehavior.Specified,
                    RequestPaths = new List<string>()
                    {
                        "/api/*",
                    },
                    StatusCode = HttpStatusCode.NotFound
                };
            });

    // Add Azure Insights telemetry

    services
        .AddAzureInsightsTelemetryClient()
        .AddAzureInsightsTelemetry();
}
```

> Notice the `StatusCodeTelemetryInitializer` configured above. This ensures that 404 responses from the API are not treated by Azure Application Insights as errors. There is also another initializer that is setup that collects request headers. For more information see [Primavera.Hydrogen.Azure.Telemetry](../ref/hydrogen-2.0/Telemetry.Azure.md).

## Azure Application Insights

Using App Insights requires setting specific configuration options. This is also generated in the application settings files in a section named `AzureInsightsTelemetryOptions`. For example:

```json
"AzureInsightsTelemetryOptions": {
    "DefaultProperties": {
        "Lithium-Service": "FileStore"
    },
    "DeveloperMode": false,
    "EnableAdaptiveSampling": false,
    "Enabled": true,
    "IgnoreErrors": true,
    "RequestTracking": {
        "IgnoreAvailabilityTestsRequests": true,
        "TrackRequestHeaders": true,
        "TrackResponseHeaders": false
    }
}
```

`AzureInsightsTelemetryOptions.InstrumentationKey` needs to be configured in a custom application settings file to point to a specific Azure Application Insights resource.

```json
"AzureInsightsTelemetryOptions": {
    "InstrumentationKey": "d54d583f-4ee3-487a-bfcc-9ddc17036774"
}
```

## Tracing

`ILogger` should be used to record tracing messages in the microservice code. The common way to use it is to inject the logger in the class constructor, like in the following example:

```csharp
internal sealed partial class FilesManager : IFilesManager
{
    private ILogger Logger
    {
        get;
    }

    public FilesManager(IServiceProvider serviceProvider, ILogger<FilesManager> logger)
    {
        // Validation

        SmartGuard.NotNull(() => logger, logger);

        // Set properties

        this.Logger = logger;
    }

    public async Task<IEnumerable<FileInfo>> GetFilesAsync(CancellationToken cancellationToken)
    {
        this.Logger.LogDebug($"Retrieving all files...");
        (...)
    }
}
```

## Exceptions

Be ware that unhandled exceptions are automatically tracked and recorded in Azure Application Insights. This is ensured by the telemetry middleware configured in `StartupBase.AddTelemetry()`.

## Custom Telemetry

You can record custom telemetry (like metrics for example). You should use the [ITelemetryClientService](../ref/hydrogen-2.0/Telemetry.Abstractions.md) for that purpose.