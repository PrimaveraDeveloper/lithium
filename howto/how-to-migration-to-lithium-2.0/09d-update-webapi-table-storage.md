# How to Upgrade a Microservice from Lithium v1.0 to Lithium v2.0

## 9d - Update References to Table Storage

### 9d.1 - Modify the instantiation of `ITableStorageService` to use dependency injection:

Code like this:

```csharp
private ITableStorageService TableStorageService
{
    get
    {
        return StorageServiceFactory.GetTableStorageService(this.TableStorageServiceConfiguration);
    }
}
```

Should be modified to:

```csharp
private ITableStorageService TableStorageService
{
    get
    {
        return this.ServiceProvider.GetRequiredService<ITableStorageService>();
    }
}
```

### 9d.2 - Replace references to `TableStorageConfiguration` with `AzureTableStorageOptions`:

```csharp
private AzureTableStorageOptions TableStorageServiceConfiguration
{
    get
    {
        return this.ServiceProvider.GetRequiredService<AzureTableStorageOptions>();
    }
}
```

> NOTE: Most references to the table storage configuration can simply be removed after changing how `ITableStorageService` is instantiated (because that dependency will be automatically injected).

## Next

> [9e - Update References to Blob Storage](./09e-update-webapi-blob-storage.md)