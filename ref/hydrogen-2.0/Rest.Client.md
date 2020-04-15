# Primavera.Hydrogen.Rest.Client

**Class library that contains types that allow communication with REST services.**

## `ServiceClient`

`ServiceClient<T>` is the base class that should be used by any service client - the entry point of a client library to communicate with the REST service.

This base class ensures multiple standard behaviors for the client library, like:

- The shape of the service actions (operations).
- Serialization of requests and deserialization of responses.
- Authentication and authorization of client applications.
- Localization.
- Specific request headers.
- Etc.

## `ServiceClientActions`

`ServiceClientActions<T>` is used by `ServiceClient<T>` to perform the actual requests to the REST service and ensures the correct handling of every kind of action, including serialization or requests and deserialization of responses.

The following methods are available:

- `ExecuteGetAsync<TResult>()` - GET without request body and with result body.
- `ExecuteGetNoResultAsync()` - GET without request body or result body.
- `ExecutePostAsync<TRequestBody, TResult>()` - POST with request body and result body.
- `ExecutePostAsync<TResult>()` - POST without request body and with result body.
- `ExecutePostNoResultAsync<TRequestBody>()` - POST with request body and without result body.
- `ExecutePostNoResultAsync()` - POST without request body or result body.
- `ExecuteDeleteNoResultAsync<TRequestBody>()` - DELETE with request body and without result body.
- `ExecuteDeleteNoResultAsync()`  DELETE without request body or result body.
- `ExecutePutAsync<TRequestBody, TResult>()` - PUT with request body and with result body.
- `ExecutePutAsync<TResult>()` - PUT without request body and with result body.
- `ExecutePutNoResultAsync<TRequestBody>()` - PUT with request body and without result body.
- `ExecutePutNoResultAsync()` - PUT without request body or result body.

## `ServiceOperationResult`

Every operation (action) on a service client should always return a `ServiceOperationResult` or a `ServiceOperationResult<T>` (if the result has a body).
This type will contain information both about the HTTP request and the HTTP response.

```csharp
Task<ServiceOperationResult<EmployeeData>> GetEmployeeAsync(
    string employeeId, 
    CancellationToken cancellationToken = default)

Task<ServiceOperationResult> DeleteDepartmentAsync(
    string departmentId, 
    CancellationToken cancellationToken = default)
```

## `ServiceException`

When a service operation does not return a valid response - it returns an unexpected status code - a `ServiceException` will be raised, including also information about the HTTP request and the HTTP response and the `ServiceError` returned by the operation.

This means that using the service client to call a service operation requires a specific pattern to handle the possible exceptions:

```csharp
try
{
    ServiceOperationResult<EmployeeData> result = await client
        .GetEmployeeAsync("id")
        .ConfigureAwait(false);
    
    EmployeeData employee = result.Body;
    
    // (...)
}
catch (ServiceException ex)
{
    ServiceError error = ex.Body;
    
    // (...)
}
```

## Authentication

All service clients should be integrated with Identity Server and thus require credentials to authenticate the client application (the application using the service client).

Typically these credentials are defined by an instance of `ServiceClientCredentials` and provided to the service client in the constructor.

This library provides a set of different credentials that can be used.

### `AccessTokenCredentials`

`AccessTokenCredentials` allows authenticating the client application by providing the access token, previously obtained from the authority server:

```csharp
string accessToken = (...)
using (MyServiceClient client = new MyServiceClient(baseUri, new AccessTokenCredentials(accessToken))
{
    // (...)
}
```

### `ClientCredentials`

`ClientCredentials` allows authenticating the client application using the client credentials flow by providing the client identifier, the client secret and an optional scope. The service client will use that identity to obtain the correct access token when needed.

```csharp
using (MyServiceClient client = new MyServiceClient(
    baseUri,
    ClientCredentials
        .ForScope(
            authorityServerUri,
            clientId,
            clientSecret,
            scope))
{
    // (...)
}
```

```csharp
using (MyServiceClient client = new MyServiceClient(
    baseUri,
    ClientCredentials
        .ForAllScopes(
            authorityServerUri,
            clientId,
            clientSecret))
{
    // (...)
}
```

### `AuthenticationCallbackCredentials`

`AuthenticationCallbackCredentials` allows providing a callback that will be invoked every time the service client needs to obtain the access token. 

The client application then provides the access token from that callback using the arguments received (that include the address of the authority server).

