# Primavera.Hydrogen.Taskbox.Abstractions

**Class library that contains types that define abstractions for asynchronous multi-task processing.**

## About

This abstraction defines the `Pipeline Design Pattern` interfaces that transforms the processing of a complex or time consuming operation into a set of small tasks that combine together to form an asynchronous unit of work, with the aim of improving performance, scalability and component reusability in any application or service.

## Abstractions

### `IPipebox`

The `IPipebox` interface defines the abstraction for the engine that runs a pipeline.

**Methods**

Method | Description
:--- | :---
UseProvider | Configures the pipebox to use the specified configuration.
UseConfig | Configures the pipebox to use the specified service provider.
UsePipeline | Configures the pipebox to execute the specified pipeline.
ExecuteAsync | Executes the specified pipeline asynchronously.
Execute | Executes the specified pipeline.

### `IPipeboxContext`

The `IPipeBoxContext` interface defines the abstraction for the context that is processed along the pipeline execution.

**Properties**

Property | Description
:--- | :---
Data | Gets the data to be processed along the pipeline.

**Methods**

Method | Description
:--- | :---
UseData | Configures the pipebox to use the specified data.

### `IPipeboxHandler`

The `IPipeboxHandler` interface defines the abstraction for the small tasks (also known as `Handlers`) that compose a pipeline.

## Supplementary types

Use the base classes provided by concrete implementations to create, customize, or extend the behavior of this design pattern.
