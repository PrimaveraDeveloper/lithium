# How to receive webhooks from a  microservice

This guide describes how to receive webhooks from a microservice in a Web application or another microservice.

## Webhooks

Webhooks are automated messages sent to subscribers (applications or other microservices) when certain events occur within the publisher (the microservice) context.

Each webhook will have a payload - data about the event - associated with it.

To receive a webhook from a microservice, first you need to have an endpoint setup to receive the callback. Then you can subscribe the webhook with the originator microservice.

## Defining the callback

The webhook will be received in a callback, which is an endpoint in the application that will need to be passed into the microservice that publishes the webhook when subscribing it.

This endpoint is required to have the following form:

- HTTP method: POST
- Expect the event payload (as JSON) to be received in the request body.

If you want to receive the webhook from a microservice in another microservice, you can use the Lithium Service Designer to model this callback. Just follow these steps:

1. Create a new controller that will contain the callback action (e.g. "Callbacks").
2. Add a new action to that controller. This is the action for the callback (e.g. "CallbackCustomerCreated").
3. Make sure the action is a POST and this it does not include any parameter.
4. Make sure the action has its return value type set to `None`.
4. Note the action route (e.g. `POST /api/v{version:apiVersion}/callbacks/customercreated`).

The concrete implementation of the callback should read the request body to obtain the event payload. It should also inspect the request headers to validate the webhook and the concrete event name.

Here's a very simple example:

```csharp
protected override async Task<IActionResult> CustomerCreatedCallbackCoreAsync()
{
    using (StreamReader reader = new StreamReader(this.Request.Body))
    {
        string body = await reader.ReadToEndAsync().ConfigureAwait(false);

        // Do something
    }

    return this.Ok();
}
```

> The callback endpoint should return `HttpStatusCode.Ok` to indicate to the publisher that the webhook was processed successfully.

## Subscribing the webhook

The publisher microservice provides an endpoint that allows subscribing multiple webhooks in one operation. You can invoke directly but you can also use the microservice's client library to subscribe webhooks.

Here's an example:

```csharp
CreateWebhookSubscriptionRequest request = new CreateWebhookSubscriptionRequest()
{
    ClientId = "a-client-id",
    EventNames = new List<string>() { "the-event-name" },
    CallbackUrl = "the-callback-address",
    Active = true,
    Secret = "a-shared-secret"
};

ServiceOperationResult<string> response = await serviceClient.Webhooks
    .CreateSubscriptionAsync(
        request)
    .ConfigureAwait(false);
```

Notice the properties in `CreateWebhookSubscriptionRequest`:

- `ClientId` - this is a unique identifier for the application subscribing the webhooks.
- `EventNames` - a list containing the names of the webhooks that should be subscribed (these names must match the names as advertised by the publisher).
- `CallbackUrl` - the address of the callback (e.g. `https://myserver/api/v1.0/callbacks/customercreated`)
- `Secret` - a secret defined by the client that will be returned in the callback headers and that can be used to authenticate the callback (to make sure it was sent by the correct publisher).

> Notice that this design allows for a single endpoint to serve as callback for multiple webhooks. When receiving each callback, the request will contain the actual event name in the headers, allowing the implementation of different logic, depending on the webhook received.

## Metadata about webhooks

The publisher also advertises the list of all the webhooks it publishes. This is also available in the client library (as other operations to inactivate a subscription, delete a subscription, etc.).

```csharp
ServiceOperationResult<IEnumerable<WebhookMetadata>> response = await serviceClient.Webhooks
    .GetWebhooksMetadataAsync()
    .ConfigureAwait(false);
```