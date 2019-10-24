# Primavera.Hydrogen.Telemetry.Abstractions

**Class library that contains types that provide generic telemetry services.**

## Scenarios

There are basically two scenarios where an application would benefit from recording telemetry:

- Server-based telemetry.
- Custom telemetry.

### Server-based telemetry

This is the case of, for example, any host application (like a Web application) we you want to automatically record telemetry for requests, exceptions, logging, etc.

Hydrogen supports this scenarios through custom implementations of telemetry and that is why there is no specific abstraction for that.

### Custom telemetry

This is the case when the application wants to explicitly record telemetry like events, exceptions, metrics and trace messages (logging).

The application can do that by instantiating a telemetry client and making it record the desired telemetry (see Telemetry Client).

## Telemetry Client (ITelemetryClientService)

The `ITelemetryClientService` service allows instantiating clients to record telemetry (events, metrics, exceptions, and traces).

Instances of this service should be created via dependency injection (see concrete implementations for more information).

For example:

```csharp
using (ITelemetryClientService service = services.GetRequiredService<ITelemetryClientService>())
{
    service.WriteEvent("myevent");
}
```
