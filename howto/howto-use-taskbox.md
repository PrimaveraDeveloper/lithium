# How to use a taskbox in a microservice

This guide describes the steps to use a taskbox in a microservice.

## Adding Packages

Start by adding the following NuGet packages to the `WebAPI` project:

 - Primavera.Hydrogen.Taskbox
 - Primavera.Hydrogen.EventBus.Azure

## Adding a the Background Worker

To subscribe the events in the host application startup you will need to [add a background service](howto-add-background-service.md).

The next step is to build the configuration for the taskbox.

## Adding the Pipebox Configuration

 First, you need to define the configuration used by the taskbox. `PipeboxConfig` describes all the events, pipelines and handlers.

 > See [PipeboxConfig](../ref/hydrogen-2.0/Pipeboxes.md) for more information.

Consider the following configuration. It consists of

- an Event called `MyEventType`. Notice that it's possible to add properties to the event.
- a single pipeline that is triggered by that event type.
- one handler inside the pipeline that will do something the events of that type.

```json
{
  "version": "1.0",
  "expressions": [
    {
      "pattern": "%MyEventType%",
      "value": "{\"eventType\":\"System.String\", \"eventTopic\":\"myTopic\"}"
    },
    {
      "pattern": "%MyEventTypeWithProperties%",
      "value": "{\"eventType\":\"System.String\", \"eventTopic\":\"mytopic\",\"eventProperties\":[{\"key\":\"test\",\"value\":\"1\"}]}"
    }
  ],
  "pipelines": [
    {
      "id": "p1", 
      "tag": "My first pipeline",
      "configStr":"[%MyEventType%]",   
      "handlers": [
        {
          "id": "h1",
          "order": "1",
          "tag": "My frist handler",
          "type": "Primavera.Lithium.Sample.WebApi.MyCustomHttpHandler, Primavera.Lithium.Sample.WebApi",
          "configStr": "httpmethod=post; endpoint=https://httpbin.org/post;",
          "active": "True"
        },
      ]
    }
  ]
}
```
In this scenario our event is a string and the topic where it's published is called `myTopic`.

Now, in the `CustomCode` folder, create a new folder called `LocalConfig`. Add a file named `myconfig.json` to it. 

## Subscribing Events

In the background service, add the following property:

```csharp
public IEventBusManager EventBusManager
{
    get
    {
        return this.ServiceProvider.GetRequiredService<IEventBusManager>();
    }
}
```

Next, locate the method named `ExecuteAsync`, and the following code:

```csharp
public override async Task ExecuteAsync(CancellationToken cancellationToken)
{
    string json = File.ReadAllText(this.localConfig);

    PipeboxConfig pipeboxConfig = PipeboxConfig.Create(json);

    List<TriggeredEvent> triggeredEvents = pipeboxConfig.GetEventTypes();

    foreach (TriggeredEvent triggeredEvent in triggeredEvents)
    {
        await this.EventBusManager.SubscribeAsync(triggeredEvent).ConfigureAwait(false);
    }
}
```

This shows a way on how to retrieving the configuration and subscribing the events.

`Primavera.Hydrogen.TaskBox` supplies a set of tools that make it easier:

- You can get all the triggers by invoking `GetEventTypes()` on `PipeboxConfig`.
- `IEventBusManager` allows subscribing and unsubscribing events.

## Receiving an Event

### `EventBusHandler<T>`

To receive the event, you will need to create an handler. To accomplish this, create a new class in the `CustomCode` folder and call it `MyCustomHandler`. It should inherit from `EventBusHandler<T>`:

```csharp
public class MyCustomHandler : EventBusHandler<string>
{
}
```

`EventBusHandler<T>` is an abstract class, so you will need to override some properties and some methods.

```csharp
public MyCustomHandler(IServiceProvider serviceProvider) 
  : base(serviceProvider)
{
}

public override ILogger<EventBusHandler<string>> Logger
{
    get
    {
        return this.ServiceProvider.GetRequiredService<ILogger<EventBusHandler<string>>>();
    }
}

public override IEventBus EventBus
{
    get
    {
        return this.ServiceProvider.GetRequiredService<IEventBus>();
    }
}

public override ITaskboxService TaskboxService
{
    get
    {
        return this.ServiceProvider.GetRequiredService<ITaskboxService>();
    }
}
    
public override Task<bool> HandleAsync(IEventBusEvent<string> eventBusEvent)
{
  (...)
}
```

Notice that:

- The service provider should be injected in the constructor.
- `IEventBus` allows raising and receiving events. See [Primavera.Hydrogen.EventBus.Azure](../ref/hydrogen-2.0/EventBus.Azure.md) for more information.
- `HandleAsync` will be called every time the event is received. The specific logic for handling it should be implemented here.

## Startup

To configure the taskbox you will also need to customize the microservice startup logic.

First, override `AddAdditionalServices()` to add the taskbox service:

```csharp
protected override void AddAdditionalServices(IServiceCollection services, HostConfiguration hostConfiguration)
{
    SmartGuard.NotNull(() => services, services);

    base.AddAdditionalServices(services, hostConfiguration);

    services.AddTaskbox();
}
```

