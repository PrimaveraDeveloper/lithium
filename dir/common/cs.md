# Certificates Service (CS)

The Certificates Service provides a centralized store for certificates used by applications.

This cloud store allows managing the certificates lifetimes and pushing updates automatically to the applications that use them without the need to update those applications.

Both the microservice and the respective client library implement multiple cache levels to improve performance and reduce communication between the client and the service.

## Specification

| Metadata | Value |
| - | - |
| Namespace | Primavera.Lithium.Certificates |
| Version | 2.0 |
| API Versions | 2.0, 1.0 |
| Client Library | Primavera.Lithium.Certificates.dll (single assembly) |
| Web UI | No |
| Depends on | IDS |

## Scopes

| Scope | Description |
| - | - |
| `lithium-certificates` | Access all the endpoints in the API |

## Environments

| Environment | Available |
| - | - |
| Production | Yes |
| Preview (WE) | Yes |
| Preview (NE) | Yes |
| Staging (WE) | Yes |
| Staging (NE) | --- |
| Development | --- |

## Reference Documentation

| Documentation | Link |
| - | - |
| Spec | [2.0](./specs/cs-spec-2.0.md) |
| Client Library | Not available yet |
| Web API | Not available yet |
