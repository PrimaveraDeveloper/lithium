# Primavera.Hydrogen.Search.Abstractions

**Class library that contains types that define abstractions for generic search services.**

## Search (`ISearchService`)

The `ISearchService` allows creating indexes and performing search operations on a search service in the cloud like the Azure Search Service.

Instances of this service should be created via dependency injection (see concrete implementations for more information).

Once the service instance is available it allows creating data sources, indexes, indexers, and executing searches on indexes.

### Data Sources

The search service executes searches on indexes (previously created and indexed).

Indexes retrieve their data - indexing is performed by "indexers" - from a given data source, which can be a SQL database, a DocumentDb database, a blob or a table.

A data source is created like this, specifying its name and type, and the properties required to connect to the source.

```csharp
ISearchService service = provider.GetRequiredService<ISearchService>();

SearchDataSource dataSource = new SearchDataSource()
{
    Name = "ds-mytable",
    Type = SearchDataSourceType.Table,
    Credentials = new SearchDataSourceCredentials(connectionString),
    Container = new SearchDataContainer("MyTable"),
    DataDeletionDetectionPolicy = new SoftDeleteColumnDeletionDetectionPolicy()
    {
        SoftDeleteColumnName = "RecordIsDeleted",
        SoftDeleteMarkerValue = "true"
    }
};

await service.DataSources.CreateAsync(dataSource).ConfigureAwait(false);
```

### Indexes

An index describes the data source and how each field should be indexed.

```csharp
ISearchService service = provider.GetRequiredService<ISearchService>();

SearchIndex index = new SearchIndex()
{
    Name = "idx-mytable",
    Fields = new List<SearchIndexField>()
    {
        new SearchIndexField()
        {
            Name = "key",
            Type = SearchIndexFieldDataType.String,
            IsKey = true,
            IsSearchable = true
        },
        new SearchIndexField()
        {
            Name = "value",
            Type = SearchIndexFieldDataType.Int32,
            IsKey = false,
            IsSearchable = false,
            IsRetrievable = true
        }
    }
}

await service.Indexes.CreateAsync(index).ConfigureAwait(false);
```

### Indexers

To index the data source, an indexer first needs to be created and executed. The execution can be scheduled and the status of the indexer can be retrieved at any moment.

```csharp
ISearchService service = provider.GetRequiredService<ISearchService>();

SearchIndexer indexer = new SearchIndexer()
{
    Name = "idxx-mytable",
    Description = $"Index for MyTable",
    DataSourceName = "ds-mytable",
    TargetIndexName = "idx-mytable",
    Schedule = new SearchIndexingSchedule()
    {
        Interval = TimeSpan.FromHours(2)
    }
};

await service.Indexers.CreateAsync(indexer).ConfigureAwait(false);
```

### Searching

Search is performed on an index client, like in the following example:

```csharp
ISearchService service = provider.GetRequiredService<ISearchService>();

ISearchServiceIndexClient indexClient = this.Fixture.SearchService.Indexes.GetClient("idx-mytable");

SearchResults results = await indexClient
    .Documents
        .SearchAsync(
            "keywork*")
        .ConfigureAwait(false);
```

SearchResults includes all the matching documents:

```csharp
foreach (SearchResultsDocument searchResult in searchResults)
{
    string key = searchResult.Document
        .GetPropertyValueOrDefault("key", string.Empty);
    
    int value = searchResult.Document
        .GetPropertyValueOrDefault("value", 0);
}
```