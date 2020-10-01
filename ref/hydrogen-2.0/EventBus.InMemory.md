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

The respective event bus path identifier and the recently instantiated `InMemoryEventBusEvent` entity should then be passed as arguments to the `Publish` or `PublishAsync` method of the `InMemoryEventBusService` instance.

The event will be finally sent as an [unicast](https://en.wikipedia.org/wiki/Unicast) to the specified path. Inside the path, the event will be [broadcasted](https://en.wikipedia.org/wiki/Broadcasting_(networking)) to all subscriptions. If the event properties matches at least one existing `InMemoryEventBusSubscriptionRule`, the event will be stored in the corresponding subscription during the defined event [time to live](https://en.wikipedia.org/wiki/Time_to_live).

Lastly, it should be noted that the type of the event is itself an implicit event property.

```csharp
/// <summary>
/// Publishes - as an unicast to the specified event bus path - an event containing a versioned message.
/// </summary>
/// <param name="eventBus">The event bus.</param>
private static void PublishVersionedMessageEvent(IEventBusService eventBus)
{
    IEventBusEvent<string> @event = new InMemoryEventBusEvent<string>()
    {
        Body = "Hack the planet!" // Since we are typing the event as a string, we need to provide a string to the event body.
    };

    @event.Properties.Add("Version", "1");

    eventBus.Publish("my-path", @event);
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

The event bus path identifier, subscription identifier and a delegate representing the previously created handling method should then be passed as arguments to the `Subscribe` or `SubscribeAsync` method of the `InMemoryEventBusService` instance.

```csharp
/// <summary>
/// Subscribes versioned message events.
/// </summary>
/// <param name="eventBus">The event bus.</param>
private static void SubscribeVersionedMessageEvents(IEventBusService eventBus)
{
    EventBusHandlerDelegate<string> messageHandler = HandleAsync;

    eventBus.Subscribe("my-path", "my-subscription", messageHandler);
}
```

## Unsubscribing from events

Detaching from a subscription (unsubscribing) is done by invoking the `Unsubscribe` or `UnsubscribeAsync` method of an `InMemoryEventBusService` with the event bus path identifier, subscription identifier and the respective `T` as arguments.

```csharp
/// <summary>
/// Unsubscribes from versioned message events.
/// </summary>
/// <param name="eventBus">The event bus.</param>
private static void UnsubscribeVersionedMessageEvents(IEventBusService eventBus)
{
    eventBus.Unsubscribe<string>("my-path", "my-subscription");
}
```

## Service management

The service structural entities (such as paths, subscriptions and subscription rules) should be provisioned using the respective management service.

The management service can be registered thought the `AddInMemoryEventBusManager` extension method for `IServiceCollection`.

The extension method also expects an `InMemoryEventBusOptions` entity as an argument.
