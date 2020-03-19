# Capability - Authorization

> Tags: OAuth, Identity Server, Access Tokens, Grant Types, Scopes, Policies, Client Applications

Authorization refers to the capability of validating the access of a client application (the caller of the Web API) to a given resource (route) of the microservice Web API.

Lithium microservices use [OAuth](https://oauth.net/) to manage authorization and are configured by default to use PRIMAVERA's authorization server - [Identity Server](https://identity.primaverabss.com/about).

The microservice developer needs to enable authorization in the service model (see [Service Designer](../ref/sdk-2.0/service-designer.md)), configure the authorization scope (or authorization policies) and associate it with controllers and/or controller actions. This ensures that the ASP.NET Core authorization handler is executed correctly (via the `Authorize` attribute) to enforce the OAuth authorization grants.

Any client application - configured previously in the authority server - can invoke the microservice Web API (either directly or using the service's [client library](./client-libraries.md)) by including a valid access token (obtained from the authority server) in the request.

The access will be granted or not to that application, depending on the access token validity and on the scopes it includes.