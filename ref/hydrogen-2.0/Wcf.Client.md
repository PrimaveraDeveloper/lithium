# Primavera.Hydrogen.Wcf.Client

**Class library that contains types that allow communication with WCF Web services.**

## `ServiceProxyBase`

`ServiceProxyBase` is a base class that should be used when implementing a proxy for a WCF Web Service.

It provides the following standard behaviors:

- `IDisposable`.
- Service provider management.
- Telemetry client.

To implement a proxy, first you need to define the contract of the WCF Web service as an interface, like in the following example:

```csharp
/// <summary>
/// Defines the interface of a proxy that allows calling the VIES Check VAT Web Service.
/// </summary>
internal partial interface IViesCheckVatServiceProxy : IDisposable
{
    #region Methods

    /// <summary>
    /// Checks if the specified VAT number is valid.
    /// If the VAT number is valid and additional information about the company is available (e.g. name) it will
    /// also be returned in the result.
    /// </summary>
    /// <param name="countryCode">The country code of the VAT number that should be checked.</param>
    /// <param name="vatNumber">The VAT number that should be checked.</param>
    /// <returns>
    /// Information about the VAT number.
    /// </returns>
    ViesCheckVatResult CheckVatNumber(string countryCode, string vatNumber);

    /// <summary>
    /// Checks if the specified VAT number is valid.
    /// If the VAT number is valid and additional information about the company is available (e.g. name) it will
    /// also be returned in the result.
    /// </summary>
    /// <param name="countryCode">The country code of the VAT number that should be checked.</param>
    /// <param name="vatNumber">The VAT number that should be checked.</param>
    /// <returns>
    /// The <see cref="Task{TResult}"/> that represents the asynchronous operation.
    /// Information about the VAT number.
    /// </returns>
    Task<ViesCheckVatResult> CheckVatNumberAsync(string countryCode, string vatNumber);

    #endregion
}
```

Then the proxy itself should be derived from `ServiceProxyBase`, like this:

```csharp
/// <summary>
/// Provides a proxy that allows calling the VIES Check VAT Web Service.
/// </summary>
/// <seealso cref="ServiceProxyBase" />
/// <seealso cref="IViesCheckVatServiceProxy" />
[SuppressMessage("Microsoft.Performance", "CA1812:AvoidUninstantiatedInternalClasses", Justification = "Used with InternalsVisible.")]
internal sealed partial class ViesCheckVatServiceProxy : ServiceProxyBase, IViesCheckVatServiceProxy
{
    #region Internal Properties

    /// <summary>
    /// Gets the underlying client proxy.
    /// </summary>
    /// <remarks>
    /// This property is internal to allow unit testing.
    /// </remarks>
    internal IServiceProxy<IViesCheckVatService> ClientProxy
    {
        get;
        private set;
    }

    #endregion

    #region Constructors

    /// <summary>
    /// Initializes a new instance of the <see cref="ViesCheckVatServiceProxy" /> class.
    /// </summary>
    /// <param name="serviceProvider">The service provider.</param>
    /// <param name="endpointUrl">The URL of the service endpoint.</param>
    public ViesCheckVatServiceProxy(IServiceProvider serviceProvider, Uri endpointUrl)
        : base(serviceProvider)
    {
        // Validation

        SmartGuard.NotNull(() => endpointUrl, endpointUrl);

        // Initialize

        this.Initialize(endpointUrl);
    }

    #endregion

    #region Public Methods

    #region CheckVat

    /// <summary>
    /// Checks if the specified VAT number is valid.
    /// If the VAT number is valid and additional information about the company is available (e.g. name) it will
    /// also be returned in the result.
    /// </summary>
    /// <param name="countryCode">The country code of the VAT number that should be checked.</param>
    /// <param name="vatNumber">The VAT number that should be checked.</param>
    /// <returns>
    /// Information about the VAT number.
    /// </returns>
    public ViesCheckVatResult CheckVatNumber(string countryCode, string vatNumber)
    {
        // Validation

        SmartGuard.NotNullOrEmpty(() => countryCode, countryCode);
        SmartGuard.NotNullOrEmpty(() => vatNumber, vatNumber);

        // Build request message

        ViesCheckVatRequest request = new ViesCheckVatRequest(new ViesCheckVatRequestBody(countryCode, vatNumber));

        // Call Web service

        ViesCheckVatResponse response = this.ClientProxy.Execute(proxy => proxy.CheckVat(request));

        // Result

        return new ViesCheckVatResult(response);
    }

    /// <summary>
    /// Checks if the specified VAT number is valid.
    /// If the VAT number is valid and additional information about the company is available (e.g. name) it will
    /// also be returned in the result.
    /// </summary>
    /// <param name="countryCode">The country code of the VAT number that should be checked.</param>
    /// <param name="vatNumber">The VAT number that should be checked.</param>
    /// <returns>
    /// The <see cref="Task{TResult}"/> that represents the asynchronous operation.
    /// Information about the VAT number.
    /// </returns>
    public async Task<ViesCheckVatResult> CheckVatNumberAsync(string countryCode, string vatNumber)
    {
        // Validation

        SmartGuard.NotNullOrEmpty(() => countryCode, countryCode);
        SmartGuard.NotNullOrEmpty(() => vatNumber, vatNumber);

        // Build request message

        ViesCheckVatRequest request = new ViesCheckVatRequest(new ViesCheckVatRequestBody(countryCode, vatNumber));

        // Call Web service

        ViesCheckVatResponse response = await this.ClientProxy.Execute(proxy => proxy.CheckVatAsync(request)).ConfigureAwait(false);

        // Result

        return new ViesCheckVatResult(response);
    }

    #endregion

    #endregion

    #region Protected Methods

    /// <summary>
    /// Releases unmanaged and - optionally - managed resources.
    /// </summary>
    /// <param name="disposing"><c>True</c> to release both managed and unmanaged resources; <c>false</c> to release only unmanaged resources.</param>
    protected override void Dispose(bool disposing)
    {
        // Called from Dispose()?

        if (disposing)
        {
            if (this.ClientProxy != null)
            {
                this.ClientProxy.Dispose();
                this.ClientProxy = null;
            }
        }

        // Default behavior

        base.Dispose(disposing);
    }

    #endregion

    #region Private Methods

    private void Initialize(Uri endpointUrl)
    {
        this.ClientProxy = new BasicHttpServiceProxy<IViesCheckVatServiceChannel>(endpointUrl);
    }

    #endregion
}
```

