# How to Upgrade a Microservice from Lithium v1.0 to Lithium v2.0

## 1 - Configure NuGet in the Service Solution

### 1.1 - Open the folder that contains the microservice solution in Windows Explorer.

### 1.2 - Edit `nuget.config` and make sure it contains only the following package sources (apart from those that are specific to the service):

```xml
<packageSources>
    <add key="Lithium" value="http://nuget.primaverabss.com:82/nuget/public-lithium-general" />
    <add key="NuGet" value="https://api.nuget.org/v3/index.json" protocolVersion="3" />
</packageSources>
```

### 1.3 - Create a folder named `.nuget` (mind the dot) and create there a file named `nuget.config` with the following contents:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <solution>
        <add key="disableSourceControlIntegration" value="true" />
    </solution>
</configuration>
```

### 1.4 - Open the service solution in Visual Studio.

### 1.5 - Add a solution folder named `.nuget` (mind the dot).

### 1.6 - Add the `nuget.config` file created in 1.3 to this solution folder.

## Next

> [2 - Configure the Projects in the Service Solution](./02-configure-projects.md)