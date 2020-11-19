# Legal Devaluation Indexes Service (LDI) Specification v1.0

The current version of this service (only available for Portugal) allows to retrieve the devaluation indexes for a reference year. 

## Legal Devaluation Indexes Operations

The official monetary devaluation indexes, published by the Ministerial Order Nr. 317/2018, on 11 December, is a rate applicable to certain assets and rights sold for a specific year. These indexes are used when fill the Portuguese models 31 and 32.

### Get Devaluation Index (Portugal)

This operation allows to get the devaluation value for the reference year for the specified region.

```csharp
public async Task<ServiceOperationResult<double>> GetDevaluationIndexAsync(int year, int referenceYear, string location, CancellationToken cancellationToken = default);

public ServiceOperationResult<double> GetDevaluationIndex(int year, int referenceYear, string location);
```
#### Parameters

| Parameter | Type | Description | Rules |
| - | - | - | - |
| `year` | `int` | The year to get the devaluation index. | > 1900.  |
| `referenceYear` | `int` | The reference year to get the specific devaluation index. | > 2000.  |
| `location` | `string` | The location where the index is applied. | Required. RegEx(`(PT)|(AO)|(MZ)`).  |

#### Returns

| Return Type | Description |
| - | - |
| `double` | Returns a double value that represents the index value for the specified year. |

### Get Devaluation Index Range (Portugal)

This operation allows to get a list of devaluation indexes for a range of years and the specified region.

```csharp
public async Task<ServiceOperationResult<IEnumerable<DevaluationIndexResource>>> GetDevaluationIndexRangeAsync(int referenceYear, int startYear, int endYear, string location, CancellationToken cancellationToken = default);

public ServiceOperationResult<IEnumerable<DevaluationIndexResource>> GetDevaluationIndexRange(int referenceYear, int startYear, int endYear, string location);
```

#### Parameters

| Parameter | Type | Description | Rules |
| - | - | - | - |
| `referenceYear` | `int` | The start reference year to get the specific devaluation index. | > 2000.  |
| `startYear` | `int` | The start year to get the specific devaluation index. | > 1900.  |
| `endYear` | `int` | The end year to get the specific devaluation index. | > 1900.  |
| `location` | `string` | The location where the index is applied. | Required.  |

#### Returns

| Return Type | Description |
| - | - |
| `IEnumerable<T>` | Returns a list of indexes values for the reference year. |

### Get Earnings And Losses (Portugal)

This operation allows to get the value amount get with the asset sale.

```csharp
public async Task<ServiceOperationResult<double>> GetEarningsAndLossesAsync(int referenceYear, int year, string location, double aquisition, double depreciation, double valueAdjustment, double assetSaleValue, CancellationToken cancellationToken = default);

public ServiceOperationResult<double> GetEarningsAndLosses(int referenceYear, int year, string location, double aquisition, double depreciation, double valueAdjustment, double assetSaleValue);
```

#### Parameters

| Parameter | Type | Description | Rules |
| - | - | - | - |
| `referenceYear` | `int` | The reference year to get the specific devaluation index. | > 2000.  |
| `year` | `int` | The year to get the devaluation index. | > 1900.  |
| `location` | `string` | The location where the index is applied. | Required. RegEx(`(PT)|(AO)|(MZ)`).  |
| `aquisition` | `double` | The asset acquisition value. | > -1.  |
| `depreciation` | `double` | The asset depreciation value. | > -1.  |
| `valueAdjustment` | `double` | The asset value adjustment. | > -1.  |
| `assetSaleValue` | `double` | The amount get with the asset sale. | > -1.  |

#### Returns

| Return Type | Description |
| - | - | - | - |
| `double` | Returns a double value that represents the earnings or losses for the specified asset. |

## Using the service

```csharp
Uri address = new Uri("[service-address]");
string clientId = "[client-id]"
string clientSecret = "[client-secret]"
int referenceYear = 2099;
int year = 1903;
string region = "PT";

using LegalDevaluationIndexClient client = new LegalDevaluationIndexClient(
    new Uri(address),
    async (args) =>
    {
        string accessToken = await ClientCredentials
            .ForAllScopes(
                new Uri(args.Authority),
                clientId,
                clientSecret)
            .RetrieveAccessTokenAsync()
            .ConfigureAwait(false);

        return accessToken;
    });
    
     ServiceOperationResult<double> response = await client.GetDevaluationIndexAsync(
                     year, referenceYear, region).ConfigureAwait(false);
     
     double incomingTax = response.Body;               
     
    // (...)
}
catch (ServiceException ex)
{
    // (...)
}
```

## Back-office

This service provides a back-office that allows some users - known as managers - to manage the service.

The back-office is accessible via the /management route and allows:

- Uploading new indexes for specific year and location.
- Listing and filtering all available indexes.
- Deleting all the indexes for a specific year and location.

The file to upload must be a `.CSV` file and follow the structure described bellow.

> Each .CSV file should contain all the indexes for a specific year.

For example:

| Ref. Year | Year | Value | 
| - | - | - | 
| 2020 | 1903 | 4778,49 | 
| 2020 | 1904 | 4778,49 | 
| 2020 | 1905 | 4778,49 |
| 2020 | 1906 | 3795,73 | 

## Storage

Files are stored in Azure Table Storage. The following configuration options set the instances used by the micro service:

```json
{
    "AzureTableStorageOptions": {
        "ConnectionString": "(...)"
    }
}
```

## Distributed Cache

The service uses a distributed cache to reduce the dependency on the table storage. This cache is transparent for clients.
