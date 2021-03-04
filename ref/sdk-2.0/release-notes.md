# Lithium SDK Release Notes

> These release notes include the releases of the Lithium SDK and the Service Designer.

### Version 62 (4/3/2021)

- Updated Service Solution to version 15.
- Setup now distributes a machine-wide NuGet.config file called `Lithium.NuGet.Config` and stored in folder `%ProgramFiles(x86)%\NuGet\Config`. This new file allows removing all `nuget.config` files from service solutions.
- Packages will no longer be restored to the `_packages` under the solution folder. Removing the `nuget.config` files will make the packages restore to the default packages cache (usually in folder `%userprofile%\.nuget\packages`), which will save a huge amount of disk space.
- New commands in the Tools menu: `Show NuGet Machine Configuration Directory`, `Show NuGet User Configuration Directory`, `Show NuGet Packages Cache Directory`, and `GUID Generator`.
- Improvements on the code generated for Authorization specs.
- Updated Hydrogen packages to version [2.0.10.379](../hydrogen-2.0/release-notes.md).
- Updated Hydrogen Design-time packages to version [3.0.0.53](../hydrogen-designtime-2.0/release-notes.md).

### Version 61 (21/2/2021)

- New domain property `ControllerType.IgnoreInWebApiDoc` to allow hiding a controller from the Web API documentation.
- New domain property `Webhook.Payload` to associate a webhook with the corresponding payload model (used to generate the Webhooks documentation).
- Webhooks documentation is now generated from the model and available from the service home page.
- Support for the active Visual Studio theme in the Service Designer surface.
- Bug fixing on Routes Explorer and Service Explorer.
- Modified the code generated for the Blob Storage, Secrets Storage, and Search dependencies to use new retry policy options.
- Updated Hydrogen packages to version [2.0.10.372](../hydrogen-2.0/release-notes.md).

### Version 60 (13/2/2021)

- Added validation to prevent having projects in `Primavera.Hydrogen.DesignTime.Configuration` misconfigured.
- Improved the code generated for `Models.Metadata.Localization.CulturesSupported`.
- New file named `Authorization.gen.authzspec.json` generated in project `Design`. This contains the authorization spec for the service (that Identity Server v5.0 will allow to import to create the service authorization resources).
- Modified the code generated for `Service.gen.lsspec.json` to exclude authorization resources.
- New environment variable `LITHIUM_LOCALDEVELOPMENT`.
- Modified the code generated for the Secrets Storage and Blob Storage dependencies to support managed identity better and add new configuration options.
- Updated Hydrogen packages to version [2.0.10.368](../hydrogen-2.0/release-notes.md).
- Updated .NET Core packages to version 3.1.12.

### Version 59 (14/1/2021)

- Refactoring of generated code concerning localization.
- Refactoring of generated for hybrid client and client credentials client.
- Updated Hydrogen packages to version [2.0.9.360](../hydrogen-2.0/release-notes.md).
- Updated .NET Core packages to version 3.1.11.

### Version 58 (5/1/2021)

- New domain property `BackgroundService.UseLocking` to enable or disable automatic locking (to prevent multiple instances of different servers from running at the same time).
- New domain property `BackgroundWorker.UseLocking` to enable or disable automatic locking.
- Refactoring on the code generated for background services to support automatic locking.
- New domain types `DateTimeOffset` and `Time`.
- Fixed bug in routes validation.
- New generated file in the `Models` project to specify constants that identity the webhooks event names.
- Updated Hydrogen packages to version [2.0.8.357](../hydrogen-2.0/release-notes.md).

### Version 57 (10/12/2020)

- Fixed generated code for `AzureKeyVaultSecretsStorageOptions`, required by changes introduced in Hydrogen.
- Changed the configuration for code analysis, now using `Microsoft.CodeAnalysis.NetAnalyzers` and rules defined in `global.editorconfig`.
- Updated Hydrogen packages to version [2.0.7.350](../hydrogen-2.0/release-notes.md).
- Updated Hydrogen Design-time packages to version [2.0.0.48](../hydrogen-designtime-2.0/release-notes.md).

