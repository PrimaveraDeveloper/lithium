# Primavera.Hydrogen.AspNetCore.Extensions

**Class library that contains types that provide extensions for Web applications developed with ASP.NET Core.**

## Device Detection

The `IDeviceDetection` service allows retrieving detailed information about the user agent (from the `User-Agent` request header).

This service provides the following information:

- `Browser` - information about the client browser (when the client application is a browser).
- `Client` - information about the client.
- `Platform` - information about the platform used by the client.
- `Device` - information about the client device.
- `Bot` - information about the client bot (when the client is a bot).

The service should be registered as follows:

```csharp
using Primavera.Hydrogen.AspNetCore.Extensions.DeviceDetection;

services.AddDeviceDetection();
```

The service can be resolved as any other service but it is also available as an extension method for HttpRequest:

```csharp
using Primavera.Hydrogen.AspNetCore.Extensions.DeviceDetection;

IDeviceDetection service = serviceProvider.GetRequiredService();

IDeviceDetection service = this.Request.DeviceDetection;
```

## reCAPTCHA

[reCAPTCHA](https://www.google.com/recaptcha/) is a Google service that implements CAPTCHA and is available here:

The current implementation supports v3, v2, and v2 invisible.

`Primavera.Hydrogen.AspNetCore.Extensions.Recaptcha.Rendering.RecaptchaHelper` and `Primavera.Hydrogen.AspNetCore.Extensions.Recaptcha.IRecaptchaService` allow implementing reCAPTCHA on any Web site.

### `RecaptchaHelper`

`RecaptchaHelper` allows rendering the client-side code required to support reCAPTCHA (in the form being validated).

The following example shows how reCAPTCHA v3 should be used in a view:

```csharp
@using Primavera.Hydrogen.AspNetCore.Extensions.Recaptcha.Rendering

(...)

@Html.RecaptchaV3("sitekey", "actionname")
```

For reCAPTCHA v2 ("I'm not a robot" checkbox) it would be:

```csharp
@Html.RecaptchaV2("sitekey", "actionname")
```

NOTES:

- `sitekey` should be replaced by the site key configured in the reCAPTCHA admin console.
- `actionname` should be replaced with a unique name for each view.

### `IRecaptchaService`

`IRecaptchaService` allows validating the reCAPTCHA score on the server-side (in the controller action).

This service should be registered at startup as follows:

```csharp
services.AddRecaptcha();
```

The following example shows how reCAPTCHA v3 should be used in a controller:

```csharp
using Primavera.Hydrogen.AspNetCore.Extensions.Recaptcha;

(...)

public async Task<IActionResult> MyAction(MyViewModel model)
{
    (...)

    // Validate reCAPTCHA

    await this.ValidateRecaptchaScoreAsync().ConfigureAwait(false);

    (...)
}

private async Task ValidateRecaptchaScoreAsync()
{
    // Get the response

    RecaptchaSettings settings = new RecaptchaSettings()
    {
        SecretKey = "secretkey",
        UseRemoteIp = true
    };

    RecaptchaResponse recaptcha = await this.RecaptchaService.ValidateAsync(
        settings, 
        this.Request)
        .ConfigureAwait(false);

    // Failed?

    if (!recaptcha.Success)
    {
        // Logging

        this.Logger.LogError($"reCAPTCHA validation failed. Score: {recaptcha.Score}. Action: {recaptcha.Action}.");

        // Add model error

        this.ModelState.AddModelError(nameof(IRecaptchaService), "Spam validation failed.");

        // Return

        return;
    }

    // Score too low?

    if (recaptcha.Score < MinimumScore)
    {
        // Logging

        this.Logger.LogError($"reCAPTCHA score is too low. Score: {recaptcha.Score}. Action: {recaptcha.Action}.");

        // Add model error

        this.ModelState.AddModelError(nameof(IRecaptchaService), "Spam validation score is too low.");

        // Return

        return;
    }
}
```

Since reCAPTCHA v2 does not return a score, the validation would be simpler:

```csharp
private async Task ValidateRecaptchaV2Async()
{
    // Get the response

    RecaptchaSettings settings = new RecaptchaSettings()
    {
        SecretKey = "secretkey",
        UseRemoteIp = true
    };

    RecaptchaResponse recaptcha = await this.RecaptchaService.ValidateAsync(
        settings, 
        this.Request)
        .ConfigureAwait(false);

    // Failed?

    if (!recaptcha.Success)
    {
        // Logging

        this.Logger.LogError($"reCAPTCHA validation failed. Action: {recaptcha.Action}.");

        // Add model error

        this.ModelState.AddModelError(nameof(IRecaptchaService), "Spam validation failed.");

        // Return

        return;
    }
}
```

NOTES:

- `secretkey` should be replaced by the secret key configured in the reCAPTCHA admin console.
- `MinimumScore` should be set to a value between 0.0 and 1.0. Google recommends starting with 0.5 and adjusting based on the traffic.