# Lithium Service Designer

The Service Designer is a domain-specific language, integrated in Visual Studio 2019, that allows modeling a microservice.

This language defines a set of concepts - domain entities - that, together, will define the shape of microservice Web API and its behavior.

The resulting model will then be used by the Lithium Modeling Framework to generate the code for the microservice.

## Design project

All the Lithium Visual Studio solutions include a project named `Design` that contains a file named `Service.lsm`.

This file describes the service model (as XML). When opened, the service designer will show up and setup a design surface where the microservice model can be constructed.

![Service Designer](./_assets/service-designer.png "Service Designer")

## Concepts (domain entities)

The Lithium Service Model includes a set of concepts to describe the microservice. Each as its own purpose and may or not be required for a particular microservice, depending on the features one wants to include.

The metamodel used by this designer adheres, in most cases, to how a REST Web API is usually described - as set of endpoints that manage resources - and, in particular, to how this kind of API concepts map to ASP.NET Core concepts.

- Resources - the pieces of data being transferred - are called "models".
- Endpoints are implemented by controller actions.
- Controllers aggregate actions logically.
- A service defines the Web API as a whole, aggregating controllers.

### Service

This is most important domain entity and it is represented in the service designer by the design surface (the canvas). It describes the microservice and allows specifying its main characteristics.

The following properties are the most important for the service:

| Property | Description | Example |
| - | - | - |
| Name | The name of the service | Settings |
| Id | A unique identifier for the service | SS |
| Namespace | The namespace associated with the service | Primavera.Lithium.Settings |
| Display Name | The display name of the service  | Settings Service |
| Version | The service version | 1.0 |
| Authorization Mode | A value indicating whether the service uses authorization (with Identity Server) or not | On |
| Default Scope | The name of the default scope used for authorization | lithium-settings |

Other configuration properties can only be modified by opening a configuration editor that is launched by clicking the ellipsis available in the `Configuration` property:

| Property | Description | Example |
| - | - | - |
| Generate Web API | A value indicating whether the microservice generates (includes) a Web API (false only if the solution is for a client library only) | True |
| Generate Client Library | A value indicating whether the client library for the service should be generated | True |
| Use Client Credentials Client | A value indicating whether a client for the client credentials authorization flow is to be used | True |
| Use Hybrid Client | A value indicating whether a client for the hybrid authorization flow is to be used  | False |

### Model

A model describes one resource managed by the microservice Web API. 

![Model](./_assets/model.png "Model")

You can have as many as needed in the model and they represent the pieces of data accepted and/or returned by the Web API endpoints.

> The Web API resources are called "models" in Lithium because the term resources is used in .NET for other concepts and that duplication would result in conflicts and misconceptions.

