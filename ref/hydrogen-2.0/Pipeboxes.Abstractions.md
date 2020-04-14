<!-- REFERENCES -->

[REF_PHP]: ./Pipeboxes.md
[REF_PHTA]: ./Taskbox.md

<!-- DOCUMENT -->

# Primavera.Hydrogen.Pipeboxes.Abstractions

**Class library that contains types that define abstractions for the Pipeline design pattern with multiple execution flows.**

## About

This abstraction defines the `Pipeline` design pattern interfaces that transforms the processing of a complex or time consuming operation into a set of small tasks that combine together to form an asynchronous unit of work, with the aim of improving performance, scalability and component reusability in any application or service. The main objective of this abstraction is to support multiple flows for the execution of tasks, leaving that responsibility to the implementation.

## When to use this pattern

Use when:
- The operation the application needs to perform can be divided into asynchronous and independent units of work (or Pipelines).
- The unit of work can be broken down into a set of smaller tasks (or Handlers) that are complementary and reusable.
- Tasks have different scalability requirements and can benefit from distributed processing.

May not be useful when:
- Tasks must be performed as part of the same transaction. In such cases, the unit of work must be designed to be reversible with another compensation unit of work.
- The amount of data that needs to be transported to be able to perform operation makes this approach inefficient.

## Abstractions

### IPipebox

The `IPipebox` interface defines the abstraction for the engine that runs a pipeline.

**Methods**

Method | Description
:--- | :---
UseProvider | Configures the pipebox to use the specified configuration.
UseConfig | Configures the pipebox to use the specified service provider.
UsePipeline | Configures the pipebox to execute the specified pipeline.
ExecuteAsync | Executes the specified pipeline asynchronously.
Execute | Executes the specified pipeline.

### IPipeboxContext

The `IPipeBoxContext` interface defines the abstraction for the context that is processed along the pipeline execution.

**Properties**

Property | Description
:--- | :---
Data | Gets the data to be processed along the pipeline.

**Methods**

Method | Description
:--- | :---
UseData | Configures the pipebox to use the specified data.

### IPipeboxHandler

The `IPipeboxHandler` interface defines the abstraction for the small tasks (Handlers) that compose a pipeline.

**Methods**

Method | Description
:--- | :---
UseProvider | Configures the handler to use the specified configuration.
UseConfig | Configures the handler to use the specified service provider.
ExecuteAsync | Executes the handler asynchronously.
Execute | Executes the handler.

### Supplementary types

Use the base classes provided in the concrete implementations to create, customize, or extend the behavior of this design pattern.

## Implementations

[Primavera.Hydrogen.Pipeboxes][REF_PHP]

## Related

To orchestrate the execution of multiple units of work you can use the [Primavera.Hydrogen.Taskbox][REF_PHTA].