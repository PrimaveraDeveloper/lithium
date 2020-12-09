# PRIMAVERA Hydrogen Release Notes

> These release notes include only the most important releases.

### <a name="2.0.7.348"></a>Version 2.0.7.348

**Primavera.Hydrogen.Search.Azure**

- Replaced the Azure Search Storage external package to use `Azure.Search.Documents` (version 11.1.1).
- Added `Search<T>` and `SearchAsync<T>` to `ISearchDocumentsOperations`.

### <a name="2.0.7.347"></a>Version 2.0.7.347

**General**

- Updated Primavera.Hydrogen.DesignTime.Configuration to 2.0.0.48.
- <span style="color:red">[Breaking]</span> Removed all obsolete types (e.g. `TestServerFixture`) and obsolete members (e.g. `SmartGuard.IsEmail()`).

**Primavera.Hydrogen.Storage.Azure**

- Replaced the Azure Blob Storage external package to use `Azure.Storage.Blobs` (version 12.7.0).
- <span style="color:red">[Breaking]</span> Removed the `IBlockBlobReference.UploadFromStreamAsync()` overload that accepted `length`.
- <span style="color:red">[Breaking]</span> Removed `mode` from `IBlockBlobReference.DownloadToFileAsync()`.
- <span style="color:red">[Breaking]</span> Removed `IBlobReference.Properties` `IBlobReference.BlobType`. Use `IBlobReference.GetPropertiesAsync()` instead.