### Version 56 (6/12/2020)

- Improved support for webhooks, including retry logic.
- Updated Hydrogen packages to version 2.0.7.341.

### Version 55 (5/12/2020)

- Improved support for webhooks, including new overloads to allows customizing requests from client applications.
- Improved the code generated for regular expressions to facilitate reuse.
- Improved the code generated for `Client.Console`.
- Bug fixing on the code generated for validating properties and controller action parameters of type `Guid`.
- Added the ability to ignore named text templates when transforming templates by adding options to the service project template.
- Updated Hydrogen Design-time packages to version 2.0.0.44.
- Updated Hydrogen packages to version 2.0.5.339.

### Version 54 (19/11/2020)

- Added support to return enumerations on controller actions.
- Improvements on code generated for validations to support additional scenarios.
- Reviewed the code generated for models validation.
- Reviewed the code generated for controller actions validation.
- Bug fixing for enumeration validation rules.
- Added support for Visual Studio 16.8.
- Updated Hydrogen Design-time packages to version 2.0.0.43.
- Updated Hydrogen packages to version 2.0.5.328.
    
### Version 53 (11/11/2020)

- Reviewed the generated code for integration tests to support additional scenarios.
- Added support for Identity Server 5 (a project still in progress).
- Updated Hydrogen packages to version 2.0.5.310.
- Updated .NET Core packages to version 3.1.10.
    
### Version 52 (7/11/2020)

- Reviewed the code generated for models validation.
- Reviewed the code generated for controller actions validation.
- Added support to model nullable enumeration properties.
- Improved the type editor for the `ModelTypeEnumProperty.DefaultValue` domain property.
- Added a new validation rule for the `Url` domain type to check that the URL is absolute.
- Added a new validation rule for all model properties with not single cardinality to check that their not empty.
- Added a new validation rule for all controller action parameters with not single cardinality to check that their not empty.
- Bug fixing on the designer.
- Bug fixing on the code generated to set the models default values.
- Updated Hydrogen Design-time packages to version 2.0.0.42.
- Updated Hydrogen packages to version 2.0.4.305.

### Version 51 (27/10/2020)

- Improved the code generated for the Taskbox dependency.
- Bug fixing on the code generated for `DELETE` controller actions.
- Updated generated code to support Azure AppConfiguration latest version.
- Updated Hydrogen Design-time packages to version 2.0.0.41.
- Updated Hydrogen packages to version 2.0.3.299.

### Version 50 (19/10/2020)

- Added domain property `ModelType.CustomAttributes` to allows specifying custom attributes to include in the code generated for the model.
- Added domain property `ModelTypeProperty.CustomAttributes` to allows specifying custom attributes to include in the code generated for the model property.
- Added domain property `ModelTypeProperty.Kind` to support calculated properties.
- Added domain property `ModelTypeProperty.GetAccessorModifier` to allow modifying the get accessor access modifier.
- Added domain property `ModelTypeProperty.SetAccessorModifier` to allow modifying the set accessor access modifier.
- Added domain property `ModelTypeProperty.ReadOnlyModifier` to allow defining properties with the get accessor only (replaces `ModelTypeProperty.GetAccessorOnly`).
- Added domain property `ModelTypeProperty.SerializationIgnore` to allow specifying that a property is not serialized (replaces `ModelTypeProperty.IgnoreSerialization`).
- Added a new validation rule for phone numbers to check for the E.164 format.
- Improved the code generated for throttling.
- Improved the code generated to validate email addresses and phone numbers.
- Added new dependency kind for Taskbox.
- Updated Hydrogen packages to version 2.0.3.293.

### Version 49 (14/10/2020)

