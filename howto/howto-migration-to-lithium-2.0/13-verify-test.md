# How to Upgrade a Microservice from Lithium v1.0 to Lithium v2.0

## 13. Verify and Test the Service

### Service Routes

If the service generates a Web API it is very important that the API routes remain unchanged after the upgrade.

Compare `Models\GeneratedCode\Routes.gen.cs` with its previous version (before the upgrade) and make sure that none of the routes have changed.

> The `v{apiVersion:apiVersion}` part of the routes is expected to change to `v{version:apiVersion}`.

### Testing with Postman

You can use Postman to test the service Web API.

As part of the upgrade, a Postman collection is now generated - see `Design\GeneratedCode\Postman.gen.postman_collection.json` - and it can be used for that purpose.

> The service should support two versions in the API (the one before the upgrade and the new one). You can verify that by editing the `apiVersion` collection variable and performing requests on the Web API using the two versions.

>This behavior is CRITICAL to ensure backward compatibility of the service after the upgrade.

### Testing with the Console Client

The console client is another way to test the behavior of the service. It should work as it worked before the upgrade.

### Attention

The following aspects of service behavior should be tested with extra attention:

- Authorization scopes and authorization policies.
- Background services and background workers.
- OIDC authentication.
- Custom user interface.
- REDIS cache.
- Table storage and blob storage.

### THAT'S IT! The microservice is upgraded to Lithium 2.0.