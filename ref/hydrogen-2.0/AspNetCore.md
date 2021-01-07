# Primavera.Hydrogen.AspNetCore

**Class library that contains types that support the development of Web applications with ASP.NET Core.**

## MVC + Web API

### Application Configuration (startup)

When you are configuring the application to use MVC, in ASP.NET Core, you either use `AddControllers()` or `AddControllersWithViews()`.

To configure MVC and use the behaviors added by Hydrogen you should use `AddApiControllers()` or `AddApiControllersWithViews()` instead (these extension methods are provided by `ApiControllersServiceCollectionExtensions`):

```csharp
services
    .AddApiControllers();

services
    .AddApiControllers(
        (options) =>
        {
            // (...)
        });

services
    .AddApiControllersWithViews();

services
    .AddApiControllersWithViews(
        (options) =>
        {
            // (...)
        });
```

These extension methods ensure the correct configuration of the behaviors (see descriptions further on this document) provided by:

- `RestApiMvcBuilderExtensions.AddApiJsonOptions()`
- `ValidationIMvcBuilderExtensions.ConfigureApiBehaviorOptionsWithoutModelInvalidResponse()`
- `ApiVersioningServiceCollectionExtensions.AddApiVersioning()`

### `ApiControllerBase`

Every controller that is part of a Web API should be derived from the `ApiControllerBase` base class.

This base class ensures the following standard behaviors:

