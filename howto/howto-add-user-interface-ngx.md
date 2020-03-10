# How to add support for Angular user interface to a microservice

This guide describes the steps required to support Angular user interface in a microservice.

## Angular User Interface

There is no out of the box support for Angular UI in the Lithium SDK. However you can access any Lithium SDK generated API with an Angular UI.

## Adding Support for CORS

You will need to add support for Cross-Origin Resource Sharing (CORS) in your API. 

### What is CORS

Browser security prevents a web page from making requests to a different domain than the one that served the web page. This restriction is called the same-origin policy. The same-origin policy prevents a malicious site from reading sensitive data from another site. Sometimes, you might want to allow other sites to make cross-origin requests to your app. For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

### Required Configuration 

To do this you will need to override the `AddAdditionalServices()` and `UseEndpoints()` methods in the `Startup` class.

Please consider that, depending on your application requirements, this policy configuration can be slightly different. For more information, see the [ASP.NET Core CORS Documentation](https://docs.microsoft.com/en-us/aspnet/core/security/cors?view=aspnetcore-3.1)


#### Overriding `AddAdditionalServices()`

```csharp
/// <inheritdoc />
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

#### Overriding `UseEndpoints()`

The call to extension method `UseCors()` must precede the call to the extension method `UseEndpoints()` like shown below.

```csharp
/// <inheritdoc />
protected override void UseEndpoints(IApplicationBuilder app, HostConfiguration hostConfiguration)
{
    // Use the CORS policy

    app.UseCors("AllowMyOrigin");

    base.UseEndpoints(app, hostConfiguration);
}

```

## Generating an Angular client library for the API

Using OpenAPITools generators you can create an Angular client library to use in your Angular application. 

The Lithium SDK provides access to the API documentation following the OpenAPI Specification, you can check the generated documentation following the link "Web API Documentation" in the microservice home page.


1 - Install the OpenAPITools CLI on your machine:

```console
npm install @openapitools/openapi-generator-cli -g
```

2 - Run the generator:

```console
openapi-generator generate -i https://localhost:20001/.doc/webapi/1.0/openapi.json -g typescript-angular -o ./ngx-lihtium-api-clientlib
```

3- What's next?

Use, customize or copy the generated client as you see fit.