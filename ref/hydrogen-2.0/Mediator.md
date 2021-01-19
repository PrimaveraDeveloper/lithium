<!-- REFERENCES -->

[REF_MDA]: ./Mediator.Abstractions.md

<!-- DOCUMENT -->

# Primavera.Hydrogen.Mediator

**Class library that contains types that implements the Mediator design pattern In Process**

## About

This class library is the default implementation of the [Primavera.Hydrogen.Mediator.Abstractions][REF_MDA].  The main purpose of this implementation is to support the InProc message processing. Also there is support for easy to use CQRS design pattern implementation using this implementation.

Although it is possible the mediator should not be used as a away to orchestrate processes, that is, invoking the mediator within the mediator. For instance if you have "CreateSomethingCommand", within the handler that handles this message you should only use the mediator to send Events, and not other Commands or Queries.


## Getting Started

There are some extension methods that help to bootstrap the mediator in your application given an `IServiceCollection' object.

Consider the following sample of code:

```csharp

using Microsoft.Extensions.DependencyInjection;
using Primavera.Hydrogen.Mediator.Abstractions;
using Primavera.Hydrogen.Mediator.Extensions

private static IMediator GetMediatorInstance()
{
    ServiceCollection services = new ServiceCollection();

    // Adds the mediator to the Service Collection.

    services.AddMediator();

    // Scans for or registers mediator middleware types in the Service Collection. This step is optional.

    services.AddMediatorMiddleware();

    // Build the service provider.

    IServiceProvider provider = services.BuildServiceProvider();

    // Retrieve and return the mediator from the Service Collection.

    return provider.GetRequiredService<IMediator>();
}

```

By default the 'AddMediator()' method extension will register the services required by mediator itself and scan the application domain assemblies for types that can be used by the mediator: 
- IQueryHandler<,>
- IQueryExceptionHandler<,,>
- ICommandHandler<,>
- ICommandExceptionHandler<,,>
- IEventHandler<>
- IEventExceptionHandler<,>


Both 'AddMediator()' and 'AddMediatorMiddleware()' extension methods can also take as input a list of types to be be used instead of scanning the domain assemblies.


# CQRS pattern integration

Using the provided CQRS abstractions in [Primavera.Hydrogen.Mediator.Abstractions][REF_MDA] the integration between the two patterns is straight forward.

A typical application contains a finite set of actions that can be applied to it, and these can be divided in to three main categories:
- Commands - an action that changes the state of the information in your application.
- Queries - an action that  retrieves information from your application.
- Events - an action that notifies that something has happened within your application.

The provided CQRS abstractions and base classes are meant to reflect this three type of categories.

Consider the following samples of code in your application intended to save an entity in the storage of your application.

#### Create the command class that will **notify** the application the intention of saving an entity named 'MetricDefinition'.

```csharp
using Primavera.Hydrogen;
using Primavera.Hydrogen.Mediator.Abstractions.Middleware.Cqrs;
using Primavera.Juice.Usage.Domain.Entities;

/// <summary>
/// Command to create the a metric definition.
/// </summary>
public class CreateMetricDefinitionCommand : ICommand<OperationResult<string>>
{
    #region Public Properties

    /// <summary>
    /// Gets or sets the metric definition.
    /// </summary>
    public MetricDefinition MetricDefinition { get; set; }

    #endregion
}

```

#### Create the command handler that will **handle** the intention of saving an entity named 'MetricDefinition'
```csharp
using Primavera.Hydrogen.Mediator.Abstractions;
using Primavera.Hydrogen.Mediator.Abstractions.Middleware.Cqrs;
using Primavera.Hydrogen.Mediator.Cqrs;

/// <summary>
/// Command handler to create a metric definition.
/// </summary>
public class CreateMetricDefinitionCommandHandler : CommandHandler<CreateMetricDefinitionCommand, OperationResult<string>>
{
    /// <inheritdoc />
    protected override Task<OperationResult<string>> HandleCommandAsync(CreateMetricDefinitionCommand command, IMediatiorContext mediatiorContext, CancellationToken cancellationToken)
    {
        // code to save the entity in the application storage

        await this.ServiceHandler.CreateAsync(message.MetricDefinition)
            .ConfigureAwait(false);

        // Create entity event

        await this.Mediator.SendAsync(
            new EntityChangedEvent()
            {
                Entity = message.MetricDefinition,
                Change = EntityChange.Created
            },
            cancellationToken: cancellationToken)
            .ConfigureAwait(false);

        // return

        return OperationResult<string>.Success("Saved..");
    }
}
```


#### To keep command handlers simple and clean **Exception Handling** can be done in the exception handlers like the one bellow that handles exceptions of type "DomainValidationException" that might occur while executing the CommandHandler.
```csharp
using Primavera.Hydrogen.Mediator.Abstractions;
using Primavera.Hydrogen.Mediator.Cqrs;

/// <summary>
/// Handles exceptions that can occur while executing the <see cref="CreateMetricDefinitionCommand" />.
/// </summary>
public class CreateMetricDefinitionCommandExceptionHandler :
    CommandExceptionHandler<CreateMetricDefinitionCommand, OperationResult<string>, DomainValidationException>
{
    #region Public Methods

    /// <inheritdoc/>
    public Task<OperationResult<string>> HandleExceptionAsync(CreateMetricDefinitionCommand message, DomainValidationException exception, IMediatiorContext mediatiorContext, CancellationToken cancellationToken)
    {
        SmartGuard.NotNull(() => exception, exception);

        this.logger.LogError(exception.ToString());

        OperationError operationError = new OperationError(
            nameof(CreateMetricDefinitionCommand), 
            Properties.Resources.RES_OperationResult_Error_Description_UnexpectedError);

        return Task.FromResult(OperationResult<string>.Failure(operationError));
    }

    #endregion
}
```

#### After setting everything up you can now **trigger** the command from the entry points in your application like an Api Controller or a Button in your Forms Application.
```csharp
/// <inheritdoc />
protected override async Task<IActionResult> CreateCoreAsync(MetricDefinition metricDefinition)
{
    SmartGuard.NotNull(() => metricDefinition, metricDefinition);

    OperationResult<string> result = await this.Mediator
        .SendAsync(
            new CreateMetricDefinitionCommand()
            {
                MetricDefinition = this.Mapper.Map<Entities.MetricDefinition>(metricDefinition)
            })
        .ConfigureAwait(false);

    if (result.IsFailure)
    {
        return this.BadRequest(
            ServiceError.FromOperationResult(result));
    }

    return this.Created(
        this.Request.GetMetricDefinitionLocationUri(metricDefinition.Key),
        new { Key = result.Data });
}

```