Notice `IViesCheckVatServiceChannel`, which is the WCF service operation contract:

```csharp
/// <summary>
/// Defines the interface of the client channel that can be used to communicate with
/// the VIES Check VAT Web service.
/// </summary>
/// <seealso cref="IViesCheckVatService" />
/// <seealso cref="IClientChannel" />
[GeneratedCode("System.ServiceModel", "4.0.0.0")]
internal partial interface IViesCheckVatServiceChannel : IViesCheckVatService, IClientChannel
{
}
```

```csharp
/// <summary>
/// Defines the interface of the VIES Check VAT Web service.
/// </summary>
[ServiceContract(Namespace = "urn:ec.europa.eu:taxud:vies:services:checkVat", ConfigurationName = "Primavera.Lithium.DataLookup.BrokeredServices.Contracts.IViesCheckVatService")]
internal partial interface IViesCheckVatService
{
    #region Methods

    /// <summary>
    /// Checks if a VAT number is valid.
    /// If the VAT number is valid and additional information about the company is available (e.g. name) it will
    /// also be returned in the response.
    /// </summary>
    /// <param name="request">The request message.</param>
    /// <returns>
    /// The response message.
    /// </returns>
    [OperationContract(Action = "", ReplyAction = "*")]
    ViesCheckVatResponse CheckVat(ViesCheckVatRequest request);

    /// <summary>
    /// Checks if a VAT number is valid.
    /// If the VAT number is valid and additional information about the company is available (e.g. name) it will
    /// also be returned in the response.
    /// </summary>
    /// <param name="request">The request message.</param>
    /// <returns>
    /// The <see cref="Task{TResult}"/> that represents the asynchronous operation.
    /// The response message.
    /// </returns>
    [OperationContract(Action = "", ReplyAction = "*")]
    Task<ViesCheckVatResponse> CheckVatAsync(ViesCheckVatRequest request);

    #endregion
}
```

Each message needs to be defined in a message contract, like in the following example:

```csharp
/// <summary>
/// Defines the message contract that represents the request of the <see cref="IViesCheckVatService.CheckVat(ViesCheckVatRequest)"/>
/// operation.
/// </summary>
[MessageContract(IsWrapped = false)]
[GeneratedCode("System.ServiceModel", "4.0.0.0")]
internal partial class ViesCheckVatRequest
{
    #region Public Fields

    /// <summary>
    /// The message body.
    /// </summary>
    [MessageBodyMember(Name = "checkVat", Namespace = "urn:ec.europa.eu:taxud:vies:services:checkVat:types", Order = 0)]
    public ViesCheckVatRequestBody Body;

    #endregion

    #region Constructors

    /// <summary>
    /// Initializes a new instance of the <see cref="ViesCheckVatRequest"/> class.
    /// </summary>
    public ViesCheckVatRequest()
    {
    }

    /// <summary>
    /// Initializes a new instance of the <see cref="ViesCheckVatRequest"/> class.
    /// </summary>
    /// <param name="Body">The message body.</param>
    public ViesCheckVatRequest(ViesCheckVatRequestBody Body)
    {
        this.Body = Body;
    }

    #endregion
}
```

## `IServiceProxy`

This is the most basic interface that a WCF service proxy should implement. It only specifies that the service is disposable and includes actions to execute its operations (`Execute()`).

## `BasicHttpServiceProxy`

`BasicHttpServiceProxy` is a an implementation of `IServiceProxy` that configures itself to use the [Basic HTTP binding](https://docs.microsoft.com/en-us/dotnet/framework/configure-apps/file-schema/wcf/basichttpbinding) to communicate with the Web service.

> Review `Initialize()` method on `ViesCheckVatServiceProxy` (above) to see how the actual proxy for the Web service is created with this binding.