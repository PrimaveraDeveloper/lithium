# Capability - Localization

> Tags: Globalization, Cultures, Accept-Language

Localization refers to the capability of presenting the user with pages, messages, errors in the correct language and formatting values according to the user culture.

In the Lithium Framework, this is supported via two related features:

- Request localization on the server, ensuring that data is produced in the culture associated with the request (thus the client application and/or the user).
- Request localization on the client, ensuring that the client application (the user in the end) has a mechanism to specify the request culture (the `Accept-Language` header for example).

## Accept-Language

Lithium microservice recognize this header and set the request culture accordingly (see below), being it set by the browser (when using the implicit or the hybrid grants) or by the client library.

On the client library, this header should be set by the caller, using the `AcceptLanguage` property:

```csharp
using SettingsClient client = new SettingsClient(...);
client.AcceptLanguage = "pt-PT";
(...)
```
> Notice that this is property is global to the service client (affects all subsequent calls), but you can set it multiple times.

## Culture Query String Provider

The middleware that runs on the server, also supports specifying the culture in the request path by using query string parameters `culture` and/or `ui-culture`:

```
http://localhost:5000/api/v1/myendpoint?culture=es-MX&ui-culture=es-MX
```

> This cannot be used with the client library, only if making requests from the browser and/or directly to the Web API endpoints.

## Request Localization

The microservice app server sets up request localization using a specific middleware, in the application startup.

`StartupBase.UseRequestLocalization()`:

```csharp
protected virtual void UseRequestLocalization(IApplicationBuilder app, HostConfiguration hostConfiguration)
{
    // Validation

    SmartGuard.NotNull(() => app, app);
    SmartGuard.NotNull(() => hostConfiguration, hostConfiguration);

    // Logging

    this.Logger.LogDebug($"Activating request localization...");

    // Options

    RequestLocalizationOptions options = new RequestLocalizationOptions()
    {
        DefaultRequestCulture = new RequestCulture(Constants.Localization.DefaultCulture),
        SupportedCultures = Constants.Localization.SupportedCulturesExtended,
        SupportedUICultures = Constants.Localization.SupportedCulturesExtended
    };

    // Add middleware

    app.UseRequestLocalization(options);
}
```

### Supported Cultures

Presently, the following cultures are supported (the fallback is `en-US`):

```csharp
internal const string PortugueseDefaultCultureString = "pt";
internal const string PortugueseFromAngolaCultureString = "pt-AO";
internal const string PortugueseFromBrazilCultureString = "pt-BR";
internal const string PortugueseFromCapeVerdeCultureString = "pt-CV";
internal const string PortugueseFromGuineaBissauCultureString = "pt-GW";
internal const string PortugueseFromMozambiqueCultureString = "pt-MZ";
internal const string PortugueseFromPortugalCultureString = "pt-PT";
internal const string PortugueseFromSaoTomeAndPrincipeCultureString = "pt-ST";
internal const string PortugueseFromTimorLesteCultureString = "pt-TL";
internal const string EnglishDefaultCultureString = "en";
internal const string EnglishFromIrelandCultureString = "en-IE";
internal const string EnglishFromGreatBritainCultureString = "en-GB";
internal const string EnglishFromUnitedStatesCultureString = "en-US";
internal const string SpanishDefaultCultureString = "es";
internal const string SpanishFromArgentinaCultureString = "es-AR";
internal const string SpanishFromChileCultureString = "es-CL";
internal const string SpanishFromMexicoCultureString = "es-MX";
internal const string SpanishFromSpainCultureString = "es-ES";
```

> These are also the recognized values for the `Accept-Language` header value.