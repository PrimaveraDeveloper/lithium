# How to Upgrade a Microservice from Lithium v1.0 to Lithium v2.0

## 9f - Update Background Services Custom Code

### 9f.1 - Update Queued Background Services Custom Code

If the service includes a queued background service with a worker, an override for `QueuedBackgroundService<TWorkItem>.Queue` will be required. The code should be like this:

```csharp
/// <inheritdoc />
public override IBackgroundWorkQueue<RequestsProcessorWorker> Queue
{
    get
    {
        return this.ServiceProvider.GetRequiredService<IBackgroundWorkQueue<RequestsProcessorWorker>>();
    }
}
```

### 9f.2 - Update Background Workers Custom Code

If the custom code includes a property named `Logger`, remove it because it is already defined in the base class.

Example:

```csharp
private ILogger Logger
{
    get
    {
        if (this.fieldLogger == null)
        {
            this.fieldLogger = this.ServiceProvider.GetRequiredService<ILogger<RequestsProcessorWorker>>();
        }

        return this.fieldLogger;
    }
}
```

## Next

> [9g - Update Application Settings](./09g-update-webapi-app-settings.md)