# Primavera.Hydrogen.Taskbox.Abstractions

**Class library that contains types that define abstractions for asynchronous multi-task processing.**

## Abstractions

### ITaskboxWorker

The `ITaskboxWorker` interface defines the abstraction for the worker.

**Properties**

Property | Description
:--- | :---
Id | Gets or sets the worker id.

**Methods**

Method | Description
:--- | :---
StartWorkerAsync | Starts the worker with a specified configuration, a context and a pipeline id
StopWorkerAsync | Stops the worker.

### IEventBusManager

The `IEventBusManager` interface defines the abstraction for the EventBusManager, which is a generic subscriber and unsubscriber.

**Methods**

Method | Description
:--- | :---
Subscribe | Subscribes the specified event.
Unsubscribe | Unsubscribes the specified event type.

## Implementations

[Primavera.Hydrogen.Taskbox][REF_PHT]

[REF_PHT]: https://github.com/PrimaveraDeveloper/lithium/blob/master/ref/hydrogen-2.0/Taskbox.md
