# Certificates Service (CS)

The Certificates Service provides a centralized store for certificates used by applications. This cloud store allows managing the certificates lifetimes and pushing updates automatically to the applications that use them without requiring to update those applications.

Both the microservice and the respective client library implement multiple cache levels to improve performance and reduce communication between the client and the service.

## Specification

| Metadata | Value |
| - | - |
| Namespace | Primavera.Lithium.Certificates |
| Version | 1.0 |
| API Version | 1.0 |
| Client Library | [Primavera.Lithium.Certificates.dll](http://nuget.primaverabss.com:82/feeds/public-lithium-general/Primavera.Lithium.Certificates) |
| Web UI | No |
| Depends on | IDS

## Scopes

| Scope | Description |
| - | - |
| lithium-certificates | Access all the endpoints in the API |

## Environments

| Environment | Address |
| - | - |
| Production | <https://lithium-certificates.primaverabss.com/> |
| Preview (WE) | <https://lithiumcertificatespdwe-lithiumcertificatespdwe-preview.azurewebsites.net/> |
| Preview (NE) | <https://lithiumcertificatespdne-lithiumcertificatespdne-preview.azurewebsites.net/> |
| Staging (WE) | <https://stg-certificates.azurewebsites.net/> |
| Staging (NE) | --- |
| Development | --- |

## Reference Documentation

| Documentation | Link |
| - | - |
| Client Library | Not available yet |
| Web API | Not available yet |
