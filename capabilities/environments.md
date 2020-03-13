# Capability - Environments

> Tags: Development, Staging, Preview, Production, ASPNETCORE_ENVIRONMENT

The microservices Web applications run in the infrastructure in a specific environment (deployment). It can be one of:

- Development
- Staging
- Preview
- Production

> Notice that Preview is expected to be exactly equal to Production, thus typically it is not considered in application code.

This concept of the environment in which an application is running on its central to .NET Core. It is controlled by the following configuration option (typically defined as an environment variable):

- `ASPNETCORE_ENVIRONMENT`

The same setting is available from the `IHostEnvironment` service and allows to adapt the microservice logic according to the active environment. This is used thoroughly in the Lithium microservices generated code. For example:

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

> Notice the `UseDevelopmentSettings` property (in `HostBuilderBase`). This allows mimicking the development environment even when the responding variable is set to a different value. Which is useful to have "development-like" behavior during application startup, for example, to detect errors and exceptions during that process (before the [error handling](./error-handling.md) middleware and [telemetry](./telemetry.md) are activated). This property can only be set in code.