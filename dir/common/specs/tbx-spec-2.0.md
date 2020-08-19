# Primavera.Lithium.Taskbox

Provides asynchronous multi-task processing for applications or services that need to improve performance, scalability and reusability. This component implements the asynchronous operation behaviour in the PRIMAVERA Elevation Platform. This process can follow different strategies.

## Taskbox Architecture

The taskbox microservice is divided into 3 different strategies. All of these use the taskbox service feature that is provided by the hydrogen library.

The following image shows how the microservice is organized.

<img src="./_assets/tbx_architecture.png" width="1000">

## Async Tasks Strategy

### EventTrigger

#### Implementation

For this strategy, the abstract class `EventTriggerAction` was used to implement the EventTrigger, it receives a `PipeboxConfig` used to subscribe the events. When the event is raised it reads the `PipeboxConfig` and retrieves the pipeline then exectes this pipeline using the `Pipebox`.

```csharp
 public class EventTrigger : EventTriggerAction<DataTransferObject>
 {
    /// <inheritdoc/>
    public override Task<PipeboxConfig> GetConfigurationsAsync()
    {
      //...
    }

    /// <inheritdoc/>
    public override async Task<bool> HandleAsync(IEventBusEvent<DataTransferObject> eventBusEvent)
    {
       //...  
    }
 }

```

Then it was added to the taskbox configuration the following task definition. Because it's an `Event Trigger Action` there is no need to have actions.

```json
{
  "id": "Task1",
  "name": "Event Task",
  "description": "The task that will handle the events.",
  "contextType": "Primavera.Hydrogen.Data.DataTransferObject, Primavera.Hydrogen.Core",
  "active": "True",
  "trigger": {
    "id": "EventTrigger",
    "name": "EventTrigger",
    "description": "Handles the events.",
    "type": "Primavera.Lithium.Taskbox.WebApi.AsyncTasks.EventTrigger, Primavera.Lithium.Taskbox.WebApi"
  }
}
```

<img src="./_assets/tbx_asynctasks_eventbus.png" width="1000">

### Async Task

The taskbox Service allows to create async tasks by Api and by client.

#### Create async task without cron expression

```csharp

AsyncTask asyncTask = new AsyncTask()
{
  Context = "Hello",
  CallbackUrl = new Uri("https://www.mycallback.com"),
  HttpRequestType = "POST",
  PersistTask = true
};

using TaskboxClient client = new TaskboxClient(...);

try
{
    ServiceOperationResult result = await client.AsyncTasks.AddAsyncTaskAsync(asyncTask).ConfigureAwait(false);
}
catch (ServiceException ex)
{
    (...)
}
```

This method creates in runtime a [`TbxTask`](../../../ref/hydrogen-2.0/Taskbox.md) based on the received `AsyncTask`. Because the `PersistTask` is set to true, this task will be added to the configurations, [`TaskboxConfig`](../../../ref/hydrogen-2.0/Taskbox.md), so that when the task box for some reason goes shuts down when it starts again it can start that task again.

The following image shows how the async task is processed.

<img src="./_assets/tbx_asyntasks_triggeraction.png" width="1000">

#### Create async task with cron expression

```csharp

AsyncTask asyncTask = new AsyncTask()
{
  Context = "Hello",
  CallbackUrl = new Uri("https://www.mycallback.com"),
  HttpRequestType = "POST",
  CronExpression = "0 */2 * ? * *"
  PersistTask = true
};

using TaskboxClient client = new TaskboxClient(...);

try
{
    ServiceOperationResult result = await client.AsyncTasks.AddAsyncTaskAsync(asyncTask).ConfigureAwait(false);
}
catch (ServiceException ex)
{
    (...)
}
```

This method creates in runtime a [`TbxTask`](../../../ref/hydrogen-2.0/Taskbox.md) based on the received `AsyncTask`. Because the `PersistTask` is set to true, this task will be added to the configurations, [`TaskboxConfig`](../../../ref/hydrogen-2.0/Taskbox.md), so that when the task box for some reason goes shuts down when it starts again it can start that task again.

The following image shows how the schedule async task is processed.

<img src="./_assets/tbx_asyntasks_schedule_triggeraction.png" width="1000">

## Webhooks Strategy

[**`UNDER DEVELOPMENT`**]

## Reminders Strategy

[**`UNDER DEVELOPMENT`**]

### EventBus

This resource is used to subscribe and unsubscribe to events on Service Bus topics. These topics and all that is related to the use of this resource is configured in the microservice settings.
To Know more about Primavera.Hydrogen.EventBus.Azure can be found [here](../../../ref/hydrogen-2.0/EventBus.Azure.md#Primavera.Hydrogen.EventBus.Azure).

### Taskbox

To Know more about Primavera.Hydrogen.Taskbox can be found [here](../../../ref/hydrogen-2.0/Taskbox.md).

## Details
<!-- markdown-link-check-disable -->
To Know more about this microservice click [here](https://tfs.primaverabss.com/tfs/P.TEC.Elevation/Lithium/_versionControl?path=%24%2FLithium%2FMicroservices%2FCommon%2FTBX%2FMainline%2Freadme.md&version=T&_a=preview).
<!-- markdown-link-check-enable -->
