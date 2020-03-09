# How to include a task box in a micro service

This guide describes the steps required to include a task box in a background service.

## Packages 

**First, you need to install the following packages:**

 - Primavera.Hydrogen.Taskbox
 - Primavera.Hydrogen.EventBus.Azure

After installing the packages, you have all the tools to create a taskbox.

## Startup

### Configurations
The Taskbox provides a class to help implement the configurations for the `EventBus` called **EventBusConfiguration** and for the `IPipeboxWorkersManager` one called **PipeboxWorkersConfig**, here you can set the maximum number of workers that will be working simultaneously and the worker waiting time when the pool of workers is full.

```json
  "PipeboxWorkersConfig": {
    "MaxWorkers": 10,
    "WorkerWaitingTime": 5000
  },
  "EventBusConfiguration": {
    "Address": "EventBusEndpoint",
    "PublicationTopic": "my-publication-topic",
    "NotificationTopic": "my-notification-topic",
    "SubscriptionTopics": ["myTopic"],
    "MaxConcurrentCalls": 10,
    "AutoComplete":false
  },
```
To add the configuration do as follows:
```csharp
  services.Configure<EventBusConfiguration>(this.Configuration.GetSection(nameof(EventBusConfiguration))).AddOptionsSnapshot<EventBusConfiguration>();
  services.Configure<PipeboxWorkersConfig>(this.Configuration.GetSection(nameof(PipeboxWorkersConfig))).AddOptionsSnapshot<PipeboxWorkersConfig>();
```

### Services
 In the startup is necessary to register all the services required for this implementation.
 ```csharp
    services.AddSingleton<IPipeboxWorkersManager, PipeboxWorkersManager>();

    services.AddTransient(typeof(PipeboxWorker<>));

    services.AddTransient(typeof(IEventBusEventHandler<>), typeof(MyCustomHandler));

    services.AddTransient(typeof(IEventBusEventFilters<>), typeof(AzureEventBusEventFilters<>));

    services.AddSingleton<IEventBusManager, EventBusManager>();

    services.AddTransient<MyCustomHttpHandler>();
 ```

 - The [`IPipeBoxWorkersManager`](../ref/hydrogen-2.0/Taskbox.md) is responsible for managing a pool of workers.
 - The [`PipeboxWorker<T>`](../ref/hydrogen-2.0/Taskbox.md) is the type of  Worker present in the pool of workers.
 - The [`MyCustomHandler<>`](../ref/hydrogen-2.0/Taskbox.md) is the handler that receives the event, it will be created further on.
 - The [`AzureEventBusEventFilters<>`](../ref/hydrogen-2.0/EventBus.Abstractions.md) is a component necessary to support filters in the event.
 - The [`IEventBusManager`](../ref/hydrogen-2.0/Taskbox.md) is a manager to subscribe and unsubscribe an event.
 - The [`MyCustomHttpHandler<>`](../ref/hydrogen-2.0/Taskbox.md) is the handler that will be defined in the configuration and created further on.


 ## Configuration

 First, you need to define what configuration will run in this taskbox sample. The `PipeboxConfig` is a configuration that holds all the events, pipelines and handlers.

 See the [PipeboxConfig](../ref/hydrogen-2.0/Taskbox.md) to know more about how to create a configuration.

Let's assume that this is our configuration, it consists in:
- an Event called *MyEventType*. Notice that it's possible to add properties to the event.
- a Pipeline that is triggered by the EventType.
- a Handler inside the pipeline that will do something with the event.

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
In this scenario our event is string typed and the topic where it's published is myTopic.

## Subscribe an Event
Now, the Taskbox must subscribe to this eventType. To accomplish this, you can use the lithium to help us modulate a worker that, when the TaskBox host is starting, will subscribe to all the events in the configuration.

The code below shows a way on how to get the configurations and subscribe to the events.
TaskBox supplies a set of tools to make it easier to implement. The first one is after creating the `PipeboxConfig` it's possible to get the list of triggers by an extension method `.GetEventTypes()`.
Another tool supplied by the TaskBox is a Manager, `EventBusManager`, which makes it easier to subscribe and unsubscribe an event.

```csharp
    public override async Task ExecuteAsync(CancellationToken cancellationToken)
    {
        string json = File.ReadAllText(filepath.json);

        PipeboxConfig pipeboxConfig = PipeboxConfig.Create(json);

        List<EventTrigger> eventTriggers = pipeboxConfig.GetEventTypes();

        foreach (EventTrigger eventTrigger in eventTriggers)
        {
            await this.EventBusManager.Subscribe(eventTrigger).ConfigureAwait(false);
        }
    }
```

## Receive an Event

### `EventBusHandler<T>`

 To receive the event when it's raised, it necessary to create a handler. So to accomplish this, it's necessary to create a new item/class, let's call it, **MyCustomHandler**,  that will inherit from the `EventBusHandler<T>` which is an abstract handler present in the Taskbox to help implement this type of handler by the obligation to override certain properties and methods.

