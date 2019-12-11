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
- `ThrottlingServiceCollectionExtensions.AddThrottling(IServiceCollection)`

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

`IRouteAnalyzer` allows retrieving information about all the routes configured in MVC (both set via controllers or Razor Pages).

You can map a route that will output the result of route analyzer as JSON. `RouteAnalyzerIEndpointRouteBuilderExtensions` allows that using `MapRouteAnalyzer()` like this:

```csharp
public class Startup
{
    // (...)

    public void Configure(IApplicationBuilder app)
    {
        // (...)
        app.UseRouting();
        // (...)
        app.UseEndpoints(
            (endpoints) =>
            {
                // (...)
                endpoints.MapRouteAnalyzer();
            });
    }
}
```

> Route Analyzer should on be active on the development environment.

After this the default route analyzer route (`/.routes`) will product a JSON output like the following:

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
  },
  {
    "path": "/api/test/route2/{id}",
    "httpMethods": "?",
    "controllerName": "Test",
    "actionName": "AnotherRoute",
    "displayName": "Primavera.Hydrogen.AspNetCore.Tests.Controllers.TestController.AnotherRouteAsync (Primavera.Hydrogen.AspNetCore.Tests)"
  },
  {
    "path": "/api/v1/employees/{employeeId}",
    "httpMethods": "GET",
    "controllerName": "Employees",
    "actionName": "Employees.GetEmployee",
    "displayName": "Primavera.Hydrogen.AspNetCore.Tests.Controllers.EmployeesApiController.GetEmployee (Primavera.Hydrogen.AspNetCore.Tests)"
  }
  (...)
]
```

> You can customize the route analyzer route by passing a different `route` value to `MapRouteAnalyzer()`.

## Configuration

### Configuration Analyzer

Configuration Analyzer allows retrieving information about the active configuration options in an application.

You can map a route that will output the result of configuration analyzer as JSON.

`ConfigurationAnalyzerIEndpointRouteBuilderExtensions` allows that using `MapConfigurationAnalyzer()` like this:

```csharp
public class Startup
{
    // (...)

    public void Configure(IApplicationBuilder app)
    {
        // (...)
        app.UseRouting();
        // (...)
        app.UseEndpoints(
            (endpoints) =>
            {
                // (...)
                endpoints.MapConfigurationAnalyzer();
            });
    }
}
```

> Configuration Analyzer should on be active on the development environment.

After this the default configuration analyzer route (`/.config`) will product a JSON output like the following, containing a dictionary with all the active configuration options:

```json
[
  {
    "Key": "ALLUSERSPROFILE",
    "Value": "C:\\ProgramData"
  },
  {
    "Key": "APPDATA",
    "Value": "C:\\Users\\machine\\AppData\\Roaming"
  },
  {
    "Key": "applicationName",
    "Value": "Primavera.Lithium.Settings.WebApi"
  },
  {
    "Key": "ASPNETCORE_ENVIRONMENT",
    "Value": "Development"
  },
  {
    "Key": "ASPNETCORE_URLS",
    "Value": "http://localhost:20000/"
  },
  (...)
]
```

> You can customize the configuration analyzer route by passing a different `route` value to `MapConfigurationAnalyzer()`.

## Hosting

### Background Services

For more information on ASP.NET Core hosted services: [Background tasks with hosted services in ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/host/hosted-services).

#### Simple Background Services

`BackgroundService` allows defining a simple background service that starts when the host starts, executed immediately and is stopped gracefully when the host stops.

The code that corresponds to the background service should be implemented in `ExecuteAsync()`. `StartAsync()` and `StopAsync()` are already implemented but can be overridden.

```csharp
public class MyBackgroundService : BackgroundService
{
    protected override Task ExecuteAsync(CancellationToken cancellationToken)
    {
        // (do work)
    }
}
```

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
    protected override Task ExecuteWorkAsync(CancellationToken cancellationToken)
    {
        // (do work)
    }
}
```

The wait interval needs to be defined in the `WaitPeriod` property:

