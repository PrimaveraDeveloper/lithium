# How to create a client library for an existing Web API (not developed with Lithium)

This guide describes the steps required to create a client library for any Web API not developed with the Lithium Framework.

You can also use the Lithium Framework to produce client libraries for Web services (REST or not) that were not developed with the framework. This is very useful to make legacy services have client libraries that are similar to Lithium microservices. An example of such a service is Data Lookup (DLS).

## Solution Template

Create the new microservice project using the "PRIMAVERA Lithium Microservice (Client Library only)" solution template.

Once the solution is created it should include 4 solution folders:

- `.nuget`: contains a single file named `nuget.config`.
- `DesignTime`: contains a single project called `Design` that allows modeling the service.
- `Runtime`: contains the client library itself (`ClientLib` and `Models`) and a console client.
- `Tests`: contains the test projects for the microservice.

> Notice that the solution does not include the `WebApi` project.

## Modeling

Next you need to open the service designer and model the service, which in this case, will be the operations that will be part of the client library..

If you open the service model configuration - either using the "Model Configuration..." menu option in the "Lithium" menu or by editing the "Configuration" property in the service model - the following properties will be set like this:

- `Solution kind`: ClientLibraryOnly.
- `Model kind`: MicroserviceClientLibrary.
- `Generate Web API`: false.
- `Generate Client Library`: true.
- `Use Client Credentials Client`: true.

## Transform Text Templates (Generate Code)

Next transform all text templates in the solution to generate the service code.

## Add Custom Code

Depending on how you modeled the service you will need to implement the controller actions (now in the `ClientLib` project) and the console client application.

These operations in the client library should then implement the logic required to invoke the underlying Web service.