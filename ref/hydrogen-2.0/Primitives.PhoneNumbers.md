# Primavera.Hydrogen.Primitives.PhoneNumbers

**Class library that contains types that allow parsing, formatting, and validating international phone numbers.**

## PhoneNumber

The `PhoneNumber` describes an international phone number and includes functionality to parse, format, and validate
phone numbers using international formats like E.164.

To parse a string as a phone number:

```csharp
PhoneNumber number = PhoneNumber.Parse("+351961717329");

bool result = PhoneNumber.TryParse("961717319", "PT", out PhoneNumber number);
```

To validate a phone number:

```csharp
PhoneNumber number = PhoneNumber.Parse("+351961717329");
bool result = number.IsValid();

bool result = PhoneNumber.Validate("961717319", "PT");

bool result = PhoneNumber.TryValidate("+351961717319", "PT");
```

To format a phone number:

```csharp
string result = PhoneNumber.Format("961717319", "PT", PhoneNumberFormat.E164);

PhoneNumber number = PhoneNumber.Parse("+351961717329");
string result1 = number.ToString();
string result2 = number.ToString(PhoneNumberFormat.International);
```

## Regions

This static class provides methods to retrieve regions and country codes used for phone numbers. It includes
the following methods:

- `GetRegionsSupported()`
- `GetRegionsAndCountryCodesSupported()`
- `GetCountryCodeForRegion()`
- `GetRegionForCountryCode()`

## Validation Attributes

### InternationalPhoneNumberAttribute

This attribute allows validating international phone numbers using the `PhoneNumber` functionality.

## Validation Methods

This library also provides an extension for SmartGuard that allows validating international
phone numbers on method parameters:

```csharp
SmartGuard.Extensions.IsInternationalPhoneNumber(() => param, param);
```