- Added support to model actions with the same name on different controllers.
- Added new dependency kind for Throttling.
- Changed the code generated for telemetry.
- Changed the code generated when the service generates OIDC support to enable setting the culture based on the user claims.
- Changed the code generated to validate email addresses.
- Bug fixing on the code generated for webhooks.
- Updated Service Solution to version 14.
- Updated Hydrogen packages to version 2.0.3.280.
- Updated .NET Core packages to version 3.1.9.

### Version 48 (6/10/2020)

- Added new domain entity `ServiceDependency`. This allows defining dependencies on other Lithium services.
- Updated Hydrogen packages to version 2.0.2.270.

### Version 47 (29/9/2020)

- Added domain property `ModelTypeProperty.Cardinality` to allow defining model properties as lists or dictionaries. Domain properties `IsList` and `IsDictionary` were removed.
- Added domain property `ModelType.Visibility` to allow defining whether a model is visible in the Web API and/or the client library.
- Added domain property `ControllerType.AccessModifier` to allow making a controller internal in the client library.
- Added domain property `ControllerType.Visibility` to allow defining whether a controller is visible in the Web API and/or the client library.
- Added domain property `ControllerTypeAction.Visibility` to allow defining whether a controller action is visible in the Web API and/or the client library.
- Added domain property `ControllerTypeAction.ReturnValueCardinality` to allow defining controller action return values as lists or dictionaries. Domain property `IsList` was removed.
- Added domain property `ControllerTypeActionParameter.Cardinality` to allow defining controller action parameters as lists or dictionaries. Domain property `IsList` was removed.
- Updated Service Solution to version 13.
- Updated Hydrogen packages to version 2.0.2.266.

### Version 46 (21/09/2020)

- Added domain property `AuthorizationPolicy.Kind` to support policies that require scopes and custom policies (entirely specified in custom code).
- Added domain property `ControllerTypeAction.AuthorizationPolicy` to allow specifying authorization policies per action (no longer only per controller).
- Added domain property `ControllerType.RouteParametersPlacement` to allow specifying where the controller parameters should be placed in the route (after or before the controller name).
- Fixed bug with generated file encoding that prevented to import Postman collections.
- Updated Service Solution to version 12.
- Updated Hydrogen packages to version 2.0.2.253.
- Updated Hydrogen Design-time packages to version 2.0.0.39.

### Version 45 (09/09/2020)

- Bug fixing on the code generated for Azure AppConfiguration.
- Updated Hydrogen packages to version 2.0.2.242.
- Updated Hydrogen Design-time packages to version 2.0.0.38.
- Updated .NET Core packages to version 3.1.8.

### Version 44 (07/09/2020)

- Added new domain entity `ControllerTypeParameter`. This allows defining parameters on controllers, used in the route, and shared among all the controller's actions.
- Deprecated domain property `RouteWithActionName` from `ControllerTypeAction`.
- Updated Hydrogen packages to version 2.0.2.240.

### Version 43 (18/08/2020)

- Added support for Visual Studio 16.7.
- Updated Hydrogen packages to version 2.0.2.231.
- Updated Hydrogen Design-time packages to version 2.0.0.34.
- Updated .NET Core packages to version 3.1.7.

### Version 42 (09/07/2020)

- Added shared base class for all API controllers.
- Added support for get accessor only properties on models.
- Added support for ignoring properties in serialization on models.
- Various improvements in the generated code.
- Updated Hydrogen packages to version 2.0.1.207.

### Version 41 (15/06/2020)

- Added support for Visual Studio 16.6.
- Added Service Explorer tool window.
- Added Routes Explorer tool window.
- Updated Hydrogen packages to version 2.0.1.196.

### Version 40 (01/06/2020)

- Bug fixing on the code generated for controller actions with body arguments.

### Version 39 (31/05/2020)

- Bug fixing.
- Updated Hydrogen packages to version 2.0.1.195.

### Version 38 (19/05/2020)

- Refactoring on the code generated for background services to support work items (fix for the service provider disposed bug).
- Bug fixing.
- Updated Hydrogen packages to version 2.0.1.186.
- Updated Hydrogen Design-time packages to version 2.0.0.31.
- Updated .NET Core packages to version 3.1.4.

