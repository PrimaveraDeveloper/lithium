# How to Upgrade a Microservice from Lithium v1.0 to Lithium v2.0

## 2 - Configure the Projects in the Service Solution

### 2.1 - Make sure that `TargetFramework` for each `.csproj` is according to this list:

- Design: `<TargetFramework>netstandard2.0</TargetFramework>`
- Client.Console: `<TargetFramework>netcoreapp3.0</TargetFramework>`
- ClientLib: `<TargetFramework>netstandard2.0</TargetFramework>`
- Models: `<TargetFramework>netstandard2.0</TargetFramework>`
- WebApi: `<TargetFramework>netcoreapp3.0</TargetFramework>`
- ClientLib.Tests: `<TargetFramework>netcoreapp3.0</TargetFramework>`
- Models.Tests: `<TargetFramework>netcoreapp3.0</TargetFramework>`
- WebApi.Tests: `<TargetFramework>netcoreapp3.0</TargetFramework>`

### 2.2 - Remove `<IsPackable>false</IsPackable>` from any `.csproj` files that includes it.

### 2.3 - Remove any package reference from `Design.csproj`.

### 2.4 - The package references for `ClientConsole.csproj` should be:

```xml
<ItemGroup>
    <PackageReference Include="Primavera.Hydrogen.DesignTime.Configuration" Version="2.0.0.17">
        <PrivateAssets>all</PrivateAssets>
    </PackageReference>
    <PackageReference Include="Primavera.Hydrogen.Console" Version="2.0.1.5" />
</ItemGroup>
```

> NOTE: Remove any other package reference that is not specific to the service.

### 2.5 - The package references for `ClientLib.csproj` should be:

```xml
<ItemGroup>
    <PackageReference Include="Primavera.Hydrogen.DesignTime.Configuration" Version="2.0.0.17">
        <PrivateAssets>all</PrivateAssets>
    </PackageReference>
    <PackageReference Include="Primavera.Hydrogen.Rest.Client" version="2.0.1.5" />
</ItemGroup>
```

> NOTE: Remove any other package reference that is not specific to the service.

### 2.6 - The package references for `Models.csproj` should be:

```xml
<ItemGroup>
    <PackageReference Include="Primavera.Hydrogen.DesignTime.Configuration" Version="2.0.0.17">
        <PrivateAssets>all</PrivateAssets>
    </PackageReference>
    <PackageReference Include="Primavera.Hydrogen.Rest" version="2.0.1.5" />
</ItemGroup>
```

> NOTE: Remove any other package reference that is not specific to the service.

### 2.7 - The package references for `WebApi.csproj` should be:

```xml
<ItemGroup>
    <PackageReference Include="Primavera.Hydrogen.DesignTime.Configuration" Version="2.0.0.17">
        <PrivateAssets>all</PrivateAssets>
    </PackageReference>
    <PackageReference Include="Primavera.Hydrogen.AspNetCore" version="2.0.1.5" />
    <PackageReference Include="Primavera.Hydrogen.AspNetCore.Authentication" Version="2.0.1.5" />
    <PackageReference Include="Primavera.Hydrogen.AspNetCore.Azure" Version="2.0.1.5" />
    <PackageReference Include="Primavera.Hydrogen.Pipelines" version="2.0.1.5" />
    <PackageReference Include="Primavera.Hydrogen.Security.Azure" version="2.0.1.5" />
    <PackageReference Include="Primavera.Hydrogen.Storage.Azure" version="2.0.1.5" />
    <PackageReference Include="Primavera.Hydrogen.Telemetry.Azure" version="2.0.1.5" />
</ItemGroup>
```

> NOTE: Remove any other package reference that is not specific to the service.

### 2.8 - The package references for `ClientLib.Tests.csproj`, `Models.Tests.csproj` and `WebApi.Tests.csproj` should be:

```xml
<ItemGroup>
    <PackageReference Include="coverlet.collector" Version="1.1.0">
        <PrivateAssets>all</PrivateAssets>
    </PackageReference>
    <PackageReference Include="Microsoft.NET.Test.Sdk" Version="16.3.0">
        <PrivateAssets>all</PrivateAssets>
    </PackageReference>
    <PackageReference Include="xunit" Version="2.4.1">
        <PrivateAssets>all</PrivateAssets>
    </PackageReference>
    <PackageReference Include="xunit.runner.visualstudio" Version="2.4.1">
        <PrivateAssets>all</PrivateAssets>
    </PackageReference>
    <PackageReference Include="Primavera.Hydrogen.DesignTime.Configuration" Version="2.0.0.17">
        <PrivateAssets>all</PrivateAssets>
    </PackageReference>
    <PackageReference Include="Primavera.Hydrogen.Core" version="2.0.1.5" />
    <PackageReference Include="Primavera.Hydrogen.DesignTime.UnitTesting" version="2.0.1.5" />
</ItemGroup>
```

> NOTE: Remove any other package reference that is not specific to the service.

## Next

> [3 - Update NuGet Package References in the Service Solution](./03-update-nuget-packages.md)