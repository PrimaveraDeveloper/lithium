# Primavera.Hydrogen.Storage.Files

**Class library that contains types that define generic storage services that use local files.**

## Isolated Storage (`FilesIsolatedStore`)

The `FilesIsolatedStore` implements the `IIsolatedStorageService` using the Windows Isolated Storage (in the local file system).

The service implementation should be registered using the following extension method for `IServiceCollection`:

```csharp
IServiceCollection services = (...);
services.AddFilesIsolatedStorage();
```

> This implementation of the service does not have any configuration options.