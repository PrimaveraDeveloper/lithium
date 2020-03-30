# Service Management Service (SM) Specification v2.0

The Service Management Service allows invoking the operations in the Service Management Web API. It acts as a simple for that Web API (thus it only provides a client library).

The client library allows executing multiple operations on the Web API, organized in the following "categories":

- Solutions
- Subscriptions
- User Subscriptions
- Static Data
- Business Partners
- Subscription Operations
- Payments
- Accounts

## Scope

The Service Management Web API requires the following scopes:

- `service_management`

## Example

```csharp
using (ServiceManagementClient client = (...))
{
    GetSubscriptionRequest request = (...);

    ServiceOperationResult<SubscriptionData> response =
        await client.Subscriptions.GetSubscriptionAsync(
            request)
            .ConfigureAwait(false);

    SubscriptionData subscription = response.Body;
}
```