# How to define complex actions on microservices

This guide describes how to model controllers and complex actions (operations) using the Service Designer.

The Web API of a microservice is defined in the service model by describing controllers that categorize the operations (endpoints) available.

These operations follow the best practices for REST API and modeling involves choosing the adequate HTTP methods (verbs), describing their inputs and expected outputs, along with other semantics. Then the Lithium Framework will use that model to generate the skeleton of the API so that the developer can then add the specific business logic.

## Controllers

A controller has 4 main purposes in the Lithium Framework:

- It organizes a set of related actions in a coherent way (envisioned by the developer, often based on the fact that these actions operate on the same resource/model).
- It corresponds to a single class generated in the `WebAPI` project that will contain all these actions as methods.
- Ensures that all its actions share the same base route.
- Enforces the same authorization policy for all its actions.

To add a controller to a microservice, select Add | Controller using the designer context menu.

This will create a new shape in the designer (called `Controller1` for example). Now:

1. Change its name to the one you want (in the properties window).
2. Review the display name.
3. Set the summary property with a description of the new controller (this will be used in the generated documentation).

The new controller will not be valid because it should have at least one action. You will need to specify those next.

### Routes

All the actions under the same controller share the same base route which is derived from the controller name.

You can review that by inspecting the `Route` property in the properties window.

> The Service Designer allows customization to some extent by specifying a prefix that will be added to the generated route. See `Route Prefix`.

### Authorization

The authorization policy is also shared by all the actions of a controller.

You can specify if authorization is on or off with the `Authorization Mode` and, when on, you can specify which scope needs to be provided by client applications to access the operations (see `Scope`).

> Note that authorization mode can be disabled completely in a microservice by modifying the `Authorization Mode` for the service. This setting supersedes the settings for each controller.

## Actions

A controller can have one or more actions (operations) that will correspond to the endpoints of the Web API.

To add an action to a controller you need to select it in the designer and the then choose Add Action in the context menu.

> This option will only show if a controller is selected in the designer.

This will create a new shape in the designer (called `Action1` for example). Now:

1. Change its name to the one you want (in the properties window).
2. Review the display name.
3. Set the summary property with a description of the new model (this will be used in the generated documentation).

The new action will not be valid yet because you will need to specify its parameters (if any) and its expected results.

### HTTP Method

A controller action should have one of the following HTTP methods (verbs), according common to REST API guidelines:

- `GET`: used typically when the action retrieves results (hence it requires that the action has a Return Value Type different from None).
- `POST`: used typically when the action creates resources.
- `PUT`: used typically when the action updates resources.
- `DELETE`: used typically when the action deletes resources.

So the first thing you need to do is decide on the HTTP method that the action shall use. This is set in the `HTTP Method` property.

> Depending on which method is selected, various behaviors of the designer will be different, specially validations.

### Expected Result

Next you need to model the expected result of the action.

First, you should set the `Success Status Code`, that indicates the HTTP status code that the endpoint will return when it **succeeds**.

Second, you should set the `Return Value Type`, which indicates the type of the result when the action **succeeds**. It can be `None`, any of the simple data types (e.g. `Integer`) or a model previously defined. If not `None`, it can also be a list of the type selected (see `Return Value Is List`).

Finally, you should set the `Produces Conflict` and `Produces NotFound` properties to true or false depending on if the action may return those status codes in case of **failure**.

> These two properties feed the Web API documentation. This information will be very useful for consumers to be able to correctly handle the errors returned by the API.

### Parameters

Finally you can add as much parameters as needed to the controller action.

To add a parameter to an action you need to select it in the designer and the then choose one of the following context menu options:

- Add Scalar Parameter
- Add Model Parameter

> These menu options will only show if an action is selected in the designer.

The new parameter will be created in the `Parameters` list of the action shape. Its properties are pretty much the same as those of a model property.

### Routes

The route associated with each action is inferred automatically from its definition and combines multiple characteristics of the action (the controller, the HTTP method, the parameters, etc.).

For obvious reasons, no two actions in the entire model can have the same route.

You can review the action route by inspecting the `Route` property in the properties window.

> The Service Designer allows further customization of this route via the following properties: Add `Action to Route`, `Route Prefix`, and `Route Suffix`. Furthermore, the `Binding` property of the action parameters also has affect in the action route.

## Generated Code

The code generated for all the controllers in a microservice is generated in the `Controllers.gen.cs` file under the `WebApi` project.

