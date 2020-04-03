# Primavera.Hydrogen.EventBus.Azure

**Class library that contains types that define generic event bus services that use Microsoft Azure Service Bus.**

This class library is a concrete implementation of the [Primavera.Hydrogen.EventBus](EventBus.Abstractions.md) abstractions that uses [Microsoft Azure Service Bus](https://docs.microsoft.com/pt-pt/azure/service-bus-messaging/service-bus-messaging-overview).

## Service options

The `AzureEventBusOptions` entity defines the configuration for both publication and subscription operations. The below table provides a brief description of each property of the entity:

| Property                                                 | Description                                                                                                                                                                     | Optional? |
|----------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------|
| ConnectionString                                         | The Azure Service Bus instance connection string.                                                                                                                               | No.       |
| EventHandlerOptions.AutoComplete                         | Flag that defines if a received event will be marked as processed as soon as it arrives to the client.                                                                          | No.       |
| EventHandlerOptions.MaxConcurrentCalls                   | The maximum number of concurrent calls to the Azure Service Bus instance.                                                                                                       | No.       |
| RetryStrategy                                            | The [exponential back-off retry strategy](Core.md#retry-strategies) to be applied to the Azure Service Bus instance. If null, a default strategy will be implemented.           | Yes.      |
| RetryStrategy.RetryCount                                 | The maximum number of retry attempts.                                                                                                                                           | Yes.      |
| RetryStrategy.MinBackoff                                 | The minimum backoff time.                                                                                                                                                       | Yes.      |
| RetryStrategy.MaxBackoff                                 | The maximum backoff time.                                                                                                                                                       | Yes.      |
| RetryStrategy.DeltaBackoff                               | The value that will be used to calculate a random delta in the exponential delay.                                                                                               | Yes.      |

## Implementing the service

The service implementation should be registered using the `AddAzureEventBus` extension method for `IServiceCollection`.

The extension method expects an `AzureEventBusOptions` entity as parameter.

```csharp
/// <summary>
/// Gets the event bus service.
/// </summary>
/// <returns>The event bus service.</returns>
private static IEventBusService GetEventBusService()
{
    IServiceCollection services = new ServiceCollection();

    services.AddAzureEventBus(
        (options) =>
        {
            options.ConnectionString = "my-connection-string";
            options.EventHandlerOptions = new AzureEventBusEventHandlerOptions(autoComplete: false, maxConcurrentCalls: 10);
            options.RetryStrategy = new ExponentialBackoffRetryStrategy();
        });

    return services.BuildServiceProvider().GetRequiredService<IEventBusService>();
}
```

## Publishing an event

In order to publish an event, a typed `AzureEventBusEvent` entity needs to be instantiated. The entity must be serializable through `System.Text.Json`.

The respective `AzureEventBusEvent` entity should then be passed as an argument to the `Publish` or `PublishAsync` method of the `AzureEventBusService` instance.

If a path is specified in the `Publish` or `PublishAsync` method, the event will be sent as a [unicast](https://en.wikipedia.org/wiki/Unicast) to the respective path. Otherwise, the event will be [broadcasted](https://en.wikipedia.org/wiki/Broadcasting_(networking)) to all the existing paths in the event bus service.

![Unicasting scenario](_assets/azure-eventbus-unicasting.png)

![Broadcasting scenario](_assets/azure-eventbus-broadcasting.png)

```csharp
/// <summary>
/// Publishes, as a broadcast to all the event bus service paths, an event containing a versioned message.
/// </summary>
/// <param name="eventBus">The event bus.</param>
private static void BroadcastVersionedMessageEvent(IEventBusService eventBus)
{
    string myMessage = "Hack the planet!";

    IEventBusEvent<string> @event = new AzureEventBusEvent<string>()
    {
        Body = myMessage // Since we are typing the event as a string, we need to provide a string to the event body.
    };

    @event.Properties.Add("Version", "1");

    eventBus.Publish(@event);
}
```

## Subscribing to events

The event subscription operation involves the creation of a typed handler class, implemented through the `IEventBusEventHandler`, that will be responsible for handling incoming events of the same type.

```csharp
/// <summary>
/// The message event handler.
/// </summary>
/// <seealso cref="Primavera.Hydrogen.EventBus.IEventBusEventHandler{T}"/>
public class MessageEventHandler : IEventBusEventHandler<string>
{
    #region Constructors

    /// <summary>
    /// Initializes a new instance of the <see cref="MessageEventHandler"/> class.
    /// </summary>
    public MessageEventHandler()
    {
    }

    #endregion

    #region Public Methods

    /// <summary>
    /// Handles the specified message event.
    /// </summary>
    /// <param name="eventBusEvent">The message event.</param>
    /// <returns>Boolean according execution success.</returns>
    public Task<bool> HandleAsync(IEventBusEvent<string> eventBusEvent)
    {
        SmartGuard.NotNull(() => eventBusEvent, eventBusEvent);

        try
        {
            Console.WriteLine($"Incoming message: '{eventBusEvent.Body}'");

            return Task.FromResult(true);
        }
        catch (EventBusServiceException e)
        {
            Console.WriteLine($"Message handler has encountered an exception: '{e.Message}'");

            return Task.FromResult(false);
        }
    }

    #endregion
}
```

The subscription source path and an instance of the typed handler class should then be passed to the `Subscribe` or `SubscribeAsync` method of the `AzureEventBusService` instance.

A collection of subscription filters defined by `IEventBusEventFilters` can also be provided.

```csharp
/// <summary>
/// Subscribes versioned message events.
/// </summary>
/// <param name="eventBus">The event bus.</param>
private static void SubscribeVersionedMessageEvents(IEventBusService eventBus)
{
    IEventBusEventHandler<string> messageEventHandler = new MessageEventHandler();
    IEventBusEventFilters<string> messageFilters = new AzureEventBusEventFilters<string>();

    messageFilters.Filters.Add("Version", "1");

    eventBus.Subscribe("my-path", messageEventHandler, messageFilters);
}
```

## Unsubscribing from events

Unsubscribing from an event type is done by invoking the `Unsubscribe` or `UnsubscribeAsync` method of an `AzureEventBusService` with the subscription source path and the respective `T`. 

A collection of subscription filters defined by `IEventBusEventFilters` can also be provided.

```csharp
/// <summary>
/// Unsubscribes from versioned message events.
/// </summary>
/// <param name="eventBus">The event bus.</param>
private static void UnsubscribeVersionedMessageEvents(IEventBusService eventBus)
{
    IEventBusEventFilters<string> messageFilters = new AzureEventBusEventFilters<string>();

    messageFilters.Filters.Add("Version", "1");

    eventBus.Unsubscribe("my-path", messageFilters);
}
```

## Service redundancy

Short lived outages are mitigated thought the [exponential back-off retry strategy](Core.md#retry-strategies) that this library implements.

In order to mitigate long lived outages or even disasters, a service that consumes this library should expose a monitoring endpoint that is able to probe the health status of the respective cloud message broker instance.
