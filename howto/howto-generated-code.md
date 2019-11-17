# How is code generated for a microservice

This guide describes how the generated code is created and organized in a microservice.

## Why Generate Code?

Code generation is a fundamental aspect of the Lithium Framework because it allows:

- Accelerating the development of the microservices because developers don't need to implement cross-cutting features each they develop a new microservice.
- Standardization of the features, implementations, and code across multiple services.
- A simpler path to upgrade technology when necessary because the most important technology-dependent features are implemented and/or enforced by the generated code shape.
- Guiding good design approaches and code quality by providing examples to implement custom code.

## `GeneratedCode` Folder

All projects in a microservice solution include a folder named `GeneratedCode`. This is where all the generated files will be produced every time the text templates are transformed (code is generated). Organizing all generated code in a single folder has benefits:

- Generated code is clearly separated from custom code (the code that the developer produces to implement the specific logic of the service)-
- This folder can easily be deleted and recreated, allowing for a simpler evolution of the code generators themselves.

## Code Generator

The Lithium Framework includes a single code generator - named `ServiceDesignerCodeGenerator` - that is part of the Lithium SDK and the Service Designer Visual Studio extensions. This code generator acts on each of the text templates in the solution and is capable of producing multiple files - with different purposes, different languages, etc. - from each of these text templates.

## Text Templates

A text template in the [T4 technology](https://docs.microsoft.com/en-us/visualstudio/modeling/code-generation-and-t4-text-templates) used by the Visual Studio Modeling SDK (DSL Tools) is the source file the includes the "instructions" to produce a single generated code file from the source model.

In the Lithium Framework the text templates are more complex than that because they allow to generate multiple files from a single text template. This is achieved in reality by producing intermediary text templates at runtime during the text transformation session. The most important benefit of this approach is that the real text template used for each generated file does not need to exist physically in the Visual Studio solution, which allows the framework to add, update, or remove such templates without requiring any change to the microservices' solutions.

> You can view these intermediary text templates in Visual Studio from a microservice solution. If you activate "Show All Files" in one of the projects, a folder named `TextTemplates` will be revealed under `GeneratedCode`. These files should never be included in the project or in the solution (or in source control) because they will be replaced every time the text templates are transformed.

Each project in a microservice solution includes a file named `Template.lsmx` that is the root microservice for that project.

The contents of that file for the Models project is:

```xml
<?xml version="1.0" encoding="utf-8"?>
<template>
    <kind>Models</kind>
    <model>..\Design\Service.lsm</model>
</template>
```

This file provides two very important pieces of information to make the code generator work correctly to determine exactly which files it should generate for each project in the solution:

- The project where this template is located (to know which files should be generated).
- The location of the source service model (used to read it when generating code).

> These files should never be modified or removed from the projects.

## Transforming Templates (Generating Code)

To generate all the code for the microservice - for all projects - you should use the "Transform All Projects" menu command under the "Lithium" menu.

You can also generate only the code for the "current project" (that is the project selected in Solution Explorer at any given moment). This is done using the "Transform Current Project" menu command.

### Generated Code

The code generated for microservices as multiple forms and multiple languages:

- C# code
- XML files
- JSON files
- RESX files
- Etc.

The most important are off course the generated C# code files.

### Double-derived Classes

Code files are generated using a technique called "double-derived classes pattern" that allows the generated code to be easily overridden.

Here is an example:

```csharp
/// <summary>
/// Defines base class for the controller that provides monitoring routes.
/// </summary>
/// <remarks>
/// This is the base class of the MVC controller.
/// </remarks>
[GeneratedCode("Lithium", "2.0")]
[SuppressMessage("Maintainability Rules", "SA1402:FileMayOnlyContainASingleType", Justification = "Because of code generation design.")]
public abstract partial class MonitoringControllerBase : ApiControllerBase, IMonitoringController
{
    #region Code

    #region Protected Constructors

    /// <summary>
    /// Initializes a new instance of the <see cref="MonitoringControllerBase" /> class.
    /// </summary>
    protected MonitoringControllerBase()        
        : base()
    {
    }

    #endregion

    #region Public Methods

    /// <inheritdoc />
    [HttpGet(Primavera.Lithium.Noapi.Models.Metadata.Routes.Monitoring.Probe)]
    public abstract Task<IActionResult> ProbeAsync();

    /// <inheritdoc />
    [Authorize(Constants.Policies.NoApi)]
    [HttpGet(Primavera.Lithium.Noapi.Models.Metadata.Routes.Monitoring.Diagnostics)]
    public abstract Task<IActionResult> DiagnosticsAsync();

    #endregion

    #endregion
}
```

```csharp
/// <summary>
/// Defines the controller that provides monitoring routes.
/// </summary>
/// <remarks>
/// This is the MVC controller.
/// </remarks>
[GeneratedCode("Lithium", "2.0")]
[SuppressMessage("Maintainability Rules", "SA1402:FileMayOnlyContainASingleType", Justification = "Because of code generation design.")]
public partial class MonitoringController : MonitoringControllerBase
{
    #region Code

    #region Public Constructors

    /// <summary>
    /// Initializes a new instance of the <see cref="MonitoringController" /> class.
    /// </summary>
    public MonitoringController()        
    {
    }

    #endregion

    #endregion
}
```

Notice that the class that we are generating - `MonitoringController` - has in fact no code generated, it just derives from a base class - `MonitoringControllerBase` - that includes the real generated code.

This design allows to override the generated code, by overriding `MonitoringControllerBase` like in the following example:

```csharp
using System.Diagnostics.CodeAnalysis;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;

namespace Primavera.Lithium.Ping.WebApi.Controllers
{
    [SuppressMessage("StyleCop:DocumentationRules", "SA1601:PartialElementsMustBeDocumented")]
    public partial class MonitoringController
    {
        #region Protected Methods

        /// <inheritdoc />
        public override Task<IActionResult> ProbeAsync()
        {
            return Task.FromResult<IActionResult>(this.Ok("OK"));
        }

        #endregion
    }
}
```

## Partial Classes

All code files are also always generated as partial classes. This simplifies overrides but also allows extending generated code with new properties or methods.

Here is an example:

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics.CodeAnalysis;

namespace Primavera.Lithium.Notifications.WebApi.Configuration
{
    [SuppressMessage("StyleCop.CSharp.DocumentationRules", "SA1601: PartialElementsMustBeDocumented")]
    public partial class HostConfiguration
    {
        #region Public Properties

        /// <summary>
        /// Gets or sets the period of time the notifications worker waits before
        /// processing notifications again.
        /// The default value is 30 seconds.
        /// </summary>
        public int WorkerWaitInterval
        {
            get;
            set;
        }

        #endregion

        #region Protected Methods

        /// <inheritdoc />
        protected override void Initialize()
        {
            // Default behavior

            base.Initialize();

            // Default values

            this.WorkerWaitInterval = (int)TimeSpan.FromSeconds(30).TotalSeconds;
        }

        #endregion
    }
}
```