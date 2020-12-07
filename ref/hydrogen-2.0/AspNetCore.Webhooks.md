# Primavera.Hydrogen.AspNetCore.Webhooks

**Class library that contains types that define services for providing webhooks in ASP.NET Core applications.**

This library provides a default implementation of the webhooks services available in Primavera.Hydrogen.AspNetCore.Webhooks.Abstractions with the following characteristics:

- Subscriptions are stored in table storage (service `TableStorageSubscriptionsService`).
- Webhooks events are published and sent to subscribers using a background service automatically setup in the client application. This background service (service `BackgroundQueuePublishService`) uses a background queue.
- Failed webhooks - that is, webhooks events that could not be published to a subscriber or whose subscriber responded with an unexpected status - will be retried by a different background service (service `TimedRetryService`) up to a certain number of times.

The implementation is extensible in the sense that these default services can be replaced and other services may be registered. This is ensured by the webhooks builder.

## Webhooks builder (`IWebhooksBuilder`)

To setup webhooks on an application the following extension method for `IServiceCollection` should be used:

```csharp
IServiceCollection services = (...);

IWebhooksBuilder builder = services.AddWebhooks(
    (options) =>
    {
        // (...)
    });
```

> This method creates the builder and the configuration options (`WebhooksOptions`). It does NOT register any other service (neither the publisher or the storage).

The builder then allows registering additional services.

## `WebhooksOptions`

This configuration class provides the following options:

- `ApplicationName` (required) (no default value) - the application name.
- `EventsSupported` (required) (no default value) - the description of all the webhook events supported by the application.
- `CallbackTimeout` (default value is 5 seconds) - the timeout when invoking the subscribers callbacks.
- `CallbackMaxRetries` (default is 10) - the maximum number of times the callback for a subscription will be invoked (if it fails) before the subscription is inactivated.
- `Events` (optional) - allows the application to receive events from the webhooks services themselves.

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

- The event name (in the form `Entity_Verb`).
- The event descriptions (indexed by culture).
- The payload descriptions (indexed by culture).

### Services events

The application can subscribe events raised by the webhooks services using the `Event` configuration option. This allows to implement application-specific behavior on new subscriptions, when publishing events, etc.

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

## Store subscriptions (`TableStorageSubscriptionsService`)

The `TableStorageSubscriptionsService` implements the `ISubscriptionsService` using Table Storage. It stores subscriptions using 4 tables:

- `WebhooksSubscriptions` - stores all the subscriptions.
- `WebhooksSubscriptionsClients` - indexes the subscriptions per client application.
- `WebhooksSubscriptionsEvents` - indexes the subscriptions per event name.
- `WebhooksFailing` - stores the webhook callback requests that failed.

To store the subscriptions, the `ISubscriptionsService` needs to be registered using the builder. This is achieved using an extension method on `IWebhooksBuilder`:

```csharp
IServiceCollection services = (...);

IWebhooksBuilder builder = services.AddWebhooks(
    (options) =>
    {
        // (...)
    });

builder.AddAzureTableStorage();
```

## Publish webhooks events (`BackgroundQueuePublishService`)

The `BackgroundQueuePublishService` implements the `IWebhooksService` using a background queue.

To publish webhooks, the `IWebhooksService` needs to be registered using the builder:

```csharp
IServiceCollection services = (...);

IWebhooksBuilder builder = services.AddWebhooks(
    (options) =>
    {
        // (...)
    });

builder.AddBackgroundQueuePublisher();
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

> Filters are supported when publishing events. For more information see [AspNetCore.Webhooks.Abstractions](./AspNetCore.Webhooks.Abstractions.md).

### Callback action

When a given event is published by the application, the webhooks service will determine the matching subscribers and invoke their callback actions, one at a time.

This message will have the following characteristics:

- The request is a **POST**.
- It includes the **payload in the request body**.
- It includes a **set of headers** that provide additional information about the callback to the client application.
- It is **expected to return OK** as a response. Otherwise it will be considered failed.
- The callback is **expected to respond within period of time** (see `WebhooksOptions.CallbackTimeout`). Otherwise it will be considered as timed-out.

### Callback payload

The payload is provided as an object by the application, serialized to JSON and then sent to the client applications' callback actions in the request body.

### Callback headers

The webhook also includes the following headers in the callback:

- `X-Webhook-Application` - the name of the application that is sending the webhook (see WebhooksOptions.ApplicationName).
- `X-Webhook-Event` - the name of the event that triggered the callback (useful because the same callback can respond to callbacks from a single subscription that subscribes multiple webhook events).
- `X-Webhook-Delivery` - a unique identifier of the callback (retries should have the same identifier).
- `X-Webhook-Properties` - a string containing all properties provided by the client application when the subscription was created/updated.
- `X-Webhook-Signature` - the HMAC SHA256 signature of the payload.

### Signature

The signature can be used by the client application to verify that the payload was not tempered with.

After the payload is serialized, the service will compute a HMAC SHA256 hash of the JSON string (the same that is sent to the client). This hash is computed using the shared secret provided by the client application when it subscribes the webhook.

### Limitations

The current implementation uses a background service and a background queue to deliver webhooks events to subscriptions, thus it is called "in-process". This is the most important limitation of this implementation. It does NOT guarantee the delivery of events to all subscribers in all cases. If there is any problem with the queue or the background service, the events occurred during that period will not be delivered.

Furthermore, if the callback for a given webhook event fails consecutively a certain number of times (see `WebhooksOptions.CallbackMaxRetries`), the service stops trying to send any event for that subscription by rendering it inactive (the client application will be required to activate it again after the problem is resolved).

## Retry (`TimedRetryService`)

The `TimedRetryService` implements a simple retry mechanism for webhook callbacks failing.

This service also needs to be registered using the builder (otherwise, failed webhooks will not be retried):

```csharp
IServiceCollection services = (...);

IWebhooksBuilder builder = services.AddWebhooks(
    (options) =>
    {
        // (...)
    });

builder.AddTimedRetry();
```

The retry policy applies the following rules:

- A failed webhook callback request is any invocation of a webhook subscription callback for a specific event that failed for any reason.
- These requests will fail if the subscriber fails to respond with 200 (OK) status code, the request times-out or any other exception occurs.
- The failed callbacks will be retried up to the maximum number of times defined in the configuration options (see `CallbackMaxRetries`).
- Each retry will be executed after a period of time (exponential) elapses since the last invocation. It starts at 10 seconds after the first try.

## Validation

The service enforces string validation rules on various pieces of data used for describing events, and subscriptions. The regular expressions used are the following:

- Application name: `^([A-Z])(\d|[A-Z])*$`
- Client identifier: `^(\d|[a-z]|[A-Z]|-)*$`
- Subscription identifier: `^(\d|[a-z]|[A-Z]|-)*$`
- Event name: `^([A-Z])([a-z]|[A-Z])*(_)([A-Z])([a-z])*$`
