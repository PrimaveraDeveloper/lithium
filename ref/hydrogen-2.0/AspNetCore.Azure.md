# Primavera.Hydrogen.AspNetCore.Azure

**Class library that contains types that support the development of Web applications with ASP.NET Core hosted on Azure.**

## Data Protection

[ASP.NET Core Data Protection](https://docs.microsoft.com/en-us/aspnet/core/security/data-protection/) provides a mechanism to store sensitive data using the cryptographic API. These services are often used to store encryption keys.

This sensitive data most of the times needs to be "shared" between all the machines behind a server farm so that data encrypted on one machine can be decrypted on another.

There are multiple ways for sharing the data protection keys. `AzureDataProtectionServiceCollectionExtensions` provides extension methods that allow sharing the protection keys using a blob on Azure Blob Storage.

Example:

```csharp
services.Configure<AzureBlobStorageDataProtectionOptions>(this.Configuration.GetSection(nameof(AzureBlobStorageDataProtectionOptions)));
services.AddDataProtectionWithBlobStorage();
```

```csharp
services.AddDataProtectionWithBlobStorage(
    (options) =>
    {
        options.ApplicationName = "my-app"
    });
```

Notice that the behavior of these extension methods can be configured using `AzureBlobStorageDataProtectionOptions`. The following options are available:

- `ApplicationName` - the unique of the application (used as the blob name, it must be unique within the machines that serve the application)
- `UseBlobStorage` - allows disabling the behavior of storing the keys in blob storage, which means that the default configuration of the Data Protection services will be applied.

> This last setting is useful for Development environments, that typically will not run on a Web farm with  multiple servers.

### Dependency on Azure Blob Storage

Since these extension methods configure the application to store the keys in a blob, the IBlobStorageService needs to be configured to use Azure Blob Storage (see [Primavera.Hydrogen.Storage.Azure](./Storage.Azure.md) for more information).

```csharp
services.Configure<AzureBlobStorageOptions>(
    this.Configuration.GetSection(nameof(AzureBlobStorageOptions)));
services.Configure<AzureBlobStorageDataProtectionOptions>(
    this.Configuration.GetSection(nameof(AzureBlobStorageDataProtectionOptions)));
services.AddAzureBlobStorage();
services.AddDataProtectionWithBlobStorage();
```