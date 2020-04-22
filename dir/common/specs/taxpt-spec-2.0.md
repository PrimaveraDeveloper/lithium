# Tax Authority (Portugal) Service (TAXPT) Specification v2.0

The Tax Authority (Portugal) Service allows integration with the Portuguese Tax Authority Web services.

The current version supports the following Tax Authority services:

- Invoices
- Dispatches

## Invoices

The TAXPT service provides the following operations to communicate invoices:

- `GetInvoiceXmlAsync()`: Allows retrieving the XML that should be used to communicate an invoice.
- `SendInvoiceAsync()`: Allows communicating an invoice by providing the XML retrieved with `GetInvoiceXmlAsync()`.
- `RegisterInvoiceAsync()`: Allows communicating an invoice in a single step.

> The service allows communicating invoices in a single step (`Register`) or in two steps (`GetXml` + `Send`). The later is provided to support scenarios where client applications may want to store the XML for diagnosis or may want to implement some kind of retry policies.

### Registering Invoices

An invoice can be registered as follows:

```csharp
InvoiceData invoice = (...);

ServiceOperationResult<InvoiceRegistrationResult> registerResult = await this.Client
    .Invoices
    .RegisterInvoiceAsync(invoice)
    .ConfigureAwait(false);

InvoiceRegistrationResult result = registerResult.Body;
if (result.Succeeded)
{
    // (...)
}
```

## Dispatches

The TAXPT service provides the following operations to communicate dispatches:

- `GetDispatchXmlAsync()`: Allows retrieving the XML that should be used to communicate a dispatch.
- `SendDispatchAsync()`: Allows communicating a dispatch by providing the XML retrieved with `GetDispatchXmlAsync()`.
- `RegisterDispatchAsync()`: Allows communicating a dispatch in a single step.

> As with invoices, the service allows also allows to communicate dispatches in a single step (Register) or in two steps (GetXml + Send).

### Registering Dispatches

The XML for communicating a dispatch can first be retrieved and then passed in to the service for sending, like in the following example:

```csharp
DispatchData dispatch = (...);

ServiceOperationResult<string> getXmlResult = await this.Client
    .Dispatches
    .GetDispatchXmlAsync(dispatch)
    .ConfigureAwait(false);

string xml = getXmlResult.Body;

// (...)

ServiceOperationResult<DispatchRegistrationResult> registerResult = await this.Client
    .Dispatches
    .SendDispatchAsync(xml)
    .ConfigureAwait(false);

DispatchRegistrationResult result = registerResult.Body;
if (result.Succeeded)
{
    // (...)
}
```

## Certificates

The service uses the following certificates, retrieved from the Certificates Service:

- `TaxServicePTPublicKey`: The Tax Authority public key, used to encrypt data.
- `TaxServicePTPRIMAVERAPrivateKey`: PRIMAVERA's private key, used to protect the SSL communication with the Web services' production environment.
- `TaxServicePTTestPrivateKey`: The testing private key, used to protect the SSL communication with the Web services' testing environment.

> All these certificates are provided by the Portuguese Tax Authority.

## Brokered Services

The Web services provided by the Tax Authority are normal SOAP (WCF) services that are brokered in the TAXPT service using a service proxy.

These Web services require:

- A special authentication process (for the sender and for the software publisher) that implies the use of HTTPS in the communication (hence the use of certificates).
- The payload to partially encrypted.
- Some headers to be included in the requests to identify the user performing them, the request date and a nonce (used to share the encryption keys).

That is why the Web services are invoked by a service proxy that uses a customized version of Basic HTTP Binding.

The invoices Web service is invoked using the `InvoicesServiceProxy` proxy:

```csharp
using (InvoicesServiceProxy proxy = await this.GetServiceProxyAsync(
    envelope.Header.Security.UsernameToken, 
    cancellationToken)
    .ConfigureAwait(false))
{
    // (...)

    RegisterInvoiceResponse response = await proxy
        .RegisterInvoiceAsync(request)
        .ConfigureAwait(false);

    // (...)
}
```

The dispatches Web services is invoked using the `DispatchesServiceProxy` proxy:

```csharp
using (DispatchesServiceProxy proxy = await this.GetServiceProxyAsync(
    envelope.Header.Security.UsernameToken, 
    cancellationToken)
    .ConfigureAwait(false))
{
    // (...)

    RegisterDispatchResponse response = await proxy
        .RegisterDispatchAsync(request)
        .ConfigureAwait(false);

    // (...)
}
```

The proxies are created like in the following example:

```csharp
private async Task<InvoicesServiceProxy> GetServiceProxyAsync(
    SoapHeaderSecurityUsernameToken userNameToken, 
    CancellationToken cancellationToken)
{
    // Create the context

    ServiceProxyContext context = new ServiceProxyContext()
    {
        UserName = userNameToken.Username,
        Password = userNameToken.Password,
        Nonce = userNameToken.Nonce,
        Created = userNameToken.Created,
        Proxy = new ProxySettings()
        {
            Address = this.Client.Configuration.Proxy?.Address == null ? null : new Uri(this.Client.Configuration.Proxy.Address),
            UserName = this.Client.Configuration.Proxy?.UserName,
            Password = this.Client.Configuration.Proxy?.Password,
            UserDomain = this.Client.Configuration.Proxy?.UserDomain
        }
    };

    // Create service proxy

    InvoicesServiceProxy result;

    if (this.Client.Configuration.Endpoints.UseTestingEndpoints)
    {
        // Set certificates

        X509Certificate2 httpCertificate = await this.CertificatesManager
            .GetCertificateAsync(
                CertificateKind.TestingEnvironment, 
                cancellationToken)
            .ConfigureAwait(false);

        X509Certificate2 publicKeyCertificate = await this.CertificatesManager
            .GetCertificateAsync(
                CertificateKind.TaxAuthorityPublicKey, 
                cancellationToken)
            .ConfigureAwait(false);

        context.Certificates = new CertificatesSettings()
        {
            HttpsCertificate = httpCertificate,
            TaxAuthorityPublicKey = publicKeyCertificate
        };

        // Create proxy

        result = new InvoicesServiceProxy(
            this.Client.ServiceProvider,
            new Uri(this.Client.Configuration.Endpoints.InvoicesTestingUri),
            context);
    }
    else
    {
        // Set certificates

        X509Certificate2 httpCertificate = await this.CertificatesManager
            .GetCertificateAsync(
                CertificateKind.PublisherPrivateKey,
                cancellationToken)
            .ConfigureAwait(false);

        X509Certificate2 publicKeyCertificate = await this.CertificatesManager
            .GetCertificateAsync(
                CertificateKind.TaxAuthorityPublicKey,
                cancellationToken)
            .ConfigureAwait(false);

        context.Certificates = new CertificatesSettings()
        {
            HttpsCertificate = httpCertificate,
            TaxAuthorityPublicKey = publicKeyCertificate
        };

        // Create proxy

        result = new InvoicesServiceProxy(
            this.Client.ServiceProvider,
            new Uri(this.Client.Configuration.Endpoints.InvoicesProductionUri),
            context);
    }

    // Result

    return result;
}
```

## More Information

<!-- markdown-link-check-disable -->
Additional technical details about the service are available [here](https://tfs.primaverabss.com/tfs/P.TEC.Elevation/Lithium/_versionControl?path=%24%2FLithium%2FMicroservices%2FCommon%2FTAXPT%2FMainline%2F.doc%2FTAXPT-Reference.md&_a=preview) (TFS).
<!-- markdown-link-check-enable -->