# How to Upgrade a Microservice from Lithium v1.0 to Lithium v2.0

## 2 - Configure the Projects in the Service Solution

### Target Framework

Make sure that the `TargetFramework` for each `.csproj` is according to this list:

- `Design`: `<TargetFramework>netstandard2.0</TargetFramework>`
- `Client.Console`: `<TargetFramework>netcoreapp3.1</TargetFramework>`
- `ClientLib`: `<TargetFramework>netstandard2.0</TargetFramework>`
- `Models`: `<TargetFramework>netstandard2.0</TargetFramework>`
- `WebApi`: `<TargetFramework>netcoreapp3.1</TargetFramework>`
- `ClientLib.Tests`: `<TargetFramework>netcoreapp3.1</TargetFramework>`
- `Models.Tests`: `<TargetFramework>netcoreapp3.1</TargetFramework>`
- `WebApi.Tests`: `<TargetFramework>netcoreapp3.1</TargetFramework>`

### IsPackable

Remove `<IsPackable>false</IsPackable>` from any `.csproj` files that includes it.

### Package References

Remove any package reference from `Design.csproj`.

The package references for `ClientConsole.csproj` should be:

```xml
<ItemGroup>
    <PackageReference Include="Primavera.Hydrogen.DesignTime.Configuration" Version="2.0.0.22">
        <PrivateAssets>all</PrivateAssets>
    </PackageReference>
    <PackageReference Include="Primavera.Hydrogen.Console" Version="2.0.1.26" />
</ItemGroup>
```

> Remove any other package reference that is not specific to the service.

The package references for `ClientLib.csproj` should be:

```xml
<ItemGroup>
    <PackageReference Include="Primavera.Hydrogen.DesignTime.Configuration" Version="2.0.0.22">
        <PrivateAssets>all</PrivateAssets>
    </PackageReference>
    <PackageReference Include="Primavera.Hydrogen.Rest.Client" version="2.0.1.26" />
</ItemGroup>
```

> Remove any other package reference that is not specific to the service.

The package references for `Models.csproj` should be:

```xml
<ItemGroup>
    <PackageReference Include="Primavera.Hydrogen.DesignTime.Configuration" Version="2.0.0.22">
        <PrivateAssets>all</PrivateAssets>
    </PackageReference>
    <PackageReference Include="Primavera.Hydrogen.Rest" version="2.0.1.26" />
</ItemGroup>
```

> Remove any other package reference that is not specific to the service.

The package references for `WebApi.csproj` should be:

```xml
<ItemGroup>
    <PackageReference Include="Primavera.Hydrogen.DesignTime.Configuration" Version="2.0.0.22">
        <PrivateAssets>all</PrivateAssets>
    </PackageReference>
    <PackageReference Include="Primavera.Hydrogen.AspNetCore.Azure" Version="2.0.1.26" />
</ItemGroup>
```

> Remove any other package reference that is not specific to the service.

The package references for `ClientLib.Tests.csproj`, `Models.Tests.csproj` and `WebApi.Tests.csproj` should be:

```xml
<ItemGroup>
    <PackageReference Include="Primavera.Hydrogen.DesignTime.Configuration" Version="2.0.0.22">
        <PrivateAssets>all</PrivateAssets>
    </PackageReference>
    <PackageReference Include="coverlet.msbuild" Version="2.8.0">
        <PrivateAssets>all</PrivateAssets>
    </PackageReference>
    <PackageReference Include="Microsoft.NET.Test.Sdk" Version="16.2.0">
        <PrivateAssets>all</PrivateAssets>
    </PackageReference>
    <PackageReference Include="xunit" Version="2.4.1">
        <PrivateAssets>all</PrivateAssets>
    </PackageReference>
    <PackageReference Include="xunit.runner.visualstudio" Version="2.4.1">
        <PrivateAssets>all</PrivateAssets>
    </PackageReference>
    <PackageReference Include="Primavera.Hydrogen.Core" version="2.0.1.26" />
    <PackageReference Include="Primavera.Hydrogen.DesignTime.UnitTesting" version="2.0.1.26" />
</ItemGroup>
```

> Remove any other package reference that is not specific to the service.

## Next

[3 - Update NuGet Package References in the Service Solution](./03-update-nuget-packages.md)