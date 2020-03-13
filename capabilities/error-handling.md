# Capability - Error Handling

> Tags: Exceptions, Logging, Telemetry, Status Code

Handling errors is an "art" that is very difficult to master for any kind of application. The Lithium Framework ensures that this becomes easier on microservices by setting standard behavior, automatically generated and configured.

The standard behavior is:

- The error handling middleware is automatically setup at application startup.
- In development environments, exceptions will be propagated to the client (without exception shielding).
- On any other environment, on non-API requests, a standard error page is presented to the client/user
- Otherwise, a normal REST error is returned to the client.

This is enforced by `StartupBase.UseErrorHandling()`:

```csharp
protected virtual void UseErrorHandling(IApplicationBuilder app, HostConfiguration hostConfiguration)
{
    // Validation

    SmartGuard.NotNull(() => app, app);
    SmartGuard.NotNull(() => hostConfiguration, hostConfiguration);

    // Logging

    this.Logger.LogDebug($"Activating error handling...");

    // Error route

    string errorRoute = Constants.Controllers.Home.Routes.Error;

    // Middleware to process unhandled exceptions

    // NOTE:
    // When in development all exceptions' details will be sent to the client: 
    // ... In the browser when on MVC routes.
    // ... In the response body when on API routes.
    // Exceptions on MVC routes will be processed by the exception handler bellow
    // to "redirect" the user to the error route to show him a friendly error view.
    // Content routes and API routes are not handled at all here.
    // The Telemetry middleware (added elsewhere) ensures that all unhandled
    // exceptions are collected.

    bool useDevelopmentSettings = this.UseDevelopmentSettings || this.CurrentEnvironment.IsDevelopment();

    if (useDevelopmentSettings)
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseWhen(
            context =>
            {
                return context.Request.PathIsNotApi() && context.Request.PathIsNotContent();
            },
            builder =>
            {
                builder.UseExceptionHandler(errorRoute);
            });
    }

    // Middleware to process status code errors
    
    // NOTE:
    // All other status code errors (e.g 404) (not 500) are handled by this middleware to
    // "redirect" the user to the same friendly error view.
    // Content routes and API routes are not handled at all here because we do not want
    // to return HTML when these errors occur.

    app.UseWhen(
        (context) =>
        {
            return context.Request.PathIsNotApi() && context.Request.PathIsNotContent();
        },
        builder =>
        {
            builder.UseStatusCodePagesWithReExecute(errorRoute, "?statusCode={0}");
        });
}
```

Unhandled exceptions are always tracked automatically and sent to [telemetry](./telemetry.md).

Important notices:

- API error responses should follow the REST practices and handle expected errors correctly.
- Unexpected exceptions should remain unhandled, so that a 500 status code is returned to the client and telemetry is collected correctly.