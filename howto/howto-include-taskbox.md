# How to include a task box in a micro service

Starting with the scenario where you already have a microservice, this document will help with the necessary steps to include a task box in a microservice. To know more about how to create a new microservice in Visual Studio [see this.](howto-create-new-microservice.md)

## Packages 

**First, you need to install the following packages:**

 - Primavera.Hydrogen.Taskbox
 - Primavera.Hydrogen.EventBus.Azure

After installing the packages, you have all the tools to create a taskbox.

## Add Background Worker

To subscribe all the events in the host startup is necessary to create a mechanism that makes it possible, the approach is to create a background worker that will do this job for you. To know more on how to add a background worker to your microservice [see this](howto-add-background-service.md).

Now that you have a background worker let's build the configuration for your taskbox.

## Add Pipebox Configuration

 First, you need to define what configuration will run in this taskbox sample. The `PipeboxConfig` is a configuration that holds all the events, pipelines and handlers.

 See the [PipeboxConfig](../ref/hydrogen-2.0/Pipeboxes.md) to know more about how to create a configuration.

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

In the CustomCode folder, create a new folder called, LocalConfig. Right-click in the previously created folder and add a new item, search for JSON file and in the name type "myconfig.json".


## Subscribing Events

In the background worker. Add the following property, after the beginning of the class.

```csharp
     public IEventBusManager EventBusManager
     {
       get
       {
          return this.ServiceProvider.GetRequiredService<IEventBusManager>();
       }
     }
```


Also in the background worker, locate the method ExecuteAsync, and in this method paste the following code, this shows a way on how to get the configurations and subscribe to the events. TaskBox supplies a set of tools to make it easier to implement. The first one is after creating the `PipeboxConfig` it's possible to get the list of triggers by an extension method `.GetEventTypes()`. Another tool supplied by the TaskBox is a Manager, `IEventBusManager`, which makes it easier to subscribe and unsubscribe an event.

```csharp
   public override async Task ExecuteAsync(CancellationToken cancellationToken)
   {
      string json = File.ReadAllText(this.localConfig);

      PipeboxConfig pipeboxConfig = PipeboxConfig.Create(json);

      List<TriggeredEvent> triggeredEvents = pipeboxConfig.GetEventTypes();

      foreach (TriggeredEvent triggeredEvent in triggeredEvents)
      {
        await this.EventBusManager.Subscribe(triggeredEvent).ConfigureAwait(false);
      }
```


## Receive an Event

### `EventBusHandler<T>`

 To receive the event when it's raised, it necessary to create a handler. So to accomplish this, it's necessary to create a new item/class in the CustomCode folder, let's call it, **MyCustomHandler**,  that will inherit from the `EventBusHandler<T>` which is an abstract handler present in the Taskbox to help implement this type of handler by the obligation to override certain properties and methods.

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


## Startup

Locate the CustomCode folder under the WebApi project and add a class named Startup. Override the method `AddAdditionalServices` as follows.

```csharp
  protected override void AddAdditionalServices(IServiceCollection services, HostConfiguration hostConfiguration)
  {
    SmartGuard.NotNull(() => services, services);

    base.AddAdditionalServices(services, hostConfiguration);

    services.AddTaskboxService(typeof(MyCustomHandler), typeof(string));
  }
```
Also in the Startup.cs, override the AddConfiguration, replace it by the code below.

```csharp

 protected override HostConfiguration AddConfiguration(IServiceCollection services)
 {
    SmartGuard.NotNull(() => services, services);

    services.AddTaskboxConfigurations(this.Configuration);

    return base.AddConfiguration(services);
 }
```

### Configurations
The Taskbox provides a class to help implement the configurations for the `EventBus` called **EventBusOptions** and for the `ITaskboxWorkersManager` one called **TaskboxOptions**, here you can set the maximum number of workers that will be working simultaneously and the worker waiting time when the pool of workers is full.
In the appsettings.json copy and paste the following code.

```json
  "TaskboxOptions": {
    "MaxWorkers": 10,
    "WorkerWaitingTime": 5000
  },
  "EventBusOptions": {
    "Address": "EventBusEndpoint",
    "PublicationTopic": "my-publication-topic",
    "NotificationTopic": "my-notification-topic",
    "SubscriptionTopics": ["myTopic"],
    "MaxConcurrentCalls": 10,
    "AutoComplete":false
  }
```
To know more on how to configure the event bus, [click here.](../ref/hydrogen-2.0/EventBus.md)

### Add Pipeline Handler

Next, in the Configuration chapter, the pipeline has a handler called `MyCustomHttpHandler`, and this was configured to be a post request, in the `configStr` section it's possible to set some properties, see [`PipeboxConfig`](../ref/hydrogen-2.0/Pipeboxes.md) documentation.
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

In the CustomCode folder add a new class, called MyCustomHttpHandler. Replace the class implementation by the following code.

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

Next, in the Startup.cs, go to the method AddAdditionalServices and add the following line of code after the AddTaskboxService previously added.
 ```csharp
    services.AddTransient<MyCustomHttpHandler>();
 ```
### Use Workers Manager

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

                TriggeredEvent trigger = eventBusEvent.BuildTriggeredEvent();

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

See the [`ITaskboxWorkersManager`](../ref/hydrogen-2.0/Taskbox.md) and [`ITaskboxWorker`](../ref/hydrogen-2.0/Taskbox.md) documentation.

The bases of this tutorial is to give a brief introduction, on how a taskbox can be built with few steps, but it's recommended to read all the documentation related to the components that are used in this tutorial.



