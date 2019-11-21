# Triggers Service (TRG) Specification v2.0

The Triggers Service allows executing triggers based on predefined schedules. Triggers are like operations (e.g. a Web API method) that the service will execute according to the specified schedule.

This is a way of executing batch jobs triggered by calls to given endpoints.

## REST Triggers

A REST trigger is a schedule to invoke a given REST endpoint, retrieve the result and then invoke a callback (another REST endpoint) to notify the creator of the trigger of that result.

### Creating a REST Trigger

To create a REST trigger you should call `RestTriggers.CreateRestTriggerAsync()`, passing a model containing the following information:

- `InvokeEndpoint` - the endpoint that should be called when the trigger schedule is due. Required.
- `CallbackEndpoint` - the endpoint that should be called after the trigger is executed to notify the creator of the result. Optional.
- `Schedule` - the trigger schedule (when it should be executed). Required.

After the trigger is created you can get the current state of the trigger `RestTriggers.GetRestTriggerStateAsync()`.

The trigger can be deleted at any time by invoking `RestTriggers.DeleteRestTriggerAsync()`.

Example:

```csharp
RestTrigger trigger = new RestTrigger()
{
    InvokeEndpoint = new RestEndpoint()
    {
        Address = new Uri("http://server/invoke"),
        Method = HttpMethod.Post,
        ExpectedResult = HttpStatusCode.NoContent
    },
    CallbackEndpoint = new RestEndpoint()
    {
        Address = new Uri("http://server/callback"),
        Method = HttpMethod.Post,
        ExpectedResult = HttpStatusCode.Any
    },
    Schedule = new TriggerSchedule()
    {
        RunAfter = DateTime.UtcNow
    }
};

ServiceOperationResult<Guid> response = await this.Client.RestTriggers
    .CreateRestTriggerAsync(trigger)
    .ConfigureAwait(false);
```

### Scheduling

The current version of the service supports only a single execution a trigger after a given moment in time.

All the triggers are executed by a worker running on the service that evaluates all the pending triggers schedule every 1 minute.

### Authorization

To invoke the Triggers Service endpoints, clients should have the corresponding required scope (see documentation).

The service itself needs the have access to the invoke and callback endpoints' scopes (if the trigger is set with `RequireAuthorization = true`).

> The access token is retrieved from the Authority Server that the Triggers Service is configured to use.