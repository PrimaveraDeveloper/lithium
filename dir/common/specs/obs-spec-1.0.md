# Open Banking Service (OBS) Specification v1.0

The Open Banking Service allows integration with third-party open banking services and other operations related with banking.

The current version of the service provides only a client library and the following validation algorithms:

- IBAN
- BIC
- NIB (Portugal only)

## Validation (and Lookup) Algorithms

The OBS service provides implements - through the client library only - a set of algorithms that allow validating banking numbers and codes. These algorithms also allow extracting useful information from those numbers/codes - like, for example, the account number from an IBAN.

The algorithms available are:

- International Bank Account Number ([IBAN](https://en.wikipedia.org/wiki/International_Bank_Account_Number)), for multiple countries, including PT, ES, AO, MZ, and CV.
- Bank International Code ([BIC](https://en.wikipedia.org/wiki/ISO_9362)), also for multiple countries.
- Número de Identificação Bancária ([NIB](https://pt.wikipedia.org/wiki/N%C3%BAmero_de_Identifica%C3%A7%C3%A3o_Banc%C3%A1ria)), for PT only.

### IBAN

### Validation

To validate an IBAN number:

```csharp
using (OpenBankingClient serviceClient = new OpenBankingClient(
    new Uri("http://localhost"),
    new NoCredentials()))
{
    ServiceOperationResult<IBANValidationResult> response = serviceClient.IBAN.ValidateIBAN("SE35 5000 0000 0549 1000 0003");

    IBANValidationResult result = response.Body;

    if (result.Result == IBANValidationState.Valid)
    {
        (...)
    }
}
```

`IBANValidationState` indicates the result of the validation. It has 3 possible values:

- `Valid`: the IBAN is valid.
- `Invalid`: the IBAN is valid.
- `NotValidated`: if, for any reason, the service is not capable of validating the number.

> Since the validation algorithm is implemented in the client library - there is no communication whatsoever with the cloud service - the service client can be created like in the previous example (the service URI is not used and there is no need to pass in the service credentials).

#### Lookup

To lookup an IBAN number:

Lookup works the same, only the result is different, including additional information about the IBAN number:

```csharp
using (OpenBankingClient serviceClient = new OpenBankingClient(
    new Uri("http://localhost"),
    new NoCredentials()))
{
    ServiceOperationResult<IBANLookupResult> response = serviceClient.IBAN.LookupIBAN("SE3550000000054910000003");

    IBANLookupResult result = response.Body;

    if (result.Result == IBANValidationState.Valid)
    {
        (...)
    }
}
```

The `IBAN` property in `IBANLookupResult` includes the additional information (only when the IBAN is valid):

- `IBAN` - the IBAN as validated by the service (after normalization).
- `CountryCode` - the country code (including if the country is a SEPA member and uses Euro).
- `CheckDigit` - the check digit.
- `BBAN` - the basic bank account number.
- `BankCode` - the bank code.
- `BranchCode` - the branch code.
- `AccountNumber` - the account number.
- `NationalCheckDigit` - the national check digit.

### BIC

### Validation

To validate a BIC code:

```csharp
using (OpenBankingClient serviceClient = new OpenBankingClient(
    new Uri("http://localhost"),
    new NoCredentials()))
{
    ServiceOperationResult<BICValidationResult> response = serviceClient.BIC.ValidateBIX("MPIOPTPL");

    BICValidationResult result = response.Body;

    if (result.Result == BICValidationState.Valid)
    {
        (...)
    }
}
```

`BICValidationState` indicates the result of the validation. It has 3 possible values:

- `Valid`: the BIC is valid.
- `Invalid`: the BIC is valid.
- `NotValidated`: if, for any reason, the service is not capable of validating the code.

#### Lookup

To lookup a BIC code:

```csharp
using (OpenBankingClient serviceClient = new OpenBankingClient(
    new Uri("http://localhost"),
    new NoCredentials()))
{
    ServiceOperationResult<BICLookupResult> response = serviceClient.BIC.LookupBIC("MPIOPTPL");

    BICLookupResult result = response.Body;

    if (result.Result == BICValidationState.Valid)
    {
        (...)
    }
}
```

The `BIC` property in `BICLookupResult` includes the additional information (only when the BIC is valid):

- `BIC` - the BIC as validated by the service (after normalization).
- `CountryCode` - the country code (including if the country is a SEPA member and uses Euro).
- `LocationCode` - the location code.
- `BankCode` - the bank code.
- `BranchCode` - the branch code.

### NIB

Validation and lookup of NIB numbers works basically the same as IBAN numbers, except that only Portuguese numbers will be validated (the country code and check digits are optional since they are not part of the NIB specification).

## Open Banking Services

The integration of OBS with third-party Open Banking services to support operations like payments initiation and accounts aggregation is currently under development and will be published as soon as possible.