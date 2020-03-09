# Capability - Authentication

> Tags: OpenID Connect, OIDC, Identity Server, Sign-in, Login, SSO

By default, the Web API endpoints of a microservice do not require user authentication. Neither do the microservice's default endpoints (like the [home page](./home-page.md)).

There are scenarios where a microservice might need to provide endpoints that require authenticating the user (for example, if the service provides a back-office to allow some users to manage it).

The Lithium Framework supports such scenarios by allowing the developer to configure [OpenID Connect (OIDC)](https://openid.net/connect/) and developing custom authorization policies and even endpoints that serve some kind of [user interface](./user-interface.md).