# How to publish events to EventBus on a Microservice

## Getting started 

If needed, a microservice can publish messages to eventbus. To publish a message, you should: 
* [Implement the EventBusService (in the Startup)](../ref/hydrogen-2.0/EventBus.Azure.md#implementing-the-service);
* Create the IEventBusManager class in the Contracts folder and add your publish method;
* Create the EventBusManager in the Managers folder and implement your publish method like:
```c#
public async Task PublishYourEvent(string message)
{
    IEventBusEvent<string> @event = new AzureEventBusEvent<string>()
    {
        Body = message
    };

    @event.Properties.Add("MyFilter", "1.0");
    @event.Properties.Add("AnotherFilter", "PublicPrimavera");

    await this.EventBus.PublishAsync("myPublicationTopic", @event).ConfigureAwait(false);
}
```

> Note: You can read more about EventBus publication [here;](../ref/hydrogen-2.0/EventBus.Azure.md#publishing-an-event)

* Inject the ```IEventBusManager``` in the class where you want to publish the event via Dependency Injection:

```c#
private IEventBusManager EventBusManager
{
    get
    {
        return this.serviceProvider.GetRequiredService<IEventBusManager>();
    }
}
```

* Call the method to publish the event in your class:

```c#
public async Task SomeExampleMethod()
{
    await this.EventBusManager.PublishYourEvent("My eventBus message!").ConfigureAwait(false);
}
```

## Full example

### Startup (DependencyInjection method)
```c#
/// <summary>
/// Called to add additional (custom) services to the service collection.
/// </summary>
/// <param name="services">The service collection.</param>
/// <param name="hostConfiguration">The host configuration.</param>
protected override void AddAdditionalServices(IServiceCollection services, HostConfiguration hostConfiguration)
{
    base.AddAdditionalServices(services, hostConfiguration);

    services.AddAzureEventBus((options) =>
    {
	    options.ConnectionString = eventBusConfiguration.Address;
	    options.EventHandlerOptions = new AzureEventBusEventHandlerOptions(eventBusConfiguration.AutoComplete, eventBusConfiguration.MaxConcurrentCalls);
	    options.RetryStrategy = new ExponentialBackoffRetryStrategy();
    })
    .AddSingleton<IEventBusManager, EventBusManager>()
    .AddSingleton<MyPublisherClassExample>();

    services.AddLogging();
}
```

### IEventBusManager & EventBusManager

```c#
public interface IEventBusManager
{
    #region Methods

    /// <summary>
    /// Publishes your event.
    /// </summary>
    /// <param name="message">The message.</param>
    /// <returns><see cref="System.Threading.Tasks.Task"/> representing the operation.</returns>
    public Task PublishYourEvent(string message);

    #endregion
}
```

```c#
public class EventBusManager : IEventBusManager
    {
        #region Fields

        /// <summary>
        /// The service provider.
        /// </summary>
        private readonly IServiceProvider serviceProvider;

        #endregion

        #region Private Properties

        /// <summary>
        /// Gets the event bus.
        /// </summary>
        /// <value>The event bus.</value>
        private IEventBusService EventBus
        {
            get
            {
                return this.serviceProvider.GetRequiredService<IEventBusService>();
            }
        }

        #endregion

        #region Constructors

        /// <summary>
        /// Initializes a new instance of the <see cref="EventBusManager"/> class.
        /// </summary>
        /// <param name="serviceProvider">The service provider.</param>
        public EventBusManager(IServiceProvider serviceProvider)
        {
            this.serviceProvider = serviceProvider;
        }

        #endregion

        #region Public Methods

        /// <summary>
        /// Publishes your event.
        /// </summary>
        /// <param name="message">The message.</param>
        /// <returns>A <see cref="Task"/> representing the asynchronous operation.</returns>
        public async Task PublishYourEvent(string message)
        {
            IEventBusEvent<string> @event = new AzureEventBusEvent<string>()
            {
                Body = message
            };

            @event.Properties.Add("MyFilter", "1.0");
            @event.Properties.Add("AnotherFilter", "PublicPrimavera");

            await this.EventBus.PublishAsync("myPublicationTopic", @event).ConfigureAwait(false);
        }

        #endregion
    }
```

### MyPublisherClassExample

```c#
/// <summary>
/// My publisher class example.
/// </summary>
public class MyPublisherClassExample
{
    #region Fields

    /// <summary>
    /// The service provider.
    /// </summary>
    private readonly IServiceProvider serviceProvider;

    #endregion

    #region Private Properties

    /// <summary>
    /// Gets the event bus.
    /// </summary>
    /// <value>The event bus.</value>
    private EventBusManager EventBusManager
    {
        get
        {
            return this.serviceProvider.GetRequiredService<EventBusManager>();
        }
    }

    #endregion

    #region Constructors

    /// <summary>
    /// Initializes a new instance of the <see cref="MyPublisherClassExample"/> class.
    /// </summary>
    /// <param name="serviceProvider">The service provider.</param>
    public MyPublisherClassExample(IServiceProvider serviceProvider)
    {
        this.serviceProvider = serviceProvider;
    }

    #endregion

    #region Methods

    /// <summary>
    /// Some example method.
    /// </summary>
    /// <returns>A <see cref="Task"/> representing the asynchronous operation.</returns>
    public async Task SomeExampleMethod()
    {
        await this.EventBusManager.PublishYourEvent("My eventBus message!").ConfigureAwait(false);
    }

    #endregion
}
```