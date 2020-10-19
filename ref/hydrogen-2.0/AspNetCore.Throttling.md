# Primavera.Hydrogen.AspNetCore.Throttling

**Class library that contains services for implementing throttling in ASP.NET Core applications.**

This library includes extensions for `IServiceCollection` that allow configuring an external service - provided by the `AspNetCoreRateLimit` library - that implements throttling on client-rate limiting.

## Client rate limiting

This kind of throttling accepts or rejects requests based on the number of requests performed by each client during a period of time.

Clients are identified by the corresponding OAuth claim (set by the authorization handler) and/or by a specific header that should be present in the request.

> Requests whose client cannot be identified, are considered anonymous and handled by the throttling middleware as sent from the Anon client. This allows to also limit request these anonymous requests using similar rules.

## Configuring Throttling

Configuring throttling in an application, involves two steps:

- Registering the throttling services and middleware in the service container.
- Providing configuration options, including the rules that should be enforced.

This library provides extension methods to register the services and middleware.

First, the services need to be added to the service container:

```csharp
IServiceCollection services = (...);
services.AddThrottlingOnClientRate(
    (options) =>
    {
        // (...)
    });
```

Then, the middleware needs to be registered in the request pipeline:

```csharp
app.UseThrottlingOnClientRate();
```

## `ThrottlingOptions`

This is the options class that allows configuring the throttling services:

- `Enabled` - indicates whether throttling is enabled.
- `ClientRateLimiting` - defines specific configuration options for client rate limit throttling.
- `Caching` - defines specific options concerning the cache used by the throttling services.

### ThrottlingClientRateLimitingOptions

- `Enabled` - indicates whether this kind of throttling is enabled.
- `ClientWhitelist` - a list of client identifiers for which throttling should NOT be applied.
- `GeneralRules` - a list of general rules which apply to any client.
- `ClientRules` - a list of client-specific rules.

### ThrottlingCachingOptions

- `ClientRulePrefix` - the prefix for the cache keys that store the client rules.
- `RateLimitCounterPrefix` - the prefix for the cache keys that store the rate limit counters (that count the requests performed on each endpoint).

## Rules

Rules can be of two types:

- General - apply to an endpoint of the API, regardless of the client invoking it.
- Client-specific - apply to an endpoint and a specific client (and override any general rule that applies to same endpoint).

Here's an example:

```json
{
    "ThrottlingOptions" : {
        "ClientRateLimiting" : {
            "ClientWhitelist": [
                "client1",
                "client2"
            ],
            "GeneralRules": [
                {
                    "Endpoint": "*:*",
                    "Period": "10m",
                    "Limit": 100
                },
                {
                    "Endpoint": "get:*",
                    "Period": "20m",
                    "Limit": 2000
                }
            ],
            "ClientRules": [
                {
                    "ClientId": "client3",
                    "Rules": [
                        {
                            "Endpoint": "*:/api/*",
                            "Period": "1m",
                            "Limit": 10
                        }
                    ]
                }
            ]
        }
        "Caching" : {
            "ClientRulePrefix" : "MYAPP_CR",
            "RateLimitCounterPrefix" : "MYAPP_RC"
        }
    }
}
```

The endpoint specification support glob patterns and it has the form `{HttpVerb}:{HttpPath}`.

> Notice how the client rule for client3 overrides both the previous two general rules.