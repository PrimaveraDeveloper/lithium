# Capability - HttpClient and HttpClientFactory

> Tags: HTTP, REST, Web Requests

It is very common that a microservice needs to call other microservices and/or resources available in Web using `HttpClient`.

This class, however, has some fundamental problems when it is wrongly used. For more details see: [Use IHttpClientFactory to implement resilient HTTP requests](https://docs.microsoft.com/en-us/dotnet/architecture/microservices/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests).

[IHttpClientFactory](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/http-requests) should be used instead. This service is registered in the application startup.

`StartupBase.AddHttpClient()`:

```csharp
protected virtual void AddHttpClient(IServiceCollection services, HostConfiguration hostConfiguration)
{
    // Validation

    SmartGuard.NotNull(() => services, services);
    SmartGuard.NotNull(() => hostConfiguration, hostConfiguration);

    // Add HttpClient

    services
        .AddHttpClient();
}
```

> Microservices should be invoked using the respective client library. Remember to use [IHttpMessageHandlerFactory](https://docs.microsoft.com/en-us/dotnet/api/system.net.http.ihttpmessagehandlerfactory) to pass an instance of `HttpMessageHandler` to the service client.