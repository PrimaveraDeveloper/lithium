# How to subscribe events from Event Bus in a Microservice

This guide describes how to setup a micro service to subscribe events from Event Bus.

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

You are now ready to receive events.

## Adding the event handler

To receive a given event, you will first need to create an event handler (a type implementing `IEventBusEventHandler<T>`).

Add a new folder named `Handlers` to the `CustomCode` folder in the `WebApi` project. Then add a class named `MyEventHandler` and implement the logic for handling the event.

Here's an example:

```csharp
/// <summary>
/// Defines an handler for an event from Event Bus.
/// </summary>
/// <seealso cref="IEventBusEventHandler{T}" />
internal partial class MyEventHandler : IEventBusEventHandler<string>
{
    #region Private Properties

    private IServiceProvider ServiceProvider
    {
        get;
    }

    private ILogger Logger
    {
        get
        {
            return this.ServiceProvider.GetRequiredService<ILogger<MyEventHandler>>();
        }
    }

    #endregion

    #region Constructors

    /// <summary>
    /// Initializes a new instance of the <see cref="MyEventHandler"/> class.
    /// </summary>
    /// <param name="serviceProvider">The service provider.</param>
    public MyEventHandler(IServiceProvider serviceProvider)
    {
        // Validation

        SmartGuard.NotNull(() => serviceProvider, serviceProvider);

        // Set properties

        this.ServiceProvider = serviceProvider;
    }

    #endregion

    #region Public Methods

    /// <inheritdoc />
    public Task<bool> HandleAsync(IEventBusEvent<string> eventBusEvent)
    {
        // Event received

        this.Logger.LogInformation($"Event received: {eventBusEvent.Body}.");

        // Handled

        return Task.FromResult(true);
    }

    #endregion
}
```

## Subscribing the event

To receive events, the microservice needs to subscribe event topics.

The easiest way of implementing this subscription logic is to add a background service and have that background service subscribe the events (registering the corresponding handlers).

1. Open the service model.
2. Add a background service named `SubscribeEvents`.
3. Validate and save the model.
4. Transform all text templates.

Now you will be to implement the custom logic for this background service.

Add a folder named `BackgroundServices` under the `CustomCode` folder in Web API. Add a class named `SubscribeEvents` and override `ExecuteAsync()` to subscribe the Event Bus event.

Here's an example:

```csharp
[SuppressMessage("StyleCop.CSharp.DocumentationRules", "SA1601:Partial elements should be documented")]
internal partial class SubscribeEventsService
{
    #region Private Properties

    private IEventBusService EventBusService
    {
        get
        {
            return this.ServiceProvider.GetRequiredService<IEventBusService>();
        }
    }

    #endregion

    #region Protected Methods

    /// <inheritdoc />
    protected override async Task ExecuteAsync(CancellationToken cancellationToken)
    {
        // Subscribe events

        IEventBusEventHandler<string> handler = new MyEventHandler(this.ServiceProvider);

        await this.EventBusService
            .SubscribeAsync("sample", "sample", handler, cancellationToken)
            .ConfigureAwait(false);
    }

    #endregion
}
```
