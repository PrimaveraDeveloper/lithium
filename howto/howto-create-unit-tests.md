# How to create unit tests for a microservice

This guide describes how to create unit tests in a microservice solution.

Unit tests should validate the individual components of a microservice. This means testing models, testing controllers, testing managers, etc., all individually, before combining them in integration tests for verify the whole microservice. If these individual components work as expected (are well tested), the chances are that the whole microservice works as expected.

## xUnit

All unit tests for microservices should be implemented using xUnit.

## `Models.Tests`

Unit tests for the `Models` project should be created in the `Models.Tests` project.

These unit tests should validate each model individually. Things like default values, validations, etc.

> The current version of Lithium does not generate any unit test in this project. That is likely to change in the future.

## `WebApi.Tests`

Unit tests for the `WebApi` project should be created in the `WebApi.Tests` project.

These unit tests should validate each controller and manager (if they exist) individually. Things like input validation, expected responses, etc.

Testing controllers will require mocking managers and the whole `HttpContext` and service collection.

Testing managers will require mocking storage and/or cache, depending on the microservice business logic.

> The current version of Lithium does not generate any unit test in this project. That is likely to change in the future.

## `ClientLib.Tests`

Unit tests for the `ClientLib` project should be created in the `ClientLib.Tests` project.

These unit tests should validate each client library controller individually. Things like input validation, expected responses, etc.

Testing these controllers will require using a mock `HttpMessageHandler`.

> The current version of Lithium does not generate any unit test in this project. That is likely to change in the future.