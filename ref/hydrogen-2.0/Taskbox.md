# Primavera.Hydrogen.Taskbox

**Class library that contains types that implement asynchronous multi-task processing.**

[REF_Taskbox_Abstractions]: Taskbox.Abstractions.md

## About

This class library is the default implementation of the [Primavera.Hydrogen.Taskbox.Abstractions][REF_Taskbox_Abstractions].

## Pipebox

The `Pipeline Design Pattern` defines the processing of a complex or time consuming operation into a set of small tasks (Handlers) that combine together to form an asynchronous unit of work (Pipeline), with the aim of improving performance, scalability and component reusability in any application or service. This pattern is implemented here by the `Pipebox` set of classes and supplementary types.

Consider the following example:

```csharp
using System.IO;
using System.Text.Json;

// Arrange the pipebox configuration

string json = File.ReadAllText("PipeboxConfig.json");
PipeboxConfig config = PipeboxConfig.Create(json);

// Arrange the service provider

var services = new ServiceCollection();
services.AddTransient<DefaultPipelineHandler<string>, DefaultPipelineHandler<string>>();
var provider = services.BuildServiceProvider();

// Arrange the pipebox context

string data = "Hello";
using var context = new PipeboxContext<string>(data);

// Arrange the pipebox engine

using var pipebox = new Pipebox<string>();
pipebox.UseProvider(provider)
       .UseConfig(config)
       .UsePipeline("p1");

// Run the pipebox to execute the pipeline "p1"

pipebox.ExecuteAsync(context).GetAwaiter().GetResult();

// Get the pipeline result

string result = context.Data;
```

### Pipebox

The `Pipebox<T>` provides the implementation of `IPipebox<TContext, PipeboxConfig>`, which is the engine that runs a specified pipeline with a `PipelineContext<T>`.

See the [IPipebox<TContext, TConfig>][REF_Taskbox_Abstractions] for more information about the interface members.

### PipeboxConfig

The `PipeboxConfig` provides the implementation of `<TConfig>`, which is the pipebox configuration that defines one or more pipelines and their corresponding handlers.

**Create a Configuration**

Consider the following configuration file:

```json
{
  "version": "1.0",
  "expressions": [
    {
      "pattern": "%p2p3%",
      "value": "p2=v2; p3=v3"
    }
  ],
  "pipelines": [
    {
      "id": "p1", 
      "tag": "pipeline 1",
      "configStr": "[{\"p1\":\"v1\"},{\"p2\":\"v2\"}]",      
      "handlers": [
        {
          "id": "h1",
          "tag": "handler 1",
          "configStr": "p1=v1; %p2p3%"
        },
        {
          "id": "h2",
          "tag": "handler 2",
          "configStr": "p1={ p11=v11; p12=v12 }; %p2p3%"
        }
      ]
    }
  ]
}
```

This configuration can be created at run time and serialized for later use like any other application setting. See the example that loads the configuration:

```csharp
string json = File.ReadAllText("PipeboxConfig.json");
PipeboxConfig config = PipeboxConfig.Create(json);
```

**Missing and Default Settings**

In the previous example, the `Create` method deserializes the configuration and implicitly executes two more operations:
*  applies (regular) expression replacements to the `configStr` values
*  applies default settings to the configuration if required values are missing
  
The end result is as follows:

```json
{
  "version": "1.0",
  "expressions": [
    {
      "pattern": "%p2p3%",
      "value": "p2=v2; p3=v3"
    }
  ],
  "pipelines": [
    {
      "id": "p1",
      "active": "True",
      "tag": "pipeline 1",
      "configStr": "[{\"p1\":\"v1\"},{\"p2\":\"v2\"}]",
      "handlers": [
        {
          "id": "h1",
          "order": "1",
          "tag": "handler 1",
          "type": "Primavera.Hydrogen.Taskbox.Pipeliners.DefaultPipelineHandler\u00601[[System.Object, System.Private.CoreLib]], Primavera.Hydrogen.Taskbox",
          "configStr": "p1=v1; p2=v2; p3=v3",
          "active": "True"
        },
        {
          "id": "h2",
          "order": "2",
          "tag": "handler 2",
          "type": "Primavera.Hydrogen.Taskbox.Pipeliners.DefaultPipelineHandler\u00601[[System.Object, System.Private.CoreLib]], Primavera.Hydrogen.Taskbox",
          "configStr": "p1={ p11=v11; p12=v12 }; p2=v2; p3=v3",
          "active": "True"
        }
      ]
    }
  ]
}
```

**Configuration String**

The `configStr` property is used to set the custom configuration of pipelines and handlers in a format that only their implementation knows. Still, there are two known formats that are easily supported:

Using the `ConfigString` class (dictionary of key/value pairs):

```csharp
// For the configuration string
string configStr = "p1=v1; p2={ p21=v21; p22=v22 }; p3=v3";

// Create the configuration string objet
var config = new ConfigString(configStr);

// Get the value of p1
string p1 = config.GetValue<string>("p1");

// Get the value of p2
ConfigStr p2 = config.GetValue<ConfigString>("p2");
string p21 = config.GetValue<string>("p21");
string p22 = config.GetValue<string>("p22");

// Get the value of p3
string p1 = config.GetValue<string>("p3");
```

Using a stringified JSON:

```csharp
// For the stringified JSON
string configStr = "[{\"p1\":\"v1\"},{\"p2\":\"v2\"}]"

// Create the JSON document
var jsonDoc = JsonDocument.Parse(configStr);
var jsonText = jsonDoc.RootElement.GetRawText();
```

**Default Type**

When the configuration does not defines the handler's type - the middleware class that implements that handler - it assumes the default value of `DefaultPipelineHandler`. This handler is explained with more details in the corresponding section below.

### PipeboxContext

The `PipeboxContext<T>` provides the implementation of `IPipeboxContext<T>`, which is the data context that is processed along the pipeline execution.

See the [IPipeboxContext<T>][REF_Taskbox_Abstractions] for more information about the interface members.

### PipeboxState

The `PipeboxState` provides the implementation of the state of execution for the `Pipebox<T>`.

### PipelineHandlerBase

The `PipelineHandlerBase<T>` provides the base implementation of `IPipelineHandler<TContext, HandlerConfig>`.

See the [IPipelineHandler<TContext, TConfig>][REF_Taskbox_Abstractions] for more information about the interface members.

### HandlerConfig

The `HandlerConfig` provides the implementation of `<TConfig>`, which is the handler configuration.

### DefaultPipelineHandler

The `DefaultPipelineHandler<T>` provides a default implementation of `IPipelineHandler<TContext, HandlerConfig>` with useful features for debugging and prototyping.

See the [IPipelineHandler<TContext, TConfig>][REF_Taskbox_Abstractions] for more information about the interface members.

## Pipebox Workers

### `IPipeboxWorkersManager`
The `IPipeboxWorkersManager` is integrated with the .NET Core dependency injection engine, so it can be easily used in any .NET Core project, in particular the ASP.NET Core projects.

This component is responsible for distributing configurations to an `IPipeboxWorker` and for managing all the `IPipeboxWorkers`, starting and stoping them as necessary. It can also be defined as an engine of `IPipeboxWorkers`. The `IPipeboxWorkersManager` configuration is defined by the PipeboxWorkersConfig. This implementation contains all the configurations needed for the `IPipeboxWorkersManager` . Here you can define the number of `IPipeboxWorkers` that will be running simultaneously and the waiting time for `IPipeboxWorker` when the pool is full.


### `IPipeboxWorker`

As the name implies, this is the worker component, it implements the [BackgroundService](https://docs.microsoft.com/en-us/dotnet/api/microsoft.extensions.hosting.backgroundservice?view=dotnet-plat-ext-3.0). 

When the `IPipeboxWorkersManager` starts a `IPipeboxWorker` it gives to the worker:
- A `PipeboxConfig`
- The event
- The pipeline id to be executed

The `IPipeboxWorker` is responsible for instantiating, configuring and executing a Pipebox.

The worker actions are `StartWorkerAsync`, `StopWorkerAsync`.

The service implementation should be registered using one of the following extension methods for `IServiceCollection`.

```csharp
IServiceCollection services = (...);

services.AddSingleton<IPipeboxWorkersManager, PipeboxWorkersManager>()
        .AddSingleton(typeof(IPipeboxWorker<>), typeof(PipeboxWorker<>))
```
It's also necessary to implement the `PipeboxWorkersConfig`, where will be defined necessary parameters for the workers to run.

```csharp
services.Configure<PipeboxWorkersConfig>(
    this.Configuration.GetSection(nameof(PipeboxWorkersConfig)))
    .AddOptionsSnapshot<PipeboxWorkersConfig>();
```

To start a `IPipeboxWorker` through the `IPipeboxWorkersManager`, consider the following example:

```csharp

// Arrange the service provider

IServiceProvider serviceProvider = this.BuildServiceProvider();

var services = new ServiceCollection();

services.Configure<PipeboxWorkersConfig>(
    this.Configuration.GetSection(nameof(PipeboxWorkersConfig)))
    .AddOptionsSnapshot<PipeboxWorkersConfig>();

services.AddSingleton<IPipeboxWorkersManager, PipeboxWorkersManager>()
        .AddSingleton(typeof(IPipeboxWorker<>), typeof(PipeboxWorker<>));

var provider = services.BuildServiceProvider();

// Get a new instance of a IPipeboxWorkersManager
IPipeboxWorkersManager engine = provider.GetRequiredService<IPipeboxWorkersManager>();

// Arrange the configurations for the worker to execute
var data = "hello";

var json = FilesService.ReadFileAsText(Constants.Files.PipeboxConfig03);

var config = PipeboxConfig.Create(json);

// Starting worker
await engine.StartWorker(config, data, "p1").ConfigureAwait(false);

```
To stop all the running `IPipeboxWorker`'s, consider the following example:

```csharp

// Arrange the service provider

IServiceProvider serviceProvider = this.BuildServiceProvider();

var services = new ServiceCollection();

services.Configure<PipeboxWorkersConfig>(
    this.Configuration.GetSection(nameof(PipeboxWorkersConfig)))
    .AddOptionsSnapshot<PipeboxWorkersConfig>();

services.AddSingleton<IPipeboxWorkersManager, PipeboxWorkersManager>()
        .AddSingleton(typeof(IPipeboxWorker<>), typeof(PipeboxWorker<>));

var provider = services.BuildServiceProvider();

// Get a new instance of a IPipeboxWorkersManager
IPipeboxWorkersManager engine = provider.GetRequiredService<IPipeboxWorkersManager>();

// Starting the workers
(...)

// Stoping the engine
await engine.StopEngine().ConfigureAwait(false);

```