In the end a model is translated into a POCO (plain old C# object). A simple C# class (with properties) that describes the data object.

The most important properties of a model are:

| Property | Description | Example |
| - | - | - |
| Name | The name of the model | ProductSettingData |
| Base Model | Another model that acts as the base for this one (in terms of inheritance) | SettingData |
| Inheritance Modifier | A modifier that allows making a model abstract (for inheritance) | None |

As you would expect, a model can have one or more properties and these can be of 3 kinds:

- Scalar - the property value is scalar, meaning that is of a framework type (string, integer, Guid, etc.)
- Enumeration - the property value is a member of an enumeration defined in the service model.
- Model - the property value is another model defined in the service model.

The following properties, among others, are available for scalar properties:

| Property | Description | Example |
| - | - | - |
| Name | The property name | Id |
| Display Name | The property display name | Identifier |
| Type | The property type | Guid |
| Default Value | The property default value | 0 |
| Is Read-only | A value indicating that the property cannot be modified after the model is first created | False |
| Is Nullable | Indicates whether the property is nullable (for value types) | False |
| Is List | Indicates whether the property is a list of elements of the specified type | False |

The following properties are available for enumeration properties:

| Property | Description | Example |
| - | - | - |
| Name | The property name | Id |
| Display Name | The property display name | Identifier |
| Referenced Enumeration | The enumeration that is referenced by the property | ValueKind |
| Default Value | The property default value | ValueKind.Value |
| Is Read-only | A value indicating that the property cannot be modified after the model is first created | False |
| Is List | Indicates whether the property is a list of elements of the specified type | False |

The following properties are available for model properties:

| Property | Description | Example |
| - | - | - |
| Name | The property name | Id |
| Display Name | The property display name | Identifier |
| Referenced Model | The model that is referenced by the property | ValueData |
| Is Read-only | A value indicating whether the property cannot be modified after the model is first created | False |
| Is List | Indicates whether the property is a list of elements of the specified type | False |

> You can model simple 1:N relations, for example, by having model A have a model property that references model B and setting that property to a be list.

### Enumeration

As the name implies, an enumeration is a special kind of model that is translated to an enumeration instead of a class, resulting in that its value can only be one of a set of predetermined values.

![Enumeration](./_assets/enumeration.png "Enumeration")

 Property | Description | Example |
| - | - | - |
| Name | The name of the enumeration | ValueKind |

You can specify one or more members for the enumeration (the possible values). Each member as a name and a unique value (an integer).

### Controller

Controllers group related actions (the API endpoints).

Typically, you should group all the operations that act on the same model in a single controller.

You should consider that controllers also define two very important aspects of the Web API behavior:

- They set the base route of the endpoints (see the "Route" property).
- They define the authorization mode (and the corresponding scope) shared by all its actions.

![Controller](./_assets/controller.png "Controller")

> In the current version of Lithium you cannot specify the scope per action. This means that any two actions that should require different scopes will need to be modeled in two different controllers.

The most important properties of a controller are:

| Property | Description | Example |
| - | - | - |
| Name | The name of the controller | UserSettings |
| Route | The base route for all the child actions | /api/v{version:apiVersion}/usersettings |
| Authorization Mode | A value indicating whether the controller actions use authorization (with Identity Server) or not | On |
| Scope | The name of the scope used for authorization by the controller actions | (default) |

### Controller Action

An action describes an endpoint in the Web, an operation provided by the microservice. They can accept parameters (or not), and return a result (or not).

These operations are modeled considering the rules of HTTP and REST.

The most important properties of a controller action are:

| Property | Description | Example |
| - | - | - |
| Name | The name of the operation | GetUserSetting |
| HTTP Method | The HTTP method used | Get |
| Success Status Code | The HTTP status code that is returned when the operation succeeds | Ok |
| Return Value Type | The type of the return value (can be scalar or a model) | Model |
| Return Type Is List | A value indicating whether the action returns a list of items of the specified type | False |
| Route | The complete route of the endpoint | GET /api/v{version:apiVersion}/usersettings/{userId}/{key} |
| Authorization Mode | A value indicating whether the action uses authorization (with Identity Server) or not | On |

> Note that an action may return a result or not depending on the HTTP method in use (for example, a GET always returns a value).

You can define zero or more parameters for the controller action and these can be of two kinds (similar to model properties):

- Scalar - the parameter value is scalar, meaning that is of a framework type (string, integer, Guid, etc.)
- Model - the parameter value references a model defined in the model.

### Dependency

Dependencies allow for two things:

- Describing the dependencies of the microservice on external services (la REDIS cache, for example).
- Adding features to the microservice.

![Dependency](./_assets/dependency.png "Dependency")

> These dependencies will affect the code generated for the microservices and, in most cases, will just work out the box (without requiring the developer to add any custom code).

A dependency as only one important property:

| Property | Description | Example |
| - | - | - |
| Kind | The kind of dependency that should be added | BlobStorage |

### Regular Expression

As you will see, both models and controller action parameters allow specifying validation rules that will be enforced automatically by the framework. One kind of these validation rules allows checking values against a regular expression.

The regular expression domain entity provides the means to specify these regular expressions only once in the service model and then reusing them in multiple validation rules.

![Regular Expression](./_assets/regex.png "Regular Expression")

The most important properties of a regular expression are:

| Property | Description | Example |
| - | - | - |
| Name | The name of the regular expression | IdsRegEx |
| Value | The actual regular expression |  |

### Background Service

The Lithium Framework allows you to add background services to the microservice solution. These services will be automatically executed when the service application starts and run work (custom logic) in the background.

![Background Service](./_assets/backgroundservice.png "Background Service")

Background services come in three flavors:

- Normal background services - a basic background service that just starts when the service application starts.
- Timed background services - a background service that executes every time a given timespan ellapses.
- Queue background services - a background service that executes every time an new item becomes available in a queue.

The most important properties of a background service are:

| Property | Description | Example |
| - | - | - |
| Name | The name of the background service | StorageSetup |
| Use Worker | A value indicating whether a worker is associated with the background service | False |

> When "Use Worker" is true, you will need to model a background worker and set it in the "Worker" property of the associated background service.

### Background Worker

Workers allow sharing behavior between different background services. You define a single worker - and later implement it's logic - and reference it in multiple background services.

### Authorization Scope

When the service's authorization mode is on, the default scope (defined in the service properties) will be used by default by all controllers and controller actions.

You can define additional authorization scopes and then reference them in controllers. The actions will then use these scopes for authorization.

![Authorization Scope](./_assets/authorizationscope.png "Authorization Scope")

The main properties for authorization scopes are:

| Property | Description | Example |
| - | - | - |
| Name | The name of the authorization scope | WriteAccess |
| Scope | The scope that should be validated by the authorization middleware | lithium-identity-writeaccess |

### Authorization Policy

Authorization policies is a new concept in the service model that allows specifying custom logic to validate access to controller actions. It is not fully developed yet.

### API Version

Each microservice as a version defined in the service properties. This version, among other things, sets the API version (used in the endpoint routes) that is validated by a API versioning middleware that is automatically setup by the framework.

![API Version](./_assets/apiversion.png "API Version")

A service can support more than one version only to some extent. Currently you can only specify that all the actions defined in the service respond to the default version (the one specified in the service properties) and to all the API versions described in the service model.

> A future version of the Lithium Framework will allow to specify different API versions for different controller actions.

An API version has a single relevant property:

| Property | Description | Example |
| - | - | - |
| Version | The version number (in the form Major.Minor) | 2.0 |

### Comment

A comment is an artifact that has the only purpose of commenting the service model. It has no effect what so ever in the service behavior.