```csharp
    public class MyCustomHandler : EventBusHandler<string>
    {
    }
```

The `EventBusHandler<T>` is an abstract class, so it's necessary to override some properties and methods.

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

    public override IPipeboxWorkersManager WorkersManager
    {
        get
        {
            return this.ServiceProvider.GetRequiredService<IPipeboxWorkersManager>();
        }
    }
        
    public override Task<bool> Handle(IEventBusEvent<string> eventBusEvent)
    {
    }
```
- In the constructor, is imperative to inject the service provider.
- IEventBus, see also the [Primavera.Hydrogen.EventBus.Azure](../ref/hydrogen-2.0/EventBus.Azure.md) documentation to help to configure the EventBus.
- Handle, is where the event will hit when raised. It is also here where all the logic related to the event should be.


### Pipeline Handler

Next, in the Configuration chapter, the pipeline has a handler called `MyCustomHttpHandler`, and this was configured to be a post request, in the `configStr` section it's possible to set some properties, see [`PipeboxConfig`](../ref/hydrogen-2.0/Taskbox.md) documentation.
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

To create an HttpHandler, the Taskbox also supplies a way to make things easier, there is an abstract and generic class called `HttpHandler<T>` and all that is necessary, is to create a class and inherit from `HttpHandler<T>`. It will obligate the implementation of four methods.

```csharp
    public class MyCustomHttpHandler : HttpHandler<string>
    {
        public override string SetEndpoint()
        {
            return this.Configuration.Endpoint;
        }

        public override Task<ByteArrayContent> SetPostObject()
        {
            string contentType = "application/json";

            byte[] buffer = System.Text.Encoding.UTF8.GetBytes(this.Data);

            ByteArrayContent byteContent = new ByteArrayContent(buffer);

            byteContent.Headers.ContentType = new MediaTypeHeaderValue(contentType);

            return Task.FromResult(byteContent);
        }

        public override Task SaveResponse(HttpResponseMessage responseMessage)
        {
            throw new NotImplementedException();
        }

        public override Task ExecuteAsync(PipeboxContext<string> context, CancellationToken cancellationToken = default)
        {
            return base.ExecuteAsync(context, cancellationToken);
        }
    }
```

- `SetEndpoint()` - is where its defined the endpoint for the Http request, note that, because you define the endpoint in the handler configuration, it's possible to access directly to this configuration parameter.
- `SetPostObject()` - is here where is built the post object. Based on the HttpMethod, provided in the handler config, this method is called or not.
- `SaveResponse()` - here is where the Http response can be processed.

### Workers Manager

To start a worker, it's necessary to have three parameters, the **PipeboxConfig**, the **PipeboxContext** and the **Pipeline Id** that will be executed. 

```csharp
await this.WorkersManager.StartWorker(pipeboxConfig, pipeboxContext, pipelineId).ConfigureAwait(false);
```

This will instantiate and start a worker with the given configurations. On a deeper level, this worker will build a `Pipebox` with those input parameters and this `Pipebox` will execute the pipeline.

Applying this logic to the handler, that was built, it would be similar to this.

```csharp
        public override Task<bool> Handle(IEventBusEvent<string> eventBusEvent)
        {
            SmartGuard.NotNull(() => eventBusEvent, eventBusEvent);

            if (!this.InProcessEvents.Any(p => p.Key == eventBusEvent.Id))
            {
                this.InProcessEvents.Add(eventBusEvent.Id, new ConcurrentBag<Task>());

                string json = File.ReadAllText(this.localConfig);

                PipeboxConfig pipeboxConfig = PipeboxConfig.Create(json);

                EventTrigger trigger = eventBusEvent.BuildEventTrigger();

                // Searches in the configuration for all the pipelines that are triggered by the event received
                List<PipelineConfig> pipelines = pipeboxConfig.Pipelines.Where(x => x.TriggerExists(trigger)).ToList();
                ConcurrentBag<Task> tasks = new ConcurrentBag<Task>();

                // Starts in parallel a set of workers based on the number of pipelines necessary to be executed
                Parallel.ForEach(pipelines, pipeline =>
                {
                    PipeboxContext<string> taskContext = new PipeboxContext<string>(trigger.EventContext as string);
                    tasks.Add(Task.Run(async () =>
                    {
                        await this.WorkersManager.StartWorker(pipeboxConfig, taskContext, pipeline.Id).ConfigureAwait(false);
                    }));
                });

            ...
        }
```

See the [`IPipeboxWorkersManager`](../ref/hydrogen-2.0/Taskbox.md) and [`IPipeboxWorker`](../ref/hydrogen-2.0/Taskbox.md) documentation.

The bases of this tutorial is to give a brief introduction, on how a taskbox can be built with few steps, but it's recommended to read all the documentation related to the components that are used in this tutorial.



