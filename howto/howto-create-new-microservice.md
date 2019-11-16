# How to create a new microservice in Visual Studio

This guide describes the steps required to create a new microservice, compile it and execute it.

## Solution Template

Start by creating a new project in Visual Studio using the "PRIMAVERA Lithium Microservice" project template.

> The "Create a new project" window in Visual Studio does not allow to view the templates organized by custom categories. You need to enter "Lithium" in the search box to view the Lithium solution templates.

Enter the microservice identifier in the "Project name" box, select the solution location, check "Place solution and project in the same directory" and click "Create".

> The microservice identifier should be 2 to 3 chars long in uppercase (e.g. "PS"). This identifier should be different from any other service (see the directory available in this documentation).

Once the solution is created it should include 4 solution folders:

- `.nuget`: contains a single file named `nuget.config`.
- `DesignTime`: contains a single project called `Design` that allows modeling the service.
- `Runtime`: contains the microservice itself (`ClientLib`, `Models` and `WebApi`) and a console client.
- `Tests`: contains the test projects for the microservice.

## Model the Service Basic Properties

For this example we just want to have the microservice running without really any feature, so it is only required to set the most basic properties in the model so that the service can be compiled and executed.

Open the `Service.lsm` file in the `Design` project to view the service designer and change the service model.

> Notice that the service model is empty.

Locate the properties window and enter the microservice identifier - the same you used to create the solution - in the `Identifier` property.

Enter the service name in the `Name` property (e.g. `Ping`).

Make sure that property `Namespace` and `Default Scope` changed accordingly (e.g. `Primavera.Lithium.Ping` and `lithium-ping`).

Enter a detailed description of the service in the `Summary` property (e.g. "Executes a ping operation in the infrastructure.").

When starting the development of a microservice you may turn off authorization. That is done by changing `Authorization Mode` to `off`.

Review all the other properties of the service and save the model.

## Transform Text Templates (Generate Code)

You can now generate the code for the service for the first time.

Select "Transform All Projects" under the "Lithium" menu (it is located in the main menu under "Extensions|Lithium|Service Designer" and in the solution explorer context menu).

Wait until a message box appears indicating that the text transformation terminated. The solution is ready now to be compiled for the first time.

## Add Minimal Custom Code

If you compile the solution now it will fail with 4 errors indicating that the following methods need to be implemented (overridden):

- `MonitoringController.ProbeAsync()`
- `MonitoringController.DiagnosticsAsync()`
- `Application.PrintCustomMenuOptions()`
- `Application.HandleCustomMenuOptions()`

The first two are related with the controller that implements health checks for the service. The other two are the implementation of the console client. You need to add minimal implementations for these methods.

### Monitoring Controller

Locate the `CustomCode` folder under the `WebApi` project and add a class named `MonitoringController`. Enter the following code:

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

        /// <inheritdoc />
        public override Task<IActionResult> DiagnosticsAsync()
        {
            return this.ProbeAsync();
        }

        #endregion
    }
}
```

> Notice the namespace used and how the two methods are overridden. For the time being they don't do much but it is enough to monitor the service in the beginning. Later you will need to evolve these implementations to check specific service behaviors.

### Client Console Application

Locate the `CustomCode` folder under the `Client.Console` project and add a class named `Application`. Enter the following code:

```csharp
using System;
using System.Diagnostics.CodeAnalysis;
using System.Threading.Tasks;

namespace Primavera.Lithium.Ping.Client.Console
{
    [SuppressMessage("StyleCop:DocumentationRules", "SA1601:PartialElementsMustBeDocumented")]
    internal sealed partial class Application
    {
        #region Protected Methods

        /// <inheritdoc />
        protected override void PrintCustomMenuOptions()
        {
            // ConsoleHelper.WriteLine("1. Ping.");
        }

        /// <inheritdoc />
        protected override async Task<bool> HandleCustomMenuOptionsAsync(ConsoleKeyInfo key)
        {
            switch (key.Key)
            {
                // case ConsoleKey.D1:
                // case ConsoleKey.NumPad1:
                //     return !await this.ShowPingMenuAsync().ConfigureAwait(false);
                default:
                    return true;
            }
        }

        #endregion
    }
}
```

> Notice the namespace and how the two methods are overridden. The commented lines of code are there just to show how this class should be modified in the future.

Now you can compile the solution and it will succeed.

## Run the Microservice

Before you run it, select "Set Startup Projects..." in the solution context menu, choose "Multiple startup projects", change "Console.Client" and "WebApi" to "Start" and click "OK".

Now hit F5 and run the service. Two console windows should appear:

- `WebApi`: shows the output of the Web API host.
- `Client.Console.exe`: shows the console client.

Now you can use the console client to verify that the service is actually responding.

Select the "Monitoring" menu (9) and then "Probe" (1).

This will execute the probe endpoint (the one implemented before in the monitoring controller) and it should respond with "OK", meaning the service is up and running.