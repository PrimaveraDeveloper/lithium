# How to add a background service to a microservice

This guide describes how to use background services in a microservice to perform asynchronous and/or repetitive work.

Background Services are a feature provided by Hydrogen, based on the .NET Core concept of hosted services (see IHostedService), that are very useful in multiple scenarios, in particular:

- To execute asynchronous work during the microservice application startup (work that may take too long to complete to be included in the startup logic).
- To execute repetitive work, like observing a queue of requests to perform some long-running tasks.

## Simple Background Service

A background service is "like a new thread" that is automatically initiated when the application starts and whose lifetime is managed by the platform, meaning that, when it is running, if the application is stopped for some reason, that background service will be notified of that request and given a short period of time to terminate gracefully (for example, to store its state so it can be resumed later).

## Timed Background Service

Timed background services are a special kind of background service that is designed to execute work periodically. You define the interval between executions, and the service will run forever (until is stopped), awaking every time that interval elapses to execute the specific custom logic.

## Queue Background Service

Unlike timed services, queue background services sleep by default and awake every time a piece of data is available in a queue (a memory queue). This kind of services are used often to respond to requests to perform long-running work.

## Background Worker

You can combine multiple background service by making them share the same worker - that is, the business logic, that actual executes the business logic.

An example of that is when you have a timed background service responding to all the requests coming and you have a queue background service responding to high priority requests. The actual business logic that needs to be executed is the same, so you implement it in a worker to avoid duplication and complexity.

## Adding a Simple Background Service

To add a simple background service to a microservice follow these steps:

1. Open the service model.
2. Using the context menu, add a Background Service.
3. Specify its name (e.g. Executor) and the documentation summary.
4. Save the model.
5. Transform all projects.

### Generated Code

The following pieces of code will be generated.

`Startup.gen.cs` - `StartupBase.AddBackgroundServices()`:

```csharp
/// <summary>
/// Called to add background services to the service collection.
/// </summary>
/// <param name="services">The service collection.</param>
/// <param name="hostConfiguration">The host configuration.</param>
/// <remarks>
/// The method is called from <see cref="ConfigureServices(IServiceCollection)"/>.
/// </remarks>
protected virtual void AddBackgroundServices(IServiceCollection services, HostConfiguration hostConfiguration)
{
    // Validation

    SmartGuard.NotNull(() => services, services);

    // Add queues


    // Add background services

    services.AddBackgroundService<ExecutorService>();
}
```

`BackgroundServices.gen.cs` - `ExecutorServiceBase`:

```csharp
/// <summary>
/// Defines the base class for the <see cref="ExecutorService" /> background service.
/// </summary>
[GeneratedCode("Lithium", "2.0")]
[SuppressMessage("Maintainability Rules", "SA1402:FileMayOnlyContainASingleType", Justification = "Because of code generation design.")]
internal abstract partial class ExecutorServiceBase : BackgroundService
{
    #region Code

    #region Protected Constructors

    /// <summary>
    /// Initializes a new instance of the <see cref="ExecutorServiceBase" /> class.
    /// </summary>
    /// <param name="serviceProvider">The service provider.</param>
    /// <param name="logger">The logger instance.</param>
    protected ExecutorServiceBase(IServiceProvider serviceProvider, ILogger logger)        
        : base(serviceProvider, logger)
    {
    }

    #endregion

    #endregion
}
```

`BackgroundServices.gen.cs` - `ExecutorService`:

```csharp
/// <summary>
/// Performs complex initialization at startup.
/// </summary>
[GeneratedCode("Lithium", "2.0")]
[SuppressMessage("Maintainability Rules", "SA1402:FileMayOnlyContainASingleType", Justification = "Because of code generation design.")]
[SuppressMessage("Microsoft.Performance", "CA1812:AvoidUninstantiatedInternalClasses", Justification = "Created via dependency injection.")]
internal partial class ExecutorService : ExecutorServiceBase
{
    #region Code

    #region Public Properties

    /// <inheritdoc />
    public override string Name
    {
        get
        {
            return "Executor";
        }
    }

    #endregion

    #region Public Constructors

    /// <summary>
    /// Initializes a new instance of the <see cref="ExecutorService" /> class.
    /// </summary>
    /// <param name="serviceProvider">The service provider.</param>
    /// <param name="logger">The logger instance.</param>
    public ExecutorService(IServiceProvider serviceProvider, ILogger<ExecutorService> logger)        
        : base(serviceProvider, logger)
    {
    }

    #endregion

    #endregion
}
```

### Custom Code

To complete the background service you will need to add a custom class for that service (`ExecutorService` in the example) and implement the `ExecuteAsync()` method.

