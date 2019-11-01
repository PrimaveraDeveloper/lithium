# How to Upgrade a Microservice from Lithium v1.0 to Lithium v2.0

## 4a - Update Versioning

Since you are upgrading the microservice only to support a new version of the Lithium Framework, it is supposed that you do not add any new feature to the service or change any of its features.

This implies that you want to end up with a new version of the service - e.g. version 2.0 - that should be completely compatible with the previous version - e.g. version 1.0. That means that the service Web API should be exactly equal to the previous version, except for its default version.

Given the way versioning works in the Lithium Framework, you want to end up with a new version of the service that supports both versions (the new and the previous) in the API.

Supporting two versions in the API is CRITICAL to ensure that client applications would still be able to use the service, regardless whether they use the old version of the client library or invoke the API directly.

> You could maintain the same version in theory but, since there is a major upgrade of .NET Core, Hydrogen, etc., that would be a bad idea. Changing the major version of the service ensures that client applications using the service will be required to make a decision on how and when to upgrade.

### 4a.1 - Increase Service Version

Increase the service version (e.g. 1.0 to 2.0) and ensure that the version property is specified in the simple for of `[Major].[Minor]`.

> The new version will be used as default API version and to set the version of all the service assemblies (see `AssemblyInfo.gen.cs`).

### 4a.2 - Add API Version for Backward Compatibility

Using the design context menu, add a new "API Version" model element. Set the version to the previous version that was being used in the service (e.g. 1.0).

> This ensures that the Web API will support both versions, the old and the new.

## Next

> [4b - Update Background Services](./04b-update-service-model-background-services.md)