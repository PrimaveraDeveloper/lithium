# Primavera.Hydrogen.EventBus.Abstractions

**Class library that contains types that define abstractions for generic event bus services.**

All of the abstract classes implement an interface and are intended to be used as base classes for a concrete event bus service implementation.

All of the interfaces and abstract classes provide an abstraction from implementation details such as the underlying cloud infrastructure.

## Abstractions

### `IEventBusService`

The `IEventBusService` interface defines the elemental operations that should be implemented in order to use an event bus service.

This interface is implemented by an abstract base class named `EventBusServiceBase`. A further implementation should inherit from `EventBusServiceBase`.

### `IEventBusEvent`

The core properties of an event are defined by `IEventBusEvent` interface.

The interface is also implemented by an abstract base class named `EventBusEventBase`. Further properties and behavior should be extended from `EventBusEventBase`.

`IEventBusEvent` and `EventBusEventBase` are generic types since a concrete type needs to be passed in order to ease serialization and encoding operations related with the payload that the event transports.

### `IEventBusEventHandler`

`IEventBusEventHandler` defines the methods that a handler of a received event should implement.

The concrete implementation of an event bus service is responsible for implementing this handler.

### `IEventBusEventFilters`

`IEventBusEventFilters` defines a collection of filters that can be associated with a operation that handles a subscription. 

A subscription associated with such filters will only receive events whose properties have a exact match for each of the key-value pairs contained in the filters.

The interface is implemented by an abstract base class named `EventBusEventFiltersBase`.

## Supplementary types

### `EventBusServiceException`

The `EventBusServiceException` class defines the exception that should be raised from unexpected behavior by an event bus service instance.

The exception has several constructors that cover different needs.

### `EventBusServiceError`

`EventBusServiceError` is an enumerator that exposes symbolic numeric error codes that should be associated with an `EventBusServiceException`.