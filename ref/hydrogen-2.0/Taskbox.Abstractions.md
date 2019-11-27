# Primavera.Hydrogen.Taskbox.Abstractions

**Class library that contains types that define abstractions for asynchronous multi-task processing.**

> This documentation is under construction.

# Pipebox

This abstraction defines the interfaces needed to transform the processing of a complex or time-consuming operation into a set of small tasks that combine to form an asynchronous unit of work. This approach improves the performance, scalability, and reuse of components of an application or service.

## Abstractions

### `IPipeBox`

The `IPipebox` interface defines an abstraction of the `Pipeline Design Pattern`. 

### `IEventBus`

The `IEventBus` interface defines the elemental operations that should be implemented in order to use an event bus service.

This interface is implemented by an abstract base class named `EventBusBase`. A further implementation should inherit from `EventBusBase`.