This form of specifying the service client credentials has the benefit of not requiring the client application to know the address of the authority server in advance. More, depending on how the REST service is configured, the client application may not even be required to know (hard-code) the scopes required to use each specific service operation).

```csharp
using (MyServiceClient client = new MyServiceClient(
    baseUri,
    new AuthenticationCallbackCredentials(
        async (args) =>
        {
            string accessToken = ClientCredentials
                .ForAllScopes(
                    args.Authority,
                    clientId,
                    clientSecret);
            return accessToken;
        }))
{
    // (...)
}
```

### `NoCredentials`

On some scenarios it may be useful to pass void to a service client. This can be achieved by the `NoCredentials` service client credentials, like in the following example:

```csharp
using (MyServiceClient client =
    new MyServiceClient(baseUri, ServiceClientCredentials.NoCredentials)
{
    // (...)
}
```

## Serialization

Request body and response bodies in a REST service are serialized to JSON.

The current version of Hydrogen uses the `System.Text.Json` libraries to perform serialization and deserialization and those operations are controlled by the options set in the `ServiceClient<T>` (`SerializationOptions` and `DeserializationOptions`).

The default behavior for serialization is implemented in `HttpRequestMessageExtensions.AddJsonContent()`.

The default behavior for deserialization is implemented in `ServiceClientActions.DeserializeResponseAsync()`.

> The current version of `System.Text.Json` does not support `TimeSpan` values by default. That is why the default `SerializationOptions` and `DeserializationOptions` add two custom converters (`Iso8601TimeSpanConverter` and `Iso8601TimeSpanNullableConverter`). These converters also need to be configured on the REST service server, using `RestApiMvcBuilderExtensions.AddJsonOptionsForWebApi()`.

## Transient Faults

On a Web server it is common that some transient errors may occur. Transient errors are those that may occur in a point in time but will not repeat if the request is retried.

`ServiceClient` supports the notion of a retry policy to allow the client application to specify how these transient errors should be handled.

### `SetRetryPolicy()`

`ServiceClient<T>.SetRetryPolicy()` allows the client application to specify which retry policy is applied for all operations.

By default, the service client will be configured to use the `ExponentialBackoffRetryStrategy` retry strategy with the `HttpStatusCodeErrorDetectionStrategy` error detection strategy.

For more information on the retry strategies and error detection strategies available, see the documentation on `Primavera.Hydrogen.Policies`.

## Localization

`ServiceClient<T>` supports requests and responses localization via the `Accept-Language` request header.

```csharp
using (MyServiceClient client = new MyServiceClient(...))
{
    client.AcceptLanguage = "pt";
    // (...)
}
```

## Custom Headers

`ServiceClient<T>` adds custom headers to the requests to support diagnosing authentication and correlating requests. The following custom headers may be added to the requests:

- `x-li-c-request-id`: the request identifier (can be used to correlate multiple requests to the service, resulting from authentication and/or retries).
- `x-li-c-credentials-kind`: the kind of credentials provided to the server for authentication (possible values are: `none`, `access-token`, `client-credentials`, and `authentication-callback`).
- `x-li-c-client-id`: the client identifier (available only when `x-li-c-credentials-kind` is `client-credentials`).
- `x-li-c-client-challenge`: indicates whether the request may be an authentication challenge (available only when `x-li-c-credentials-kind` is `authentication-callback`).

## Webhooks

The `Primavera.Hydrogen.Rest.Client.Webhooks` includes a client that allows subscribing webhooks from a server (provided that the webhooks on that server were modeled with the Lithium SDK).

### `WebhooksSubscriptionsClient`

This client allows creating, updating, and deleting webhooks subscriptions on a server.

Its usage is very straightforward:

```csharp
using WebhooksSubscriptionsClient subscriptionsClient = new WebhooksSubscriptionsClient();

WebhookSubscriptionResponse createResponse = await subscriptionsClient.CreateSubscriptionAsync(
    new Uri("https://localhost:20001/"),
    createRequest)
    .ConfigureAwait(false);
if (!createResponse.IsError)
{
    (...)
}

GetWebhookSubscriptionsResponse getResponse = await subscriptionsClient.GetSubscriptionsAsync(
    new Uri("https://localhost:20001/"), 
    "WHR")
    .ConfigureAwait(false);
if (!getResponse.IsError)
{
    (...)
}
```