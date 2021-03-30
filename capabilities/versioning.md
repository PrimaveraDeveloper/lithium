# Capability - Versioning

> Tags: Compatibility

Versioning is very important for any application. And it is particularly important for microservices.

All microservices should be explicitly versioned, considering that any breaking change to the Web API, should result in a new version and decisions on how the previous versions should be supported.

The service model supports defining the current service version and specifying additional API versions (fully retro-compatible).

By default, all endpoints will respond to all API versions (the service version plus all API versions added to the model). You can then restrict controllers (and all its actions) and individual actions to accept only one or more of the API versions set in the service.

For more information, see [Service Designer](../ref/sdk-2.0/service-designer.md).

The following How To articles are also available:

- [How to add multiple versions to a microservice](../howto/howto-add-multiple-versions.md)