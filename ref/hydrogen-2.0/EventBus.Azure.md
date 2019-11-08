# Primavera.Hydrogen.EventBus.Azure

**Class library that contains types that define generic event bus services that use Microsoft Azure Service Bus.**

## About

This class library is a concrete implementation of the [Primavera.Hydrogen.EventBus](EventBus.Abstractions.md) abstractions that uses [Microsoft Azure Service Bus](https://docs.microsoft.com/pt-pt/azure/service-bus-messaging/service-bus-messaging-overview).

This document contains explanations and examples on how to consume the class library.

## Service options

The `AzureEventBusOptions` entity defines the configuration for both publication and subscription operations. The below table provides a brief description of each property of the entity:

| Property                                                 | Description                                                                                            | Optional? |
|----------------------------------------------------------|--------------------------------------------------------------------------------------------------------|-----------|
| ConnectionString                                         | The Azure Service Bus instance connection string.                                                      | No.       |
| PublisherOptions.PublicationTopic                        | The Azure Service Bus topic that the events should be published to.                                    | No.       |
| SubscriberOptions.SubscriptionTopic                      | The Azure Service Bus topic whose events should be subscribed.                                         | No.       |
| SubscriberOptions.SubscriptionIdentifier                 | The unique identifier of the client that subscribes to the Azure Service Bus topic.                    | No.       |
| SubscriberOptions.EventHandlerOptions.AutoComplete       | Flag that defines if a received event will be marked as processed as soon as it arrives to the client. | No.       |
| SubscriberOptions.EventHandlerOptions.MaxConcurrentCalls | The maximum number of concurrent calls to the Azure Service Bus instance.                              | No.       |


## Implementing the service

The service implementation should be registered using the `AddAzureEventBus` extension method for `IServiceCollection`.

The extension method expects an `AzureEventBusOptions` entity as parameter.

```csharp
/// <summary>
/// Gets the event bus service.
/// </summary>
/// <returns>The event bus service.</returns>
private static IEventBus GetEventBusService()
{
    IServiceCollection services = new ServiceCollection();

    services.AddAzureEventBus(
        (options) =>
        {
            AzureEventBusEventHandlerOptions eventHandlerOptions = new AzureEventBusEventHandlerOptions(false, 10);

            options.ConnectionString = "my-connection-string";
            options.PublisherOptions = new AzureEventBusPublisherOptions("my-topic");
            options.SubscriberOptions = new AzureEventBusSubscriberOptions("my-topic", "my-topic-subscription-identifier", eventHandlerOptions);
        });

    return services.BuildServiceProvider().GetRequiredService<IEventBus>();
}
```

## Publishing an event

In order to publish an event, a typed `AzureEventBusEvent` entity needs to be instantiated.

The respective `AzureEventBusEvent` entity should then be passed as an argument to the `Publish` or `PublishAsync` method of the `AzureEventBus` instance.

```csharp
/// <summary>
/// Publishes an event containing a message.
/// </summary>
/// <param name="eventBus">The event bus.</param>
private static async void PublishMessageEventAsync(IEventBus eventBus)
{
    string myMessage = "Hack the planet!";

    IEventBusEvent<string> @event = new AzureEventBusEvent<string>()
    {
        Body = myMessage // Since we are typing the event as a string, we need to provide a string to the event body.
    };

    await eventBus.PublishAsync(@event).ConfigureAwait(false);
}
```

## Subscribing to events

The event subscription operation involves the creation of a typed handler class, implemented through  the `IEventBusEventHandler`, that will be responsible for handling incoming events of the same type.

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
    public Task<bool> Handle(IEventBusEvent<string> eventBusEvent)
    {
        SmartGuard.NotNull(() => eventBusEvent, eventBusEvent);

        try
        {
            Console.WriteLine($"Incoming message: '{eventBusEvent.Body}'");

            return Task.FromResult(true);
        }
        catch (EventBusException e)
        {
            Console.WriteLine($"Message handler has encountered an exception: '{e.Message}'");

            return Task.FromResult(false);
        }
    }

    #endregion
}
```

An instance of the typed handler class should then be passed to the `Subscribe` method of the `AzureEventBus` instance to be properly registered.

```csharp
/// <summary>
/// Subscribes the message events.
/// </summary>
/// <param name="eventBus">The event bus.</param>
private static void SubscribeMessageEvents(IEventBus eventBus)
{
    IEventBusEventHandler<string> messageEventHandler = new MessageEventHandler();

    eventBus.Subscribe(messageEventHandler);
}
```

## Unsubscribing from events

Unsubscribing from events is done by simply invoking the `Unsubscribe` method of an `AzureEventBus` instance.

Keep in mind that the above method will close the connection as soon as possible and will then dispose the subscriber client. A new instance needs to be created in order to subscribe to events again.

```csharp
/// <summary>
/// Unsubscribes from message events.
/// </summary>
/// <param name="eventBus">The event bus.</param>
private static void UnsubscribeMessageEvents(IEventBus eventBus)
{
    eventBus.Unsubscribe<string>();
}
```