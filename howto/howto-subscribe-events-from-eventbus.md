# How to subscribe events from Event Bus in a Microservice

This guide describes how to setup a micro service to subscribe events from Event Bus.

Event Bus is a component in Hydrogen that supports event-driven architectures and publish-subscribe design pattern.

## Adding the EventBus service

First, you will need to add the `IEventBusService` to the microservice.

Add the `Primavera.Hydrogen.EventBus.Azure` package to the `WebApi` project.

Register the service in the application startup:

1. In the `CustomFolder` create a class named Startup.
2. Override `AddConfiguration()` and register the Event Bus configuration options (`AzureEventBusOptions`).
2. Override `AddAdditionalServices()` and register the service implementation.

```csharp
using System.Diagnostics.CodeAnalysis;
using Microsoft.Extensions.DependencyInjection;
using Primavera.Hydrogen.EventBus.Azure;
using Primavera.Lithium.EventBusSubscriber.WebApi.Configuration;

namespace Primavera.Lithium.EventBusSubscriber.WebApi
{
    [SuppressMessage("StyleCop.CSharp.DocumentationRules", "SA1601:Partial elements should be documented")]
    public partial class Startup
    {
        #region Protected Methods

        /// <inheritdoc />
        protected override HostConfiguration AddConfiguration(IServiceCollection services)
        {
            // Default behavior

            HostConfiguration hostConfiguration = base.AddConfiguration(services);

            // Register Event Bus configuration

            services.Configure<AzureEventBusOptions>(
                this.Configuration.GetSection(nameof(AzureEventBusOptions)));

            // Result

            return hostConfiguration;
        }

        /// <inheritdoc />
        protected override void AddAdditionalServices(IServiceCollection services, HostConfiguration hostConfiguration)
        {
            // Default behavior

            base.AddAdditionalServices(services, hostConfiguration);

            // Register Event Bus

            services.AddAzureEventBus();
        }

        #endregion
    }
}
```

You also need to setup the connection string to the Azure Service Bus instance in the `appsettings-development.json` file:

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

        this.Logger.LogDebug($"Event received: {eventBusEvent.Body}.");

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
            .SubscribeAsync("sample", handler)
            .ConfigureAwait(false);
    }

    #endregion
}
```
