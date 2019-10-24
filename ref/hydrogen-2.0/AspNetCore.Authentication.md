# Primavera.Hydrogen.AspNetCore.Authentication

**Class library that contains types that support authentication of Web applications with ASP.NET Core.**

## `JwtBearer`

The `Primavera.Hydrogen.AspNetCore.Authentication.JwtBearer` includes types and helper methods for JWT Bearer authentication (as used by OAuth and OIDC).

### HTTP Bearer Challenge

According to the [OAuth 2 specification](https://tools.ietf.org/html/rfc6750) when a request to a protected resource does not include authentication credentials, the resource server should return a `WWW-Authenticate` header in the response.

This behavior his leveraged by the `ServiceClient` to support authentication via a callback, allowing the client application to request the necessary access token from the correct authority server without knowing its address upfront.

#### HttpBearerChallengeBuilder

`HttpBearerChallengeBuilder` provides a method to build the `WWW-Authenticate` header correctly when the JWT Bearer is being prepared during authentication.

It should be invoked as follows, from the resource server, when configuring the JWT Bearer middleware:

```csharp
public virtual void ConfigureServices(IServiceCollection services)
{
    // (...)

    services
        .AddAuthentication("Bearer")
        .AddJwtBearer(
            (options) =>
            {
                // (...)

                options.Events = new JwtBearerEvents()
                {
                    OnChallenge = this.OnChallenge
                };
            });

    // (...)
}

private Task OnChallenge(JwtBearerChallengeContext context)
{
    HttpBearerChallengeBuilder.BuildHeader(context);
    return Task.CompletedTask;
}
```

#### HttpBearerChallengeEvents

`HttpBearerChallengeEvents` allows configuring the HTTP Bearer challenge in a different way, simpler:

```csharp
public virtual void ConfigureServices(IServiceCollection services)
{
    // (...)

    services
        .AddAuthentication("Bearer")
        .AddJwtBearer(
            (options) =>
            {
                // (...)

                options.Events = new HttpBearerChallengeEvents()
                {
                    OnAuthenticationFailed = (context) =>
                    {
                        // Runs when authentication fails
                        // (...)
                    },
                    OnForbidden = (context) =>
                    {
                        // Runs when a Forbidden response is received
                        // (...)
                    },
                    OnMessageReceived = (context) =>
                    {
                        // Runs when a message is received
                        // (...)
                    },
                    OnTokenValidated = (context) =>
                    {
                        // Runs when the token is validated
                        // (...)
                    },
                    OnChallenge = (context) =>
                    {
                        // Runs before a challenge is sent to the caller
                        // just after the HTTP builder challenge is constructed with
                        // HttpBearerChallengeBuilder
                        // (...)
                    },
                };
            });

    // (...)
}
```
