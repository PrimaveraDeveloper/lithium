# PRIMAVERA Hydrogen Release Notes

> These release notes include only the most important releases.

### <a name="2.0.10.368"></a>Version 2.0.10.368

- Updated dependencies to .NET Core 3.1.12.

**Primavera.Hydrogen.Azure.Client**

- `ChainedTokenCredentialBuilder` and `ChainedTokenCredential` allow building chains of credentials to be used as `TokenCredential` instances to initialize Azure SDK client libraries.  

**Primavera.Hydrogen.Storage.Azure**

- `IBlobStorageService` now supports authorization with the Azure Active Directory (like `ISecretsStorageService`).
- New configuration options in `AzureBlobStorageOptions` to support connecting using the Service URI and credentials instead of a connection string.

**Primavera.Hydrogen.Security.Azure**

- **[Breaking]** `AzureKeyVaultSecretsStorageOptions` was modified to support `ChainedTokenCredentialBuilder` and deal better with authorization with Azure Activity Directory using managed identity, developer credentials, and client secret credentials.

**Primavera.Hydrogen.Core**

- Added subject and body encodings to `SmtpClient`.

**Primavera.Hydrogen.DesignTime.Specs**

- **[Breaking]** `ServiceSpec` no longer includes specification for authorization resources (API resources, clients, etc.).
- Added `AuthorizationSpec` to specify authorization resources (API scopes, API resources, clients, and authorization policies). This type is fully compatible with Identity Server v5.0.

### <a name="2.0.9.360"></a>Version 2.0.9.360

- Updated dependencies to .NET Core 3.1.11.

### <a name="2.0.8.357"></a>Version 2.0.8.357

**Primavera.Hydrogen.AspNetCore**

- Refactored `BackgroundService` and `BackgroundWorker` to support automatic locking (optional), a pessimistic locking mechanism that uses a shared blob to prevent different instances from running at the same time and create race conditions (e.g. performing the same work multiple times).
- **[Breaking]** `BackgroundService.AcquireLockAsync()` is replaced with `BackgroundService.TryAcquireLockAsync()` and `BackgroundService.ReleaseLockAsync()` is replaced with `BackgroundService.TryReleaseLockAsync().` These new methods should not be invoked directly from the service's custom code.
- **[Breaking]** `BackgroundWorker.AcquireLockAsync()` is replaced with `BackgroundWorker.TryAcquireLockAsync()` and `BackgroundWorker.ReleaseLockAsync()` is replaced with `BackgroundWorker.TryReleaseLockAsync()`. These new methods should not be invoked directly from the worker's custom code.
- **[Breaking]** `BackgroundService` and `BackgroundWorker` now include an abstract property called `UseLocking` to specify whether automatic locking is enabled.

### <a name="2.0.7.350"></a>Version 2.0.7.350

**Primavera.Hydrogen.Security.Azure**

- Replaced the Azure KeyVault external package to use `Azure.Security.KeyVault.Certificates` (version 4.1.0), `Azure.Security.KeyVault.Keys` (version 4.1.0) and `Azure.Security.KeyVault.Secrets` (version 4.1.0).
- **[Breaking]** Added property `TenantId` to `AzureKeyVaultSecretsStorageOptions` to set the client application client credentials (KeyVault service clients no longer accept authentication callbacks to identify the authorization server and the tenant identifier).
- **[Breaking]** Renamed property `AutomaticAuthenticationEnabled` in `AzureKeyVaultSecretsStorageOptions` to `ManagedIdentityEnabled`.
- Renamed member `Automatic` in `AzureKeyVaultAuthenticationKind` to `ManagedIdentity` and added `Developer`.
- **[Breaking]** `ISecretsStorageService.DeleteSecretAsync()` now returns a `Task`, not a `Task<Secret>`.
- **[Breaking]** Properties `CreatedOn`, `UpdatedOn`, `NotBefore`, `Expires` of `Attributes` are now `DateTimeOffset?`, not `DateTime?`.
- **[Breaking]** `Certificate.SecretIdentifier` is now a `Uri`, not a `SecretIdentifier`.
- **[Breaking]** `Certificate.KeyIdentifier` is now a `Uri`, not a `KeyIdentifier`.
- **[Breaking]** Properties `Identifier` and `Storage` in `CertificateIdentifier`, `KeyIdentifier` and `SecretIdentifier` are now `Uri`, not `string`.
- `IAzureTokenService` and the whole `Primavera.Security.Azure.AppAuthentication` namespace were removed.

### <a name="2.0.7.348"></a>Version 2.0.7.348

**Primavera.Hydrogen.Search.Azure**

- Replaced the Azure Search external package to use `Azure.Search.Documents` (version 11.1.1).
- Added `Search<T>` and `SearchAsync<T>` to `ISearchDocumentsOperations`.

### <a name="2.0.7.347"></a>Version 2.0.7.347

**General**

- Updated Primavera.Hydrogen.DesignTime.Configuration to 2.0.0.48.
- **[Breaking]** Removed all obsolete types (e.g. `TestServerFixture`) and obsolete members (e.g. `SmartGuard.IsEmail()`).

**Primavera.Hydrogen.Storage.Azure**

- Replaced the Azure Blob Storage external package to use `Azure.Storage.Blobs` (version 12.7.0).
- **[Breaking]** Removed the `IBlockBlobReference.UploadFromStreamAsync()` overload that accepted `length`.
- **[Breaking]** Removed `mode` from `IBlockBlobReference.DownloadToFileAsync()`.
- **[Breaking]** Removed `IBlobReference.Properties` `IBlobReference.BlobType`. Use `IBlobReference.GetPropertiesAsync()` instead.