# How to publish events to Event Bus from a Microservice

This guide describes how to setup a micro service to publish events to the Event Bus.

Event Bus is a component in Hydrogen that supports event-driven architectures and publish-subscribe design pattern.

## Adding the Event Bus

To add the Event Bus dependency to a microservice follow these steps:

1. Open the service model.
2. Using the context menu, add a Dependency.
3. Select `EventBus` as the dependency kind.
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

    // Event Bus

    this.AddAzureEventBus(services, hostConfiguration);
}
```

`StartupBase.AddAzureEventBus`:

```csharp
/// <summary>
/// Called when configuring services to configure the Event Bus services.
/// </summary>
/// <param name="services">The service collection.</param>
/// <param name="hostConfiguration">The host configuration.</param>
protected virtual void AddAzureEventBus(IServiceCollection services, HostConfiguration hostConfiguration)
{
    // Validation

    SmartGuard.NotNull(() => services, services);
    SmartGuard.NotNull(() => hostConfiguration, hostConfiguration);

    // Azure Event Bus

    services
        .AddAzureEventBus();
}
```

`StartupBase.AddConfiguration`:

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

    // Event bus options

    services
        .Configure<AzureEventBusOptions>(
            this.Configuration.GetSection(nameof(AzureEventBusOptions)));

    // Host configuration snapshot

    services
        .AddOptionsSnapshot<HostConfiguration>();

    // Resolve the host configuration instance

    IServiceProvider provider = services.BuildServiceProvider();
    return provider.GetRequiredService<HostConfiguration>();
}
```

## Configuration

Next you only need to set the connection string for the Azure Service Bus instance you wish to use in the application configuration, by setting `ConnectionString` in a section called `AzureEventBusOptions`:

```json
{
    "AzureEventBusOptions": {
        "ConnectionString": "(...)",
        "EventHandlerOptions": {
            "AutoComplete": false,
            "MaxConcurrentCalls": 10
        }
    }
}
```

You are now ready to publish events from custom code (from any controller, manager, etc.).

## Publishing the event

Suppose that you need to publish the event from a API controller.

First you will need to have access to the `IEventBusService` instance:

```csharp
private IEventBusService EventBusService
{
    get
    {
        return this.HttpContext.RequestServices.GetRequiredService<IEventBusService>();
    }
}
```

Then, publishing the event is straightforward. First, you build the event data, then you use the service to publish it.

Like in the following example (a unicast from one controller action):

```csharp
/// <inheritdoc />
protected override async Task<IActionResult> PublishSimpleEventCoreAsync()
{
    // Build the event

    IEventBusEvent<string> @event = new AzureEventBusEvent<string>()
    {
        Body = "This is a simple event"
    };

    // Publish the event

    await this.EventBusService.PublishAsync("sample", @event)
        .ConfigureAwait(false);

    // Done

    return this.NoContent();
}
```
