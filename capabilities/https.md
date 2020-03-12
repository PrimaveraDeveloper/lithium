# Capability - HTTPS

> Tags: HTTP, HSTS

The microservices Web API is expected to respond only to HTTPS requests.

This ensured by default in the application startup by activating HTTPS redirection and HSTS.

`StartupBase.UseHttps()`:

```csharp
protected virtual void UseHttps(IApplicationBuilder app, HostConfiguration hostConfiguration)
{
    // Validation

    SmartGuard.NotNull(() => app, app);
    SmartGuard.NotNull(() => hostConfiguration, hostConfiguration);

    // Logging

    this.Logger.LogDebug($"Activating HTTPS...");

    // Not for development...

    bool useDevelopmentSettings = this.UseDevelopmentSettings || this.CurrentEnvironment.IsDevelopment();

    if (!useDevelopmentSettings)
    {
        // HSTS

        app.UseHsts();
    }

    // HTTPS redirection

    app.UseHttpsRedirection();
}
```

Notice also that the default ports for local development (localhost) are defined (and may be modified) in `launchSettings.json`:

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:30000/",
      "sslPort": 44392
    }
  },
  "profiles": {
    "On DotNet Cli": {
      "commandName": "Project",
      "launchBrowser": false,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      },
      "applicationUrl": "http://localhost:20000;https://localhost:20001"
    },
    "On IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": false,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```