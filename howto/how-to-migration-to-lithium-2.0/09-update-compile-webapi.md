# How to Upgrade a Microservice from Lithium v1.0 to Lithium v2.0

## 9 - Update and Compile WebApi

Compile the `WebApi.csproj` project individually.

Depending on the service design, one or more of the following fixes may be required in the custom code.

- [9.1 - Update Startup Custom Code](./09.1-update-webapi-startup.md)
- [9.2 - Update Controllers](./09.2-update-webapi-controllers.md)
- [9.3 - Update Web UI Custom Code](./09.3-update-webapi-webui.md)
- [9.4 - Update References to Table Storage](./09.4-update-webapi-table-storage.md)
- [9.5 - Update References to Blob Storage](./09.5-update-webapi-blob-storage.md)
- [9.6 - Update Background Services Custom Code](./09.6-update-webapi-background-services.md)
- [9.7 - Update Application Settings](./09.7-update-webapi-app-settings.md)

> Do not advance to the next step before this project is compiling without any error or warning.

## Next

[9.1 - Update Startup Custom Code](./09.1-update-webapi-startup.md)