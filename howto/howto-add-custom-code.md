# How to add custom code to a microservice

This guide describes how custom code should be added to a microservice solution.

An important part of the code in a microservice is generated using text templates. Most of the times this code just needs to be "finished" with custom code - the monitoring controller is an example of that - but there are also scenarios where generated code needs to be extended and new interfaces, classes, etc. need to be created from scratch. This is all custom code.

## `CustomCode` Folder

All custom code for each of the microservices projects should be placed inside a folder named `CustomCode`. Avoid adding this code on other locations in the project because that will result in confusion as the service grows in complexity.

## Mind the Namespace

When you add a new class to a folder in Visual Studio, it will suggest a namespace for the new class containing the name of the folder. This is not a good idea for this `CustomFolder`, either when you are customizing generated code (the namespace will need to match the one used in the generated code) or not (`CustomCode` does not add meaning to the namespace of the new class).

The first thing you need to do after adding a code file (interface, class, etc.) to the `CustomCode` folder is to correct the namespace suggested.

## `CustomCode` Organization

Files under the `CustomCode` folder should be organized as suitable in folders according to namespaces.

Typically, the `CustomCode` folder will contain folders like these:

- BackgroundServices
- Controllers
- Configuration
- Managers
- Services
- Views
- Etc.

## Overriding Generated Code

To override generated code, you create a partial class/interface under the CustomCode folder with the name of the derived class (e.g. MonitoringController) and implement the overridden methods.

Example:

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

> Notice that the partial class does not need to be documented completely. You should suppress the `SA1601` error in the partial class and add `<inheritdoc />` to XML code comments in the overridden methods.

## Extending Generated Code

You can also add additional behavior to the generated code. That can be done either on the derived class or the base class, depending on the case in hand.

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