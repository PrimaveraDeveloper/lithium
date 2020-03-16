# File Store Service (FS) Specification v2.0

The File Store Service provides a centralized store for files that allows distributing files (versioned) to applications.

## Categories

The files are organized logically in categories, which allows, for example, retrieving all the files on a given category.

## Files

Files are versioned.

`FileInfo`:

- `Category`: `String`
- `Name`: `String`
- `Versions`: list of `FileVersionInfo`

`FileVersionInfo`:

- `Version`: `Integet`
- `ValidFrom`: `DateTime`
- `ValidTo`: `DateTime?`
- `ContentType`: `String`
- `Address`: `Url`,
- `Metadata`: list of `MetadataItemInfo`

`MetadataItemInfo`:

- `Key`: `String`
- `Value`: `String`

### Validity

Notice that you can specify the validity of a file version by specifying `ValidFrom` and, optionally `ValidTo`.

When downloading a specific file or file version, only those that remain valid will be returned.

## Uploading Files

To upload a new version of a file, you should use the `CreateFileVersion` operation:

```csharp
CreateFileVersionRequest request = new CreateFileVersionRequest()
{
    Category = (...),
    Name = (...),
    Version = (...),
    ValidFrom = (...),
    ValidTo = (...),
    ContentType = (...),
    Content = (...)
}

using FileStoreClient client = new FileStoreClient(...);

ServiceOperationResult<Uri> response = await client
    .Files
    .CreateFileVersionAsync(request)
    .ConfigureAwait(false);
```

## Downloading Files

You can retrieve a file using the operations available in the API:

```csharp
ServiceOperationResult<FileInfo> response = await client
    .Files
    .GetFileAsync(category, name)
    .ConfigureAwait(false);

ServiceOperationResult<FileInfo> response = await client
    .Files
    .GetFileValidAsync(category, name)
    .ConfigureAwait(false);

ServiceOperationResult<FileInfo> response = await client
    .Files
    .GetFileVersionAsync(category, name, version)
    .ConfigureAwait(false);
```

All file versions are also downloadable from a public address that is set in `FileVersionInfo`.

## Storage

Files are stored in Azure Blob Storage. The metadata about these files is stored in table storage.

The following configuration options set the instances used by the micro service:

```json
{
    "AzureBlobStorageOptions": {
        "ConnectionString": "(..."
    },
    "AzureTableStorageOptions": {
        "ConnectionString": "(...)"
    }
}
```