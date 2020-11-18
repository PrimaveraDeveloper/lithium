# How to **define validation rules** on microservices models and actions parameters

This guide describes how to used the properties in the Service Designer to specify validation rules that will be automatically evaluated on models and actions parameters.

These validation rules depend essentially on the data type of the underlying element - either a model property or a controller action parameter.

The following validation rules are available:

| Data Type | Validations |
| - | - |
| `Boolean` | `IsRequired` (\*), `NotEmpty` (\*) |
| `ByteArray` | `IsRequired`, `NotEmpty` (\*) |
| `DateTime` | `IsRequired` (\*), `NotEmpty` (\*) |
| `Double` | `IsRequired` (\*), `GreaterThan`, `LessThan`, `NotEmpty` (\*) |
| `Email` | `IsRequired`, `MaxLength`, `MinLength`, `NotEmpty` (\*), `RegularExpression` |
| `Guid` | `IsRequired` (\*), `NotEmpty` (\*) |
| `Integer` | `IsRequired` (\*), `GreaterThan`, `LessThan`, `NotEmpty` (\*) |
| `Long` | `IsRequired` (\*), `GreaterThan`, `LessThan`, `NotEmpty` (\*) |
| `Password` | `IsRequired`, `MaxLength`, `MinLength`, `NotEmpty` (\*), `RegularExpression` |
| `PhoneNumber` | `E.164`, `IsRequired`, `NotEmpty` (\*) |
| `String` | `IsRequired`, `MaxLength`, `MinLength`, `NotEmpty` (\*), `RegularExpression` |
| `Url` | `Absolute`, `IsRequired`, `NotEmpty` (\*) |
| `Enumeration` | `IsRequired` (\*), `NotEmpty` (\*) |
| `Model` | `IsRequired`, `NotEmpty` (\*) |

> Rules marked with (\*) are only available if the property/parameter has a cardinality of `List` or `Dictionary`.

## Validation Rules

The validation rules available are the following:

| Rule | Input | Description |
| - | - | - |
| `Absolute` | -- | The value should be an absolute URI. |
| `E.164` | -- | The value should a phone number expressed in the E.164 format. |
| `IsRequired` | -- | The value should not be null. |
| `GreaterThan` | `MinValue` | The value should be greater than `MinValue`. |
| `LessThan` | `MaxValue` | The value should be less than `MaxValue`. |
| `MaxLength` | `MaxLen` | The length of the value should be less than or equal to `MaxLen`. |
| `MinLength` | `MinLen` | The length of the value should be greater than or equal to `MinLen`. |
| `NotEmpty` | -- | The value should not be empty. |
| `RegularExpression` | `RegEx` | The value should match the regular expression `RegEx`. |

## Validation Rules on Model Properties

To specify the validation rules of a model property:

1. Select that property in the service designer.
2. In the properties window, expand the property `Validation Rules`.
3. Set the value of the desired validation rules.

> Notice that the validation rules available vary depending the property type.

## Validation Rules on Action Parameters

To specify the validation rules of a controller action parameter:

1. Select that parameter in the service designer.
2. In the properties window, expand the property `Validation Rules`.
3. Set the value of the desired validation rules.

## Validation Rules on Controller Parameters

To specify the validation rules of a controller parameter:

1. Select that parameter in the service designer.
2. In the properties window, expand the property `Validation Rules`.
3. Set the value of the desired validation rules.

## Lists or Dictionaries

When the model property or controller parameter has a cardinality of `List` or `Dictionary`, the validation rules also apply to the elements in the list/dictionary.

This means, for example, that if a model property has a cardinality of `List` and a validation rule `IsRequired`, the property cannot be null, neither any element in the list can be null.

## Generated Code

The validation rules specified in the microservice model will be automatically enforced by code generated for the microservice.

### Model Properties

In the case of the model properties, most validations are implemented as validation attributes, like in the following example (see `Models.gen.cs`):

```csharp
/// <summary>
/// The properties associated with the certificate.
/// </summary>
[Display(Name = nameof(ModelsResources.RES_Display_CertificateData_Properties), ResourceType = typeof(ModelsResources))]
[JsonPropertyName("properties")]
[NotEmpty(ErrorMessageResourceName = nameof(ModelsResources.RES_Validation_CertificateData_Properties_NotEmpty), ErrorMessageResourceType = typeof(ModelsResources))]
[Required(ErrorMessageResourceName = nameof(ModelsResources.RES_Validation_CertificateData_Properties_Required), ErrorMessageResourceType = typeof(ModelsResources))]
public virtual IList<Primavera.Lithium.Certificates.Models.PropertyData> Properties
{
    get;
    set;
}
```

### Controller and Action Parameters

For controller and action parameters, the validations are set in the Web API controller (see `WebApi\Controllers.gen.cs`) and in the Client Library controller (see `ClientLib\Controllers.gen.cs`):

```csharp
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
```

```csharp
public virtual Task<ServiceOperationResult> DeleteUserSettingAsync(string userId, string key, CancellationToken cancellationToken = default(CancellationToken))
{
    // Begin Validation

    SmartGuard.NotNullOrEmpty(() => userId, userId);
    SmartGuard.MaxLength(() => userId, userId, 200);
    SmartGuard.RegularExpression(() => userId, userId, @"^(\d|[a-z]|[A-Z]|\.|-)*$");

    SmartGuard.NotNullOrEmpty(() => key, key);
    SmartGuard.MaxLength(() => key, key, 200);
    SmartGuard.RegularExpression(() => key, key, @"^(\d|[a-z]|[A-Z]|\.|-|_)*$");

    // End Validation

    // Route

    Uri requestUri = Models.Metadata.Routes.Instance.Resolve(
        this.Client.BaseUri,
        Models.Metadata.Routes.UserSettings.DeleteUserSetting,
        new Dictionary<string, object>()
        {
            ["version"] = this.Client.ApiVersion,
            ["userId"] = userId,
            ["key"] = key,
        });

    // Result

    return this.Actions.ExecuteDeleteNoResultAsync(requestUri, HttpStatusCode.NoContent, cancellationToken);
}
```