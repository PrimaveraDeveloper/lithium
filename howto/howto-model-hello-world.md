# How to model a Hello World microservice

This guide describes the steps required to model a "Hello World" microservice using the service designer and add the necessary custom code.

For the purpose of this guide, the Hello World Service (HWS) should accept a request containing a person's name (e.g. "John Doe") and respond with a hello response (e.g. "Hello John Doe").

To achieve that, we will model a controller action (operation) accepting a parameter (the person's name) and returning a response with the message.

## Solution Template

Create a new microservice project named "HWS".

## Modeling

### Service Properties

Open the service designer file (`Service.lsm`) and set the following properties for the service:

- `Identifier`: HWS
- `Name`: HelloWorld
- `Authorization Mode`: off
- `Summary`: Provides hello world operations.

### Controller

Using the service model context menu, add a new "Controller" and set the following properties:

- `Name`: HelloWorld
- `Summary`: Provides hello world operations.

> Controllers provide the means to organize the operations (actions) in a service logically. Their name affects the actions' routes and also provide support for more advanced features like having different authorization policies for different actions.

### Action

Select the controller that you just created in the designer and, using the context menu, add an "Action". A new shape will appear in the designer. Set the following properties:

- `Name`: GetHelloWorld
- `Return Value Type`: String
- `Summary`: Returns a hello world message based on the input name.

Having selected this new action, use the context menu again to add a "Scalar Parameter" and set the following properties:

- `Name`: InputName
- `Type`: String
- `Summary`: The input name.

The model is done. Save it.

## Transform Text Templates (Generate Code)

Transform the text templates for all projects using the command available in the "Lithium" menu and await until the transformation is completed.

## Add Custom Code

Now you need to add the custom code for the service action that you just created.

> As with any other new service, the monitoring controller and the console client logic will also need to be implemented.

### Hello World Controller

```csharp
using System.Diagnostics.CodeAnalysis;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;

namespace Primavera.Lithium.Helloworld.WebApi.Controllers
{
    [SuppressMessage("StyleCop:DocumentationRules", "SA1601:PartialElementsMustBeDocumented")]
    public partial class HelloWorldController
    {
        #region Protected Methods

        /// <inheritdoc />
        protected override Task<IActionResult> GetHelloWorldCoreAsync(string inputName)
        {
            return Task.FromResult<IActionResult>(
                this.Ok($"Hello {inputName}!"));
        }

        #endregion
    }
}
```

> Validation of the input is not required because it is already implemented in the generated code. Open the `Controllers.gen.cs` file under `GeneratedCode` and locate the method `GetHelloWorldAsync()` method in `HelloWorldControllerBase`. Review how the code is generated and how the custom method `GetHelloWorldCoreAsync()` is invoked.

### Monitoring Controller

Locate the `CustomCode` folder under the `WebApi` project and add a class named `MonitoringController`. Enter the following code:

```csharp
using System.Diagnostics.CodeAnalysis;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;

namespace Primavera.Lithium.Helloworld.WebApi.Controllers
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

### Client Console Application

Locate the `CustomCode` folder under the `Client.Console` project and add a class named `Application`. Enter the following code:

```csharp
using System;
using System.Diagnostics.CodeAnalysis;
using System.Threading.Tasks;
using Primavera.Hydrogen;
using Primavera.Hydrogen.Rest.Client;

namespace Primavera.Lithium.Helloworld.Client.Console
{
    [SuppressMessage("StyleCop:DocumentationRules", "SA1601:PartialElementsMustBeDocumented")]
    internal sealed partial class Application
    {
        #region Protected Methods

        /// <inheritdoc />
        protected override void PrintCustomMenuOptions()
        {
            ConsoleHelper.WriteLine("1. Hello World.");
        }

        /// <inheritdoc />
        protected override async Task<bool> HandleCustomMenuOptionsAsync(ConsoleKeyInfo key)
        {
            switch (key.Key)
            {
                case ConsoleKey.D1:
                case ConsoleKey.NumPad1:
                    return !await this.ShowHelloWorldMenuAsync().ConfigureAwait(false);
                default:
                    return true;
            }
        }

        #endregion

        #region Private Methods

        #region Menus

        private async Task<bool> ShowHelloWorldMenuAsync()
        {
            // Menu

            bool terminate = false;
            bool cont = true;
            while (cont)
            {
                ConsoleHelper.WriteLine();
                ConsoleHelper.WriteLine("========================================");
                ConsoleHelper.WriteLine("Hello World Menu");
                ConsoleHelper.WriteLine("========================================");
                ConsoleHelper.WriteLine();
                ConsoleHelper.WriteLine("1. Get Hello World.");
                ConsoleHelper.WriteLine("<. Back.");
                ConsoleHelper.WriteLine("Q. Quit.");
                ConsoleHelper.Write(">> ");

                ConsoleKeyInfo key = ConsoleHelper.ReadKey();
                ConsoleHelper.WriteLine();

                switch (key.Key)
                {
                    case ConsoleKey.Q:
                        cont = false;
                        terminate = true;
                        break;
                    case ConsoleKey.LeftArrow:
                        cont = false;
                        break;
                    case ConsoleKey.D1:
                    case ConsoleKey.NumPad1:
                        await this.GetHelloWorldAsync().ConfigureAwait(false);
                        break;
                    default:
                        break;
                }
            }

            // Result

            return terminate;
        }

        #endregion

        [SuppressMessage("Design", "CA1031:Do not catch general exception types", Justification = "Want to print all exceptions.")]
        private async Task GetHelloWorldAsync()
        {
            ConsoleHelper.WriteLine();
            ConsoleHelper.WriteLine();

            // Get the parameters

            string inputName = this.GetValueString("input name");

            // Call the Web API

            ConsoleHelper.WriteLine();
            ConsoleHelper.WriteInformationLine("Calling the Web API...");

            try
            {
                ServiceOperationResult<string> response = await this.Client.HelloWorld.GetHelloWorldAsync(inputName).ConfigureAwait(false);

                ConsoleHelper.WriteInformationLine("Web API call succeeded.");
                ConsoleHelper.WriteInformationLine(response.Body);
            }
            catch (ServiceException ex)
            {
                this.WriteServiceException(ex);
            }
            catch (Exception ex)
            {
                ConsoleHelper.WriteErrorLine(ex);
            }
        }

        #endregion
    }
}
```

Now you can compile the solution.

## Run the Microservice

Set the startup projects to `Client.Console` and `WebApi`.

Hit F5 and run the service.

Now you can use the console client to check that the service is actually responding correctly.

Select 1 ("Hello World") in the menu and then 1 again ("Get Hello World").

Enter a name (e.g. "Joe") and await for the response ("Hello Joe").