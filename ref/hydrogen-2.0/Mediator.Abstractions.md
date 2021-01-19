# Primavera.Hydrogen.Mediator.Abstractions

**Class that contains types that define abstractions for the mediator pattern and cqrs pattern integration**

The mediator pattern allows for a greater decoupling between different application layers, [loose coupling](https://en.wikipedia.org/wiki/Loose_coupling). Any layer in the application can send a message through the mediator and who ever is interested in that message type can handle the message as an input and produce an output to the caller.

## Abstractions

### `IMediator`

The `IMediator` interface defines the elemental operations that should be implemented.

### `IMessage<TResult>`

The `IMessage` interface defines the basic properties or operations available to a message being sent through the mediator.

### `IMediatiorContext`

The `IMediatiorContext` interface defines the basic properties or operations available in the mediator context. Each time a message is sent through the mediator there is a context associated with it that will be preserved throughout the entire message processing.

### `IMessageHandler`

The `IMessageHandler` interface defines the basic properties or operations available to any handler that processes/handles a mediator message.

### `IMessageExceptionHandler`

The `IMessageExceptionHandler` interface defines the basic properties or operations available to a 'special' type of handler that can handle specific type of exceptions that can occur while processing a message.

### `IMiddleware`

The `IMiddleware` interface defines the basic properties or operations for objects that can act as a middleware which can be added to the mediator pipeline.

### `IMessageProcessor`

The `IMessageProcessor` interface defines the basic properties or operations for objects that can process mediator messages.


## CQRS Abstractions

There are several abstrations included that allow for easier CQRS pattern integration within the mediator. These are intended to improved easy of use between the two patterns. 
Comands an queries can only have one handler, while events can have multiple handlers.

**Commands** refer to an action in the application that changes the state of the information within the application.

### `ICommand<TResult>`

The `ICommand<TResult>` interface defines the basic properties or operations for a command message.

### `ICommandHandler<in TCommand, TResult>`

The `ICommandHandler<in TCommand, TResult>` interface defines a mediator command handler that changes the state of the information in the application.

### `ICommandExceptionHandler<in TCommand, TResult, in TException>`

The `ICommandExceptionHandler<in TCommand, TResult, in TException>` interface defines a mediator command exception handler that handles exceptions occured while processing a `ICommandHandler<in TCommand, TResult>` message.


**Queries** refer to an action in the application that can retrieve information from the application.

### `Query<TResult>`

The `Query<TResult>` interface defines the basic properties or operations for a query message that retrieves information from the application.

### `IQueryHandler<in TQuery, TResponse>`

The `IQueryHandler<in TQuery, TResponse>` interface defines a mediator query handler that retrieves information from the application.

### `IQueryExceptionHandler<in TQuery, TResponse, in TException>`

The `IQueryExceptionHandler<in TQuery, TResponse, in TException>` interface defines a mediator query exception handler that handles exceptions occured while processing a `IQueryHandler<in TQuery, TResponse>` message.



**Events** refer to something that occured within the application and the application can notify whoever is interested in the event.

The `IEvent` interface defines the basic properties or operations for an event message that notifies of something happened in the application.

### `IEventHandler<in TEvent>`

The `IEventHandler<in TEvent>` interface defines a mediator event handler that handles an event that occured within the application.

### `IEventExceptionHandler<in TEvent, in TException>`

The `IEventExceptionHandler<in TEvent, in TException>` interface defines a mediator event exception handler that handles exceptions occured while processing a `IEventHandler<in TEvent>` message.