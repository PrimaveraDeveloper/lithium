# Lithium NuGet Packages Sources

When the Lithium SDK is installed on a machine a file named `Lithium.NuGet.Config` will be installed on the machine-wide NuGet configuration directory (`%ProgramFiles(x86)%\Config`).

This file contains references to the default packages sources used by the Lithium Framework and Lithium microservices.

> This allows avoing solution-scoped `NuGet.config` files and packages caches.

These packages sources are:

- Lithium: http://nuget.primaverabss.com:82/nuget/public-lithium-general
- Telerik: http://nuget.primaverabss.com:82/nuget/_external_public-telerik/