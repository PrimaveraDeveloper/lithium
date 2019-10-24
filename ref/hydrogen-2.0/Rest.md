# Primavera.Hydrogen.Rest

**Class library that contains types for applications that define and/or use REST services.**

## ServiceError

`ServiceError` allows defining the response of a REST service action (operation) as an error that will be understood and handled by `ServiceClient` and `ServiceException`.

To return an error, the API controller action should serialize instances of `ServiceError` in the response, like in the following example:

```csharp
public IActionResult AddCustomerAddress(string customerId, [FromBody] AddressData address)
{
    // (...)

    ServiceError error = new ServiceError("CustomerNotFound", "The customer does not exist.");
    return this.NotFound(error);
}
```

## Serialization

The `Primavera.Rest.Serialization` namespace defines types that allow customizing the serialization of requests and responses to and from REST services.

These include the following converters for the `System.Text.Json` serializer:

- `Iso8601TimeSpanConverter`: serializes `TimeSpan` values according to the ISO 8601 specification.
- `Iso8601TimeSpanNullableConverter`: serializes `TimeSpan?` values according to the ISO 8601 specification.

> NOTE: These converters are configured by default in the `ServiceClient` serialization and deserialization options.

## Routing

The `Primavera.Hydrogen.Rest.Routing` namespace provides type to manipulate routes.

### RouteTemplateResolver

`RouteTemplateResolver` allows resolving routes - that is parsing and formatting the route with route values.
Routes are defined according to the MVC rules but `RouteTemplateResolver` also allows resolving routes with
query strings, like they are used in service clients to make requests.

```csharp
string route = RouteTemplateResolver
    .Resolve(
        "/api/file/{id}",
        new
        {
            Id = "file"
        });
```

Like in MVC, routes can have optional arguments, arguments with default values, and query strings. Examples:

```csharp
"api/test/action1"
"api/test/action6/{value}/execute"
"api/test/action11/{value1}/search/{value2?}"
"api/test/action12/{value1=joe}/search/{value2}?pageIndex={pageIndex}"
"api/test/action10/{value1}/search/{value2}?pageIndex={pageIndex}&pageSize={pageSize}&address={address}"
```

> NOTE: Arguments in the query string cannot be optional or have default values.

Route values - the values of the arguments - can be specified either by:

- A `IDictionary<string, object>` instance.
- A `IDictionary<string, string>` instance.
- Any `object` instance (the properties will set the values).