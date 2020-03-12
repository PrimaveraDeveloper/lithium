# Capability - Client Libraries

> Tags: Client Libraries, C#, HttpClient, Web API

The Lithium SDK supports generating a client library in C# for the microservice Web API.

> Currently it is only generated a C# client library. In the future other programming languages may be supported. In the meantime, specific client libraries can also be generated from the Web API documentation that is automatically generated.

The client library sets up a service client (see [Primavera.Hydrogen.Rest.Client](../ref/hydrogen-2.0/Rest.Client.md)) that acts as the entry point for the API endpoints and deals with multiple features that standardize how these endpoints are invoked. Things like:

- Routing
- Serialization
- Authorization
- Localization
- Retry policies
- Etc.

The client applications can simply reference these libraries - single assemblies with no dependencies - to invoke the Web API, like in the following example:

```csharp
using (SettingsClient client = new SettingsClient(
    baseUri,
    new AuthenticationCallbackCredentials(
        async (args) =>
        {
            string accessToken = ClientCredentials
                .ForAllScopes(
                    args.Authority,
                    clientId,
                    clientSecret);
            return accessToken;
        }))
{
    try
    {
        ProductSettingData setting = new ProductSettingData()
        {
            (...)
        };
        
        ServiceOperationResult result = await client
            .ProductSettings
                .SaveProductSettingAsync(
                    setting)
            .ConfigureAwait(false);
    }
    catch (ServiceException ex)
    {
        (...)
    }
}
```