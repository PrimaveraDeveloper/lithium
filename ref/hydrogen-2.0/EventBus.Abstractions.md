# Primavera.Hydrogen.EventBus.Abstractions

**Class library that contains types that define abstractions for generic event bus services.**

All of the abstract classes implement an interface and are intended to be used as base classes for a concrete event bus service implementation.

All of the interfaces and abstract classes provide an abstraction from implementation details such as the underlying cloud infrastructure.

## Abstractions

### `IEventBus`

The `IEventBus` interface defines the elemental operations that should be implemented in order to use an event bus service.

This interface is implemented by an abstract base class named `EventBusBase`. A further implementation should inherit from `EventBusBase`.

### `IEventBusEvent`

The core properties of an event are defined by `IEventBusEvent` interface.

The interface is also implemented by an abstract base class named `EventBusEvent`. Further properties and behavior should be extended from `EventBusBase`.

`IEventBusEvent` and `EventBusBase` are generic types since a concrete type needs to be passed in order to ease serialization and encoding operations related with the payload that the event transports.

### `IEventBusOptions`

The `IEventBusOptions` contains the options needed to configure an event bus service instance and is implemented by `EventBusOptionsBase`.

As of writing of this document, since there's no base options for a generic event bus service, both the referred types are empty.

### `IEventPublisherOptions`

`IEventPublisherOptions` specifies the options related with the publication operations of an generic event bus service.

The interface is implemented by `IEventPublisherOptions`. Both are empty as of the writing of this document.

### `IEventSubscriberOptions`

`IEventSubscriberOptions` specifies the options related with the subscription operations of an generic event bus service.

The interface is implemented by `EventSubscriberOptionsBase`. Both are empty as of the writing of this document.

### `IEventBusEventHandler`

`IEventBusEventHandler` defines the methods that a handler of a received event should implement.

The concrete implementation of an event bus service is responsible for implementing this handler.

### `IEventBusEventFilters`

`IEventBusEventFilters` defines a collection of filters that can be associated with a operation that handles a subscription. 

A subscription associated with such filters will only receive events whose properties have a exact match for each of the key-value pairs contained in the filters.

The interface is implemented by an abstract base class named `EventBusEventFiltersBase`.

## Supplementary types

### `EventBusException`

The `EventBusException` class defines the exception that should be raised from unexpected behavior by an event bus service instance.

The exception has several constructors that cover different needs.

### `EventBusError`

`EventBusError` is an enumerator that exposes symbolic numeric error codes that should be associated with an `EventBusException`.