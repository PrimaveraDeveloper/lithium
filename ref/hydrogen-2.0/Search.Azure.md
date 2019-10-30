# Primavera.Hydrogen.Search.Azure

**Class library that contains types that define generic search services that use Microsoft Azure search services.**

## Search (`AzureSearchService`)

The `AzureSearchService` implements the `ISearchService` using the Microsoft Azure Search services.

The service implementation should be registered using one of the following extension methods for `IServiceCollection`:

```csharp
IServiceCollection services = (...);
services.AddAzureSearch();
```

This will register the service using default configuration options set my a configuration section named `AzureSearchOptions`.

```csharp
IServiceCollection services = (...);
services.AddAzureSearch(
    (options) =>
    {
        // (...)
    });
```

This will register the service using the specified configuration delegate, after reading the default configuration section (if found).

### `AzureSearchOptions`

This options class provides the following configuration options:

- `ServiceName` (required) (no default value) - the name of the Azure search service.
- `AdminApiKey` (required) (default is 5 seconds) - the API key that should be used to connect to the service.