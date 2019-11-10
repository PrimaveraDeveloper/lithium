# Primavera.Hydrogen.EventBus.InMemory

**Class library that contains types that define generic event bus services that use an in-memory implementation.**

## About

This class library is a concrete implementation of the [Primavera.Hydrogen.EventBus](EventBus.Abstractions.md) abstractions that uses an in-memory implementation.

This document contains explanations and examples on how to consume the class library.


## Implementing the service

The service can be implemented by simply instantiating the `InMemoryEventBus` class. There's no configuration associated.

```csharp
/// <summary>
/// Gets the event bus service.
/// </summary>
/// <returns>The event bus service.</returns>
private static IEventBus GetEventBusService()
{
    return new InMemoryEventBus();
}
```

## Publishing an event

In order to publish an event, a typed `InMemoryEventBusEvent` entity needs to be instantiated.

The respective `InMemoryEventBusEvent` entity should then be passed as an argument to the `Publish` or `PublishAsync` method of the `InMemoryEventBus` instance.

```csharp
/// <summary>
/// Publishes an event containing a message.
/// </summary>
/// <param name="eventBus">The event bus.</param>
private static async void PublishMessageEventAsync(IEventBus eventBus)
{
    string myMessage = "Hack the planet!";

    IEventBusEvent<string> @event = new InMemoryEventBusEvent<string>()
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

An instance of the typed handler class should then be passed to the `Subscribe` method of the `InMemoryEventBus` instance to be properly registered.

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

Unsubscribing from a event type is done by invoking the `Unsubscribe` method of an `InMemoryEventBus` with the respective `T` type.

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

