# Capability - Validation

> Tags: Models, Properties, Controller Actions, Parameters

One important aspect of Web API design is validating the inputs and returning meaningful responses to the caller so that they correct their requests.

The [Service Designer](../ref/sdk-2.0/service-designer.md) includes features to model such validation on model properties and controller actions' parameters.

The generated code relies on validation attributes for model validation:

```csharp
[Display(Name = nameof(ModelsResources.RES_Display_EmailNotificationData_EmailTemplateId), ResourceType = typeof(ModelsResources))]
[JsonPropertyName("emailTemplateId")]
[RegularExpression(@"^(\d|[a-z]|[A-Z]|_|-)*$", ErrorMessageResourceName = nameof(ModelsResources.RES_Validation_EmailNotificationData_EmailTemplateId_RegularExpression), ErrorMessageResourceType = typeof(ModelsResources))]
[Required(AllowEmptyStrings = false, ErrorMessageResourceName = nameof(ModelsResources.RES_Validation_EmailNotificationData_EmailTemplateId_Required), ErrorMessageResourceType = typeof(ModelsResources))]
public virtual string EmailTemplateId
{
    get;
    set;
}
```

And in the [ApiControllerBase](../ref/hydrogen-2.0/AspNetCore.md) component for controller action' parameters validations.

```csharp
public virtual Task<IActionResult> SendEmailNotificationAsync(
    [FromBody] Primavera.Lithium.Notifications.Models.EmailNotificationData notification)
{
    // Begin Validation

    if (!this.Validate()
        .Required(
            notification, 
            ErrorCodes.SendEmailNotificationNotificationRequired, ValidationResources.RES_Error_SendEmailNotification_Notification_Required)
        .Result(
            ErrorCodes.RequestArgsInvalid, 
            ValidationResources.RES_Error_RequestArgsInvalid, 
            out ServiceError validationError))
    {
        return Task.FromResult<IActionResult>(this.BadRequest(validationError));
    }

    // Result

    return this.SendEmailNotificationCoreAsync(notification);
}
```