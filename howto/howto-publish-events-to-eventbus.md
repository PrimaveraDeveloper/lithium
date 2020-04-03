# How to publish events to Event Bus from a Microservice

This guide describes how to setup a micro service to publish events to the Event Bus.

Event Bus is a component in Hydrogen that supports event-driven architectures and publish-subscribe design pattern.

## Adding the EventBus service

First, you will need to add the `IEventBusService` to the microservice.

Add the `Primavera.Hydrogen.EventBus.Azure` package to the `WebApi` project.

Register the service in the application startup:

1. In the `CustomFolder` create a class named Startup.
2. Override `AddConfiguration()` and register the Event Bus configuration options (`AzureEventBusOptions`).
2. Override `AddAdditionalServices()` and register the service implementation.

```csharp
using System.Diagnostics.CodeAnalysis;
using Microsoft.Extensions.DependencyInjection;
using Primavera.Hydrogen.EventBus.Azure;
using Primavera.Lithium.EventBusPublisher.WebApi.Configuration;

namespace Primavera.Lithium.EventBusPublisher.WebApi
{
    [SuppressMessage("StyleCop.CSharp.DocumentationRules", "SA1601:Partial elements should be documented")]
    public partial class Startup
    {
        #region Protected Methods

        /// <inheritdoc />
        protected override HostConfiguration AddConfiguration(IServiceCollection services)
        {
            // Default behavior

            HostConfiguration hostConfiguration = base.AddConfiguration(services);

            // Register Event Bus configuration

            services.Configure<AzureEventBusOptions>(
                this.Configuration.GetSection(nameof(AzureEventBusOptions)));

            // Result

            return hostConfiguration;
        }

        /// <inheritdoc />
        protected override void AddAdditionalServices(IServiceCollection services, HostConfiguration hostConfiguration)
        {
            // Default behavior

            base.AddAdditionalServices(services, hostConfiguration);

            // Register Event Bus

            services.AddAzureEventBus();
        }

        #endregion
    }
}
```

You also need to setup the connection string to the Azure Service Bus instance in the `appsettings-development.json` file:

```json
{
    "AzureEventBusOptions": {
        "ConnectionString": "(...)",
        "EventHandlerOptions": {
            "AutoComplete": false,
            "MaxConcurrentCalls": 10
        }
    }
}
```

You are now ready to publish events from custom code (from any controller, manager, etc.).

## Publishing the event

Suppose that you need to publish the event from a API controller.

First you will need to have access to the `IEventBusService` instance:

```csharp
private IEventBusService EventBusService
{
    get
    {
        return this.HttpContext.RequestServices.GetRequiredService<IEventBusService>();
    }
}
```

Then, publishing the event is straightforward. First, you build the event data, then you use the service to publish it.

Like in the following example (a unicast from one controller action):

```csharp
/// <inheritdoc />
protected override async Task<IActionResult> PublishSimpleEventCoreAsync()
{
    // Build the event

    IEventBusEvent<string> evt = new AzureEventBusEvent<string>()
    {
        Body = "This is a simple event"
    };

    // Publish the event

    await this.EventBusService.PublishAsync(
        "sample", 
        evt)
        .ConfigureAwait(false);

    // Done

    return this.NoContent();
}
```