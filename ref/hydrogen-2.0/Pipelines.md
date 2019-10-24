# Primavera.Hydrogen.Pipelines

**Class library that contains types that implement the Pipeline design pattern.**

This implementation is inspired and based in the ASP.NET Core middleware pipeline. In fact it is kind of an
abstraction of that implementation that allows building pipelines on any data context (not only the HttpContext
like in ASP.NET Core).

This design pattern is very useful in scenarios where you want multiple pieces of code (the handlers) to process a given piece
of data sequentially, allowing each hander to decide whether to delegate processing or not to the next handler. Furthermore,
allowing the handlers to execute additional logic when the response is returning, after all the next handlers in the pipeline
have finished their work.

A good example of this kind of processing is in the DataLookup Service. Given a VAT number, the service creates a pipeline
containing a number of handlers that can return the data for that VAT number. The first one tries to read the data from
a memory cache and returns immediately if the VAT number is found or delegates processing if not. The second one
tries to read the data from a database. If this handler returns the data, the first one gets executed again (in the return), thus
allowing it to add that data to the cache so it is available there the next time it is requested.

## IPipelineBuilder

`IPipelineBuilder` allows creating pipelines by configuring how handlers are setup in the sequence (the pipeline).

The builder is integrated with the .NET Core dependency injection engine, so it can be easily used in any .NET Core project, in
particular the ASP.NET Core projects.

Creating pipelines is very straightforward:

```csharp
services
    .AddPipeline<MyContext>()
    .Use<FirstHandler>()
    .Use<SecondHandler>()
    .Use<ThirdHandler>();
```

Notice that `MyContext` is the data that will be shared by all the handlers (as in the request they will be processing). When
pipelines are used with `IServiceCollection` there can only be one pipeline for each type of context.

The `Use()` method allows registering the handler delegates. These can be functions or middleware classes. The rules here are
exactly the same that are used in the ASP.NET Core middleware:

- When using functions, these should have two parameters - `context` (the data context), and `next` (the next delegate in the pipeline) - and
return a `Task`.
- When using classes, these can either implement the `IPipelineHandler<TContext>` interface or, if not, they can be any class
as long as they include a public constructor receiving a `PipelineDelegate<TContext>` as the first parameter and a method called `Invoke` or `InvokeAsync`,
receiving a `TContext` as the first argument and returning a `Task`.

Consider the following examples:

```csharp
services
    .AddPipeline<MyContext>()
    .Use(
        async (context, next) =>
        {
            // logic executed when the request is incoming

            await next.Invoke(context).ConfigureAwait(false);

            // logic executed when the response is returning
        })
    .Use<SecondHandler>()
    .Use<ThirdHandler>();

public class SecondHandler : IPipelineHandler<MyContext>
{
    public SecondHandler(IAnotherInjectedService service)
    {
        // (...)
    }

    public Task InvokeAsync(MyContext context, PipelineDelegate<MyContext> next, IMyInjectedService service)
    {
        // logic executed when the request is incoming

        await next.Invoke(context).ConfigureAwait(false);

        // logic executed when the response is returning
    }
}

public class ThirdHandler
{
    public PipelineDelegate<MyContext> Next
    {
        get;
    }

    public ThirdHandler(PipelineDelegate<MyContext> next, IAnotherInjectedService service)
    {
        this.Next = next;

        // (...)
    }

    public async Task InvokeAsync(MyContext context, IMyInjectedService service)
    {
        // logic executed when the request is incoming

        await this.Next.Invoke(context).ConfigureAwait(false);

        // logic executed when the response is returning
    }
}
```

After configure the pipelines in the application startup initialization, these can be "consumed" anywhere in the application
using simple dependency injection:

```csharp
IPipeline<MyContext> pipeline = this.ServiceProvider.UsePipeline<MyContext>();
await pipeline.InvokeAsync(contextInstance).ConfigureAwait(false);
```

## PipelineBuilder

`PipelineBuilder` can also be used to create pipelines for scenarios where dependency injection is not available or is not
the best solution:

```csharp
IPipeline<MyContext> pipeline = new PipelineBuilder<MyContext>()
    .Use(
        async (context, next) =>
        {
            // logic executed when the request is incoming

            await next.Invoke(context).ConfigureAwait(false);

            // logic executed when the response is returning
        })
    .Use<SecondHandler>()
    .Use<ThirdHandler>()
    .Build();

await pipeline.InvokeAsync(contextInstance).ConfigureAwait(false);
```