```csharp
public class MyTimedBackgroundService : TimedBackgroundService
{
    public override TimeSpan WaitPeriod
    {
        get
        {
            return TimeSpan.FromSeconds(1);
        }
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

The actual work should be implemented in ExecuteWorkAsync:

```csharp
public class MyQueuedBackgroundService : QueuedBackgroundService<string>
{
    protected override Task ExecuteWorkAsync(string workItem, CancellationToken cancellationToken)
    {
        // (do work)
    }
}
```

The background service requires a queue to get its work items. This is defined in the Queue property and tipically (not necessarilly) it should be provided via dependency injection (to allow the external sources to queue work items):

```csharp
public class MyQueuedBackgroundService : QueuedBackgroundService<string>
{
    public override IBackgroundWorkQueue<string> Queue
    {
        get
        {
            return this.ServiceProvider.GetRequiredService<IBackgroundWorkQueue<string>>();
        }
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

The extension methods for IServiceCollection are:

- `AddBackgroundServiceWithWorker<TBackgroundService, TBackgroundWorker>()`
- `AddBackgroundServiceTimedWithWorker<TTimedBackgroundService, TBackgroundWorker>()`
- `AddBackgroundServiceQueuedWithWorker<TQueuedBackgroundService, TBackgroundWorker>()`

Here is an example:

```csharp
public class BackgroundServiceTimed : TimedBackgroundService
{
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
        MyWorker worker = this.ServiceProvider.GetRequiredService<MyWorker>();

        this.Queue.Enqueue(worker);

        return Task.CompletedTask;
    }
}

public class BackgroundServiceQueued : QueuedBackgroundServiceWithWorker<MyWorker>
{
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

## Throttling

The `Primavera.Hydrogen.AspNetCore.Throttling` namespace provides features to add throttling to a Web API (or any ASP.NET Core MVC app).

The current version supports the following kinds of throttling:

- **Client-rate limiting** - limits requests based on the number of requests from a client application during a period of time.

### Throttling Mode

Throttling is configured by adding a middleware to the ASP.NET Core middleware pipeline.

In theory, throttling should happen as soon as possible in the request processing pipeline to reduce workload on the server for requests
that are blocked by the throttling components.

The best way to achieve that is to use the middleware mode and add the middleware as or one of the first middleware in the pipeline.

> Since the throttling components rely on the authentication middleware to identify client applications, it needs to be added after that middleware (when authentication is in place).

### Throttling Handlers

The throttling middleware applies throttling by running all the throttling handlers registered. These handlers
are recognized by the `IThrottlingHandler` interface and need to be registered using the `IThrottlingBuilder`.

The throttling builder and the throttling handlers should be configured in the application startup as in the following example:

```csharp
services
    .AddThrottling()
    .AddHandler<MyHandler>();
```

The middleware needs to be added too:

```csharp
app.UseThrottling();
```

> The moment when the middleware is called is determined by its order in the `Configure()` method.

### Client-rate Limiting

As said before, client-rate limiting blocks requests based on the number of requests coming from a specific client application during a period
of time.

You can define rules per endpoint (HttpVerb + Path) stating the maximum number of requests accepted during a given period of time. These rules
can be applied to any client application or to a specific client (based on its client identifier).

> Beware that client-rate limiting means that if multiple users use the same client application to access the Web API, they will all add to
the same counter. If, for example, you have a limit of 100 requests per second, then if user A executes 100 requests and then user B executes a single
request, the request from user B will still be blocked because it would exceed the rule for that client application.

To setup the client-rate limiting middleware, you should follow these steps:

(1) In `Startup.ConfigureServices()`, create the throttling builder and set the throttling handler:

```csharp
services
    .AddThrottling()
    .AddClientRateHandler();
```

> Both `AddThrottling()` and `AddClientRateHandler()` provide multiple overloads that allow you to set configuration for `ThrottlingOptions`
and `ClientRateThrottlingOptions`.

(2) Then add the throttling middleware as described above.

#### How are client applications identified?

Clients (the applications calling the application) are identified by client id and this client id will be extracted either from the
request identity claims or the request headers.

The throttling handler will search for any claim name defined in `ClientRateOptions.ClientIdClaims` or, if none found, by any header name defined
in `ClientRateOptions.ClientIdHeaders`.

By default these collections contain the following values:

- `ClientIdClaims`: `{ client_id }`
- `ClientIdHeaders`: `{ X-ClientId }`.

If the client identifier is not found in the request, the client will be considered anonymous and the client id will be `anon`. This allows you to define
specific rules for "anonymous calls" using that client id.

> There is a setting in `ThrottlingOptions` that allows you to disable anonymous calls. If `AllowAnonymousCalls` is false, any request with client id `anon`
will be blocked regardless of the matching rules.

#### Defining Throttling Rules

Client-rate limiting rules can be configured through `ClientRateThrottlingOptions`.

These options can be defined in code:

```csharp
services
    .AddThrottling()
    .AddClientRateHandler(
        (options) =>
        {
            options.GeneralRules = new RateLimitRule[]
            {
                new RateLimitRule("*", TimeSpan.FromSeconds(1), 2),
                new RateLimitRule("post:/api/*", TimeSpan.FromSeconds(1), 1)
            };
            options.ClientRules = new ClientSpecificRateLimitRule[]
            {
                new ClientSpecificRateLimitRule()
                {
                    ClientId = "jasmin",
                    Rules = new RateLimitRule[]
                    {
                        new RateLimitRule("post:/api/v*", TimeSpan.FromSeconds(1), 4),
                        new RateLimitRule("get:/api/values", TimeSpan.FromSeconds(1), 3)
                    }
                }
            };
        })
```

Or in the `appsettings.json` file:

```json
{
    "ClientRateThrottlingOptions": {
        "GeneralRules": [
            {
                "Endpoint": "*",
                "Period": "00:00:01",
                "Limit": 2
            },
            {
                "Endpoint": "post:/api/*",
                "Period": "00:00:01",
                "Limit": 1
            }
        ],
        "ClientRules": [
            {
                "ClientId": "jasmin",
                "Rules": [
                    {
                        "Endpoint": "post:/api/v*",
                        "Period": "00:00:01",
                        "Limit": 4
                    },
                    {
                        "Endpoint": "get:/api/values",
                        "Period": "00:00:01",
                        "Limit": 3
                    }
                ]
            }
        ]
    }
}
```

There are two kinds of rules:

- The **general rules** apply to the specified endpoint(s) and are independent from the calling client.
- The **client rules** apply to the specified endpoint(s) when called by a specific client (identifier). This kind of rules have priority over the general rules.

Each rule defines:

- The **endpoint** (the path of the request) to which it should be applied. Notice that the endpoint is defined in the form `[HttpVerb]:[Path]`. `[HttpVerb]` can be any of the valid verbs or `*` to match any verb. `[Path]` supports glob pattern matching (e.g. `/api/*`). A value of `*` for the endpoint will match any verb and any path.
- The **period** of the rule. This is a TimeSpan for the rate counter.
- The **limit** is the maximum number of requests that will be accepted for the duration of the specified period.

If multiple rules are applicable to any given request, they will be processed as follows:

- The most restrictive rules (in terms of limit) will be evaluated first.
- Duplicate rules (in terms of period) will be ignored and only the first (most restrictive) will be evaluated.

#### Behavior Restrictions

Each handler will be executed only once for each request.

### Customizing Throttling

There are multiple ways to extend the throttling behavior.

By default the client-rate limiting handler will store the counters (that count the number of requests per client) in a distributed memory cache store
(configured with `services.AddDistributedMemoryCache()`). This store is implemented by
`DistributedMemoryRateLimitsCountersStore`.

A memory cache store is also available in `MemoryRateLimitsCountersStore`. Do not use this one in production environments with multiple Web servers (for obvious reasons).

You can modify the store used or setup your own one as follows:

```csharp
services
    .AddThrottling()
    .AddClientRateHandler()
    .AddCountersStore<IRateLimitCountersStore, MyCountersStore>();
```

> The class that implements the store should implement the `IRateLimitCountersStore` interface.

You can also add a new throttling handler:

```csharp
services
    .AddThrottling()
    .AddClientRateHandler()
    .AddHandler<MyThrottlingHandler>();
```

> `MyThrottlingHandler` needs to implement `IThrottlingHandler`.
