# Primavera.Hydrogen.Taskbox

**Class library that contains types that implement asynchronous multi-task processing.**

## About

This class library is the default implementation of the [Taskbox.Abstractions][TBXA].

[TBXA]: https://github.com/PrimaveraDeveloper/lithium/blob/master/ref/hydrogen-2.0/Taskbox.Abstractions.md

## Pipebox

The `Pipeline Design Pattern` defines the processing of a complex or time consuming operation into a set of small tasks (Handlers) that combine together to form an asynchronous unit of work (Pipeline), with the aim of improving performance, scalability and component reusability in any application or service. This pattern is implemented here by the `Pipebox` set of classes and supplementary types.

Consider the following example:

```csharp
using System.IO;
using System.Text.Json;

// Arrange the pipebox configuration

var json = File.ReadAllText("PipeboxConfig.json");
var config = PipeboxConfig.Create(json);

// Arrange the service provider

var services = new ServiceCollection();
services.AddTransient<DefaultPipelineHandler<string>, DefaultPipelineHandler<string>>();
var provider = services.BuildServiceProvider();

// Arrange the pipebox context

var data = "Hello";
using var context = new PipeboxContext<string>();
context.UseData(data);

// Arrange the pipebox engine

using var pipebox = new Pipebox<string>();
pipebox.UseProvider(provider).UseConfig(config).UsePipeline("p1");

// Run the pipebox to execute the pipeline "p1"

pipebox.ExecuteAsync(context).GetAwaiter().GetResult();

// Get the pipeline result

string result = context.Data;
```

### Pipebox

The `Pipebox<T>` provides the implementation of `IPipebox<TContext, TConfig>`, which is the engine that runs a specified pipeline with a `PipelineContext<T>`.

**Methods**

Method | Description
:--- | :---
UseProvider | Configures the pipebox to use the specified configuration.
UseConfig | Configures the pipebox to use the specified service provider.
UsePipeline | Configures the pipebox to execute the specified pipeline.
ExecuteAsync | Executes the specified pipeline asynchronously.
Execute | Executes the specified pipeline.

**Remarks**
(todo)

### PipeboxContext

The `PipeboxContext<T>` provides the implementation of `IPipeboxContext<T>`, which is the data context that is processed along the pipeline execution.

(todo: table of properties and methods, remarks and examples)

### PipeboxConfig

The `PipeboxConfig` provides the implementation of `<TConfig>`, which is the pipebox configuration that defines one or more pipelines and their corresponding handlers.

(todo: table of properties and methods, remarks and examples)

### DefaultPipelineHandler

The `DefaultPipelineHandler<T>` provides a default implementation of `IPipelineHandler<TContext, TConfig>` with useful features for debugging and prototyping.

(todo: table of properties and methods, remarks and examples)

### PipelineHandlerBase

The `PipelineHandlerBase<T>` provides the base implementation of `IPipelineHandler<TContext, TConfig>`.

(todo: table of properties and methods, remarks and examples)

### PipeboxState

The `PipeboxState` provides the implementation of the state of execution for the `Pipebox<T>`.

(todo: table of properties and methods, remarks and examples)

---

## Workers (this content needs a full revision)

This library also contains a group of components that will help deploying and managing the taskbox workers.

### `ITaskboxHandler`
The `ITaskboxHandler` is integrated with the .NET Core dependency injection engine, so it can be easily used in any .NET Core project, in particular the ASP.NET Core projects.

The 'ITaskboxHandler' allows handling a list of events given a list of pipelines configurations.

The `ITaskboxHandler` uses the `IEventBus`, `IWorkersEngine` and the `IWorker` so it's necessary to implement this services.

The service implementation should be registered using one of the following extension methods for `IServiceCollection`.

```csharp
IServiceCollection services = (...);

services.AddSingleton<ITaskboxHandler, TaskboxHandler>()
        .AddSingleton<IEventBusManager, EventBusManager>()
        .AddSingleton<IWorkersEngine, WorkersEngine>()
        .AddTransient<IWorker,Worker>();
```
It's also necessary to implement the taskbox configuration, where will be defined necessary parameters for the taskbox to run.

```csharp
.Configure<TaskboxConfiguration>(
    this.Configuration.GetSection(nameof(TaskboxConfiguration)))
    .AddOptionsSnapshot<TaskboxConfiguration>();
```

Then its only needed to get a new instance of the 'ITaskboxHandler' and use the handle method that receives a array of events and pipeline configurations.

```csharp

this.TaskboxHandler = (ITaskboxHandler)this.ServiceProvider.GetRequiredService(typeof(ITaskboxHandler));

this.TaskboxHandler.Handle(events, pipelineConfigs);

```
The `Handle` method subscribes all the event types received.


### `EventTaskboxHandler`

This handler is called in the `Handle` method, above, which implements the [IEventBusEventHandler<T>](EventBus.Abstractions.md), this will be triggered when the event is raised.


### [`IEventBus`](EventBus.Abstractions.md)

To know more about this implementation view [this](EventBus.Abstractions.md).

### `IWorkersEngine`

This component is responsible for distributing events for the workers pool and for managing all the workers, starting and stoping them as necessary. The Workers Engine configuration is defined by the `ITaskboxConfiguration`. This implementation contains all the configurations needed to start the workers engine. Here you can define the number of workers that will be running simultaneously, the event waiting time for a worker and many others.

### `IWorker`

As the name implies, this is the worker component, it implements the [BackgroundService](https://docs.microsoft.com/en-us/dotnet/api/microsoft.extensions.hosting.backgroundservice?view=dotnet-plat-ext-3.0). 

The worker actions are `StartAsync`, `StopAsync` and `ExecuteAsync`.
