# How to Upgrade a Microservice from Lithium v1.0 to Lithium v2.0

## 3 - Update NuGet Package References in the Service Solution

Select "Manage NuGet Packages for Solution..." from the Visual Studio context menu.

Update all the previous configured NuGet packages to their latest versions.

> Do NOT update any package that is added to the solution as a dependency of Hydrogen (e.g. `Microsoft.Extensions.Logging`). These will be updated correctly when new versions of Hydrogen become available.

## Next

[4 - Update the Service Model](./04-update-service-model.md)