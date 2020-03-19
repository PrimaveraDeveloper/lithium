# Data Lookup Service (DLS) Specification v2.0

The Data Lookup Service provides a centralized store for reference data, automatically updated (when applicable) from external sources (e.g. InformaDB). 

- VAT numbers.
- Exchange rates from EUR.

## VAT Numbers

The service includes the following features on VAT numbers:

- Validation: based on the algorithms specific to PT, ES, and CV.
- Lookup: retrieval, when available, of information about the "owner" (for companies).
- Search: based on a given search term that may include the company name, trademarks, etc.

### Country Codes

VAT numbers are specific to each country, so they need to be associated with a country code. The country codes used by DLS should be specified using the [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) coding system.

### Validation

The validation of VAT numbers is performed according to the algorithms applied by the Tax Authorities of the following countries: Portugal (PT), Spain (ES) and Cape Verde.

It can be performed "in-process", by the service client library, but it is also available in Web API.

```csharp
using DataLookupClient client = new DataLookupClient(...);

try
{
    string countryCode = (...);
    string vatNumber = (...);
    ServiceOperationResult<VatNumberValidationResult> result = await client.VatNumber
        .ValidateVatNumberAsync(
            countryCode, 
            vatNumber)
        .ConfigureAwait(false);
    VatNumberValidationResult result = result.Body;
    if (result.Result == VatNumberValidation.Valid)
    {
        // (...)
    }
}
catch (ServiceException ex)
{
    // (...)
}
```

#### Caching

The client library performs caching of validation results for a given country code and VAT number. This mechanism is enabled by default and may be configured through the service client configuration options.

#### Client validation versus service validation

The validation is performed by default by the client library itself - thus in the client application process. The service client also includes a configuration option to force the validation to occur either in the client library or in the service.

#### Configuration

The service client includes a property named `Configuration` (of type `DataLookupClientConfiguration`) that allows configuring the service client behavior. The options available, concerning VAT number validation (property `VatNumberValidation`) are the following:

- `ClientValidationEnabled`: indicates whether the validation should be performed by the client library. The default value is true.
- `LocalMemoryCacheEnabled`: indicates whether the memory cache is enabled. The default value is true. It applies either when the validation is performed by the client library or the service.
- `LocalMemoryCacheAbsoluteExpiration`: sets the absolute expiration time (in seconds) of the memory cache. The default value is 30 minutes.

### Lookup

The service allows looking up a given VAT number to retrieve information about it's owner (only for companies).

```csharp
using DataLookupClient client = new DataLookupClient(...);

try
{
    string countryCode = (...);
    string vatNumber = (...);
    ServiceOperationResult<VatNumberInfo> result = await client.VatNumber
        .LookupVatNumberAsync(
            countryCode, 
            vatNumber)
        .ConfigureAwait(false);
    VatNumberInfo result = result.Body;
    // (...)
}
catch (ServiceException ex)
{
    // (...)
}
```

#### Result

The result of the lookup is set in the `State` property, which can have the following values:

- `Valid`: the VAT number is valid and the associated data could be retrieved.
- `Invalid`: the VAT number is invalid.
- `Inconclusive`: the VAT number may or not be valid and no data could be retrieved.

> The `Inconclusive` result may occur when the VAT number is for a country that is not fully supported by the VIES service.

#### Sources

The data source used for lookup depends on the country code:

- Portugal ("PT"): the InformaDB database (updated daily).
- EU countries: the [VIES Web Service](http://ec.europa.eu/taxation_customs/vies/).

> The service does support any other country code.

### Search

The service also allows to search for companies based on the information associated with their VAT numbers.

> This operation is only available for Portugal, since it uses the InformaDB database as data source.

```csharp
using DataLookupClient client = new DataLookupClient(...);

try
{
    string countryCode = (...);
    string searchTerm = (...);
    ServiceOperationResult<VatNumberSearchResults> result = await client.VatNumber
        .SearchVatNumberAsync(
            countryCode, 
            searchTerm)
        .ConfigureAwait(false);
    VatNumberSearchResults result = result.Body;
    foreach (VatNumberSearchResult item in result.Results)
    {
        // (...)
    }
}
catch (ServiceException ex)
{
    // (...)
}
```

> Each result has a score associated that hints the importance of each match.

#### Search Term

The search term allows searching the VAT numbers records using not only a VAT number, but also other data about the company (like its name, address, trademarks, etc.).

## Exchange Rates

The service includes the following features on exchange rates (to and from Euro):

- Lookup: retrieval, when available, of the exchange rate between two currencies.

### Lookup

The service allows looking up the exchange rate between any two given currencies:

```csharp
using DataLookupClient client = new DataLookupClient(...);

try
{
    string sourceCurrency = (...);
    string targetCurrency = (...);
    DateTime referenceData = DateTime.Now;
    ServiceOperationResult<ExchangeRateInfo> result = await client.ExchRate
        .LookupExchangeRateAsync(
            sourceCurrency, 
            targetCurrency,
            referenceDate)
        .ConfigureAwait(false);
    ExchangeRateInfo result = result.Body;
    double rate = result.ExchangeRate;
    // (...)
}
catch (ServiceException ex)
{
    // (...)
}
```

> Although the client library accepts a DateTime as the reference date, the corresponding action in the Web API requires the reference date to be specified as an [Unix timestamp](https://www.unixtimestamp.com/).

#### Result

The result of the lookup is set in the `State` property, which can have the following values:

- `Available`: the exchange rate could be determined.
- `Unavailable`: the exchange rate could not be determined.

#### Source

The data source used for lookup is the [BCE Euro Exchange Reference Rates](https://www.ecb.europa.eu/stats/policy_and_exchange_rates/euro_reference_exchange_rates/) (updated daily).

> These rates are always in reference to the Euro (EUR). This means that looking up the exchange rate between two currencies not EUR (e.g. currencies A and B) returns a result that is the product of two exchange rates (A to EUR and EUR to B).

