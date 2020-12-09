# PRIMAVERA Hydrogen Release Notes

> These release notes include only the most important releases.

### <a name="2.0.7.347"></a>Version 2.0.7.347

**General**

- Updated Primavera.Hydrogen.DesignTime.Configuration to 2.0.0.48.
- [Breaking] Removed all obsolete types (e.g. `TestServerFixture`) and obsolete members (e.g. `SmartGuard.IsEmail()`).

**Primavera.Hydrogen.Storage.Azure**

- Replaced the Azure Blob Storage external package to use `Azure.Storage.Blobs` (version 12.7.0).
- [Breaking] Removed the `IBlockBlobReference.UploadFromStreamAsync()` overload that accepted `length`.
- [Breaking] Removed `mode` from `IBlockBlobReference.DownloadToFileAsync()`.
- [Breaking] Removed `IBlobReference.Properties` `IBlobReference.BlobType`. Use `IBlobReference.GetPropertiesAsync()` instead.