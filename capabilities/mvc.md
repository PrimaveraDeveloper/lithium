# Capability - MVC

> Tags: ASP.NET, User Interface, Web API, Controllers

The microservice Web API is built with ASP.NET MVC, which is configured in the application startup.

`StartupBase.AddMvc()`:

```csharp
protected virtual void AddMvc(IServiceCollection services, HostConfiguration hostConfiguration)
{
    // Validation

    SmartGuard.NotNull(() => services, services);
    SmartGuard.NotNull(() => hostConfiguration, hostConfiguration);

    // API controllers with views
    
    IMvcBuilder builder = services
        .AddApiControllersWithViews(
            (options) =>
            {
                // API versioning

                options.UseVersioning = true;
                options.DefaultApiVersion = new ApiVersion(
                    Primavera.Lithium.RosePeople.Models.Metadata.ApiVersions.Values.DefaultVersion.Major,
                    Primavera.Lithium.RosePeople.Models.Metadata.ApiVersions.Values.DefaultVersion.Minor);

                options.ConfigureApiVersioningOptions = this.ConfigureApiVersioningOptions;
            });

    // Custom configuration

    this.ConfigureMvc(services, builder, hostConfiguration);
}
```

Apart from the Web API controllers, is also used to build the service [home page](./home-page.md) and can also be used to add custom [user interface](./ui.md) to the service.