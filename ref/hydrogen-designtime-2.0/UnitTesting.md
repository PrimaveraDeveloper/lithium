# Primavera.Hydrogen.DesignTime.UnitTesting

**Class library that contains types, helpers, and extension methods for unit testing.**

## xUnit

Unit tests in Lithium microservices should be created using xUnit.

The `Primavera.Hydrogen.DesignTime.UnitTesting` library provides several components and types that provide additional features to be used with that framework.

## Test Server

`TestServer` on [Microsoft.AspNetCore.TestHost](https://docs.microsoft.com/en-us/aspnet/core/test/integration-tests) allows executing integration tests on Web servers and Web APIs.

Hydrogen includes two xUnit test fixtures that make the development of integration tests on the microservices Web API much more simple.

### `TestServerFixture`

This test fixture allows creating a host to execute a Web server by providing actions to configure the host.

Example:

```csharp
[Fact]
[Trait(TraitName, TraitValue)]
public async Task ApiModelValidation_PlainModel_Valid()
{
    using TestServerFixture fixture = new TestServiceFixture()
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
                    services
                        .AddSingleton<CustomersStore>()
                        .AddSingleton<MarketsStore>();
                    services
                        .AddControllers()
                        .ConfigureApiBehaviorOptionsWithoutModelInvalidResponse()
                        .AddApplicationPart(typeof(CustomersController).Assembly);
                    services.AddApiVersioning(new ApiVersion(1, 0));
                },
            ConfigureAction =
                (app) =>
                {
                    app.UseStaticFiles();
                    app.UseRouting();
                    app.UseEndpoints(
                        (endpoints) =>
                        {
                            endpoints.MapControllerRoute("default", "{controller=Home}/{action=Index}/{id?}");
                        });
                }
        };
            
    fixture.Build();

    using HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Post, "http://localhost/api1/v1/markets");
    this.AddJson(
        request, 
        new MarketData()
        {
            Id = "market1",
            Name = "Market 1",
            Share = 10.1
        });

    using HttpResponseMessage response = await this.GetResponseAsync(request, fixture.TestServerFixture.TestServer.CreateHandler()).ConfigureAwait(false);
    response.StatusCode.Should().Be(HttpStatusCode.Created);

    response.Headers.Location.AbsoluteUri.Should().Be("http://localhost/api1/v1/markets/market1");

    string content = await response.Content.ReadAsStringAsync().ConfigureAwait(false);
    content.Should().Be("market1");

    response.Content.Headers.ContentType.ToString().Should().Be("text/plain; charset=utf-8");
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

Since Lithium microservices are protected by Identity Server, to develop full integration tests on their Web API we need a mechanism to "mock" Identity Server. This can also be achieved using the test host with a test fixture that runs Identity Server in memory.

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

## Other Components

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

### Mocks

The following mock types are available.

#### `MockHttpMessageHandler`

A common scenario in unit tests is the need to mock a Web API invoked by `HttpClient` instances.

`MockHttpMessageHandler` provides a mechanism for that where the developer can specify the "expected result" of the Web API, like in the following example:

```csharp
using MockHttpMessageHandler handler = MockHttpMessageHandler.ReturnError(HttpStatusCode.NotFound, "Not found");
using DiscoveryClient client = new DiscoveryClient(handler);
DiscoveryDocumentResponse response = await client.GetDocumentAsync("https://myserver");
response.IsError.Should().BeTrue();
response.HttpStatusCode.Should().Be(HttpStatusCode.NotFound);
```