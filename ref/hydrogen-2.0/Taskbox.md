<!-- REFERENCES -->

[REF_TBX]: ./Taskbox.md
[REF_TBX_ABS]: ./Taskbox.Abstractions.md
[REF_EBINMEM]: ./EventBus.InMemory.md

<!-- DOCUMENT -->

# Primavera.Hydrogen.Taskbox

**Class library that contains types that implement the taskbox design pattern for asynchronous multi-task processing.**

## About

This class library is the default implementation of the [Primavera.Hydrogen.Taskbox.Abstractions][REF_TBX_ABS]. The main purpose of this implementation is to support the taskbox design pattern. The taskbox is a multi-task asynchronous processing engine for client applications with large or complex business processes that can be broken down into smaller work activities to improve overall application performance and maximize code reuse.

## Taskbox Config

The TaskboxConfig provides the implementation of `<TConfig>` this is a way to define the tasks that are required to execute by the `ITaskboxService{TTask, TConfig}`.

### Task

If you think of what a task box is, you quickly think of something related to task execution or some task management mechanism. The idea behind this implementation is a task-oriented pattern, where the task has all the protagonism.

### `TaskConfig`

The `TaskConfig` defines the task in this implementation, provides the implementation of `<TTask>` used by the `ITaskboxService{TTask, TConfig}`. This task is composed of a `TriggerConfig` and one or more `ActionConfig`'s.

- `TriggerConfig`, is responsible for starting the actions in a certain period, this holds all the configuration parameters of a trigger.
- `ActionConfig`, defines the operation that the task has to accomplish, this holds all the configuration parameters of action.

When the task is executed, the trigger will publish an event. The action, on the other hand, subscribes to this event and when the event is raised, it performs a certain operation.

### Components

#### Trigger

All of this trigger types implement the `ITrigger{T, TConfig}`, to know more see the [Primavera.Hydrogen.Taskbox.Abstractions][REF_TBX_ABS].

- `Trigger{T}`, its purpose is to publish an event with the context.

```csharp
 public class TriggerExample : Trigger<string>
 {
    /// <inheritdoc/>
    public override Task<string> ExecuteAsync()
    {
        this.Logger.LogInformation($"Trigger {this.GetType().Name} executed at {DateTime.Now}");

        return Task.FromResult("Hello");
    }
 }
```

- `ScheduleTrigger{T}`, its purpose is to publish an event with a given context in a certain period or with some periodicity. This uses Quartz .Net to schedule a Job and delegates the responsibility of publishing to the `ScheduledJob{T}`.

```csharp
 public class ScheduledTriggerExample : ScheduleTrigger<string>
 {
    /// <inheritdoc/>
    public override Task<string> ExecuteAsync()
    {
        this.Logger.LogInformation($"Trigger {this.GetType().Name} executed at {DateTime.Now}");

        return Task.FromResult("Hello");
    }
 }
```

- `ContinuousTrigger{T}`, its purpose is to publish events continuously subject to a condition, ContinueAsync().

```csharp
public class ContinuousTriggerExample : ContinuousTrigger<string>
{
    /// <inheritdoc/>
    public override Task<string> ExecuteAsync()
    {
        this.Logger.LogInformation($"Trigger {this.GetType().Name} executed at {DateTime.Now}");

        return Task.FromResult("Hello");
    }

    /// <inheritdoc/>
    public override Task<bool> ContinueAsync()
    {
        return Task.FromResult(true);
    }
}

```

- `SaTrigger`, stand-alone trigger, its purpose is to execute some logic defined by those who implement this component. This component is used when there is no claim to have an trigger-action relationship but only a trigger.

```csharp
public class TriggerActionExample : TriggerAction<string>
{
    /// <inheritdoc/>
    public override async Task ExecuteAsync()
    {
        this.Logger.LogInformation($"Trigger {this.GetType().Name} executed at {DateTime.Now}");

        await Task.Delay(1000).ConfigureAwait(false);
    }
}

```