```csharp
[SuppressMessage("StyleCop:DocumentationRules", "SA1601:PartialElementsMustBeDocumented")]
internal partial class ExecutorService
{
    #region Protected Methods

    /// <inheritdoc />
    protected override Task ExecuteAsync(CancellationToken cancellationToken)
    {
        // (...)

        return Task.CompletedTask;
    }

    #endregion
}
```

## Adding a Timed Background Service

To add a timed background service to a microservice follow these steps:

1. Open the service model.
2. Using the context menu, select Add Background Service (Timed).
3. Specify its name (e.g. TimedExecutor) and the documentation summary.
4. Save the model.
5. Transform all projects.

### Generated Code

Notice the code generated in `BackgroundServices.gen.cs` for this new background service.

### Custom Code

To complete the background service you will need to add a custom class for that service (`TimedExecutorService` in the example) and implement the `WaitPeriod` property and the `ExecuteWorkAsync()` method.

```csharp
[SuppressMessage("StyleCop:DocumentationRules", "SA1601:PartialElementsMustBeDocumented")]
internal partial class TimedExecutorService
{
    #region Public Properties

    /// <inheritdoc />
    public override TimeSpan WaitPeriod => TimeSpan.FromMinutes(5);

    #endregion

    #region Protected Methods

    /// <inheritdoc />
    protected override Task ExecuteWorkAsync(CancellationToken cancellationToken)
    {
        // (...)

        return Task.CompletedTask;
    }

    #endregion
}
```

## Adding a Queue Background Service

To add a queue background service to a microservice follow these steps:

1. Open the service model.
2. Using the context menu, select Add Background Service (Queue).
3. Specify its name (e.g. QueuedExecutor) and the documentation summary.
4. Save the model.
5. Transform all projects.

### Generated Code

Notice the code generated in `BackgroundServices.gen.cs` for this new background service.

### Custom Code

To complete the background service you will need to add a custom class for that service (`QueuedExecutor` in the example) and implement the `Queue` property and the `ExecuteWorkAsync()` method.

```csharp
[SuppressMessage("StyleCop:DocumentationRules", "SA1601:PartialElementsMustBeDocumented")]
internal partial class QueuedExecutorService
{
    #region Public Properties

    /// <inheritdoc />
    public override IBackgroundWorkQueue<object> Queue => this.ServiceProvider.GetRequiredService<IBackgroundWorkQueue<object>>();

    #endregion

    #region Protected Methods

    /// <inheritdoc />
    protected override Task ExecuteWorkAsync(object workItem, CancellationToken cancellationToken)
    {
        // (...)

        return Task.CompletedTask;
    }

    #endregion
}
```

## Adding a Background Worker

To add a timed background worker to a microservice follow these steps:

1. Open the service model.
2. Using the context menu, select Add Background Worker.
3. Specify its name (e.g. SharedExecutor) and the documentation summary.
4. Select each background service that should use this worker and set the Use Worker property to true and the Worker property to reference the new worker.
5. Save the model.
6. Transform all projects.

### Generate Code

Notice the code generated in `BackgroundWorkers.gen.cs` for this new background worker.

Inspect also `BackgroundServices.gen.cs` to view how the background services reference the worker.

### Custom Code

To complete the background worker you will need to add a custom class for that service (`SharedExecutor` in the example) and implement the `ExecuteAsync()` method.

```csharp
[SuppressMessage("StyleCop:DocumentationRules", "SA1601:PartialElementsMustBeDocumented")]
internal partial class SharedExecutorWorker
{
    #region Public Methods

    /// <inheritdoc />
    public override Task ExecuteAsync(CancellationToken cancellationToken)
    {
        // (...)

        return Task.CompletedTask;
    }

    #endregion
}
```

Any timed background service that uses the background worker should override `WaitPeriod` and `Worker` only:

```csharp
[SuppressMessage("StyleCop:DocumentationRules", "SA1601:PartialElementsMustBeDocumented")]
internal partial class TimedExecutorService
{
    #region Public Properties

    /// <inheritdoc />
    public override TimeSpan WaitPeriod => TimeSpan.FromMinutes(5);

    /// <inheritdoc />
    public override SharedExecutorWorker Worker
    {
        get
        {
            return new SharedExecutorWorker(
                this.ServiceProvider,
                this.ServiceProvider.GetRequiredService<ILogger<SharedExecutorWorker>>());
        }
    }

    #endregion
}
```

Any queue background service that uses the background worker should override `Queue`:

```csharp
[SuppressMessage("StyleCop:DocumentationRules", "SA1601:PartialElementsMustBeDocumented")]
internal partial class QueuedExecutorService
{
    #region Public Properties

    /// <inheritdoc />
    public override IBackgroundWorkQueue<SharedExecutorWorker> Queue => this.ServiceProvider.GetRequiredService<IBackgroundWorkQueue<SharedExecutorWorker>>();

    #endregion
}
```