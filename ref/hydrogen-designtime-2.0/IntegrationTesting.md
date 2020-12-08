# Primavera.Hydrogen.DesignTime.IntegrationTesting

**Class library that contains types that provide features to develop integration tests for Lithium components and microservices.**

Integration tests allow testing the combination of multiple components/features to verify if they work correctly together (integrate correctly). These tests are usually more expensive, in the sense that they are more complex to implement and in the sense that they take longer to execute.

With Hydrogen, the integration tests can be implemented using the same techniques used for unit tests. All tests that require accessing cloud resources (on Windows Azure for example) should be considered integration tests.

This library includes types and components that provide features that simplify the implementation of integration tests.

## External Test Resources

`ExternalTestResources` defines constants that identify external resources (e.g. connection strings to Windows Azure Table Storage instances) that can be used in the integration tests.

Example:

```csharp
SearchDataSource dataSource = new SearchDataSource()
{
    Name = this.DataSourceName,
    Type = SearchDataSourceType.Table,
    Credentials = new SearchDataSourceCredentials(ExternalTestResources.AzureResources.ClassicTableStorageConnectionString),
    Container = new SearchDataContainer(tableName)
};
```

## Test Server

[Microsoft.AspNetCore.TestHost.TestServer](https://docs.microsoft.com/en-us/aspnet/core/test/integration-tests) allows executing integration tests on Web servers and Web APIs.

## Mocking Web Applications

Hydrogen provides features that make the development of integration tests on the microservices Web API and on Web applications in general much more simple.

### `WebApplicationTestHostBuilder` and `IWebApplicationTestHost`

`WebApplicationTestHostBuilder` allows building a test host to execute a Web server. It leverages the features of the `TestServer` mentioned above.

Example:

```csharp
public class IntegrationTestFixture
{
    public IWebApplicationTestHost Application
    {
        get;
        private set;
    }
        
    public IntegrationTestFixture()
    {
        this.Initialize();
    }

    private void Initialize()
    {
        this.Application = WebApplicationTestHostBuilder
            .Create()
            .UseStartup<MockStartup>()
            .Build();
    }
}

[Fact]
public async Task ApiResourcesController_Create()
{
    using IntegrationTestFixture fixture = new IntegrationTestFixture();
            
    Uri requestUri = Routes.Instance.Resolve(
        new Uri("https://localhost"),
        Routes.ApiResources.Create,
        new Dictionary<string, object>()
        {
            ["version"] = ApiVersions.Literals.DefaultVersionLiteral
        });

    using HttpResponseMessage httpResponse1 = await fixture.Application.CreateRequest(
        HttpMethod.Post,
        requestUri)
        .AddJsonContent(
            new ApiResourceData()
            {
                DisplayName = "API Resource"
            })
        .SendAsync()
        .ConfigureAwait(false);

    httpResponse1.Should().Be400BadRequest();
}
```

### `IdentityServer4TestHostBuilder`

Since Lithium microservices are protected by Identity Server, to develop full integration tests on their Web APIs, we need a mechanism to "mock" Identity Server.

This can also be achieved using a test host that runs Identity Server in memory.

Example:

```csharp
public class IntegrationTestFixture
{
    public IWebApplicationTestHost AuthorityServer
    {
        get;
        private set;
    }
        
    public IWebApplicationTestHost Application
    {
        get;
        private set;
    }
        
    public IntegrationTestFixture()
    {
        this.Initialize();
    }

    private void Initialize()
    {
        this.AuthorityServer = this.BuildAuthorityServer(
            this.ClientId,
            this.ClientSecret);

        this.Application = this.BuildApplication();
    }

    private IWebApplicationTestHost BuildAuthorityServer()
    {
        IList<ApiResource> apiResources = (...);

        IList<Client> clients = (...);

        return IdentityServer4TestHostBuilder
            .Create()
            .UseApiResources(apiResources)
            .UseClients(clients)
            .Build();
    }

    private IWebApplicationTestHost BuildApplication()
    {
        return WebApplicationTestHostBuilder
            .Create()
            .ConfigureAuthorityServer(this.AuthorityServer)
            .UseStartup<MockStartup>()
            .Build();
    }
}

[Fact]
public async Task ApiResourcesController_Create()
{
    using IntegrationTestFixture fixture = new IntegrationTestFixture();
            
    string accessToken = await fixture.AuthorityServer.GetClientCredentialsAccessTokenAsync(
        fixture.ClientId,
        fixture.ClientSecret,
        "scope")
        .ConfigureAwait(false);

    Uri requestUri = Routes.Instance.Resolve(
        new Uri("https://localhost"),
        Routes.ApiResources.Create,
        new Dictionary<string, object>()
        {
            ["version"] = ApiVersions.Literals.DefaultVersionLiteral
        });

    using HttpResponseMessage httpResponse1 = await fixture.Application.CreateRequest(
        HttpMethod.Post,
        requestUri)
        .AddJsonContent(
            new ApiResourceData()
            {
                DisplayName = "API Resource"
            })
        .AddAuthorizationHeader("Bearer", accessToken)
        .SendAsync()
        .ConfigureAwait(false);

    httpResponse1.Should().Be400BadRequest();
}
```

> `IdentityServer5TestHostBuilder` is the equivalent of `IdentityServer4TestHostBuilder` for Identity Server version 5.