- Derives from `ControllerBase` (not `Controller`).
- Adds the `[ApiExplorer]` attribute to the controller. More information on this attribute and why it is important [here](https://www.strathweb.com/2018/02/exploring-the-apicontrollerattribute-and-its-features-for-asp-net-core-mvc-2-1/).
- Provides access to an instance of the `ApiControllerValidator` to simplify the validation of controller actions input arguments.

### Validation

Since `ApiControllerBase` ensures that all Web API controllers have the `[ApiController]` attribute, the MVC infrastructure will automatically perform model
binding and model validation before the controller actions are executed (meaning, the implementation code for those actions). By default, this attribute
will also make the action automatically return a `BadRequest` response when the action model is invalid, serializing the errors from the model state.

However, `ServiceClient` expects errors to be returned as `ServiceError` instances and for that to work, the behavior above needs to be customized so that
the automatic response is not returned, allowing the controller action to return the expected error response (and performing additional or different validations).

`ValidationIMvcBuilderExtensions` provides an extension method - `ConfigureApiBehaviorOptionsWithoutModelInvalidResponse()` - to perform that customization,
that should be called in the application startup:

```csharp
services
    .AddControllers()
    .ConfigureApiBehaviorOptionsWithoutModelInvalidResponse();
```

### `ApiControllerValidator`

`ApiControllerValidator` allows validating input arguments on controller actions and to return `ServiceError` when the inputs are invalid. An instance of this validator is
always available through the `Validate()` method of `ApiControllerBase`and can be used like in the following example:

```csharp
[HttpPost("api1/v1/customers")]
[ActionName("Customers.CreateCustomer")]
public IActionResult CreateCustomer(CustomerData customer)
{
    // Validation

    if (!this.Validate()
        .Result("InvalidRequestArgs", "Invalid request arguments.", out ServiceError serviceError))
    {
        return this.BadRequest(serviceError);
    }

    // (...)
}
```

`ApiValidatorController` has a fluent API that allows performing multiple validations, like this:

```csharp
if (!this.Validate()
    .Required(id, "IdRequired", "The identifier is required.")
    .GreaterThan(value, 0, "ValueInvalid", "The value should be greater than 0.")
    .Result("InvalidRequestArgs", "Invalid request arguments.", out ServiceError serviceError))
{
    return this.BadRequest(serviceError);
}
```

IMPORTANT: Keep in mind that this validator will also consider the errors present in `ControllerBase.ModelState` (unless model binding or model validation is disabled explicitly). So, even if all the validation
methods called on the validator pass, the result can still be false if the model binding validation results in model state errors (`ModelState.IsValid == false`).

### REST (Serialization)

Since in ASP.NET 3.0 the serialization is performed by `System.Text.Json`, the service clients for Web API will also use the same serializer.

There are, however, some differences in serialization `Json.NET` and `System.Text.Json`, being one of the most remarkable that `System.Text.Json` does not support `TimeSpan` values and will not serialize/deserialize them correctly.

That is why `ServiceClient` uses two custom converters to support that data type and that is why the serialization options in MVC also need
to be configured to use those converters. That can be done using extension methods available in `RestApiMvcBuilderExtensions`:

```csharp
services
    .AddControllers()
    .AddApiJsonOptions();
```

or

```csharp
services
    .AddControllersWithViews()
    .AddApiJsonOptions(
        (options) =>
        {
            // (...)
        });
```

### Versioning

Versioning REST Web APIs is very important and is implemented in Hydrogen through the [`Microsoft.AspNetCore.Mvc.Versioning`](https://github.com/Microsoft/aspnet-api-versioning/) package.

`ApiVersioningServiceCollectionExtensions` provides extension methods that allow configuring the versioning services using standard options or configuration delegates:

```csharp
services
    .AddApiVersioning(new ApiVersion(1, 0));

services
    .AddApiVersioning(
        new ApiVersion(2, 0),
        (options1) =>
        {
            // (...)
        },
        (options2) =>
        {
            // (...)
        }
    )
```

### Routing

#### Route Analyzer

`IRouteAnalyzerService` allows retrieving information about all the routes configured in MVC (both set via controllers or Razor pages).

This service should be added to the application using dependency injection:

```csharp
using Primavera.Hydrogen.AspNetCore.Mvc.Routing;

public class Startup
{
    // (...)

    public void ConfigureServices(IServiceCollection services)
    {
        // (...)

        services.AddRouteAnalyzer();
    }

    // (...)
}
```

This services produces a list of `RouteInfo` instances:

```csharp
using Primavera.Hydrogen.AspNetCore.Mvc.Routing;
using Primavera.Hydrogen.Rest.Routing;

IRouteAnalyzerService service = this.ServiceProvider
    .GetRequiredService<IRouteAnalyzerService>();

IEnumerable<RouteInfo> routes = service.Analyze();
```

This list, serialized as JSON, would look like this:

```json
[
  {
    "path": "/api/test/delete1/{id}",
    "httpMethods": "DELETE",
    "controllerName": "Test",
    "actionName": "Test:Delete",
    "displayName": "Primavera.Hydrogen.AspNetCore.Tests.Controllers.TestController.DeleteAsync (Primavera.Hydrogen.AspNetCore.Tests)"
  },
  {
    "path": "/api/test/post1/{id}",
    "httpMethods": "POST",
    "controllerName": "Test",
    "actionName": "Test:PostOrPut",
    "displayName": "Primavera.Hydrogen.AspNetCore.Tests.Controllers.TestController.Something (Primavera.Hydrogen.AspNetCore.Tests)"
  },
  {
    "path": "/api/test/put1/{id}",
    "httpMethods": "PUT",
    "controllerName": "Test",
    "actionName": "Test:PostOrPut",
    "displayName": "Primavera.Hydrogen.AspNetCore.Tests.Controllers.TestController.Something (Primavera.Hydrogen.AspNetCore.Tests)"
  },
  {
    "path": "/api/test/route1/{id}",
    "httpMethods": "?",
    "controllerName": "Test",
    "actionName": "Test:Route",
    "displayName": "Primavera.Hydrogen.AspNetCore.Tests.Controllers.TestController.Route (Primavera.Hydrogen.AspNetCore.Tests)"
  }
]
```

#### Endpoint Analyzer

Unlike `IRouteAnalyzerService`, which only outputs the routes configured in MVC, `IEndpointAnalyzerService` outputs all the endpoints (when endpoint routing is used).

The service should be added to the application using dependency injection:

```csharp
using Primavera.Hydrogen.AspNetCore.Routing;

public class Startup
{
    // (...)

    public void ConfigureServices(IServiceCollection services)
    {
        // (...)

        services.AddEndpointAnalyzer();
    }

    // (...)
}
```

It produces a list of `EndpointInfo` instances:

```csharp
using Primavera.Hydrogen.AspNetCore.Routing;
using Primavera.Hydrogen.Rest.Routing;

IEndpointAnalyzerService service = this.ServiceProvider
    .GetRequiredService<IEndpointAnalyzerService>();

IEnumerable<EndpointInfo> endpoints = service.Analyze();
```

This list, serialized as JSON, would look like this:

```json
[
    {
        "httpMethods": "GET",
        "routePattern": "/api/v{version:apiVersion}/certificates/{certificateName}",
        "endpointName": null,
        "routeName": null,
        "order": 0,
        "displayName": "Primavera.Lithium.Certificates.WebApi.Controllers.CertificatesController.GetCertificateAsync (Primavera.Lithium.Certificates.WebApi)",
        "controllerName": "Certificates",
        "actionName": "GetCertificate",
        "secured": true,
        "apiVersions": null
    },
    {
        "httpMethods": "GET",
        "routePattern": "/.doc/clientlib",
        "endpointName": null,
        "routeName": null,
        "order": 0,
        "displayName": "Primavera.Lithium.Certificates.WebApi.Controllers.ClientLibDocumentationController.Documentation (Primavera.Lithium.Certificates.WebApi)",
        "controllerName": "ClientLibDocumentation",
        "actionName": "ClientLibDocumentation.Documentation",
        "secured": false,
        "apiVersions": null
    }
]
```

## Configuration

### Configuration Analyzer

`IConfigurationAnalyzerService` allows retrieving information about the active configuration options in an application.

The service should be added to the application using dependency injection:

```csharp
using Primavera.Hydrogen.AspNetCore.Configuration;

public class Startup
{
    // (...)

    public void ConfigureServices(IServiceCollection services)
    {
        // (...)

        services.AddConfigurationAnalyzer();
    }

    // (...)
}
```

It produces a dictionary containing the configuration options:

```csharp
using Primavera.Hydrogen.AspNetCore.Configuration;

IConfigurationAnalyzerService service = this.ServiceProvider
    .GetRequiredService<IConfigurationAnalyzerService>();

IDictionary<string, string> configuration = service.Analyze();
```

This dictionary, serialized as JSON, would look like this:

```json
{
    "Logging": null,
    "Logging:IncludeScopes": "False",
    "Logging:Debug": null,
    "Logging:Debug:LogLevel": null,
    "Logging:Debug:LogLevel:Default": "Trace",
    "Logging:Console": null,
    "Logging:Console:LogLevel": null,
    "Logging:Console:LogLevel:Default": "Trace",
    "Logging:ApplicationInsights": null,
    "Logging:ApplicationInsights:LogLevel": null,
    "Logging:ApplicationInsights:LogLevel:Default": "Trace",
    "HostConfiguration": null,
    "HostConfiguration:XXX_RedisCacheAbsoluteExpiration": "7200",
    "HostConfiguration:SecretsStorageAddresses": null,
    "HostConfiguration:SecretsStorageAddresses:0": null,
    "HostConfiguration:SecretsStorageAddresses:0:StorageUrl": "https://elevationdev.vault.azure.net/",
    "HostConfiguration:SecretsStorageAddresses:0:CertificateName": "*",
    "HostConfiguration:IdentityServerBaseUri": "https://stg-identity.primaverabss.com",
    "ENVIRONMENT": "Development",
    "ASPNETCORE_URLS": "http://localhost:20002;https://localhost:20003",
    "ASPNETCORE_HTTPS_PORT": "20003",
    "ASPNETCORE_ENVIRONMENT": "Development",
    "applicationName": "Primavera.Lithium.Certificates.WebApi"
}
```

## Hosting

### Background Services

For more information on ASP.NET Core hosted services: [Background tasks with hosted services in ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/host/hosted-services).

#### Simple Background Services

`BackgroundService` allows defining a simple background service that starts when the host starts, executed immediately and is stopped gracefully when the host stops.

The code that corresponds to the background service should be implemented in `ExecuteAsync()`. `StartAsync()` and `StopAsync()` are already implemented but can be overridden.

```csharp
public class MyBackgroundService : BackgroundService
{
    public override bool Enabled
    {
        get
        {
            // (service is enabled?)
        }
    }
    
    protected override Task ExecuteAsync(CancellationToken cancellationToken)
    {
        // (do work)
    }
}
```

> `Enabled` allows enabling or disabling the background service based on some condition (e.g. configuration). This is particularly important when hosting the applications in Azure (see Concurrency).

This kind of background services should be registered in the application startup using the extension methods available in `BackgroundServicesServiceCollectionExtensions`:

```csharp
services.AddBackgroundService<MyBackgroundService();
```

#### Timed Background Services

`TimedBackgroundService` allows building timed background services, which are background services that execute work repeatedly, with a wait interval between executions, until their are stopped.

The actual work should be implemented in `ExecuteWorkAsync`:

```csharp
public class MyTimedBackgroundService : TimedBackgroundService
{
    public override bool Enabled
    {
        get
        {
            // (service is enabled?)
        }
    }
    
    protected override Task ExecuteWorkAsync(CancellationToken cancellationToken)
    {
        // (do work)
    }
}
```

The wait interval needs to be defined in the `WaitPeriod` property:

```csharp
public override TimeSpan WaitPeriod
{
    get
    {
        return TimeSpan.FromSeconds(1);
    }
}
```

Timed background services should be registered like this:

```csharp
services.AddBackgroundServiceTimed<MyTimedBackgroundService();
```

#### Queued Background Services

`QueuedBackgroundService<TWorkItem>` allows building background services that execute work when work items become available in a queue. This is particularlly useful when you want to
have the background service act on response of some event hapenning in the application. The work item itself can be a class of any kind and can be used to pass in contextual data to drive the execution of the background service.

The actual work should be implemented in `ExecuteWorkAsync`:

```csharp
public class MyQueuedBackgroundService : QueuedBackgroundService<string>
{
    public override bool Enabled
    {
        get
        {
            // (service is enabled?)
        }
    }
    
    protected override Task ExecuteWorkAsync(string workItem, CancellationToken cancellationToken)
    {
        // (do work)
    }
}
```

The background service requires a queue to get its work items. This is defined in the Queue property and tipically (not necessarilly) it should be provided via dependency injection (to allow the external sources to queue work items):

```csharp
public override IBackgroundWorkQueue<string> Queue
{
    get
    {
        return this.ServiceProvider.GetRequiredService<IBackgroundWorkQueue<string>>();
    }
}
```

Queued background services should be registered like this:

```csharp
services.AddBackgroundServiceQueued<MyQueuedBackgroundService, string>();
```

#### Background Workers

Background workers allow "sharing" the code that executes work between different background services.

A worker is any class derived from `BackgroundWorker`:

```csharp
public class MyWorker : BackgroundWorker
{
    public override bool Enabled
    {
        get
        {
            // (worker is enabled?)
        }
    }
    
    public override Task ExecuteAsync(CancellationToken cancellationToken)
    {
        // (do work)
    }
}
```

This concept allows creating timed and queued background services that share a common background worker. The following base classes allow defining background services with workers:

- `BackgroundServiceWithWorker<TWorker>`
- `TimedBackgroundServiceWithWorker<TWorker>`
- `QueuedBackgroundServiceWithWorker<TWorker>`

The extension methods for `IServiceCollection` are:

- `AddBackgroundServiceWithWorker<TBackgroundService, TBackgroundWorker>()`
- `AddBackgroundServiceTimedWithWorker<TTimedBackgroundService, TBackgroundWorker>()`
- `AddBackgroundServiceQueuedWithWorker<TQueuedBackgroundService, TBackgroundWorker>()`

> Notice that background workers are NOT registered in the service collection by these extension methods. You decide wether to instantiate them directly or by resolving them using the service provider, provided that you register them in the service collection.

Here is an example:

```csharp
public class BackgroundServiceTimed : TimedBackgroundService
{
    public override bool Enabled => !this.HostEnvironmentExtension.IsPreviewSlot();
    
    public override TimeSpan WaitPeriod
    {
        get
        {
            return TimeSpan.FromSeconds(1);
        }
    }

    private IBackgroundWorkQueue<MyWorker> Queue
    {
        get
        {
            return this.ServiceProvider.GetRequiredService<IBackgroundWorkQueue<MyWorker>>();
        }
    }

    protected override Task ExecuteWorkAsync(CancellationToken cancellationToken)
    {
        // NOTE: MyWorker is registered in the service collection

        MyWorker worker = this.ServiceProvider.GetRequiredService<MyWorker>();

        this.Queue.Enqueue(worker);

        return Task.CompletedTask;
    }
}

public class BackgroundServiceQueued : QueuedBackgroundServiceWithWorker<MyWorker>
{
    public override bool Enabled => !this.HostEnvironmentExtension.IsPreviewSlot();
    
    public override IBackgroundWorkQueue<MyWorker> Queue
    {
        get
        {
            return this.ServiceProvider.GetRequiredService<IBackgroundWorkQueue<MyWorker>>();
        }
    }
}
```

In this example, the services would be registered like in this:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddTransient<MyWorker>();
    services.AddSingleton<IBackgroundWorkQueue<MyWorker>>(new BackgroundWorkQueue<MyWorker>());
    services.AddBackgroundServiceTimed<BackgroundServiceTimed>();
    services.AddBackgroundServiceQueuedWithWorker<BackgroundServiceQueued, MyWorker>();
}
```

#### Concurrency

You need to keep in mind the host application environment when building background services. Depending on that environment, you will need to deal with concurrency - in the sense of multiple instances of the same background service executing the same "job".

When you host an application in Azure, typically you also implement scalling and fail-over mechanisms and that will result in multiple instances of the application running at the same time. The imediate effect of that is that you will have multiple instances of the same background service running at the same time.

So you will need to implement a strategy to avoid them conflicting with each other (e.g. executing the same operation at the same, resulting in duplicate behavior).

The BackgroundService and BackgroundWorker base classes provide a feature to help with that: **automatic locking**.

This mechanism uses a blob - see [Blob Storage](../../capabilities/storage.md) - to hold a lock that ensures that only one instance is running at any time.

To active it, you need to override the UseLocking property in the background service or background worker:

```csharp
public partial class MyBackgroundService
{
    protected override bool UseLocking
    {
        get
        {
            return true;
        }
    }
}
```

> Notice that background services that use workers should not have locking enabled. Locking should occur in the background worker.

This behavior triggers the `TryAcquireLockAsync()` and `TryReleaseLockAsync()` methods - that implement the feature - and can be customized further overriding the following properties: `LockContainerName`, `LockName`, `LockRetries`, `LockWaitTime`, and `LockAbsoluteExpiration`.

Other features may also help:

- The `Enabled` property allows to simply disable the service/worker instance based on some condition (e.g. the environment where the service/worker is running).
- `IHostEnvironmentExtensions` is a service that provides additional information about the environment that can be used in complement with the two previous features. For example, the `IsPreviewSlot()` method allows recognizing that the service/worker is running in the preview slot of the production environment (to avoid conflicts between the "old instances", those running in the preview slot, and the "new instances", those running in production).