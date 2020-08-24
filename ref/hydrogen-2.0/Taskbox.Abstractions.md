<!-- REFERENCES -->

[REF_TBX]: ./Taskbox.md

<!-- DOCUMENT -->

# Primavera.Hydrogen.Taskbox.Abstractions

**Class library that contains types that define abstractions for the taskbox design pattern.**

## About

This abstraction defines taskbox design pattern interfaces. This abstraction helps to implement a taskbox for a multi-task asynchronous processing engine for client applications with large or complex business processes that can be broken down into smaller work activities to improve overall application performance and maximize code reuse.

## Abstractions

### ITaskboxService

The `ITaskboxService` interface defines the abstraction for the engine that executes a task, this can use a specified configuration with the task definition. Also, it can execute functions thus providing greater versatility.  

Method | Description
:--- | :---
StartAsync | Starts the taskbox service.
StartAsync(TConfig config) | Starts the taskbox service with the specified configuration.
StopAsync | Stops the taskbox service.
ExecuteTask(`TTask` task) | Executes the specified task asynchronously.
CancelTask(int id) | Cancels the task with the specified id.
GetRunningTasks() | Gets the running tasks.
ExecuteTaskAsync(funcs) |  Executes the specified task asynchronously.

### IBackgroundTaskboxQueueTask

The `IBackgroundTaskboxQueueTask{TTask}` interface defines the abstraction for the background queue that can be used by the `ITaskboxService{TTask, TConfig}` to queue the tasks.

Method | Description
:--- | :---
QueueTask | Queues the `TTask`.
QueueTasks |  Queues the `TTask`'s.
DequeueTaskAsync | Dequeues the `TTask` asynchronouly.

### ITaskboxInvoker

The `ITaskboxInvoker{TTask}` interface defines abstraction, used to invoke a task that follows the trigger-action pattern.

Method | Description
:--- | :---
InvokeTrigger | Invokes the `TTask` trigger.
InvokeActions |  Invokes the `TTask` actions.

### IEventBusManager

The `IEventBusManager{T}` interface defines abstraction, responsible for subscribing and unsubscribing events.

Method | Description
:--- | :---
SubscribeAsync | Subscribes the specified event.
UnsubscribeAsync |  Unsubscribes the specified event type.

## Channels

### IChannelGenerator

The `IChannelGenerator` interface defines abstraction, the purpose of this is to provide methods to generate
channels in a faster, easier, and generic way.

Method | Description
:--- | :---
CreateChannelAsync{T} | Create a channel with the given type.
CreateChannelAsync{T} (int capacity) |  Create a channel with the given type and capacity.
CreateChannelAsync{T} (T context) |  Create a channel with the given type and writes the context.
CreateChannelAsync{T} (T context, int capacity) |  Create a channel with the given type and capacity and writes the context.
CreateGenericChannelAsync (string type) |  Create a channel with the given type.
CreateGenericChannelAsync (string type, UnboundedChannelOptions options) |  Create a channel with the given type and unbounded channel options.
CreateGenericChannelAsync (string type, int capacity) |  Create a channel with the given type and capacity.
CreateGenericChannelAsync (string type, BoundedChannelOptions options) |  Create a channel with the given type and bounded channel options.
CreateGenericChannelsAsync (string type, int n) |  Creates n channels with the given type.

### ITrigger

The `ITrigger{T, TConfig}` interface defines abstraction, the trigger is responsible for starting the actions indirectly.

Method | Description
:--- | :---
RunAsync(channels, config) | Asynchronously runs the trigger.

### IAction

The `IAction{T, TConfig}` interface defines abstraction, an action is the task operational/goal.

Method | Description
:--- | :---
RunAsync(channel, config) | Asynchronously runs the action.

### Supplementary types

Use the abstract classes provided in the concrete implementations to create, customize, or extend the behavior of this design pattern.

## Implementations

[Primavera.Hydrogen.Taskbox][REF_TBX]
