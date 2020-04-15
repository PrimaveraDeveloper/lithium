# Primavera.Hydrogen.AspNetCore.Webhooks.Abstractions

**Class library that contains types that define abstractions for providing webhooks in ASP.NET Core applications.**

## Webhooks

Webhooks are automated messages - invoked via HTTP - that are sent to subscribers when certain events occur within the publisher context.

Hydrogen includes 2 services that allow implementing this feature in applications:

- One to manage the subscriptions from a given client application to the webhooks available on another (server) application.
- One to publish the webhooks events and route them to the adequate subscribers.

## Store subscriptions (`IWebhooksSubscriptionsService`)

The `IWebhooksSubscriptionsService` provides operations to manage the subscriptions (create, update, delete, etc.). This service can be "exposed", for example, by API routes on the webhooks server to allow the client applications to subscribe the webhooks.

Example:

```csharp
IWebhooksSubscriptionService service = this.ServiceProvider.GetRequiredService<IWebhooksSubscriptionService>();

CreateWebhookSubscriptionRequest request = new CreateWebhookSubscriptionRequest()
{
    ClientId = "MYAPP",
    EventNames = new List<string>()
    {
        "Customer_Created"
    },
    CallbackUrl = new Uri("https://myserver.com/customers/callback),
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