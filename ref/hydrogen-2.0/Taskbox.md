# Primavera.Hydrogen.Taskbox

**Class library that contains types that implement asynchronous multi-task processing.**

## About

The taskbox is a multi-task asynchronous processing engine for client applications with large or complex business processes that can be broken down into smaller work activities to improve overall application performance and maximize code reuse.

## Taskbox Service

### `ITaskboxService`
The `ITaskboxService` is integrated with the .NET Core dependency injection engine, so it can be easily used in any .NET Core project, in particular the ASP.NET Core projects.

This component is responsible for distributing configurations to an `ITaskboxWorker` and for managing all the `ITaskboxWorkers`, starting and stopping them as necessary. It can also be defined as an engine of `ITaskboxWorkers`. The `ITaskboxService` configuration is defined by the TaskboxOptions. This implementation contains all the configurations needed for the `ITaskboxService` . Here you can define the number of `ITaskboxWorkers` that will be running simultaneously and the waiting time for `ITaskboxWorker` when the pool is full.


### `ITaskboxWorker`

As the name implies, this is the worker component, it implements the [BackgroundService](https://docs.microsoft.com/en-us/dotnet/api/microsoft.extensions.hosting.backgroundservice?view=dotnet-plat-ext-3.0). 

When the `ITaskboxService` starts a `ITaskboxWorker` it gives to the worker:
- A `PipeboxConfig`
- The event
- The pipeline id to be executed

The `ITaskboxWorker` is responsible for instantiating, configuring and executing a Pipebox.

The worker actions are `StartAsync`, `StopAsync`.

The service should be registered using one of the following extension methods for `IServiceCollection`.

```csharp
IServiceCollection services = (...);

services.AddSingleton<ITaskboxService, TaskboxService>()
        .AddSingleton(typeof(TaskboxWorker<>))
```
It's also necessary to implement the `TaskboxOptions`, where will be defined necessary parameters for the workers to run.

```csharp
services.Configure<TaskboxOptions>(
    this.Configuration.GetSection(nameof(TaskboxOptions)))
    .AddOptionsSnapshot<TaskboxOptions>();
```

To start a `ITaskboxWorker` through the `ITaskboxService`, consider the following example:

```csharp

// Arrange the service provider

IServiceProvider serviceProvider = this.BuildServiceProvider();

var services = new ServiceCollection();

services.Configure<TaskboxOptions>(
    this.Configuration.GetSection(nameof(TaskboxOptions)))
    .AddOptionsSnapshot<TaskboxOptions>();

services.AddSingleton<ITaskboxService, ITaskboxService>()
        .AddSingleton(typeof(TaskboxWorker<>));

var provider = services.BuildServiceProvider();

// Get a new instance of a ITaskboxService
ITaskboxService taskboxService = provider.GetRequiredService<ITaskboxService>();

// Arrange the configurations for the worker to execute
var data = "hello";

var json = FilesService.ReadFileAsText(Constants.Files.PipeboxConfig03);

var config = PipeboxConfig.Create(json);

// Starting worker
await taskboxService.StartWorker(config, data, "p1").ConfigureAwait(false);

```
To stop all the running `ITaskboxWorker`'s, consider the following example:

```csharp

// Arrange the service provider

IServiceProvider serviceProvider = this.BuildServiceProvider();

var services = new ServiceCollection();

services.Configure<TaskboxOptions>(
    this.Configuration.GetSection(nameof(TaskboxOptions)))
    .AddOptionsSnapshot<TaskboxOptions>();

services.AddSingleton<ITaskboxService, ITaskboxService>()
        .AddSingleton(typeof(TaskboxWorker<>));

var provider = services.BuildServiceProvider();

// Get a new instance of a ITaskboxService
ITaskboxService taskboxService = provider.GetRequiredService<ITaskboxService>();

// Starting the workers
(...)

// Stopping the engine
await taskboxService.StopAsync().ConfigureAwait(false);

```

## Dependency Injection

### Options Class's

- `TaskboxOptions` it's a class that holds the configurations for the workers manager.

### Service collection extensions

- `TaskboxServiceCollectionExtensions` this class provides methods to help add and configure a taskbox.

## Event Handling

This section holds all the components related to the event handling of a Taskbox.

### `EventBusHandler<T>`
It's an abstract handler that helps to build in a faster way a custom handler to receive events from the EventBus.



### TriggeredEvent
It's a class that represents the event, this holds the event type, the event body, the event topic and it's properties. This class also contains a set of methods that helps to manage a list of events.

### EventBusManager

It's a generic subscriber and unsubscriber, it can do both operations for any event type that is given. This manager works with the `TriggeredEvent` class.

## Abstractions

To help with the implementation of this component two interfaces can be used.

### ITaskboxWorker

The `ITaskboxWorker` interface defines the abstraction for the worker.

**Properties**

Property | Description
:--- | :---
Id | Gets or sets the worker id.

**Methods**

Method | Description
:--- | :---
StartAsync | Starts the worker with a specified configuration, a context and a pipeline id
StopAsync | Stops the worker.

### IEventBusManager

The `IEventBusManager` interface defines the abstraction for the EventBusManager, which is a generic subscriber and unsubscriber.

**Methods**

Method | Description
:--- | :---
SubscribeAsync | Subscribes the specified event.
UnsubscribeAsync | Unsubscribes the specified event type.