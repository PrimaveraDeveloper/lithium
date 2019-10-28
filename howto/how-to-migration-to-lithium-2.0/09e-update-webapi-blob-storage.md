# How to Upgrade a Microservice from Lithium v1.0 to Lithium v2.0

## 9e - Update References to Blob Storage

### 9e.1 - Modify the instantiation of `IBlobStorageService` to use dependency injection:

Code like this:

```csharp
private IBlobStorageService BlobStorageService
{
    get
    {
        return StorageServiceFactory.GetTableStorageService(this.BlobStorageServiceConfiguration);
    }
}
```

Should be modified to:

```csharp
private IBlobStorageService BlobStorageService
{
    get
    {
        return this.ServiceProvider.GetRequiredService<IBlobStorageService>();
    }
}
```

### 9d.2 - Replace references to `BlobStorageServiceConfiguration` with `AzureBlobStorageOptions`:

```csharp
private AzureBlobStorageOptions BlobStorageServiceConfiguration
{
    get
    {
        return this.ServiceProvider.GetRequiredService<AzureBlobStorageOptions>();
    }
}
```

> NOTE: Most references to the blob storage configuration can simply be removed after changing how the `IBlobStorageService` is instantiated (because that dependency will be automatically injected).

## Next

> [9f - Update Background Services Custom Code](./09f-update-webapi-background-services.md)