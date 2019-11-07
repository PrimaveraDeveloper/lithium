# Primavera.Hydrogen.Taskbox

**Class library that contains types that implement the task box design patterns.**

## About

This class library contains interfaces, abstract classes and supplementary types needed for a taskbox implementation.
This document explains the purpose of each component.

## Workers

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


### `EventHandlerTaskbox`

This handler is called in the `Handle` method, above, which implements the [IEventBusEventHandler<T>](EventBus.Abstractions.md), this will be triggered when the event is raised.


### [`IEventBus`](EventBus.Abstractions.md)

To know more about this implementation view [this](EventBus.Abstractions.md).

### `IWorkersEngine`

This component is responsible for distributing events for the workers pool and for managing all the workers, starting and stoping them as necessary. The Workers Engine configuration is defined by the `ITaskboxConfiguration`. This implementation contains all the configurations needed to start the workers engine. Here you can define the number of workers that will be running simultaneously, the event waiting time for a worker and many oders.

### `IWorker`

As the name implies, this is the worker component, it implements the [BackgroundService](https://docs.microsoft.com/en-us/dotnet/api/microsoft.extensions.hosting.backgroundservice?view=dotnet-plat-ext-3.0). 

The worker actions are `StartAsync`, `StopAsync` and `ExecuteAsync`.

