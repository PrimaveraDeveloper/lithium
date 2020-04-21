# ROSE People Service (RPS) Specification v1.0

The Rose People Service provides features for ROSE People.

The current version of the service allows storing IRS tables for calculating income taxes.

## IRS Operations

IRS is the tax applied to all employee earnings. It is calculated with a range of static tables and the employee's contract information.

Each country has its own calculation logic and its own income tax tables. RPS provides one operation for each location.

### Calculate Income Tax (method 1) (Portugal)

This operation allows to calculate the incoming tax for Portugal.
To using this operation it's needed know the table number and position to calculate income tax.

```csharp
public async Task<ServiceOperationResult<double>> GetIncomeTaxPTAsync(string region, int table, int position, DateTime date, double amount, CancellationToken cancellationToken = default);

public ServiceOperationResult<double> GetIncomeTaxPT(string region, int table, int position, DateTime date, double amount);
 ```

#### Parameters

| Parameter | Type | Description | Rules |
| - | - | - | - |
| `region` | `string` | The location of the contract. | Required. RegEx(`(PTC)|(PTA)|(PTM)`).  |
| `table` | `int` | The identification of the IRS table applicable to the employee. | > 0. < 10.  |
| `position` | `int` | The identification of the IRS table column. | > -1. < 6.  |
| `date` | `DateTime` | The specific moment that will used to get the correct IRS table. |  |
| `amount` | `double` | The value used to obtain the income tax. |  |


#### Returns

| Return Type | Description |
| - | - | - | - |
| `double` | Returns a double value that represents the calculated income tax. |



### Calculate Income Tax (method 2) (Portugal)

This operation allows to calculate the incoming tax for Portugal.
Uses all mandatory parameters needed to return income tax.

```csharp
public async Task<ServiceOperationResult<IncomeTaxPT>> IncomeTaxPTAsync(string region, int earningType, int fiscalState, bool disabilityDegreeHolder, int earningHolders, bool secondHolderMajorEarnig, bool secondHolderDisabilityDegree, int dependants, int disabilityDegreeDependents, DateTime date, double amount, double fixedTax, CancellationToken cancellationToken = default);

public ServiceOperationResult<IncomeTaxPT> IncomeTaxPT(string region, int earningType, int fiscalState, bool disabilityDegreeHolder, int earningHolders, bool secondHolderMajorEarnig, bool secondHolderDisabilityDegree, int dependants, int disabilityDegreeDependents, DateTime date, double amount, double fixedTax);
 ```

#### Parameters

| Parameter | Type | Description | Rules |
| - | - | - | - |
| `region` | `string` | The location of the contract. | Required. RegEx(`(PTC)|(PTA)|(PTM)`).  |
| `earningType` | `int` | The earning type identifier. | > 0. < 3.  |
| `fiscalState` | `int` | The fiscal state identifier. | > 0. < 3.  |
| `disabilityDegreeHolder` | `bool` | Indicates if the holder has a disability degree => 60%. |  |
| `earningHolders` | `int` | The number of earning holders. | > -1.  |
| `secondHolderMajorEarnig` | `bool` | Indicates if the second holder have global earning => 95%. |  |
| `secondHolderDisabilityDegree` | `bool` | Indicates if the second holder has a disability degree => 60%. |  |
| `dependants` | `int` | The number of dependents. | > -1.  |
| `disabilityDegreeDependents` | `int` | The number of dependents with disability degree => 60%. | > -1.  |
| `date` | `DateTime` | The specific moment that will used to get the correct IRS table. |  |
| `amount` | `double` | The value used to obtain the income tax. |  |
| `fixedTax` | `double` | The value to return when fixed tax > calculated tax. | > -1.  |


#### Returns
| Return Type | Description |
| - | - |
| `Primavera.Lithium.RosePeople.Models.IncomeTaxPT` | Returns a model with the calculated income tax, table number and column position. |

### Models Classes

#### <a name="IncomeTaxPT"></a>`IncomeTaxPT`

Model that contains the properties with the calculated income tax.

- Namespace: `Primavera.Lithium.RosePeople.Models`
- Inheritance: `IncomeTaxPTBase`

##### Properties

| Property | Type | Description | Rules |
| - | - | - | - |
| `Table` | `int` | The indentification of the table. |  |
| `Position` | `int` | The identification of the column position. |  |
| `IncomeTax` | `double` | The calculated income tax. |  |

### Enumerations Classes

#### <a name="EarningType"></a>`EarningType`

Defines the type of earning.

- Namespace: `Primavera.Lithium.RosePeople.Models`

##### Members

| Member | Value | Description |
| - | - | - |
| `PaidEmployement` | 1 | The paid employment member. | 
| `Pensioner` | 2 | The pensioner member. | 

[^ Back to top](#root)

#### <a name="FiscalState"></a>`FiscalState`

Defines the fiscal state.

- Namespace: `Primavera.Lithium.RosePeople.Models`

##### Members

| Member | Value | Description |
| - | - | - |
| `MarriedOrLifePartners` | 1 | The married or life partners member. | 
| `Others` | 2 | The other fiscal states. | 


#### <a name="Region"></a>`Region`

Defines the region for IRS tables.

- Namespace: `Primavera.Lithium.RosePeople.Models`

##### Members

| Member | Value | Description |
| - | - | - |
| `PTM` | 1 | PT - Madeira item. | 
| `PTC` | 2 | PT - Continental item. | 
| `PTA` | 3 | PT - Azores item. | 

### Implementing IRS income tax

```csharp
Uri address = new Uri("[your-service-endpoint]");
string clientId = "[your-client-id]"
string clientSecret = "[your-client-secret]"
string region = "PTC";
int table = 1;
int position = 0;
DateTime date = DateTime.Now;
double ammount = 2500;

try
{
    using RosePeopleClient client = new RosePeopleClient(
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
    
     ServiceOperationResult<double> response = await client.GetIncomeTaxPTasync(
                    region, table, position, date, ammount).ConfigureAwait(false);
     
     double incomingTax = response.Body;               
     
    // (...)
}
catch (ServiceException ex)
{
    // (...)
}
```

## Back-office

The Rose People Service also provides a back-office that allows some users - known as managers - to manage the service.

The back-office is accessible via the /management route and allows:

- Uploading new IRS tables for specific locations.
- Listing and filtering all available IRS tables.
- Deleting all the tables for a specific location and date.

The file to upload must be a `.CSV` file and follow the structure described bellow.

> Each .CSV file should contain all the tables for a specific location and apply date

> The `9999999` amount indicates the last row of each table with the maximum taxes.

For example:

| Table | Amount | 0 dep. | 1 dep. | 2 dep. | 3 dep. | 4 dep. | 5 or more dep. |
| - | - | - | - | - | - | - | - |
| 1 | 659 | 0 | 0 | 0 | 0 | 0 | 0 |
| 2 | 659 | 0 | 0 | 0 | 0 | 0 | 0 |
| 3 | 12535 | 29,4 | 28,7 | 28,4 | 27,3 | 27 | 25,8 |
| 4 | 9999999 | 30,2 | 29,5 | 29,2 | 28,1 | 27,8 | 26,6 |

## Storage

Files are stored in Azure Table Storage. The data about these income tax tables is stored in table storage.

The following configuration options set the instances used by the micro service:

```json
{
    "AzureTableStorageOptions": {
        "ConnectionString": "(...)"
    }
}
```

## Distributed Cache

The service uses a distributed cache to reduce the dependency on the table storage. This cache is transparent for clients.
