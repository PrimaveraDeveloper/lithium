# Primavera.Hydrogen.TextMessaging.Abstractions

**Class library that contains types that define abstraction for text messages (SMS).**

## Text Messaging (`ITextMessagingService`)

The `ITextMessagingService` allows sending SMS messages using a gateway like Twilio.

Instances of this service should be created via dependency injection (see concrete implementations for more information).

### Composing Text Messages

SMS messages should be composed using the `TextMessage` type like in the following example:

```csharp
TextMessage message = TextMessage
    .Compose
    .Sender("+351961010100")
    .To("+351962020200")
    .Body("This is the message.");
```

### Sending Text Messages

```csharp
ITextMessagingService service = provider.GetRequiredService<ITextMessagingService>();
await service.SendMessageAsync(message).ConfigureAwait(false);
```