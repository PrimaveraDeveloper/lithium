# Licensing Service (LIC)

The Licensing Service provides the operations required to support the ERP licensing (V10+).

## Specification

| Metadata | Value |
| - | - |
| Namespace | Primavera.Lithium.Licensing |
| Version | 1.0 |
| API Version | 1.0 |
| Client Library | [Primavera.Lithium.Licensing.dll](http://nuget.primaverabss.com:82/feeds/public-lithium-general/Primavera.Lithium.Licensing/) |
| Web UI | Yes |
| Depends on | IDS

## Authentication

### API Resources

| Name | Description |  Scopes | Claims
| - | - | - | - |
| lithium-licensing | PRIMAVERA Licensing Service | lithium-licensing

### Scopes

| Scope | Description |
| - | - |
| lithium-licensing | Access all the endpoints in the API |

### Flows

| oAuthFlow | Description | Scopes |
| - | - | - |
| Client Credentials | Used to access the licensing api endpoints  | lithium-licensing
| Hybrid | Used to access to licensing web UI. | email openid profile lithium-licensing

## Environments

| Environment | Address |
| - | - |
| Production | <https://lithium-licensing.primaverabss.com/> |
| Preview (WE) | <https://lithiumlicensingpdwe-lithiumlicensingpdwe-preview.azurewebsites.net/> |
| Preview (NE) | <https://lithiumlicensingpdne-lithiumlicensingpdne-preview.azurewebsites.net/> |
| Staging (WE) | <https://stg-licensing.azurewebsites.net/> |
| Staging (NE) | --- |
| Development | --- |

## Reference Documentation

| Documentation | Link |
| - | - |
| Client Library | [TFS](https://tfs.primaverabss.com/tfs/P.TEC.Elevation/Lithium/_versionControl?path=%24%2FLithium%2FMicroservices%2FERP%2FLIC%2FMainline%2FReadme.md&version=T&_a=preview) |
| Web API | [TFS](https://tfs.primaverabss.com/tfs/P.TEC.Elevation/Lithium/_versionControl?path=%24%2FLithium%2FMicroservices%2FERP%2FLIC%2FMainline%2FReadme.md&version=T&_a=preview) |
