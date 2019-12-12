# How to add complex models to a microservice

This guide describes how to define complex models using the Service Designer.

The operations in a microservice API accept inputs and (may) produce outputs (results). Both can be scalar values (strings, integers, etc.) or complex objects, which are called Models in the Lithium Framework.

> In literature about REST this kind of complex objects are often called Resources. The concept is exactly the same.

The Lithium Service Designer allows to describe these models. That will be required most of the times, even for the most simple microservices.

## Models

To add a model to a microservice, select Add | Model using the designer context menu.

This will create a new shape in the designer (called `Model1` for example). Now:

1. Change its name to the one you want (in the properties window).
2. Review the display name.
3. Set the summary property with a description of the new model (this will be used in the generated documentation).

The new model is valid but it still is empty, with no properties. You will need to specify those next.

## Enumerations

Enumerations are like a special kind of model that generates an enumeration instead of a class.

You can add them to the designer using the Add | Enumeration context menu.

> Unlike models, enumerations cannot be used as parameters or results in a service operation. They can only be used to compose complex models.

## Model Properties

Any microservice can have three kinds of properties - Scalar, Enumeration or Model.

To add a property to a model you need to select it in the designer and the then choose one of the following context menu options:

- Add Scalar Property
- Add Enumeration Property
- Add Model Property

> These menu options will only show if a model is selected in the designer.

### Scalar Properties

A scalar property allows storing data "contained" the model. They can be of the following types:

- `Boolean`
- `ByteArray`
- `DateTime`
- `Double`
- `Email`
- `Guid`
- `Integer`
- `Long`
- `Password`
- `PhoneNumber`
- `String`
- `Url`

### Enumeration Properties

As the name implies, an enumeration property allows referencing an enumeration previously added to the designer.

After you create a property of this kind, you will need to select the correct enumeration in the `Referenced Enumeration` property.

### Model Properties

This kind of property allows referencing another (different) model previously created.

After you add the property, you will need to select the correct model in the `Referenced Model` property.

### Default Values

Model properties - depending on their kind and their type - can have default values. These default values will be set when a new instance of the model class is instantiated.

The default value is set in the "Default Value" property and should be set as a code string, meaning that if, for example, the property is of type `String` the value should be set as string (e.g.: "value"), including the quotation marks.

### Read-only Properties

You can also set a property to be read-only to indicate that any value set by the Web API consumer will have no effect.

> Due to serialization limitations, read-only properties will be generated with a getter and a setter. The setter is not supposed to be used by consumers as the service will ignore the values set.

### Lists

A property can either be simple (e.g. String or ModelX) or a list (e.g. List of String or List of ModelX).

You specify that a property should be a list by setting the property `Is List` to true in the properties window.

### Validation

You can also specify validations that should be performed on the values set on model properties.

For more information, see [how to define validation rules on microservices models and actions parameters](./howto-define-validation-rules.md).

## Inheritance

Depending on the microservice complexity and the complexity of the models defined, it may be desirable to set inheritance between models.

This allows, for example, to define a set of properties in a base model to be shared among multiple other models.

First you need to create the base model as you would do for any other model. Add the model and then add its properties.

Finally you set that model as the base model of the other models by selecting it in the `Base Model` property.

> By default, base models are generated as all other models and the consumer will be allowed to create instances of that base models. We can avoid that by making the base models abstract. Just set the `Inheritance Modifier` property of the base model to `Abstract`.

## Generated Code

The code generated for all the models in a microservice is generated in the `Models.gen.cs` file under the `Models` project.

For each model in the designer, two classes will be generated. One is the base class - with all the properties specified. The other is the model class itself, empty, to allow overriding the generated code.

Here is an example:

```csharp
/// <summary>
/// Defines the base class for <see cref="CertificateData"/>.
/// </summary>
[GeneratedCode("Lithium", "2.0")]
public abstract partial class CertificateDataBase
{
    #region Code

    #region Public Properties

    /// <summary>
    /// The certificate data.
    /// </summary>
    [Display(Name = nameof(ModelsResources.RES_Display_CertificateData_Data), ResourceType = typeof(ModelsResources))]
    [JsonPropertyName("data")]
    [Required(AllowEmptyStrings = false, ErrorMessageResourceName = nameof(ModelsResources.RES_Validation_CertificateData_Data_Required), ErrorMessageResourceType = typeof(ModelsResources))]
    public virtual byte[] Data
    {
        get;
        set;
    }

    /// <summary>
    /// A value indicating whether the certificate has a private key.
    /// </summary>
    [Display(Name = nameof(ModelsResources.RES_Display_CertificateData_HasPrivateKey), ResourceType = typeof(ModelsResources))]
    [JsonPropertyName("hasPrivateKey")]
    public virtual bool HasPrivateKey
    {
        get;
        set;
    }

    /// <summary>
    /// The certificate identifier.
    /// </summary>
    [Display(Name = nameof(ModelsResources.RES_Display_CertificateData_Identifier), ResourceType = typeof(ModelsResources))]
    [JsonPropertyName("identifier")]
    [Required(ErrorMessageResourceName = nameof(ModelsResources.RES_Validation_CertificateData_Identifier_Required), ErrorMessageResourceType = typeof(ModelsResources))]
    public virtual Primavera.Lithium.Certificates.Models.CertificateIdentifierData Identifier
    {
        get;
        set;
    }

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

    #endregion

    #region Protected Constructors

    /// <summary>
    /// Initializes a new instance of the <see cref="CertificateDataBase"/> class.
    /// </summary>
    protected CertificateDataBase()        
    {
        // Initialize list properties

        this.Properties = new List<Primavera.Lithium.Certificates.Models.PropertyData>();

        // Initialize default values

        this.SetDefaultValues();
    }

    #endregion

    #region Protected Methods

    /// <summary>
    /// Sets the default values.
    /// </summary>
    /// <remarks>
    /// This method should be overridden to set the default values of properties.
    /// </remarks>
    protected virtual void SetDefaultValues()
    {
    }

    #endregion

    #endregion
}
```

```csharp
/// <summary>
/// Describes a certificate.
/// </summary>
[GeneratedCode("Lithium", "2.0")]
public partial class CertificateData : CertificateDataBase
{
    #region Code

    #region Public Constructors

    /// <summary>
    /// Initializes a new instance of the <see cref="CertificateData"/> class.
    /// </summary>
    public CertificateData()        
        : base()
    {
    }

    #endregion

    #endregion
}
```

## Custom Code

You can customize the behavior of any model by:

- Adding a partial class to include additional properties, methods, etc..
- Adding a partial class and overriding the generated code (to set up additional default values for example).

You can also define models entirely in custom code. The only limitation of these models is that they will not be recognized by the service model, so you will not be able to reference them in that model (to use in service actions for example).

> If you add additional properties to a model in custom code, you should set the `Custom Properties` property in the designer.

> If you add custom models in custom code, you should set the `Custom Models` property in the designer.

> If you add custom enumerations in custom code, you should set the `Custom Enumerations` property in the designer.