- `EventTrigger`, its purpose is to subscribe to events given a `PipeboxConfig` and to process them.

```csharp
  public class EventTriggerExample : EventTriggerAction<string>
  {
    /// <inheritdoc/>
    public override Task<PipeboxConfig> GetConfigurationsAsync()
    {
        // get the pipebox configuration...
    }

    /// <inheritdoc/>
    public override async Task<bool> HandleAsync(IEventBusEvent<string> eventBusEvent)
    {
        // process the event...
    }

  }

```

#### Action

All of this action types implement the `IAction{T, TConfig}`, to know more see the [Primavera.Hydrogen.Taskbox.Abstractions][REF_TBX_ABS].

- `Action{T}`, its purpose is to subscribe to an event and when this is raised it will execute some logic based in the received event payload/context.

```csharp
  public class ActionExample : Primavera.Hydrogen.Taskbox.Action<string>
  {
    /// <inheritdoc/>
    public override async Task ExecuteAsync(string context)
    {
        // process the received context...
    }
  }

```

Consider the following example of an `TaskboxConfig` using a `EventTrigger` and a `ScheduleTrigger`.

```json
{
  "version": "",
  "expressions": [],
  "tasks": [
    {
      "id": "Task1",
      "name": "Event Task",
      "description": "The task that will handle the events.",
      "contextType": "System.String",
      "active": "True",
      "trigger": {
        "id": "EventTrigger",
        "name": "EventTrigger",
        "description": "Handles the events.",
        "type": "Primavera.Hydrogen.Taskbox.EventTrigger, Primavera.Hydrogen.Taskbox",
      }
    },
    {
      "id": "Task2",
      "name": "Scheduled task",
      "description": "The scheduled task.",
      "contextType": "System.String",
      "active": "True",
      "trigger": {
        "id": "ScheduledTrigger1",
        "name": "ScheduledTrigger",
        "description": "The schedule trigger",
        "type": "Primavera.Hydrogen.Taskbox.ScheduledTrigger, Primavera.Hydrogen.Taskbox",
        "configStr": "",
        "context":"",
        "cronExp": "Seconds=0; Minutes=29; Hours=12;"
      },
      "actions": [
        {
          "id": "ScheduledAction1",
          "name": "ScheduledAction 1",
          "description": "Scheduled Action 1",
          "type": "Primavera.Hydrogen.Taskbox.ScheduledAction, Primavera.Hydrogen.Taskbox",
          "configStr": "",
          "continuousExecution": "False"
        }
      ]
    }
  ]
}
```

### Task Details

Name | Description
:--- | :---
`id` | The task identifier.
`name` | The task name.
`description` | The task description.
`active` | Used to determinate if the task is active, if true the taskbox will process.

### Trigger Details

Name | Description
:--- | :---
`id` | The trigger identifier.
`name` | The trigger name.
`description` | The trigger description.
`type` | The trigger assembly name, this is used by the invoker in order to instantiate this.
`configStr` | Used to pass parameters to the trigger.
`context` | Used to pass a context to the trigger.

### Action Details

Name | Description
:--- | :---
`id` | The action identifier.
`name` | The action name.
`description` | The action description.
`type` | The action assembly name, this is used by the invoker in order to instantiate this.
`configStr` | Used to pass parameters to the action.

**Notice**, that the communication between the trigger and the actions is established using the `Primavera.Hydrogen.EventBus.InMemory`. To know more about this component see [this.][REF_EBINMEM]

## Taskbox Service

### `TaskboxService`

The `TaskboxService` is integrated with the .NET Core dependency injection engine, so it can be easily used in any .NET Core project, in particular the ASP.NET Core projects.

This component is responsible for starting and stopping the `TaskboxEngine`, this can order the engine to process a `TaskboxConfig` or it can queue a `TbxTask` in a background queue where the engine is listening to. Furthermore, it can pass functions for the engine to process.

