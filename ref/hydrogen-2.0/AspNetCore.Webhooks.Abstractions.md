# Primavera.Hydrogen.AspNetCore.Webhooks.Abstractions

**Class library that contains types that define abstractions for providing webhooks in ASP.NET Core applications.**

## Webhooks

Webhooks are automated messages - invoked via HTTP - that are sent to subscribers when certain events occur within the publisher context.

Hydrogen includes 2 services that allow implementing this feature in applications and micro services:

- One to manage the subscriptions from a given client application to the webhooks available.
- One to publish the webhooks events and route them to the adequate subscribers.

## Store subscriptions (`IWebhooksSubscriptionsService`)

The `IWebhooksSubscriptionsService` provides operations to manage the subscriptions (create, update, delete, etc.). This service is designed to be exposed, for example, via API routes on the webhooks server, so that client applications can subscribe the webhooks available.

Example:

```csharp
IWebhooksSubscriptionService service = this.ServiceProvider.GetRequiredService<IWebhooksSubscriptionService>();

CreateWebhookSubscriptionRequest request = new CreateWebhookSubscriptionRequest()
{
    ClientId = "MYAPP",
    EventNames = new List<string>()
    {
        "Customer_Created",
        "Customer_Deleted"
    },
    CallbackUrl = new Uri("https://myserver.com/customers/callback"),
    Secret = "a-shared-secret",
    Active = true
}

OperationResult<string> result = await service.CreateSubscriptionAsync(request).ConfigureAwait(false);
if (result.IsFailure)
{
    // (...)
}

string subscriptionId = result.Data;
```

> Notice that a client can subscribe more than one event (webhook). This means that it can receive different events on the same callback action. It can do that or it can choose to subscribe one time for each different event.

### Filters

The webhook subscription can also include filters, if the publisher supports and allows that:

```csharp
IWebhooksSubscriptionService service = this.ServiceProvider.GetRequiredService<IWebhooksSubscriptionService>();

CreateWebhookSubscriptionRequest request = new CreateWebhookSubscriptionRequest()
{
    ClientId = "MYAPP",
    EventNames = new List<string>()
    {
        "Customer_Created",
        "Customer_Deleted"
    },
    CallbackUrl = new Uri("https://myserver.com/customers/callback"),
    Secret = "a-shared-secret",
    Filters = Dictionary<string, string>()
    {
        ["Subscription"] = "20000/20001"
    },
    Active = true
}
```

These filters provided by the client will be compared against the filters provided by the publisher, when it publishes these events. The client callback will be called **only if ALL filters match**.

## Publish webhooks events (`IWebhooksService`)

The `IWebhooksService` provides the operation to publish a specific webhook event with the associated payload (data, serialized in JSON):

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

All clients that subscribed the `Customer_Created` event will receive a callback.

### Filters

Again, depending on the publisher's own logic, it may support filters. In that case, it should provide the appropriate filter values when publishing events:

```csharp
CustomerPayload payload = new CustomerPayload()
{
    (...)
};

await this.WebhooksService.PublishEventAsync(
    "Customer_Created",
    payload,
    new Dictionary<string, string>()
    {
        ["Subscription"] = "30000/30001"
    })
    .ConfigureAwait(false);
```

Be aware that the filters names and values logic is a responsibility of the publisher. The Hydrogen services make no assumptions on this except the following:

- The filter values are treated as string values.
- A match between the publisher's and the client's filters occurs only if ALL filters match (comparison is case-insensitive)