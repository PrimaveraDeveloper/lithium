# Data Lookup Service (DLS)

The Data Lookup Service provides a centralized store for reference data, automatically updated (when applicable) from external sources (e.g. InformaDB). 

The current version supports the following reference data:

- VAT numbers, retrieved from InformaDB (for Portuguese VAT numbers) or VIES (EU VAT numbers).
- Exchange rates from EUR, retrieved from the BCE reference data.

The client library also includes validation algorithms for VAT numbers according to the rules applicable to Portugal, Spain and Cape Verde.

## Specification

<!-- markdown-link-check-disable -->
| Metadata | Value |
| - | - |
| Namespace | Primavera.Lithium.DataLookup |
| Version | 2.0 |
| API Versions | 2.0, 1.0 |
| Client Library | [Primavera.Lithium.DataLookup.dll](http://nuget.primaverabss.com:82/feeds/public-lithium-general/Primavera.Lithium.DataLookup) (single assembly) |
| Web UI | No |
| Depends on | IDS |
<!-- markdown-link-check-enable -->

## Scopes

| Scope | Description |
| - | - |
| `lithium-datalookup-vatnumber` | Access endpoints in the API for VAT numbers |
| `lithium-datalookup-exchrate` | Access endpoints in the API for exchange rates |

## Environments

| Environment | Available |
| - | - |
| Production | Yes |
| Preview (WE) | Yes |
| Preview (NE) | --- |
| Staging (WE) | Yes |
| Staging (NE) | --- |
| Development | --- |

## Reference Documentation

| Documentation | Link |
| - | - |
| Spec | [2.0](./specs/dls-spec-2.0.md) |
| Client Library | Not available yet |
| Web API | Not available yet |
