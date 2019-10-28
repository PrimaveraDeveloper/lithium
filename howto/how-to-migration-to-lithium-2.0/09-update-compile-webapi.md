# How to Upgrade a Microservice from Lithium v1.0 to Lithium v2.0

## 9 - Update and Compile WebApi

### 9.1 - Compile the `WebApi.csproj` project individually.**

Depending on the service design, one or more of the following fixes may be required in the custom code.

| Step | Obs |
| - | - |
| [9a - Update Startup Custom Code](./09a-update-webapi-startup.md) | |
| [9b - Update Controllers](./09b-update-webapi-controllers.md) | |
| [9c - Update Web UI Custom Code](./09c-update-webapi-webui.md) | Required if the service includes custom ASP.NET Web UI with OIDC |
| [9d - Update References to Table Storage](./09d-update-webapi-table-storage.md) | |
| [9e - Update References to Blob Storage](./09e-update-webapi-blob-storage.md) | |
| [9f - Update Background Services Custom Code](./09f-update-webapi-background-services.md) | |
| [9g - Update Application Settings](./09g-update-webapi-app-settings.md) | |

> NOTE: Do not advance to the next step before this project is compiling without any error or warning.

## Next

> [9a. Update Startup Custom Code](./09a-update-webapi-startup.md)