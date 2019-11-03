# Primavera.Hydrogen.TextMessaging.Twilio

**Class library that contains types that allow managing text messages (SMS) using the Twilio gateway.**

## Text Messaging (`TwilioTextMessagingService`)

The `TwilioTextMessagingService` implements the `ITextMessagingService` using the Twilio gateway.

The service implementation should be registered using one of the following extension methods for `IServiceCollection`:

```csharp
IServiceCollection services = (...);
services.AddTwilioTextMessaging();
```

This will register the service using default configuration options set my a configuration section named `TwilioTextMessagingOptions`.

```csharp
IServiceCollection services = (...);
services.AddTwilioTextMessaging(
    (options) =>
    {
        // (...)
    });
```

This will register the service using the specified configuration delegate, after reading the default configuration section (if found).

### `TwilioTextMessagingOptions`

This options class provides the following configuration options:

- `AccountSid` (required) (no default value) - the Twilio account SID.
- `AuthToken` (required) (no default value) - the authorization token for the Twilio account.
