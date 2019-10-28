# How to Upgrade a Microservice to Lithium v2.0

This guide describes the steps required to convert a microservice solution built with Lithium 1.0 to Lithium 2.0 and .NET Core 3.0. 

> NOTE: This guide may not include the instructions required to upgrade some edge scenarios. If you find any of these scenarios in the service you are trying to upgrade, please contact the Lithium development team.

| Step | Obs. |
| - | - |
| [1 - Configure NuGet in the Service Solution](./01-configure-nuget.md) | |
| [2 - Configure the Projects in the Service Solution](./02-configure-projects.md) | |
| [3 - Update NuGet Package References in the Service Solution](./03-update-nuget-packages.md) | |
| [4 - Update the Service Model](./04-update-service-model.md) | |
| [5 - Replace Deprecated Namespaces](./05-replace-deprecated-namespaces.md) | |
| [6 - Replace Deprecated Types](./06-replace-deprecated-types.md) | |
| [7 - Replace Deprecated Members](./07-replace-deprecated-members.md) | |
| [8 - Update and Compile Models](./08-update-compile-models.md) | |
| [9 - Update and Compile WebApi](./09-update-compile-webapi.md) | |
| ... [9a - Update Startup Custom Code](./09a-update-webapi-startup.md) | |
| ... [9b - Update Controllers](./09b-update-webapi-controllers.md) | |
| ... [9c - Update Web UI Custom Code](./09c-update-webapi-webui.md) | Required if the service includes custom ASP.NET Web UI with OIDC |
| ... [9d - Update References to Table Storage](./09d-update-webapi-table-storage.md) | |
| ... [9e - Update References to Blob Storage](./09e-update-webapi-blob-storage.md) | |
| ... [9f - Update Background Services Custom Code](./09f-update-webapi-background-services.md) | Required if the service includes background services |
| ... [9g - Update Application Settings](./09g-update-webapi-app-settings.md) | |
| [10 - Update and Compile ClientLib](./10-update-compile-clientlib.md) | |
| [11. Update and Compile Client.Console](./10-update-compile-clientconsole.md) | |
| [12. Update and Compile Tests Projects](./12-update-compile-testprojects.md) | |
| [13. Verify and Test the Service](./13-verify-test.md) | |

## Next

> [1 - Configure NuGet in the Service Solution](./01-configure-nuget.md)