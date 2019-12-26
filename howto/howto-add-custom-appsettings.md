# How to add custom application settings

This guide describes how to add custom application settings to a microservice host.

The behavior of a microservice can be made configurable by providing custom application settings.

These settings need to be added to the host, so they can be set in the application settings JSON files and/or at runtime in the microservice environments (on Azure).

## `HostConfiguration`

The microservice host includes a class named `HostConfiguration` that is designed to hold all the configuration options used by the service.

By default, this class is generated as follows (see `Configuration.gen.cs`):

```csharp
/// <summary>
/// Defines the configuration settings used by the service host.
/// </summary>
[GeneratedCode("Lithium", "2.0")]
[SuppressMessage("Maintainability Rules", "SA1402:FileMayOnlyContainASingleType", Justification = "Because of code generation design.")]
public partial class HostConfiguration : HostConfigurationBase
{
    #region Code

    #region Public Constructors

    /// <summary>
    /// Initializes a new instance of the <see cref="HostConfiguration"/> class.
    /// </summary>
    public HostConfiguration()        
        : base()
    {
    }

    #endregion

    #endregion
}
```

HostConfigurationBase is the base class and it defines only the following configuration options, required by any service:

- `IdentityServerBaseUri`: the address of the Identity Server that should be used by the authentication and authorization logic.
- `Information`: contains general information about the service (like its name, version, copyright, etc.)

So, to add new configuration options, the `HostConfiguration` type needs to be extended with new properties.

## Defining Custom Application Settings in Code

First, you need to a new file named `HostConfiguration.cs` to the `CustomFolder` (it it doesn't exist already) and add the new configuration options as properties.

Example:

```csharp
[SuppressMessage("StyleCop.CSharp.DocumentationRules", "SA1601: PartialElementsMustBeDocumented")]
public partial class HostConfiguration
{
    #region Public Properties

    /// <summary>
    /// Gets or sets a value indicating whether the notifications worker is enabled.
    /// The default value is true.
    /// </summary>
    public bool WorkerEnabled
    {
        get;
        set;
    }

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

        this.WorkerEnabled = true;
        this.WorkerWaitInterval = (int)TimeSpan.FromSeconds(30).TotalSeconds;
    }

    #endregion
}
```

> The properties should always be defined with a public getter and a public setter.

### Default Values

Notice in the previous example, how the default values for the new configuration options can be set, overriding `Initialize()`.

## Defining Custom Application Settings in the Configuration Files

Now that the new configuration options are defined, their values can be set in the configuration files.

These configuration files can be defined per runtime environment. This means that the options can have different values depending on the environment where the microservice is running on.

Create the application settings file in the `CustomFolder` (e.g. `appsettings-development.json`) and set the new configuration options using the correct JSON notation.

Example:

```json
{
    "HostConfiguration": {
        "WorkerEnabled": false,
        "WorkerWaitInterval": 15
    }
}
```

That's it. The new configuration options should be correctly read by the microservice host startup logic and it's now a matter of making the custom business logic behave accordingly.

## Defining Custom Application Settings in the Service Specification

Each microservice solution includes a specification that is used to automate deployment. This specification is combined from two files, one generated and one that should include any custom logic not generated.

- `Design\GeneratedCode\Service.gen.lsspec.json`
- `Design\CustomCode\Service.lsspec.json`

To automate the configuration, this specification needs to include all the application settings used by the microservice. So, when you create a custom application setting, you need to add it to `Design\CustomCode\Service.lsspec.json`.

Example:

```json
{
    "appSettings": {
        "custom": [
            {
                "name": "HostConfiguration:WorkerEnabled",
                "value": {
                    "pd": true
                }
            },
            {
                "name": "HostConfiguration:WorkerWaitInterval",
                "value": {
                    "pd": 30
                }
            }
        ]
    }
}
```