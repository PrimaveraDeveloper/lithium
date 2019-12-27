# How to specify documentation on a microservice

This guide describes how to specify the documentation of a microservice.

All Lithium microservices will produce documentation for end-users. The Web API documentation (Open API) and the client library documentation (if a client library is created for the service). All this documentation is constructed from the XML code comments produced. That XML code comments are produced from the documentation specified in the service model.

It is paramount that you take care of specifying correct and meaningful documentation in the microservice model. That will ensure that service is easily and correctly used by end users.

## Service Documentation

To specify the documentation of the service:

1. Open the service model.
2. Select the service (click an empty area in the designer).
3. Check the Documentation section in the properties window.

The following documentation pieces are available:

- Summary (required): a general summary of the service (`<summary>` section in XML comments).
- Remarks: additional remarks about the service (`<remarks>` section in XML comments)
- Notes: notes about the service in the context of the service model (these notes are not included in the generated documentation).

Here is an example of how this documentation is then used in the generated code:

```csharp
/// <summary>
/// Called to add the OpenAPI documentation.
/// </summary>
/// <param name="services">The service collection.</param>
/// <param name="hostConfiguration">The host configuration.</param>
/// <remarks>
/// The method is called from <see cref="ConfigureServices(IServiceCollection)"/>.
/// </remarks>
protected virtual void AddOpenApiDocumentation(
    IServiceCollection services, 
    HostConfiguration hostConfiguration)
{
    // Default version

    services.AddOpenApiDocument(
        (options) =>
        {
            options.DocumentName = "2.0";
            options.Title = "PRIMAVERA Lithium Notifications Service (NS) Web API";
            options.Description = "Allows sending notifications to users based on templates.";
            options.Version = "2.0";
            
            options.ApiGroupNames = new string[] { "2.0" };

            options.FlattenInheritanceHierarchy = true;
            options.IgnoreObsoleteProperties = true;

            options.SchemaGenerator = new NoTitleFromDisplayNameOpenApiSchemaGenerator(
                options.SchemaGenerator.Settings);
        });
}
```

## Models Documentation

Specifying the documentation of models and model properties is crucial to make the service Web API and the client library easy to use by consumers.

The following pieces of documentation are available for models and model properties:

- Summary (required)
- Remarks
- Notes

This documentation is included in the C# classes generated:

```csharp
/// <summary>
/// Defines the base class for <see cref="ParameterData"/>.
/// </summary>
[GeneratedCode("Lithium", "2.0")]
public abstract partial class ParameterDataBase
{
    /// <summary>
    /// The parameter name.
    /// </summary>
    (...)
    public virtual string Name
    {
        get;
        set;
    }

    /// <summary>
    /// The parameter value.
    /// </summary>
    (...)
    public virtual string Value
    {
        get;
        set;
    }

    #endregion

    (...)
}

/// <summary>
/// Describes a parameter used in notifications.
/// </summary>
[GeneratedCode("Lithium", "2.0")]
public partial class ParameterData : ParameterDataBase
{
    (...)
}
```

## Enumerations Documentation

The following pieces of documentation are available for enumerations and enumeration members:

- Summary (required)
- Remarks
- Notes

This documentation is included in the C# enumerations generated:

```csharp
/// <summary>
/// Defines the state of an email notification.
/// </summary>
[GeneratedCode("Lithium", "2.0")]
public enum EmailNotificationState
{
    #region Members

    /// <summary>
    /// The notification is queued for sending.
    /// </summary>
    Queued = 0,

    /// <summary>
    /// The notification was sent.
    /// </summary>
    Sent = 1,

    /// <summary>
    /// The notification was canceled.
    /// </summary>
    Canceled = 2,

    /// <summary>
    /// The notification failed (not sent).
    /// </summary>
    Failed = 3,

    #endregion
}
```

## Controllers Documentation

The following pieces of documentation are available for controllers:

- Summary (required)
- Remarks
- Notes

Here is an example of how this documentation is then used in the generated code:

```csharp
/// <summary>
/// Provides actions to create, update, and delete email templates.
/// </summary>
/// <seealso cref="System.IDisposable" />
[GeneratedCode("Lithium", "2.0")]
public partial interface IEmailTemplatesOperations : IDisposable
{
    (...)
}
```

## Controller Actions Documentation

The controller actions define the operations provided by the Web API. They are the most important part of the Web API documentation.

The following pieces of documentation are available for controller actions:

- Summary (required)
- Remarks 
- Returns (required): describes the result of the Web API operation (when the operation produces a result).
- Notes

For controller action parameters the following pieces of documentation are available:

- Summary (required)
- Remarks 
- Notes

Here is an example of how this documentation is then used in the generated code:

```csharp
/// <summary>
/// Provides actions to create, update, and delete email templates.
/// </summary>
/// <seealso cref="System.IDisposable" />
[GeneratedCode("Lithium", "2.0")]
public partial interface IEmailTemplatesOperations : IDisposable
{
    /// <summary>
    /// Creates a new email template.
    /// </summary>
    /// <param name="emailTemplate">The email template that should be created.</param>
    /// <param name="cancellationToken">The cancellation token.</param>
    /// <returns>
    /// The <see cref="Task{TResult}"/> that represents the asynchronous operation.
    /// The <see cref="ServiceOperationResult{T}"/> result.
    /// </returns>
    /// <exception cref="ServiceException">Raised when the operation returns an unexpected status code.</exception>
    Task<ServiceOperationResult<string>> CreateEmailTemplateAsync(
        Primavera.Lithium.Notifications.Models.EmailTemplateData emailTemplate, 
        CancellationToken cancellationToken = default(CancellationToken));

    /// <summary>
    /// Creates a new email template.
    /// </summary>
    /// <param name="emailTemplate">The email template that should be created.</param>
    /// <returns>
    /// The <see cref="ServiceOperationResult{T}"/> result.
    /// </returns>
    /// <exception cref="ServiceException">Raised when the operation returns an unexpected status code.</exception>
    ServiceOperationResult<string> CreateEmailTemplate(
        Primavera.Lithium.Notifications.Models.EmailTemplateData emailTemplate);

    (...)
}
```

## Mind the Quality of the Documentation

Keep in mind that this documentation is the cornerstone of the Web API documentation and of the client library documentation.

Consumers will be able to understand the features provided by the microservice and use it effectively if this documentation is good and meaningful.

Be sure to keep the documentation in the service model correct and updated.