### Version 37 (18/05/2020)

- Added new scalar domain type decimal.
- Added new domain entity `ControllerTypeActionEnumParameter` to support defining parameters on controller actions that reference enumerations.
- Updated Hydrogen packages to version 2.0.1.184.

### Version 36 (11/05/2020)

- Removed domain property `IsReadOnly` from `ModelType`.
- Added support to define dictionary properties on models (domain property `IsDictionary`).
- Added support to ignore model properties in serialization (domain property `IgnoreSerialization`).
- Added support to define model properties only with get accessor (domain property `GetAccessorOnly`).
- Added support for result status code `Redirect`.
- Added new command to recheck the current solution (after upgrades or updating packages).
- Added the base model name to the model shape.
- Bug fixing.
- Updated Hydrogen packages to version 2.0.1.180.

### Version 35 (21/04/2020)

- Added new dependency kind for Multi-model Database.
- Added support for externalized application settings using Azure AppConfiguration service.
- Improved validations on service properties.
- Updated Hydrogen packages to version 2.0.1.165.

### Version 34 (14/04/2020)

- New domain entity Webhooks. This allows defining webhooks for the Web API.
- Updated Hydrogen packages to version 2.0.1.152.

### Version 33 (10/04/2020)

- New domain entity Webhooks. This allows defining webhooks for the Web API.
- Modified the menu command to add background services.
- Added a new menu command to edit the service configuration.
- Added new dependency kind for Event Bus.
- Bug fixing.
- Updated Hydrogen packages to version 2.0.1.151.
- Updated Hydrogen Design-time packages to version 2.0.0.28.
- Updated .NET Core packages to version 3.1.3.

### Version 32 (30/03/2020)

- Modified the configuration monitoring endpoint to dump options on the server.
- Modified code generated for monitoring endpoints to not require authorization.
- Added additional commands to the Lithium CLI.
- Updated Hydrogen packages to version 2.0.1.134.

### Version 31 (20/03/2020)

- Added the Lithium command-line interface (li.exe).
- Updated Hydrogen packages to version 2.0.1.122.

### Version 30 (02/03/2020)

- Added support for the endpoints and configuration monitoring endpoints.
- Updated Hydrogen packages to version 2.0.1.105.

### Version 29 (24/02/2020)

- New property on `ServiceModel` to activate the code generation required to setup OIDC (OpenID Connect).
- New domain entity `AuthorizationPolicy`. This allows specifying custom authorization policies.
- Updated Hydrogen packages to version 2.0.1.98.

### Version 28 (17/02/2020)

- Modified code generation to support the client library single assembly.
- Updated Hydrogen packages to version 2.0.1.87.
- Updated Hydrogen Design-time packages to version 2.0.0.25.
- Updated .NET Core packages to version 3.1.2.

### Version 27 (09/02/2020)

- Updated Service Solution to version 11.
- Updated Hydrogen packages to version 2.0.1.77.

### Version 26 (09/02/2020)

- Support for .NET Core 3.1.
- Bug fixing.
- Updated Service Solution to version 10.
- Updated Hydrogen packages to version 2.0.1.76.

### Version 25 (11/12/2019)

- Fixed packages references in test projects.
- Improved validation on documentation properties.
- Bug fixing.
- Updated Service Solution to version 9.
- Updated Hydrogen packages to version 2.0.1.53.

### Version 24 (09/12/2019)

- Added support for default values in model properties.
- New branding.
- Bug fixing.
- Prepared templates for code coverage in tests (on builds).
- Updated Service Solution to version 8.
- Updated Hydrogen packages to version 2.0.1.48.
- Updated Hydrogen Design-time packages to version 2.0.0.22.

### Version 23 (23/11/2019)

