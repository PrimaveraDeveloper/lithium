# How to add support for Angular user interface to a microservice

This guide describes the steps required to support Angular user interface in a microservice.

## Angular User Interface

There is no out of the box support for Angular UI in the Lithium SDK. However you can access any Lithium SDK generated API with an Angular UI.

## Add Support for CORS

You will need to add support for Cross-Origin Resource Sharing (CORS) in your API. 

### What is CORS

Browser security prevents a web page from making requests to a different domain than the one that served the web page. This restriction is called the same-origin policy. The same-origin policy prevents a malicious site from reading sensitive data from another site. Sometimes, you might want to allow other sites make cross-origin requests to your app. For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

### Required configuration 

To do this you will need to override the **AddAdditionalServices** and **UseEndpoints** methods on the Startup class.

Please consider that depending on your application requirements this policy configuration can be slightly different. For more information, see the [ASP.NET Core CORS Documentation](https://docs.microsoft.com/en-us/aspnet/core/security/cors?view=aspnetcore-3.1)


#### Override AddAdditionalServices method

```csharp

/// <summary>
/// Called to add additional (custom) services to the service collection.
/// </summary>
/// <param name="services">The service collection.</param>
/// <param name="hostConfiguration">The host configuration.</param>
/// <remarks>
/// The method is called from <see cref="ConfigureServices(IServiceCollection)"/>.
/// </remarks>
protected override void AddAdditionalServices(IServiceCollection services, HostConfiguration hostConfiguration)
        {
            base.AddAdditionalServices(services, hostConfiguration);

            services.AddCors(options =>
            {
                options.AddPolicy("AllowMyOrigin", 
                    builder => 
                    {
                        builder.WithOrigins("*")
                            .AllowAnyHeader()
                            .AllowAnyMethod());
                    }
            });
        }

```

#### Override UseEndpoints method

The call to extension method **UseCors** must precede the call to extension method **UseEndpoints** like shown below.

```csharp
/// <summary>
/// Called to activate endpoint routing.
/// </summary>
/// <param name="app">The application builder.</param>
/// <param name="hostConfiguration">The current host configuration.</param>
/// <remarks>
/// The method is called from <see cref="Configure(IApplicationBuilder, HostConfiguration, ILogger{Startup})"/>.
/// </remarks>
protected override void UseEndpoints(IApplicationBuilder app, HostConfiguration hostConfiguration)
{
    // Use the CORS policy
    app.UseCors("AllowMyOrigin");

    base.UseEndpoints(app, hostConfiguration);
}

```

## Generating an Angular client library for the API.

Using OpenAPITools generators you can create an angular client lib to use in your angular app. The Lithium SDK provides access to the API documentation following the OpenAPI Specification, you can check the generated documentation following the link "Web API Documentation" provided in the generated homepage of your service. [Localhost Address](https://localhost:20001/.doc/webapi/index.html)


1 - Install the opentapitools cli on your machine.

```console
npm install @openapitools/openapi-generator-cli -g
```

2 - Run the generator 

```console
openapi-generator generate -i https://localhost:20001/.doc/webapi/1.0/openapi.json -g typescript-angular -o ./ngx-lihtium-api-clientlib
```

3- What's next?

Use, customize or copy the generated client as you see fit.