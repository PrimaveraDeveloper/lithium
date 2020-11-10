# Primavera.Taskbox.Abstractions

**Class library that contains types that define abstractions for the taskbox design pattern.**

## About

This abstraction defines taskbox design pattern interfaces. This abstraction helps to implement a multi-task asynchronous processing engine for client applications with large or complex business processes that can be broken down into smaller work activities to improve overall application performance and maximize code reuse.

## Abstractions

### ITaskboxService

The `ITaskboxService` interface defines the abstraction for the engine that executes a task, this can use a specified configuration with the task definition. Also, it can execute functions thus providing greater versatility.  

Method | Description
:--- | :---
`StartAsync` | Starts the taskbox service.
`StartAsync(TConfig config)` | Starts the taskbox service with the specified configuration.
`StopAsync` | Stops the taskbox service.
`ExecuteTaskAsync(TTask task)` |  Executes the specified task asynchronously.
`CancelTask(int id)` | Cancels the task with the specified id.
`GetTasksAsync()` | Gets the tasks.
`GetTasksAsync(int state)` | Gets the tasks by state.

### ITaskboxInvoker

The `ITaskboxInvoker{TTask}` interface defines the abstraction used to invoke a task that follows the trigger-action pattern.

Method | Description
:--- | :---
`InvokeTrigger` | Invokes the `TTask` trigger.
`InvokeActions` |  Invokes the `TTask` actions.

### IEventBusManager

The `IEventBusManager{T}` interface defines the abstraction used for subscribing and unsubscribing events.

Method | Description
:--- | :---
`SubscribeAsync` | Subscribes the specified event.
`UnsubscribeAsync` |  Unsubscribes the specified event type.

### IEventBusServiceFactory

The `IEventBusServiceFactory{TService, TStrategy}` interface defines the abstractions used to implement an EventBus service factory based on strategies.

Method | Description
:--- | :---
`GetEventBusService` | Gets the event bus service based on a strategy.

### ITaskboxSchedulerFactory

The `ITaskboxSchedulerFactory{TScheduler}` interface defines the abstractions used to implement a scheduler factory.
Method | Description
:--- | :---
`GetSchedulerAsync` | Gets the scheduler instance.

## Components

### ITrigger

The `ITrigger{T, TConfig}` interface defines the abstraction for the trigger, which is responsible for starting the actions indirectly.

Method | Description
:--- | :---
`RunAsync(channels, config)` | Asynchronously runs the trigger.

### IAction

The `IAction{T, TConfig}` interface defines the abstraction for the action, which is the task operational/goal.

Method | Description
:--- | :---
`RunAsync(channel, config)` | Asynchronously runs the action.

### Supplementary types

Use the abstract classes provided in the concrete implementations to create, customize, or extend the behavior of this design pattern.

## Implementations

[Primavera.Hydrogen.Taskbox](./Taskbox.md)