For each controller in the designer, two classes will be generated. One is the base class - with all the actions specified. The other is the controller class itself, empty, to allow overriding the generated code.

Here is an example:

```csharp
/// <summary>
/// Provides operations to retrieve certificates.
/// </summary>
/// <remarks>
/// This is the base class of the MVC controller.
/// </remarks>
[GeneratedCode("Lithium", "2.0")]
[Authorize(Constants.Policies.Certificates)]
[SuppressMessage("Maintainability Rules", "SA1402:FileMayOnlyContainASingleType", Justification = "Because of code generation design.")]
public abstract partial class CertificatesControllerBase : ApiControllerBase, ICertificatesController
{
    #region Code

    #region Protected Properties

    /// <summary>
    /// Gets the logger.
    /// </summary>
    protected ILogger Logger
    {
        get
        {
            return this.HttpContext.RequestServices.GetRequiredService<ILogger<CertificatesControllerBase>>();
        }
    }

    #endregion

    #region Protected Constructors

    /// <summary>
    /// Initializes a new instance of the <see cref="CertificatesControllerBase" /> class.
    /// </summary>
    protected CertificatesControllerBase()        
        : base()
    {
    }

    #endregion

    #region Public Methods

    /// <inheritdoc />
    [HttpGet(Primavera.Lithium.Certificates.Models.Metadata.Routes.Certificates.GetCertificate)]
    [ProducesResponseType(typeof(Primavera.Lithium.Certificates.Models.CertificateData), (int)HttpStatusCode.OK)]
    [ProducesResponseType(typeof(ServiceError), (int)HttpStatusCode.BadRequest)]
    [ProducesResponseType(typeof(ServiceError), (int)HttpStatusCode.NotFound)]
    public virtual Task<IActionResult> GetCertificateAsync([FromRoute] string certificateName)
    {
        // Begin Validation

        if (!this.Validate()
            .Required(certificateName, ErrorCodes.GetCertificateCertificateNameRequired, ValidationResources.RES_Error_GetCertificate_CertificateName_Required)
            .RegularExpression(certificateName, @"^(\d|[a-z]|[A-Z]|\.|-|_)*$", ErrorCodes.GetCertificateCertificateNameInvalid, ValidationResources.RES_Error_GetCertificate_CertificateName_Invalid)
            .Result(ErrorCodes.RequestArgsInvalid, ValidationResources.RES_Error_RequestArgsInvalid, out ServiceError validationError))
        {
            return Task.FromResult<IActionResult>(this.BadRequest(validationError));
        }

        // Result

        return this.GetCertificateCoreAsync(certificateName);
    }

    #endregion

    #region Protected Methods

    /// <summary>
    /// Allows retrieving the specified certificate.
    /// </summary>
    /// <param name="certificateName">The certificate name.</param>
    /// <returns>
    /// The action result.
    /// </returns>
    protected abstract Task<IActionResult> GetCertificateCoreAsync(string certificateName);

    #endregion

    #endregion
}
```

> Notice that the method corresponding to the controller action - `GetCertificateAsync()` - includes the validations of parameters in the generated code and delegates the execution of the specific business login to a method - `GetCertificateCoreAsync()` - that the developer will be required to override.

```csharp
/// <summary>
/// Provides operations to retrieve certificates.
/// </summary>
/// <remarks>
/// This is the MVC controller.
/// </remarks>
[GeneratedCode("Lithium", "2.0")]
[SuppressMessage("Maintainability Rules", "SA1402:FileMayOnlyContainASingleType", Justification = "Because of code generation design.")]
public partial class CertificatesController : CertificatesControllerBase
{
    #region Code

    #region Public Constructors

    /// <summary>
    /// Initializes a new instance of the <see cref="CertificatesController" /> class.
    /// </summary>
    public CertificatesController()        
        : base()
    {
    }

    #endregion

    #endregion
}
```

## Custom Code

The developer will always be required to implement the specific business logic for each controller action - `GetCertificateCoreAsync()` in the example above.

This requires creating a partial class for each controller and implementing the abstract methods (the actions).

You can customize further the behavior of any controller or action by:

- Adding a partial class to include additional properties, methods, etc..
- Adding a partial class and overriding the generated code (to include additional validations for example).

You can also define controllers and/or actions models entirely in custom code. These new controllers and/or actions models will not be recognized by the service model.

> If you add additional actions to a controller in custom code, you should set the `Custom Actions` property in the designer.

> If you add custom controllers in custom code, you should set the `Custom Controllers` property in the designer.