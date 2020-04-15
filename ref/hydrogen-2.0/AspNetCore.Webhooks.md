# Primavera.Hydrogen.AspNetCore.Webhooks

**Class library that contains types that define services for providing webhooks in ASP.NET Core applications.**

This library provides a default implementation of the webhooks services with the following characteristics:

- Subscriptions are stored in table storage (service `TableStorageWebhooksSubscriptionsService`).
- Webhooks events are published (service `InProcessWebhooksService`) and sent to subscribers using a background service automatically setup in the client application. This background service uses a background queue. It does not retry publications when the client application does not respond, and it will stop trying to publish a given webhook event if it fails a certain number of times consecutively.

The implementation is extensible in the sense that these default services can be replaced and other services may be registered. This is ensured by the webhooks builder.

## Webhooks builder (`IWebhooksBuilder`)

To setup webhooks on an application the following extension method for IServiceCollection should be used:

```csharp
IServiceCollection services = (...);

IWebhooksBuilder builder = services.AddWebhooks(
    (options) =>
    {
        // (...)
    });
```

> This method creates the builder, the configuration options (`WebhooksOptions`) and the subscription service (`IWebhooksSubscriptionsService`). It does NOT register the publisher.

The builder then allows registering additional services.

## `WebhooksOptions`

This configuration class provides the following options:

- `ApplicationName` (required) (no default value) - the application name.
- `EventsSupported` (required) (no default value) - the description of all the webhook events supported in the application.
- `CallbackTimeout` (default value is 5 seconds) - the timeout when invoking the subscribers callbacks.
- `CallbackMaxRetries` (default is 3) - the maximum number of times the callback for a subscription will be invoked (if it fails) before the subscription is inactivated.
- `Events` (optional) - allows the application to receive events from the webhooks services.

### Events supported

The application needs to describe all the webhooks events that it may trigger. This is required so that the `IWebhooksSubscriptionsService` can manage client application's subscriptions:

```csharp
IWebhooksBuilder builder = services.AddWebhooks(
    (options) =>
    {
        // (...)

        options.EventSupported = new List<WebhookEvent>()
        {
            new WebhookEvent(
                "Entity_Verb",
                new Dictionary<string, string>()
                {
                    ["en"] = "The event description.",
                    // (...)
                },
                new Dictionary<string, string>()
                {
                    ["en"] = "The payload description.",
                    // (...)
                }
            ),
            // (...)
        }

        // (...)
    }
)
```

Notice that each event requires the following information:

- The event name, in the form `Entity_Verb`.
- The event descriptions (indexed by culture).
- The payload descriptions (indexed by culture).

### Services events

The application can subscribe events raised by the webhooks services using the `Event` option. This exists to support custom behavior on new subscriptions, when publishing events, etc.

For example:

```csharp
IWebhooksBuilder builder = services.AddWebhooks(
    (options) =>
    {
        // (...)

        options.Events.OnCreatingSubscriptionAsync = (context, cancellationToken) =>
        {
            // (...)
        }

        options.Events.OnPublishedWebhookEventAsync = (context, cancellationToken) =>
        {
            // (...)
        }

        // (...)
    }
)
```

## Store subscriptions (`TableStorageWebhooksSubscriptionsService`)

The `TableStorageWebhooksSubscriptionsService` implements the `IWebhooksSubscriptionsService` using Table Storage. It stores subscriptions using 3 tables:

- `WebhooksSubscriptions` - stores all the subscriptions.
- `WebhooksSubscriptionsClients` - indexes the subscriptions per client application.
- `WebhooksSubscriptionsEvents` - indexes the subscriptions per event name.

## Publish webhooks events (`InProcessWebhooksService`)

The `InProcessWebhooksService` implements the `IWebhooksService` using a background service.

As stated above, when the webhooks builder is initialized only the subscriptions service is registered. This means that, to publish webhooks, the `IWebhooksService` needs to be registered using the builder.

This is achieved using an extension method on IWebhooksBuilder:

```csharp
IServiceCollection services = (...);

IWebhooksBuilder builder = services.AddWebhooks(
    (options) =>
    {
        // (...)
    });

builder.AddPublisherInProcess();
```

Publishing a webhook event is straightforward:

```csharp
private IWebhooksService WebhooksService
{
    get
    {
        return this.HttpContext.RequestServices.GetRequiredService<IWebhooksService>();
    }
}

CustomerPayload payload = new CustomerPayload()
{
    (...)
};

await this.WebhooksService.PublishEventAsync(
    "Customer_Created",
    payload)
    .ConfigureAwait(false);
```

### Callback action

When a given event is published by the application, the webhooks service will determine the subscribers and invoke their callback action one at a time.

This message will have the following characteristics:

- The request is a POST.
- It includes the payload in the request body.
- It includes a set of headers that provide additional information about the callback to the client application.
- It is expected to return OK as a response. Otherwise it will be considered failed.
- The callback is expected to respond within period of time (see `WebhooksOptions.CallbackTimeout`). Otherwise it will be considered as timed-out.

### Callback payload

The payload is provided as an object by the application, serialized to JSON and then sent to the client applications' callback actions in the request body.

### Callback headers

The webhook also includes the following headers in the callback:

- `X-Webhook-Application` - the name of the application that is sending the webhook (see WebhooksOptions.ApplicationName).
- `X-Webhook-Event` - the name of the event that triggered the callback (useful because the same callback can respond to callbacks from a single subscription that subscribes multiple webhook events).
- `X-Webhook-Delivery` - a unique identifier of the callback (retries should have the same identifier).
- `X-Webhook-Signature` - the HMAC SHA256 signature of the payload.

### Signature

The signature can be used by the client application to verify that the payload was not tempered with.

After the payload is serialized, the service will compute a HMAC SHA256 hash of the JSON string (the same that is sent to the client). This hash is computed using the shared secret provided by the client application when it subscribes the webhook.

### Limitations

The current implementation uses a background service and a background queue to deliver webhooks events to subscriptions, thus it is called "in-process". This is the most important limitation of this implementation. It does NOT guarantee the delivery of events to all subscribers in all cases. It there is any problem with the queue or the background service, the events occurred during that period will not be delivered.

The service also does not implement any retry mechanism when the client application callback fails for some reason. It simply marks it as failed and does not retry to send the event.

Furthermore, if the callback for a given subscription fails consecutively a certain number of times (see `WebhooksOptions.CallbackMaxRetries`), the service stops trying to send events by rendering the subscription inactive (the client application will be required to activate it again after the problem is resolved).

## Validation

The service enforce string validation rules on various pieces of data used for describing events, and subscriptions. The regular expressions used are the following:

- Application name: `^([A-Z])(\d|[A-Z])*$`
- Client identifier: `^(\d|[a-z]|[A-Z]|-)*$`
- Subscription identifier: `^(\d|[a-z]|[A-Z]|-)*$`
- Event name: `^([A-Z])([a-z])*(_)([A-Z])([a-z])*$`

## Future work

As stated above, the current version of these services have some limitations. In the future, these services may evolve or new services may me implemented to implement features like:

- Resilience when invoking the callback actions.
- Retry policies when invoking the callback actions.
- Events storage (e.g. Event Bus).
- Different kinds of webhooks (e.g. fire and forget, ensure delivery, etc.)
- Etc.