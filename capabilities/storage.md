# Capability - Storage

> Tags: Tables, Blobs, Files, Isolated Storage, Database, Entity Framework

As any application, microservices often require the capability of storing data (any kind of state) to implement its specific business logic.

The Lithium Framework supports the following kinds of storage:

- Table storage: a NoSQL structured tabular storage, optimized for storing data indexed by only 2 columns (partition and row key).
- Blob storage: a file system-like storage system, optimized for storing raw data (bytes), text, images and files.
- Isolated storage: a data storage mechanism with isolation and location abstraction.

> Using relational databases (like SQL Server) for storage in microservices is not recommended, thus it is not supported out of the box by the Lithium Framework.

## Table Storage

Table storage is provided by [ITableStorageService](../ref/hydrogen-2.0/Storage.Abstractions.md).

Various microservices use this kind of storage. [Notifications Service](../dir/common/ns.md) is one example.

## Blob Storage

Blob storage is provided by [IBlobStorageService](../ref/hydrogen-2.0/Storage.Abstractions.md).

Various microservices use this kind of storage. [Settings Service](../dir/common/ss.md) is one example.

## Isolated Storage

Isolated storage is provided by [IIsolatedStorageService](../ref/hydrogen-2.0/Storage.Abstractions.md).

The [Certificates Service](../dir/common/cs.md) uses this kind of storage in the client library.