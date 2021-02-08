# Capability - Authorization Specification

> Tags: Authorization Authentication Identity Canvas

The Lithium Framework provides, as part of the artifacts generated for a microservice, the authorization specification, which complements the service specification.

This file, generated in the `DesignTime` project, is called `Authorization.gen.authzspec.json`.

It includes the specification of the authorization resources required by the service that can be directly inferred from the service model:

- API resource.
- API scopes.
- Authorization policies.
- Clients.

Being a machine-readable file (JSON), this specification can then be used to automate the creation of these resource in the Authority Server (in fact, version 5.0 of Identity Server supports importing this kind of files).