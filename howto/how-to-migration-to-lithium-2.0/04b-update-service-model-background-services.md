# How to Upgrade a Microservice from Lithium v1.0 to Lithium v2.0

## 4b - Update Background Services

### 4b.1 - Queued Background Services

If the service includes a Queued Background Service, make sure that the property "Use Worker" is true and the property "Worker" points to the correct background worker.

### 4b.2 - Timed Background Services

If the service includes a Timed Background Service, make sure that the property "Use Worker" is true and the property "Worker" points to the correct background worker.

## Next

> [5 - Replace Deprecated Namespaces](./05-replace-deprecated-namespaces.md)