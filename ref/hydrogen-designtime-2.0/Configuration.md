# Primavera.Hydrogen.DesignTime.Configuration

**Class library used to generate the NuGet package that set default design-time configurations that should be used on .NET Core and .NET Standard projects.**

## Build Configuration

The `Primavera.Hydrogen.DesignTime.Configuration` NuGet package should be added to all Lithium Visual Studio projects and configures these projects with the following settings:

- Sets the assembly signature key
- Disables AssemblyInfo automatic generation
- Provides a default .editorconfig file
- Provides a default .tfignore file
- Adds the default Code Analyzers
- Sets the default rules for the code analyzers

## Code Analysis

Lithium leverages the following Roslyn analyzers to enforce coding standards and code quality at design-time (when the developer is coding and when the projects are built):

- `Microsoft.CodeAnalysis.FxCopAnalyzers`
- `StyleCop.Analyzers`

Code analysis adheres to a rule set specified by the framework and distributed by the `Primavera.Hydrogen.DesignTime.Configuration` NuGet package.