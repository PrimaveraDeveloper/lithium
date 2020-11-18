# How to create integration tests for a microservice

This guide describes how to create integration tests in a microservice solution.

Integration tests provide the means to validate that all the components of a system work correctly when combined. For a Lithium microservice this means testing that the whole microservice works as expected, not only the Web API but also its client library.

## xUnit

All unit tests for microservices should be implemented using xUnit.

Since integration tests can be implemented using the same techniques as unit tests, they should also be implemented using xUnit.

## `Integration.Tests` Project

When the Lithium microservice solution includes a Web API, it also includes a project called `Integration.Tests`.

All integration tests should be created in this project.

## `TestServer` and `IdentityTestServer`

Developing integration tests for Lithium microservices requires invoking the microservice using the client library. This ensures that no only the Web API works correctly, but also that all the other features - like authorization and the client library itself - work correctly.

To implement such integration tests there are two features that need to be "mocked" to simulate the runtime behavior of microservices, without having to host them in real servers (thus allowing the tests to be executed on development machines and build servers):

- Invoking the Web API (executing HTTP requests).
- Integrating with Identity Server to support the authorization of client applications.

.NET Core provides the means for the first feature, via a type called `TestServer` that allows to host a Web server in memory.

Hydrogen provides the means for the second feature, via a type called `IdentityTestServer`, which allows hosting a copy of Identity Server using a test server in memory, configurable using a storage in-memory.

## `IntegrationTestFixture`

In the code generated for the `Integration.Tests` project you will find a class named `IntegrationTestFixture`.

This is a xUnit test fixture that configures both the test host for the Web API and the Identity Server test host. This fixture is all you will need to use in the integration tests to mock the Lithium microservice.

## `MonitoringTests`

`MonitoringTests` is a test class that is also generated in `Integration.Tests`.

This test class provides a set of integration tests that validate the monitoring endpoints of the microservice in question.

Use this test class as a model to develop your own integration tests.

Here is an example of such a test:

```csharp
/// <summary>
/// Tests the probe endpoint.
/// </summary>
/// <returns>
/// The <see cref="Task"/> that represents the asynchronous operation.
/// </returns>
[Fact]
[SuppressMessage("Microsoft.Naming", "CA1707:IdentifiersShouldNotContainUnderscores")]
public async Task Monitoring_Probe()
{
    // Create fixture

    using IntegrationTestsFixture fixture = new IntegrationTestsFixture();

    // Create service client
    
    using NotificationsClient serviceClient = await fixture.GetServiceClientAsync()
        .ConfigureAwait(false);

    // Execute action

    ServiceOperationResult<string> result1 = await serviceClient.Monitoring.ProbeAsync().ConfigureAwait(false);
    result1.Response.StatusCode.Should().Be(HttpStatusCode.OK);
    result1.Body.Should().Be("OK");

    ServiceOperationResult<string> result2 = serviceClient.Monitoring.Probe();
    result2.Response.StatusCode.Should().Be(HttpStatusCode.OK);
    result2.Body.Should().Be("OK");
}
```

> Notice that hosting the microservice (and Identity Server) to execute the test is as simple as instantiating `IntegrationTestsFixture` and creating the service client using the `GetServiceClientAsync()` method on that fixture.