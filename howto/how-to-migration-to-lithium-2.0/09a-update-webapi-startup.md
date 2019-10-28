# How to Upgrade a Microservice from Lithium v1.0 to Lithium v2.0

## 9a - Update Startup Custom Code

### 9a.1 - `UseStaticFiles()`

Remove any override of `Startup.UseStaticFiles()` if that override was only intended to configure the MVC default route (`app.UseMvcWithDefaultRoute()`).

That behavior is already added in the generated code.

If an override is needed nevertheless, the signature should be as follows:

```csharp
protected override void UseStaticFiles(IApplicationBuilder app, HostConfiguration env)
```

### 9a.2 - `UseErrorHandling()`

Remove any override of `Startup.UseErrorHandling()` if that override was only intended to add behavior like status code re-execute (`builder.UseStatusCodePagesWithReExecute()`).

That behavior is already added in the generated code.

If an override is needed nevertheless, the signature should be as follows:

```csharp
protected override void UseErrorHandling(IApplicationBuilder app, HostConfiguration hostConfiguration)
```

### 9a.3 - `ValidateConfiguration()`

Change the signature of the override of `Startup.ValidateConfiguration()` to the following:

```csharp
protected override void ValidateConfiguration(IApplicationBuilder app, HostConfiguration hostConfiguration)
```

### 9a.4 - `AddAuthentication()`

If you have overridden `AddAuthentication()` to setup OICD, make sure that override is similar to this:

```csharp
/// <inheritdoc />
protected override void AddAuthentication(IServiceCollection services)
{
    // Add authentication

    AuthenticationBuilder builder = services
        .AddAuthentication(
            (options) =>
            {
                options.DefaultScheme = OidcConstants.AuthenticationSchemes.Cookies;
                options.DefaultChallengeScheme = OidcConstants.AuthenticationSchemes.Oidc;
            });

    // Add cookies

    builder
        .AddCookie(
            (options) =>
            {
                options.AccessDeniedPath = Constants.Controllers.Home.Routes.AccessDenied;
            });

    builder
        .AddOpenIdConnect(
            OidcConstants.AuthenticationSchemes.Oidc,
            (options) =>
            {
                options.SignInScheme = Config.Constants.Authentication.Cookies;
                options.Authority = builder.Services.GetHostConfiguration().IdentityServerBaseUri?.Trim();
                options.RequireHttpsMetadata = false;
                options.ClientId = Constants.Credentials.Hybrid.ClientId;
                options.ClientSecret = Constants.Credentials.Hybrid.ClientSecret;
                options.ResponseType = OpenIdConnectResponseType.CodeIdTokenToken;
                options.SaveTokens = true;
                options.GetClaimsFromUserInfoEndpoint = true;

                options.Scope.Add(<Your-scope-here>);
                options.Scope.Add(JwtClaimTypes.Email);
                options.Scope.Add(JwtClaimTypes.Profile);

                options.TokenValidationParameters = new TokenValidationParameters
                {
                    NameClaimType = "name",
                    RoleClaimType = "role"
                };

                options.Events = new OpenIdConnectEvents()
                {
                    OnRedirectToIdentityProvider = this.OnRedirectToIdentityProvider,
                    OnAuthorizationCodeReceived = this.OnAuthorizationCodeReceived,
                    OnTokenResponseReceived = this.OnTokenResponseReceived,
                    OnUserInformationReceived = this.OnUserInformationReceived,
                };
            });

    // Add JWT Bearer

    this.AddJwtBearer(services, builder);
}
```

## Next

> [9b - Update Controllers](./09b-update-webapi-controllers.md)