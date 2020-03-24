# ROSE People Service (RPS) Specification v1.0

The Rose People Service provides features to help Rose People product.

In the 1.0 version, service provides features to maintain the IRS tables for calculating incoming taxes based on a set of parameters.

## IRS Operations

IRS is the tax applyed to all employee earnings. It's calculated with a range of static tables and with the set of contract employee information.

Each country have your calculation logic and your IRS tables. RPS provides one operation for each location.

### Calculate Incoming Tax PT

This operations allows to calculate the incoming tax for Portugal with your calculation logic.

#### Parameters

- `Region`: the location of the employee contract. Required. `RegEx((PTC)|(PTA)|(PTM))`.
- `Table`: the identification of the IRS table that employee uses. Required. `> 0. < 10`.
- `Position`: the identification of the IRS table column that employee uses. Required. `> -1. < 6`.
- `Date`: the apply date for specific moment that will used to get the correct IRS table. Required.
- `Ammount`: the value used to obtain the correct row of IRS table. Required.

#### Returns

- Returns a `double` value that represents the calculated incoming tax.

#### Calculating IRS incoming tax

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

### All IRS Operations

The service provides follow operations for IRS.

- `GetIncomeTaxPT`: Retrive incoming tax for Portugal.
- `GetIncomeTaxPTAsync`: Retrive incoming tax for Portugal async.

## Back-office

The Rose People Service also provides a back-office that allows some users - know as managers - to manage the service.
Thes back-office is accessible via the /management route and allows:

- Upload new IRS tables for available locations.
- List and filter all available IRS tables.
- Delete all tables for a specific location and date.

The file to upload must be a `.CSV` file and follow the below structure.

> One .CSV file contains all tables for the specific location and apply date

> where `9999999` ammount indicates the last row of each table with the maximum taxes.

<!-- markdown-link-check-disable -->
| Table | Ammount | 0 dep. | 1 dep. | 2 dep. | 3 dep. | 4 dep. | 5 or more dep. |
| - | - | - | - | - | - | - | - |
| 1 | 659 | 0 | 0 | 0 | 0 | 0 | 0 |
| 2 | 659 | 0 | 0 | 0 | 0 | 0 | 0 |
| 3 | 12535 | 29,4 | 28,7 | 28,4 | 27,3 | 27 | 25,8 |
| 4 | 9999999 | 30,2 | 29,5 | 29,2 | 28,1 | 27,8 | 26,6 |
<!-- markdown-link-check-enable -->



