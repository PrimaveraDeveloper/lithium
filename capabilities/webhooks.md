# Capability - Webhooks

> Tags: REST, JSON, Events, Subscriptions

One important capability for a Web API is the ability to notify client applications when certain events happen. This is typically implemented using webhooks.

The Lithium Framework supports adding webhooks to microservices and publishing those events to client applications (the subscribers).

The following How To articles describe that:

- [How to publish webhooks events from a microservice](../howto/howto-publish-webhooks.md)
- [How to receive webhooks events from a microservice](../howto/howto-receive-webhooks.md)

Hydrogen includes a set of services that support webhooks:

- [AspNetCore.Webhooks.Abstractions](../ref/hydrogen-2.0/AspNetCore.Webhooks.Abstractions.md)
- [AspNetCore.Webhooks](../ref/hydrogen-2.0/AspNetCore.Webhooks.md)

There are also [samples](../samples/README.md) available.