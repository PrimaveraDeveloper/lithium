# Primavera.Hydrogen.EventBus.InMemory

**Class library that contains types that define generic event bus services that use an in-memory implementation.**

## Service options

The `InMemoryEventBusOptions` entity defines the configuration for both publication and subscription operations. The below table provides a brief description of each property of the entity:

| Property | Description                                                                                                                       | Optional? |
| -------- | --------------------------------------------------------------------------------------------------------------------------------- | --------- |
| Strategy | The strategy to be applied to the service: multi-threaded, single-threaded or single-threaded with parallel subscriber execution. | No.       |

## Implementing the service

The service implementation should be registered using the `AddInMemoryEventBus` extension method for `IServiceCollection`.

The extension method expects an `InMemoryEventBusOptions` entity as parameter.

```csharp
/// <summary>
/// Gets the event bus service.
/// </summary>
/// <returns>The event bus service.</returns>
private static IEventBusService GetEventBusService()
{
    IServiceCollection services = new ServiceCollection();

    services.AddInMemoryEventBus(
        (options) =>
        {
            options.Strategy = InMemoryEventBusStrategy.MultiThreaded;
        });

    return services.BuildServiceProvider().GetRequiredService<IEventBusService>();
}
```

## Publishing an event

In order to publish an event, a typed `InMemoryEventBusEvent` entity needs to be instantiated.

The respective `InMemoryEventBusEvent` entity should then be passed as an argument to the `Publish` or `PublishAsync` method of the `InMemoryEventBusService` instance.

If a path is specified in the `Publish` or `PublishAsync` method, the event will be sent as a [unicast](https://en.wikipedia.org/wiki/Unicast) to the respective path. Otherwise, the event will be [broadcasted](https://en.wikipedia.org/wiki/Broadcasting_(networking)) to all the existing paths in the event bus service.

![Unicasting scenario](_assets/inmemory-eventbus-unicasting.png)

![Broadcasting scenario](_assets/inmemory-eventbus-broadcasting.png)

```csharp
/// <summary>
/// Publishes, as a broadcast to all the event bus service paths, an event containing a versioned message.
/// </summary>
/// <param name="eventBus">The event bus.</param>
private static void BroadcastVersionedMessageEvent(IEventBusService eventBus)
{
    string myMessage = "Hack the planet!";

    IEventBusEvent<string> @event = new InMemoryEventBusEvent<string>()
    {
        Body = myMessage // Since we are typing the event as a string, we need to provide a string to the event body.
    };

    @event.Properties.Add("Version", "1");

    eventBus.Publish(@event);
}
```

## Subscribing to events

The event subscription operation involves the creation of a method capable of handling incoming events for a given type.

```csharp
/// <summary>
/// Handles the specified <see cref="IEventBusEvent{T}"/>.
/// </summary>
/// <typeparam name="T">The event type.</typeparam>
/// <param name="event">The event.</param>
/// <returns>The <see cref="Task"/> representing the operation.</returns>
private static Task<bool> HandleAsync<T>(IEventBusEvent<T> @event)
{
    try
    {
        SmartGuard.NotNull(() => @event, @event);

        Console.WriteLine(@event.Body);

        return Task.FromResult(true);
    }
    catch (EventBusServiceException e)
    {
        Console.WriteLine($"\nException has been raised: '{e.Message}'.");

        return Task.FromResult(false);
    }
}
```

The subscription source path and a delegate representing the handling method previously created should then be passed to the `Subscribe` or `SubscribeAsync` method of the `InMemoryEventBusService` instance.

A collection of subscription filters defined by `IEventBusFilters` can also be provided.

```csharp
/// <summary>
/// Subscribes versioned message events.
/// </summary>
/// <param name="eventBus">The event bus.</param>
private static void SubscribeVersionedMessageEvents(IEventBusService eventBus)
{
    EventBusHandlerDelegate<string> messageHandler = HandleAsync;

    IEventBusFilters<string> messageFilters = new InMemoryEventBusFilters<string>();

    messageFilters.Filters.Add("Version", "1");

    eventBus.Subscribe("my-path", messageHandler, messageFilters);
}
```

The **type of the event in conjunction with the subscription filters** are then used to correlate the events that should be sent to the subscriber.

As can be seen in the above example, events with the **logical conjunction** of the following characteristics are being subscribed:

- Events of `string` type;
- Originated from *my-path* path;
- Where a event property with key *Version* and value *1* exists.

The logical conjunction is obtained through hashing operations. Be aware that different subscriptions filters will result in distinct hashes.

## Unsubscribing from events

Unsubscribing from a event type is done by invoking the `Unsubscribe` or `UnsubscribeAsync` method of an `InMemoryEventBusService` with the subscription source path and respective `T` type.

A collection of subscription filters defined by `IEventBusFilters` can also be provided.

```csharp
/// <summary>
/// Unsubscribes from versioned message events.
/// </summary>
/// <param name="eventBus">The event bus.</param>
private static void UnsubscribeVersionedMessageEvents(IEventBusService eventBus)
{
    IEventBusFilters<string> messageFilters = new InMemoryEventBusFilters<string>();

    messageFilters.Filters.Add("Version", "1");

    eventBus.Unsubscribe("my-path", messageFilters);
}
```
