# How to publish webhooks from a  microservice

This guide describes how to setup a microservice to publish webhooks.

## Webhooks

Webhooks are automated messages sent to subscribers (applications or other microservices) when certain events occur within the publisher (the microservice) context.

Each webhook will have a payload - data about the event - associated with it.

To add webhooks to a microservice, first you need to describe the events available - so that subscribers can subscribes to receive the webhooks for those events - and then publish the actual events from microservice's custom logic.

## Adding webhooks

To describe the webhook events follow these steps:

1. Open the service model.
2. Using the context menu, add a new Webhook.
3. Specify the webhook name.
4. Add the summary, the event description, and the payload description.
5. Save the model and transform all text templates.

You can add as many webhooks as needed.

> The event name must be in the form 'Entity_Verb' to describe the entity affected and what happened. Example: 'Customer_Created'.

> The event and the payload descriptions are important so that the subscribers can understand what the event represents and what data it will receive when the webhook is triggered.

### Generated code

The generated code will be affected in multiple forms to add all the features associated with the webhooks.

#### Startup (webhooks services and webhooks events)

First, the microservice is setup to include the webhooks services and the descriptions for the webhooks events.

`StartupBase.ConfigureServices()`:

```csharp
public virtual void ConfigureServices(IServiceCollection services)
{
    // (...)

    // Webhooks

    this.AddWebhooks(services, hostConfiguration);

    // (...)
}
```

`StartupBase.AddWebhooks()`:

```csharp
protected virtual void AddWebhooks(IServiceCollection services, HostConfiguration hostConfiguration)
{
    // Validation

    SmartGuard.NotNull(() => services, services);
    SmartGuard.NotNull(() => hostConfiguration, hostConfiguration);

    // Webhooks

    services
        .AddWebhooks(this.ConfigureWebhooksOptions)
        .AddPublisherInProcess();
}
```

StartupBase.ConfigureWebhooksOptions():

```csharp
/// <summary>
/// Called to configure the webhooks services options.
/// </summary>
/// <param name="options">The options.</param>
protected virtual void ConfigureWebhooksOptions(WebhooksOptions options)
{
    // Validation

    SmartGuard.NotNull(() => options, options);

    // Application name

    options.ApplicationName = "WHP";

    // Events supported

    options.EventsSupported = new List<WebhookEvent>()
    {
        new WebhookEvent()
        {
            EventName = "Customer_Created",
            EventDescriptions = new Dictionary<string, string>(StringComparer.OrdinalIgnoreCase)
            {
                ["en"] = this.GetResourceValueForCulture("RES_Webhook_Customer_Created_EventDescription", "en", typeof(WebhooksResources)),
                ["pt"] = this.GetResourceValueForCulture("RES_Webhook_Customer_Created_EventDescription", "pt", typeof(WebhooksResources)),
                ["es"] = this.GetResourceValueForCulture("RES_Webhook_Customer_Created_EventDescription", "es", typeof(WebhooksResources))
            },
            PayloadDescriptions = new Dictionary<string, string>(StringComparer.OrdinalIgnoreCase)
            {
                ["en"] = this.GetResourceValueForCulture("RES_Webhook_Customer_Created_PayloadDescription", "en", typeof(WebhooksResources)),
                ["pt"] = this.GetResourceValueForCulture("RES_Webhook_Customer_Created_PayloadDescription", "pt", typeof(WebhooksResources)),
                ["es"] = this.GetResourceValueForCulture("RES_Webhook_Customer_Created_PayloadDescription", "es", typeof(WebhooksResources))
            },
        }
    };
}
```

#### Controllers (webhooks subscriptions)

To allow for applications to subscribe the webhooks, the microservice will include a new set of API routes. These are implemented by the `WebhooksController`:

Here's an example of one of these controller actions:

```csharp
/// <inheritdoc />
[HttpPost(Primavera.Lithium.WebhooksPublisher.Models.Metadata.Routes.Webhooks.CreateSubscription)]
[ProducesResponseType(typeof(ServiceError), (int)HttpStatusCode.BadRequest)]
[ProducesResponseType(typeof(ServiceError), (int)HttpStatusCode.Conflict)]
[ProducesResponseType(typeof(string), (int)HttpStatusCode.Created)]
public virtual async Task<IActionResult> CreateSubscriptionAsync(CreateWebhookSubscriptionRequest request)
{
    // Use service

    OperationResult<string> result = await this.WebhooksSubscriptionsService
        .CreateSubscriptionAsync(
            request)
        .ConfigureAwait(false);

    // Failure?

    if (result.IsFailureWith(WebhooksErrorCodes.EventAlreadySubscribed))
    {
        // Conflict

        return this.BadRequest(
            ServiceError.FromOperationResult(
                result));
    }
    else if (result.IsFailure)
    {
        // BadRequest

        return this.BadRequest(
            ServiceError.FromOperationResult(
                result));
    }

    // Created

    return this.Created(
        this.HttpContext.Request.GetWebhookSubscriptionLocationUri(
            request.ClientId, 
            result.Data),
        result.Data);
}
```

> Notice that these actions use the `IWebhooksSubscriptionService` available in Hydrogen.

> The webhooks subscriptions routes are start with `/api/webhooks`.

#### Client library (webhooks subscriptions)

This new controller is also exposed in the microservice client library. This means that applications will be able to use the client library to subscribe the webhooks events.

```csharp
/// <summary>
/// Defines the base class for the type that manages webhooks on the service.
/// </summary>
/// <seealso cref="IWebhooksOperations" />
/// <seealso cref="IDisposable" />
[GeneratedCode("Lithium", "2.0")]
[SuppressMessage("Maintainability Rules", "SA1402:FileMayOnlyContainASingleType", Justification = "Because of code generation design.")]
public abstract partial class WebhooksOperationsBase : IWebhooksOperations, IDisposable
{
    // (...)
}
```

#### Console client (webhooks subscriptions)

The console client also provides operations on this new controller, like it happens for the monitoring controller:

![Console Client](./_assets/console-webhooks.png "Console Client")

## Publishing webhooks events

Now that the microservice is configured to use the webhooks services and includes the webhooks events' descriptions, the only thing missing is to publish the actual webhooks in the correct places.

This cannot be automatically generated, so it needs to be placed in the appropriate custom logic spot, which depends, off course, on the nature of the microservice and on the event itself.

To publish an event, you should use the `IWebhooksService` as in the following example:

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