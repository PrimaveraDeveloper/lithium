# Capability - Integration Tests

> Tags: Unit Tests, Probe, Test Host, xUnit

Integration tests combine individual components of a microservice (from its client library to the lower layer in the Web API) and test them as a whole.

Any Lithium microservice solution that includes the Web API, it will also include a test project called `Integration.Tests`.

This project includes automatically generated integration tests in the `GeneratedCode` folder. These tests validate the microservice monitoring endpoints (see `MonitoringTests.gen.cs`), using a [Test Host](../ref/hydrogen-designtime-2.0/IntegrationTesting.md) and xUnit fixtures to invoke the microservice via its client library.

You should add new custom integration tests in the `CustomCode` folder, using the same technique, to increase code coverage.