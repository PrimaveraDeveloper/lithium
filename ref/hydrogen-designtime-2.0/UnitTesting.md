# Primavera.Hydrogen.DesignTime.UnitTesting

**Class library that contains types that provide features to develop unit tests for Lithium components and microservices.**

Unit tests in Lithium microservices should be created using [xUnit.net](https://xunit.net/).

This library includes types and components that provide features that simplify the implementation of unit tests.

## Mocks

The following mock objects are available to be used in unit tests.

### `MockHttpMessageHandler`

A common scenario in unit tests is the need to mock a Web API invoked by `HttpClient` instances.

`MockHttpMessageHandler` provides a mechanism for that, where the developer can specify the "expected result" of the Web API, like in the following example:

```csharp
using MockHttpMessageHandler handler = MockHttpMessageHandler.ReturnError(HttpStatusCode.NotFound, "Not found");
using DiscoveryClient client = new DiscoveryClient(handler);
DiscoveryDocumentResponse response = await client.GetDocumentAsync("https://myserver");
response.IsError.Should().BeTrue();
response.HttpStatusCode.Should().Be(HttpStatusCode.NotFound);
```

## Services

The following static services are also available to simplify some common scenarios.

### `DependencyInjectionService`

Allows creating instances of `IServiceCollection`.

```csharp
IServiceCollection services = DependencyInjectionService.BuildServiceCollection();
```

### `EmbeddedResourcesService`

Allows reading embedded resources from unit tests libraries.

```csharp
string disco = EmbeddedResourcesService.ReadText<DiscoveryClientTests>("Primavera.Hydrogen.IdentityModel.UnitTests._Files.discovery.json");
```

### `FilesService`

Allows reading files.

```csharp
string json = FilesService.ReadFileAsText(Constants.Files.PipelinerConfig01);
```

### `LoggingService`

Allows building `ILogger` instances.

```csharp
ILogger<MyType> logger = LoggingService.BuildNullLogger<MyType>();
```

### `MocksService`

Allows building mock objects using [moq](https://github.com/moq/moq4).

```csharp
IEventBusEventStore store = MocksService.BuildMockObjectOf<IEventBusEventStore>();
```

## Other Components

### Debug-only Unit Tests

If you want to include a unit test that runs only on debug builds, you can use the `[FactDebugOnly] `attribute instead of `[Fact]`.

```csharp
[FactDebugOnly]
public async Task SmtpHostCredentials_Real_SendAsync_Handler_Dispose()
{
    // (...)
}
```

### Culture-specific Test Classes

It is not uncommon to have unit tests that run well on the development machine but fail in the build machine simply because both machines have a different default culture (result, for example, of having different versions of the OS).

To overcome this, it is useful to set the culture of a test class independently of the machine where the unit tests are executed.

The following types allow that:

- `CultureSpecificTestClass`
- `EnglishCultureTestClass`

Example:

```csharp
public class MyTestClass : CultureSpecificTestClass
{
    #region Constructors

    public MyTestClass()
        : base("pt-PT")
    {
    }

    #endregion

    #region Test Methods

    [Fact]
    [Trait]
    public void MyTest()
    {
        // All tests will always run using the PT culture
    }

    #endregion
}
```