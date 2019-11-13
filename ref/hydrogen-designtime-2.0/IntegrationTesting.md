# Primavera.Hydrogen.DesignTime.UnitTesting

**Class library that contains types that provide features to develop integration tests for Lithium components and microservices.**

Integration tests allow testing the combination of multiple components/features to verify if they work correctly together (integrate correctly). These tests are usually more expensive, in the sense that they are more complex to implement and in the sense that they take longer to execute.

With Hydrogen, the integration tests can be implemented using the same techniques used for unit tests. All tests that require accessing cloud resources (on Windows Azure for example) should be considered integration tests.

This library includes types and components that provide features that simplify the implementation of integration tests.

## External Test Resources

ExternalTestResources defines constants that identify external resources (e.g. connection strings to Windows Azure Table Storage instances) that can be used in the integration tests.

Example:

```csharp
SearchDataSource dataSource = new SearchDataSource()
{
    Name = this.DataSourceName,
    Type = SearchDataSourceType.Table,
    Credentials = new SearchDataSourceCredentials(ExternalTestResources.Azure.ClassicTableStorageConnectionString),
    Container = new SearchDataContainer(tableName)
};
```

## Test Server

[Microsoft.AspNetCore.TestHost.TestServer](https://docs.microsoft.com/en-us/aspnet/core/test/integration-tests) allows executing integration tests on Web servers and Web APIs.

## Fixtures

Hydrogen provides a set of fixtures that make the development of integration tests on the microservices Web API much more simple.

### `TestServerFixture`

This test fixture allows creating a host to execute a Web server by providing actions to configure the host.

Example:

```csharp
[Fact]
public async Task ApiModelValidation_PlainModel_Valid()
{
    using TestServerFixture fixture = new TestServiceFixture()
        {
            ConfigureLoggingAction =
                (context, builder) =>
                {
                    // (...)
                },
            ConfigureServicesAction =
                (context, services) =>
                {
                    // (...)
                },
            ConfigureAction =
                (app) =>
                {
                    // (...)
                }
        };
            
    fixture.Build();

    using HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Post, "http://localhost/api1/v1/markets");
    
    using HttpResponseMessage response = await this.GetResponseAsync(request, 
    
    using HttpClient httpClient = new HttpClient(fixture.TestServerFixture.TestServer.CreateHandler());
    using HttpResponseMessage response = await httpClient.SendAsync(request).ConfigureAwait(false);

    response.StatusCode.Should().Be(HttpStatusCode.Created);
}
```

### `TestServerFixture<T>`

This fixture provides the same behavior but the test host can be configured using a startup class.

Example:

```csharp
public sealed partial class ServiceClientTestsNoAuth : IClassFixture<TestServerFixture<ServiceClientNoAuthStartup>>
{
    public ServiceClientTestsNoAuth(TestServerFixture<ServiceClientNoAuthStartup> fixture)
    {
        this.TestServerFixture = fixture.Build();
    }
}
```

## Identity Server Test Server

Since Lithium microservices are protected by Identity Server, to develop full integration tests on their Web APIs, we need a mechanism to "mock" Identity Server. This can also be achieved using the test host with a test fixture that runs Identity Server in memory.

`IdentityServerTestServer` uses `TestServer` internally and allows the developer to program the Identity Server configuration (API resources, clients, etc.) in the test setup.

Example:

```csharp
public class ServiceClientFixture<TStartup> : IDisposable
    where TStartup : IServiceClientStartup, new()
{
    #region Fields

    private bool disposed;

    #endregion

    #region Public Properties

    public TestServerFixture TestServerFixture
    {
        get;
        private set;
    }

    public IdentityServerTestServer IdentityServer
    {
        get;
        private set;
    }

    #endregion

    #region Constructors

    public ServiceClientFixture()
    {
        // Initialize Identity Server test server

        IEnumerable<IdentityServer4.Models.ApiResource> apiResources = new List<IdentityServer4.Models.ApiResource>()
        {
            new IdentityServer4.Models.ApiResource(Constants.Audiences.EmployeesApi, "Hydrogen Employees API")
        };

        IEnumerable<IdentityServer4.Models.Client> clients = new List<IdentityServer4.Models.Client>()
        {
            new IdentityServer4.Models.Client()
            {
                ClientId = Constants.Clients.EmployeesClientCredentials,
                AllowedGrantTypes = IdentityServer4.Models.GrantTypes.ClientCredentials,
                ClientSecrets =
                {
                    new IdentityServer4.Models.Secret(
                        ComputeHashString.Sha256(
                            Constants.ClientSecrets.EmployeesClientCredentials))
                },
                AllowedScopes =
                {
                    Constants.Scopes.EmployeesApi
                }
            }
        };

        this.IdentityServer = new IdentityServerTestServer(
            null,
            apiResources,
            clients,
            null,
            null,
            null);

        this.IdentityServer.Build();

        // Initialize API test server

        TStartup startup = new TStartup();

        this.TestServerFixture = new TestServerFixture()
        {
            ConfigureLoggingAction =
                (context, builder) =>
                {
                    builder.SetMinimumLevel(LogLevel.Trace);
                    builder.AddDebug();
                },
            ConfigureServicesAction =
                (context, services) =>
                {
                    startup.ConfigureServices(services, this.IdentityServer.TestServer.CreateHandler());
                },
            ConfigureAction =
                (app) =>
                {
                    startup.Configure(app);
                }
        };

        this.TestServerFixture.Build();
    }

    #endregion

    #region Public Methods

    public EmployeesServiceClient GetClientCredentialsClientWithCallback(
        string clientId = Constants.Clients.EmployeesClientCredentials,
        string clientSecret = Constants.ClientSecrets.EmployeesClientCredentials)
    {
        TokenCache tokenCache = new TokenCache();

        return new EmployeesServiceClient(
            new Uri("https://localhost"),
            async (args) =>
            {
                System.Diagnostics.Debug.WriteLine("Retrieving client credentials...");

                string accessToken = await ClientCredentials
                    .ForAllScopes(
                        new Uri(args.Authority), 
                        clientId, 
                        clientSecret, 
                        this.IdentityServer.TestServer.CreateHandler())
                    .WithTokenCache(tokenCache)
                    .RetrieveAccessTokenAsync().ConfigureAwait(false);

                return accessToken;
            },
            this.TestServerFixture.TestServer.CreateHandler(),
            false);
    }

    public void Dispose()
    {
        this.Dispose(true);
        GC.SuppressFinalize(this);
    }

    #endregion

    #region Protected Methods

    protected virtual void Dispose(bool disposing)
    {
        // Already disposed?

        if (this.disposed)
        {
            return;
        }

        // Called from Dispose()?

        if (disposing)
        {
            if (this.TestServerFixture != null)
            {
                this.TestServerFixture.Dispose();
                this.TestServerFixture = null;
            }

            if (this.IdentityServer != null)
            {
                this.IdentityServer.Dispose();
                this.IdentityServer = null;
            }
        }

        // Set flag

        this.disposed = true;
    }

    #endregion
}
```

For the Web API to use the Identity Server test server it is required that the configuration sets the back-channel HTTP handler, like in the following example:

```csharp
protected virtual void AddAuthentication(IServiceCollection services, HttpMessageHandler backchannelHttpHandler)
{
    services
        .AddAuthentication(Constants.Auth.Bearer)
        .AddJwtBearer(
            (options) =>
            {
                options.Authority = "https://localhost";
                options.Audience = Constants.Audiences.EmployeesApi;
                options.RequireHttpsMetadata = false;
                options.IncludeErrorDetails = true;
                options.RefreshOnIssuerKeyNotFound = true;
                options.SaveToken = true;
                options.BackchannelHttpHandler = backchannelHttpHandler;
                options.Events = new HttpBearerChallengeEvents();
            });
}
```