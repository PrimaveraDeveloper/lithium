# How to Upgrade a Microservice to Lithium v2.0

This guide describes the steps required to convert a microservice solution built with Lithium 1.0 to Lithium 2.0 and .NET Core 3.0. 

- [1 - Configure NuGet in the Service Solution](./01-configure-nuget.md)
- [2 - Configure the Projects in the Service Solution](./02-configure-projects.md)
- [3 - Update NuGet Package References in the Service Solution](./03-update-nuget-packages.md)
- [4 - Update the Service Model](./04-update-service-model.md)
    - [4.1 - Update Versioning](./04.1-update-versioning.md)
    - [4.2 - Update Background Services](./04.2-update-service-model-background-services.md)
    - [4.3 - Validate and Transform Text Templates](./04.3-validate-transform   .md)
- [5 - Replace Deprecated Namespaces](./05-replace-deprecated-namespaces.md)
- [6 - Replace Deprecated Types](./06-replace-deprecated-types.md)
- [7 - Replace Deprecated Members](./07-replace-deprecated-members.md)
- [8 - Update and Compile Models](./08-update-compile-models.md)
- [9 - Update and Compile WebApi](./09-update-compile-webapi.md)
    - [9.1 - Update Startup Custom Code](./09.1-update-webapi-startup.md)
    - [9.2 - Update Controllers](./09.2-update-webapi-controllers.md)
    - [9.3 - Update Web UI Custom Code](./09.3-update-webapi-webui.md)
    - [9.4 - Update References to Table Storage](./09.4-update-webapi-table-storage.md)
    - [9.5 - Update References to Blob Storage](./09.5-update-webapi-blob-storage.md)
    - [9.6 - Update Background Services Custom Code](./09.6-update-webapi-background-services.md)
    - [9.7 - Update Application Settings](./09.7-update-webapi-app-settings.md)
- [10 - Update and Compile ClientLib](./10-update-compile-clientlib.md)
- [11 Update and Compile Client.Console](./11-update-compile-clientconsole.md)
- [12 Update and Compile Tests Projects](./12-update-compile-testprojects.md)
- [13 Verify and Test the Service](./13-verify-test.md)

> The instructions required to upgrade some edge scenarios may not be included. If you find any of these scenarios in the service you are trying to upgrade, please contact the Lithium development team.

## Next

[1 - Configure NuGet in the Service Solution](./01-configure-nuget.md)