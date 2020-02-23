# Primavera.Hydrogen.IdentityModel.Client

**Class library that contains core types, helpers and client libraries for OpenID Connect, OAuth and claims-based Identity.**

## DiscoveryClient

`DiscoveryClient` allows retrieving discovery documents from authorization servers.

```csharp
using (IDiscoveryClient client = new DiscoveryClient())
{
    DiscoveryDocumentResponse response = await client.GetDocumentAsync("https://server.io").ConfigureAwait(false);
    if (!response.IsError)
    {
        string tokenEndpoint = response.TokenEndpoint;
    }
}
```

### Addresses

`DiscoveryClient` allows passing in the authority server base address or the discovery endpoint address directly. In either case it will
retrieve the discovery document correctly.

```csharp
client.GetDocumentAsync("https://server.io").ConfigureAwait(false);
client.GetDocumentAsync("https://server.io/.well-known/openid-configuration").ConfigureAwait(false);
```

### Discovery Policy

By default the discovery client applies a policy (see `DiscoveryPolicy`) to validate the request and the response to the endpoint.
This can be customized in the request:

```csharp
using (IDiscoveryClient client = new DiscoveryClient())
{
    DiscoveryDocumentResponse response = await client.GetDocumentAsync(
        new DiscoveryDocumentRequest("https://server.io")
        {
            Policy = new MyCustomPolicy(),
        }).ConfigureAwait(false);
    if (!response.IsError)
    {
        string tokenEndpoint = response.TokenEndpoint;
    }
}
```

### Caching

Responses from each distinct discovery endpoint are cached in memory for a default duration of one day.

The cache used can also be customized:

```csharp
using (IDiscoveryClient client = new DiscoveryClient())
{
    DiscoveryDocumentResponse response = await client.GetDocumentAsync(
        new DiscoveryDocumentRequest("https://server.io")
        {
            Cache = new MyCustomCache(),
        }).ConfigureAwait(false);
    if (!response.IsError)
    {
        string tokenEndpoint = response.TokenEndpoint;
    }
}
```

## TokenClient

`TokenClient` allows retrieving tokens from authorization servers.

```csharp
using (TokenClient client = new TokenClient())
{
    TokenClientOptions options = new TokenClientOptions()
    {
        ClientId = "myclientid",
        ClientSecret = "myclientsecret"
    };

    TokenResponse response = await client.GetClientCredentialsAsync(
        "https://server.io/connect/token", 
        options, 
        scope)
        .ConfigureAwait(false);
    
    if (!response.IsError)
    {
        string tokenEndpoint = response.TokenEndpoint;
    }
}
```

### TokenClientOptions

The type `TokenClientOptions` allows settings the custom properties for each request to the token endpoint.

Typically the grant type requested is inferred from the method used to make the request or from the request itself (when using `GetTokenAsync()`).

Depending on the grant type requested, other properties - like the client identifier - may need to be set using `TokenClientOptions`.

### Addresses

`TokenClient` allows passing in either the token endpoint full address or the authority server base address.

If the address provided in the request is the authority server, then the token client will use `DiscoveryClient` to determine the actual token endpoint address.

```csharp
client.GetClientCredentialsTokenAsync(
    "https://server.io", 
    options)
    .ConfigureAwait(false);

client.GetClientCredentialsTokenAsync(
    "https://server.io/connect/token", 
    options)
    .ConfigureAwait(false);

client.GetTokenAsync(
    new ClientCredentialsTokenRequest("https://server.io")
    {
        // ...
    })
    .ConfigureAwait(false);

client.GetTokenAsync(
    new ClientCredentialsTokenRequest("https://server.io/connect/token")
    {
        // ...
    })
    .ConfigureAwait(false);
```

There are cases where will want to send an address in the request that is the authority server base but than token client will not be able
to detect as such because it is not a normal base URL (e.g.: https://login.windows.net/ab96bea5-d5d5-4cb2-a841-5eda0da9ada1).

`TokenClient` allows you to indicate these scenarios both in `TokenClientOptions` and in `TokenRequest`:

```csharp
TokenClientOptions options = new TokenClientOptions()
{
    // ...
    AddressIsAuthority = true
}

TokenRequest request = new TokenRequest()
{
    // ...
    AddressIsAuthority = true
}
```

### Requests

The following types model requests to `ITokenClient`:

#### `ClientCredentialsTokenRequest`

`ClientCredentialsTokenRequest` performs a request to obtain an access token for the client credentials grant type.

Typically the following values will be required:

- `ClientId`
- `ClientSecret`
- `Scope` (optional)

#### `AzureClientCredentialsTokenRequest`

`AzureClientCredentialsTokenRequest` is basically the same as `ClientCredentialsTokenRequest` but is designed to be used against
an Azure Active Directory authority server because it allows setting the Resource parameter directly (usually required
by Azure AD).

Typically the following values will be required:

- `ClientId`
- `ClientSecret`
- `Resource` (optional)

#### `AuthorizationCodeTokenRequest`

`AuthorizationCodeTokenRequest` performs a request to obtain an access token for the authorization code grant type.

Typically the following values will be required:

- `ClientId`
- `ClientSecret`
- `Code`
- `RedirectUri`
- `CodeVerifier` (optional)

#### `PasswordTokenRequest`

`PasswordTokenRequest` performs a request to obtain an access token for the password grant type.

Typically the following values will be required:

- `ClientId`
- `ClientSecret`
- `UserName`
- `Password`
- `Scope` (optional)

#### `RefreshTokenRequest`

`RefreshTokenRequest` performs a request to obtain an access token for the refresh token grant type.

Typically the following values will be required:

- `ClientId`
- `ClientSecret`
- `RefreshToken`
- `Scope` (optional)

### Custom Requests

It is also possible to perform custom requests (requests where all the parameters are set by the caller) using the `GetTokenAsync()`:

Example:

```csharp
using (TokenClient client = new TokenClient())
{
    TokenRequest request = new TokenRequest("https://server.io/connect/token")
    {
        GrantType = "delegation",
        ClientId = "myclientid",
        ClientSecret = "myclientsecret"
        Parameters = new Dictionary<string, string>()
        {
            ["scope"] = "myscope",
            ["token"] = (...),
        }
    };

    TokenResponse response = await client.GetTokenAsync(
        tokenRequest)
        .ConfigureAwait(false);
    
    (...)
}
```

### Discovery Policy

You can also customize the discovery policy used by `DiscoveryClient` when called from `TokenClient` to resolve the token endpoint.

```csharp
TokenRequest request = new TokenRequest()
{
    // ...
    DiscoveryPolicy = new MyDiscoveryPolicy()
}
```

> If `TokenRequest.DiscoveryPolicy` is set to null, then the default discovery policy will be used by `DiscoveryClient`.

### Caching

Responses from each distinct endpoint are cached in memory for a default duration of 12 hours or until the token itself expires.

The cache used can be customized:

```csharp
using (ITokenClient client = new TokenClient())
{
    TokenResponse response = await client.GetDocumentAsync(
        new ClientCredentialsTokenRequest("https://server.io")
        {
            // ...
            Cache = new MyCustomCache()
        }).ConfigureAwait(false);
}
```
