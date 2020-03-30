# How to create a microservice that does not require a client library

This guide describes the steps required to create a new microservice without creating the corresponding client library.

There is a special scenario where, although the Lithium architecture indicates that all microservices should have a C# client library, you may not produce it because it does not add any useful feature to the microservice. This scenario occurs when the microservice only has UI features for users and no concrete Web API that can be used by client applications (using the client credentials authorization flow). An example of such a service is Postman (PS).

## Solution Template

Create the new microservice project using the "PRIMAVERA Lithium Microservice (Web API only)" solution template.

Once the solution is created it should include 4 solution folders:

- `.nuget`: contains a single file named `nuget.config`.
- `DesignTime`: contains a single project called `Design` that allows modeling the service.
- `Runtime`: contains the microservice itself (`Models` and `WebApi`).
- `Tests`: contains the test projects for the microservice.

> Notice that the solution does not include the `ClientLib` and `Client.Console` projects.

## Modeling

Next you need to open the service designer and model the service.

If you open the service model configuration - either using the "Model Configuration..." menu option in the "Lithium" menu or by editing the "Configuration" property in the service model - the following properties will be set like this:

- `Solution kind`: `WebApiOnly`.
- `Model kind`: `MicroserviceWebApi`.
- `Generate Web API`: `true`.
- `Generate Client Library`: `false`.
- `Use Client Credentials Client`: `false`.

## Transform Text Templates (Generate Code)

Next transform all text templates in the solution to generate the service code.

## Add Custom Code

Depending on how you modeled the service you will need to implement the controller actions and the monitoring controller actions.