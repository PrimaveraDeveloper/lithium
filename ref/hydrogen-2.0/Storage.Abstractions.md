# Primavera.Hydrogen.Storage.Abstractions

**Class library that contains types that define abstractions for generic storage services.**

## Table Storage (`ITableStorageService`)

The `ITableStorageService` allows performing operations on a structured (tabular) NoSQL data store in the cloud like the Azure Table Storage.

Instances of this service should be created via dependency injection (see concrete implementations for more information).

Once the service instance is available it allows retrieving a reference to a table (that may or may not exist in the storage) and perform operations
on that table, like creating it or deleting it, or on the records in the table (insert, replace, retrieve).

### Records vs Entities

You can manipulate the records in a table using the general-purpose `TableRecord` class that holds the record's keys and a dictionary containing all the properties.

For example:

```csharp
ITableStorageService service = services.GetRequiredService<ITableStorageService>();
ITableReference table = service.GetTable("MyTable");

TableRecord record = new TableRecord("Key1", "Key2);
record["Property1"] = "value1";
record["Property2"] = 10;

await table.InsertRecordAsync(record).ConfigureAwait(false);

TableRecord retrieved = await table.RetrieveRecordAsync("Key1", "Key2");
```

You can also model a TableEntity, which is a class that derives from `TableEntity` or implements `ITableEntity`, basically a POCO. Then you can use that type
to manipulate the records:

```csharp
ITableStorageService service = services.GetRequiredService<ITableStorageService>();
ITableReference table = service.GetTable("MyTable");

MyEntity entity = new MyEntity("Key1", "Key2")
{
    Property1 = "value",
    Property2 = 10
};

await table.InsertEntityAsync(entity).ConfigureAwait(false);

MyEntity retrieved = await table.RetrieveEntityAsync<MyEntity>("Key1", "Key2");
```

### Supported Data Types

You can storage properties using the ITableStorageService for the following types:

- `byte[]`
- `bool` and `bool?`
- `DateTimeOffset` and `DateTimeOffset?`
- `DateTime` and `DateTime?`
- `double` and `double?`
- `int` and `int?`
- `long` and `long?`
- `Guid` and `Guid?`
- `string`

When using entities that following types are also supported (although stored as strings):

- `TimeSpan` and `TimeSpan?`
- `Enum` and `Enum?`

> NOTE: The `DateTimeOffset` values will always be stored in UTC.

### Retrieving Data

The service provides multiple functions to retrieve records or entities from the table.

You can retrieve all records:

```csharp
IList<TableRecord> records = await service.RetrieveRecordsAsync().ConfigureAwait(false);
IList<MyEntity> entities = await service.RetrieveEntitiesAsync<MyEntity>().ConfigureAwait(false);
```

Retrieve all the retrieve the records for specific first key:

```csharp
IList<TableRecord> records = await service.RetrieveRecordsAsync("key1").ConfigureAwait(false);
IList<MyEntity> entities = await service.RetrieveEntitiesAsync<MyEntity>("key1").ConfigureAwait(false);
```

Retrieve the records that satisfy a given query:

```csharp
ITableQuery query = (...)
IList<TableRecord> records = await service.RetrieveRecordsAsync(query).ConfigureAwait(false);
IList<MyEntity> entities = await service.RetrieveEntitiesAsync<MyEntity>(query).ConfigureAwait(false);
```

> NOTE: How you create `ITableQuery` instances depends on the concrete implementation of the service. See that documentation for more details.

### Modeling Entities

Entities can either simply implement the `ITableEntity` interface or derive from the base class `TableEntity`.

The service will try to store and retrieve all the properties, given they:

- Are not named "Key1", "Key2", or "Timestamp".
- Are not marked to be ignored (see IgnorePropertyAttribute).
- The property is public and as a getter and a setter.
- The data type is supported.

Here's an example:

```csharp
public class MyEntity : TableEntity
{
    public MyEntity()
        : base()
    {
    }

    public MyEntity(string key1, string key2)
        : base(key1, key2)
    {
    }

    public string Value1
    {
        get;
        set;
    }

    public int Value2
    {
        get;
        set;
    }
}
```

> NOTE: Any table entity must always have a parameter-less constructor.

Properties in a table entity can be ignored by annotating them with the IgnorePropertyAttribute, like this:

```csharp
public class MyEntity : TableEntity
{
    (...)

    [IgnoreProperty]
    public double Value3
    {
        get;
        set;
    }
}
```

# Blob Storage (`IBlobStorageService`)

The `IBlobStorageService` allows performing operations on a binary/text data store in the cloud like the Azure Blob Storage.

Instances of this service should be created via dependency injection (see concrete implementations for more information).

Once the service instance is available it allows retrieving a reference to a container (that may or may not exist in the storage) and perform operations on that it, like creating it or deleting it, or on the blobs in that container (upload, download, etc.).

## Manipulating Containers

Once a reference container is obtained, the container can be created, deleted, a blob reference can be retrieved, etc.

Example:

```csharp
IAzureBlobStorageService service = serviceProvider.GetRequiredService<IAzureBlobStorageService>();
IContainerReference container = service.GetContainer("mycontainer);
await container.CreateAsync();
IBlockBlobReference blob = container.GetBlockBlob("myblob");
(...)
```

>  Both `IContainerReference` and `IBlockBlobReference` provide asynchronous methods - e.g. `CreateIfNotExistsAsync()` - and synchronous methods - e.g. `Delete()`.

## Manipulating Blobs

To manipulate a blob, as seen above, a reference to it must be obtained first. Then it is is possible to upload or download data, or delete it.

```csharp
IContainerReference = (...);
IBlockBlobReference blob = container.GetBlockBlob("myblob");
blob.CreateIfNotExists();
```

## Uploading Data

A block blob support uploading data in different ways:

- Uploading the blob bytes: `UploadFromByteArrayAsync()`.
- Uploading text: `UploadTextAsync()`.
- Uploading from a file: `UploadFromFileAsync()`.
- Uploading from a stream: `UploadFromStreamAsync()`.

## Downloading Data

The blob can also be downloaded in different ways:

- Downloading the blob bytes: `DownloadToByteArrayAsync()`.
- Downloading text: `DownloadTextAsync()`.
- Downloading from a file: `DownloadToFileAsync()`.
- Downloading from a stream: `DownloadToStreamAsync()`.

## Permissions and Public Addresses

By default, when a container is created, its blobs  will not be accessible directly using a public URL.

You can change the access permissions of a container, either:

- By specifying the permissions when you create the blob: `Create(ContainerPublicAccessType)`.
- Later, using the `SetPermissions(IContainerPermissions)` method.

The public address of a blob is available from: `IBlobReference.Uri`.