Consider the following examples on how to interact with the `TaskboxService`.

- Start

```csharp
 TaskboxConfig config = TaskboxConfig.Create(json);

 await this.TaskboxService.StartAsync(config).ConfigureAwait(false);
```

```csharp
 await this.TaskboxService.StartAsync().ConfigureAwait(false);
```

- Stop

```csharp
 await this.TaskboxService.StopAsync().ConfigureAwait(false);
```

- Execute Task

```csharp
 this.TaskboxService.ExecuteTask(TbxTask);
```

- Cancel Task

```csharp
 this.TaskboxService.CancelTask("taskId");
```

- Get running tasks

```csharp
 this.TaskboxService.GetRunningTasks();
```

### `TaskboxEngine`

The `TaskboxEngine` is the hearth of this service, it implements the [BackgroundService](https://docs.microsoft.com/en-us/dotnet/api/microsoft.extensions.hosting.backgroundservice?view=dotnet-plat-ext-3.0).  
This is responsible for executing all the tasks and funcs that are received from the `TaskboxService`. To accomplish the execution of those tasks the engine uses the `TaskboxInvoker` to invoke the trigger and the actions. This engine also has a BackgroundQueue where it's waiting for tasks to process.

### `TaskboxInvoker`

The `TaskboxInvoker`, as the name implies is responsible for invoking all the components that compose a `TbxTask`, triggers and actions.

### Event Handling

This section holds all the components related to event handling.

#### `EventMetaData`

This is used to map an event regardless of is type, making this class a resource that allows us to handle events generically.

### `EventBusManager`

It's a generic subscriber and unsubscriber, it can do both operations for any event type that is given. This manager works with the `EventMetaData` class.

### `EventBusServiceFactory`

The `EventBusServiceFactory` provides a way to get an  `IEventBusService` instance based on a strategy(InMemory or Azure).

### `SchedulerFactory`

The `SchedulerFactory` provides a way to get an `IScheduler` instance. The `IScheduler` is used to schedule and run tasks in a certain period or with some periodicity.

### Dependency Injection

#### Options Class's

- `TaskboxOptions` it's a class that holds the configurations for taskbox service.

#### Service collection extensions

- `TaskboxServiceCollectionExtensions` this class provides methods to help add and configure a taskbox.

Consider the following example, on how to add the taskbox service to your app.

```csharp

ServiceCollection services = new ServiceCollection();

services.AddTaskbox();

```

**Notice** , that while adding the taskbox service to your service collection, this will carry two external services, the [Pipeboxes](./Pipeboxes.md)  and the [Quartz .Net Scheduler](https://www.quartz-scheduler.net/).  

## Scheduling

The `Primavera.Hydrogen.Scheduling` namespace provides types that support scheduling operations.

### `Cron`

Type capable of handling and interpreting [CRON expressions](https://en.wikipedia.org/wiki/Cron#CRON_expression).

After being instantiated through an UNIX-like or a configuration string based CRON expression, the resulting `Cron` object is able to return the expression next occurrence as a time instant or a time interval thought the `GetNextOccurrenceInstant` or `GetNextOccurrenceInterval` method.

```csharp
Cron unixCron = new Cron("0 30 9 ? * * *");

DateTimeOffset instant = unixCron.GetNextOccurrenceInstant();
```

```csharp
Cron configStringCron = new Cron("Seconds=0; Minutes=30; Hours=9;");

TimeSpan interval = configStringCron.GetNextOccurrenceInterval();
```

### `CronCollection`

Type capable of storing a collection of `Cron` types. The collection is able to evaluate all its elements and return the nearest occurrence as a time instant or a time interval.

```csharp
IEnumerable<string> crons = new List<string>
{
    "Seconds=0; Minutes=30; Hours=9;",
    "0 0 10 ? * * *"
};

CronCollection cronCollection = new CronCollection(crons);

DateTimeOffset instant = cronCollection.GetNextOccurrenceInstant();

```