Override `AddConfiguration()`:

```csharp
protected override HostConfiguration AddConfiguration(IServiceCollection services)
{
    SmartGuard.NotNull(() => services, services);

    services
        .Configure<TaskboxOptions>(
            this.Configuration.GetSection(nameof(TaskboxOptions)));

    services
        .AddOptionsSnapshot<TaskboxConfiguration>();

    return base.AddConfiguration(services);
}
```

### Configuration

The following configuration options also need to be specified to setup the taskbox service. These need to be added to `appsettings.json`:

```json
{
    "TaskboxOptions": {
    "MaxWorkers": 10,
    "WorkerWaitingTime": 5000,
    "EventHandling": {
      "EventType": "System.String",
      "HandlerType": "Primavera.Lithium.Sample.WebApi.MyCustomHandler, Primavera.Lithium.Sample.WebApi",
      "PublicationTopic": "my-publication-topic",
      "NotificationTopic": "my-notification-topic",
      "SubscriptionTopics": [ "myTopic" ]
    }
  },
}
```

### Adding the Pipeline Handler

The pipeline handler also needs to be configured:

```json
{
    "id": "h1",
    "order": "1",
    "tag": "My frist handler",
    "type": "Primavera.Lithium.Sample.WebApi.MyCustomHttpHandler, Primavera.Lithium.Sample.WebApi",
    "configStr": "httpmethod=post; endpoint=https://httpbin.org/post;",
    "active": "True"
}
```

Primavera.Hydrogen.Taskbox also provides an abstraction to implement an HTTP handler (an handler capable of performing HTTP requests).

In the `CustomCode` folder add a new class name `MyCustomHttpHandler`. Add the following code:

```csharp
public class MyCustomHttpHandler : HttpHandler<string>
{
    public override string SetRequestEndpoint()
    {
        return this.Configuration.Endpoint;
    }

    public override Task<ByteArrayContent> PrepareRequestAsync()
    {
        string contentType = "application/json";

        byte[] buffer = System.Text.Encoding.UTF8.GetBytes(this.Data);

        ByteArrayContent byteContent = new ByteArrayContent(buffer);

        byteContent.Headers.ContentType = new MediaTypeHeaderValue(contentType);

        return Task.FromResult(byteContent);
    }

    public override Task HandleResponseAsync(HttpResponseMessage responseMessage)
    {
        throw new NotImplementedException();
    }

    public override Task ExecuteAsync(PipeboxContext<string> context, CancellationToken cancellationToken = default)
    {
        return base.ExecuteAsync(context, cancellationToken);
    }
}
```

Notice the following:

- `SetRequestEndpoint()` is where the HTTP request endpoint is defined (since that endpoint is specified in the handler configuration, it is available from the `Configuration` property).
- `PrepareRequestAsync()` is where it's prepared the request data.
- `HandleResponseAsync()` is where the response received can be processed.

You will need to add this new handler to the service collection.

Add the following to `AddAdditionalServices()`:

```csharp
services.AddTransient<MyCustomHttpHandler>();
```

### Using the TaskboxService

To start a worker you will need the `PipeboxConfig`, the `PipeboxContext` and the identifier of the pipeline that should be executed:

```csharp
await this.TaskboxService.StartWorkerAsync(pipeboxConfig, pipeboxContext, pipelineId).ConfigureAwait(false);
```

This will instantiate and start a worker with the given configuration options. On a deeper level, this worker will build a `Pipebox` with those input parameters and it will be that `Pipebox` that executes the pipeline.

Implementing this to the previous handler, MyCustomHandler, would be similar to the following:

```csharp
public override Task<bool> HandleAsync(IEventBusEvent<string> eventBusEvent)
{
    SmartGuard.NotNull(() => eventBusEvent, eventBusEvent);

    if (!this.InProcessEvents.Any(p => p.Key == eventBusEvent.Id))
    {
        this.InProcessEvents.Add(eventBusEvent.Id, new ConcurrentBag<Task>());

        string json = File.ReadAllText(this.localConfig);

        PipeboxConfig pipeboxConfig = PipeboxConfig.Create(json);

        TriggeredEvent trigger = eventBusEvent.BuildTriggeredEvent();

        // Searches in the configuration for all the pipelines that are triggered by the event received
        
        List<PipelineConfig> pipelines = pipeboxConfig.Pipelines
          .Where(x => x.TriggerExists(trigger))
          .ToList();
        
        ConcurrentBag<Task> tasks = new ConcurrentBag<Task>();

        // Starts in parallel a set of workers based on the number of pipelines necessary to be executed

        List<Task> tasks = new List<Task>();

        pipelines.ForEach(p => tasks.Add(this.TaskboxService.StartWorker(pipeboxConfig, taskContext, pipeline.Id)));

        this.InProcessEvents[trigger.Id] = tasks;

        await Task.WhenAll(tasks).ConfigureAwait(false);

        ...
    }
}
```

Check the [Primavera.Hydrogen.Taskbox](../ref/hydrogen-2.0/Taskbox.md) documentation for more details.