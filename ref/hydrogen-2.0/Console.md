# Primavera.Hydrogen.Console

**Class library that contains types, helpers, and extension methods for console applications.**

This library is intended to be referenced from console applications as it provides methods that allow interacting
with `Console` in a simpler fashion and it provides means to host services in console applications, supporting dependency
injection pretty much like a ASP.NET Core application.

## `ConsoleHelper`

`ConsoleHelper` extends and enriches `Console` with additional static methods that make it easier to interact with the console output
and input.

Examples:

- `ConsoleHelper.WriteError()`
- `ConsoleHelper.WriteInformationLine()`

## Hosting

.NET Core supports hosting services in a console application using the Generic Host. This library extends the behavior of that component
via the `IConsoleHost` that supports that (it reuses the generic host behavior) but allows the console application to interact with
the user (e.g. `ReadKey()`) at the same time.

To achieve that the host should be built with `IConsoleHostBuilder` and a `IConsoleInteractiveApplication` implementation should be provided
to provide the interaction logic. See the `ConsoleApplication` sample for a complete implementation.

### `IConsoleHostBuilder` and `ConsoleHostBuilder`

The IConsoleHostBuilder allows to configure the host, the application and all the dependency injection required by the application, including hosted
services:

Example:

```csharp
IConsoleHostBuilder builder = new ConsoleHostBuilder()
  .ConfigureHostConfiguration(
    config =>
    {
      config.SetBasePath(Environment.CurrentDirectory);
      config.AddJsonFile("hostsettings.json", optional: true);
      config.AddEnvironmentVariables(prefix: "PREFIX_");
      config.AddCommandLine(args);
    })
    .ConfigureAppConfiguration(
      (context, config) =>
      {
        config.AddJsonFile("appsettings.json", optional: true);
        config.AddJsonFile($"appsettings.{context.HostingEnvironment.EnvironmentName}.json", optional: true);
        config.AddEnvironmentVariables(prefix: "PREFIX_");
        config.AddCommandLine(args);
      })
      .ConfigureServices(
        (context, services) =>
        {
          services.AddHostedService<TimedHostedService>();
        })
      .ConfigureLogging(
        (context, config) =>
        {
          config.AddConfiguration(context.Configuration.GetSection("Logging"));
          config.AddConsole();
          config.AddDebug();
        });
```

### `IConsoleHost`

The builder will setup a IConsoleHost that is used to start the application:

```csharp
IConsoleHost host = builder.Build();
await host.RunAsync().ConfigureAwait(false);
```

### `IConsoleInteractiveApplication`

When the console application needs to interact with the user it just needs to provide an implementation
of `IConsoleInteractiveApplication` to the builder:

```csharp
IConsoleHost host = builder.Build<Application>();
await host.RunAsync().ConfigureAwait(false);

internal class Application : IConsoleInteractiveApplication
{
  public Task RunAsync(CancellationToken cancellationToken = default)
  {
    ConsoleHelper.WriteInformationLine("This is the application.");
    ConsoleHelper.ReadKey();
  }
}
```
