# Capability - Web API

> Tags: REST, JSON, Models, Resources, HTTP

The most important feature of the Lithium Framework is the capability of modeling and generating the Web API for a microservice.

The resulting Web API adheres to the [RESTful principles](https://restfulapi.net/) and includes a set of additional features "out of the box":

- The endpoints are secured by default, using OAuth [authorization](./authorization.md).
- All the endpoints use [versioning](./versioning-web-api.md).
- [Validation](./validation.md) of arguments can be modeled and automatically generated.
- Common [errors are handled automatically](./error-handling.md).
- The [documentation](./doc-web-api.md) is automatically generated.
- A C# [client library](./client-libraries.md) is automatically generated.
- Etc.

The Web API endpoint routes are generated using the following form:

`{server}/api/v{version}/{route}`

Like in the following examples:

`POST https://lithium-settings.primaverabss.com/api/v1.0/productsettings`

`GET https://lithium-settings.primaverabss.com/api/v1.0/usersettings/102948457/appTheme`