- Added support for generating Open API (Swagger) documentation.
- Added support for generating Client library documentation.
- Added new domain properties on controller actions to specify Open API documentation specifics.
- Added a new documentation property to specify the returns section in controller action.
- Added new dependency kind for Secrets Storage.
- First integration tests generated for the monitoring endpoints.
- Several improvements in generated code.
- Updated Hydrogen packages to version 2.0.1.24.
- Updated Hydrogen Design-time packages to version 2.0.0.20.

### Version 22 (11/11/2019)

- Added a new test project called `Integration.Tests` specifically for integration tests.
- Updated Service Solution to version 7.
- Updated Hydrogen packages to version 2.0.1.16.
- Updated Hydrogen Design-time packages to version 2.0.0.19.

### Version 21 (6/11/2019)

- Added generation of a Postman collection to test the service (in Design project).
- Brought back the `Is Read-only` property for `ModelProperty`, although it has no effect yet on how models are generated.
- New domain entity `ApiVersion`. This allows modeling backwards-compatibility for the service API versions.
- Updated Hydrogen packages to version 2.0.1.14.
- Updated Hydrogen Design-time packages to version 2.0.0.18.

### Version 20 (23/10/2019)

- Upgrade to .NET Core 3.0 (see Lithium documentation for instructions about changes required in custom code, settings, etc.).
- New property on Model to allow specifying a base model (inheritance).
- New property on Model to support modeling abstract models.
- New domain entity `RegularExpression` to support reusing the same regular expression on multiple validation rules.
- New property on `ControllerAction` to model its authorization mode.
- New property on `BackgroundService`, `TimedBackgroundService`, and `QueueBackgroundService` to allow specifying if the background service uses a worker or not.
- New dependency kind DataProtection to configure data protection services using a shared blob storage.
- Removed property `Is Read Only` from ModelProperty because the new `JsonSerializer` does not support deserializing read-only properties.
- Copy/paste is now fully supported in the designer.
- Major refactoring on generated code (because of .NET Core 3).
- New menu command to access the help site.
- Updated Hydrogen packages to version 2.0.1.5.

### Version 15 (22/08/2019)

- The SDK was updated to support Visual Studio 2019 only.
- The LITHIUM main menu is now under the EXTENSIONS menu.
- Updated Hydrogen packages to v.1.1.3.71.

### Version 14 (24/05/2019)

- Support for data type phone number.
- Fixed code generation for dependency Search.
- Fixed code generation when using default values in controller action parameters.
- Changed Startup code generation to include a telemetry initializer to set 404 responses on API routes as success.
- Updated Hydrogen packages to v.1.1.3.59.

### Version 13 (02/05/2019)

- Models without properties, to support scenarios where they need to be completely coded in partial classes.
- New property on Service to disable the generation of the service client constructors.

### Version 12 (22/04/2019)

- SDK is now installable using a setup (MSI).
- New project template: `Lithium Microservice (Client Library only)`. Allows modeling only a client library for an existing Web API or for InProc services.
- New project template: `Lithium Microservice (Web API only)`. Allows modeling a microservice without generating the client library.
- Added support to request localization on client libraries (property AcceptLanguage).
- Updated Hydrogen packages to v.1.1.3.47.

### Version 11 (15/04/2019):

- Added property `Identifier` to the service domain class. This property must be a globally unique string used to identify the service. The value should be a string with 2..5 characters (e.g. 'NS').
- Fixed a bug in transformation that caused problems on check-in with resources files introduced by the Orinoco translation process.

### Version 10 (08/04/2019):

- New domain entity `BackgroundWorker`. This allows sharing the same worker between different background services.
- Support for `Queue background services`. This kind of background services run the underlying worker automatically when a new instance becomes available in a queue (IBackgroundWorkerQueue).
- Support for `Timed background services`. This kind of background services run the underlying worker automatically at fixed time intervals, by queuing their execution to a queue shared with a queue background service.
- Refactoring on the code generated for background services to include timed and queue background services.
- New menu command to view the release notes.
- Updated Hydrogen packages to v.1.1.3.46.