# Taskbox Clustering Configuration

When running a TaskBox in a microservice, you can configure the clustering as the microservice needs, since it can have different requirements for the cluster. An example is when you need to accelerate the instance heartbeat so you can track more precisely the task states when processing critical tasks. Another example is increasing the retry count for the task failures when it is expected to a task to fail sometimes.

## Clustering Options

Follows the example of a full Clustering options with its default values:

```json
{
  "TaskboxOptions": {
    "Clustering": {
      "CancelledTasksTimeToLive": 604800,
      "ClusteringInstanceTimeToLive": 86400,
      "CompletedTasksTimeToLive": 86400,
      "ErrorTasksRetryCount": 3,
      "ErrorTasksTimeToLive": 2629744,
      "LockAbsoluteExpiration": "00:03:00",
      "InstanceName": "auto",
      "MaxNumberofTasks": -1,
      "SetInstanceAsNotRespondingAfter": "00:01:30",
      "StorageOptions": {
        "Container": "TaskBox-Tasks-Clustering",
        "Database": "lithium",
        "DefaultTimeToLive": -1
      },
      "WaitPeriod": "00:01:00"
    }
  }
}
```

> Note: If options not provided in the `appsettings.json`, default values will be applied.

### CancelledTasksTimeToLive

Indicates the cancelled task time to live in seconds in the cluster database. Default is 7 days.

### ClusteringInstanceTimeToLive

Indicates the clustering instance time to live in seconds in the cluster database when the instance is not updating its heartbeat. Default value is 1 day.

### CompletedTasksTimeToLive

Indicates the completed task time to live in seconds in the cluster database. Default is 1 day.

### ErrorTasksRetryCount

Indicates how many times an instance can retry to process the failed task. Can be set to `-1` to retry the process unlimited times. Default value is 3 times.

### ErrorTasksTimeToLive

Indicates the error task time to live in seconds in the cluster database. Default is 1 month.

### LockAbsoluteExpiration

Indicates the lock absolute expiration. It removes the read lock when it has not been released after the given time. This removes the possibility of a dead lock. Default value is 3 minutes.

### InstanceName

Indicates the instance name that is used to register the instance on the cluster. It can be extremely helpful for development purposes to run 2 instances on same machine and separate them on the cluster. It can be set as `auto` to be automatically generated using the machine instance Id. An example a generated instance name of an instance running on Azure is `Taskbox-RD0050F204CB3F.reddog.microsoft.com`. Default value is `auto`.

> Warning: When deployed to Azure, these settings need to be set as `auto` to correctly separate the instances on Azure when using a plan that provides always more than 1 instance or when resilience is on. If running in more than 1 instance and the name is not set as `auto` the clustering will not work as expected, leading to incorrect behaviour.

### MaxNumberofTasks

Indicates the maximum number of tasks that this instance can process per lock. It can be tunned to work as some kind of load balancer so each instance will take only few tasks at a time, and not all the task currently available. If set to `-1`, it processes all the task available on the cluster at that moment. Default value is `-1`.

### SetInstanceAsNotRespondingAfter

Indicates when to assume that the instance is not updating its heartbeat to the cluster after the provided time and triggers the task recovery mechanism available on the cluster to assign tasks from this "dead" instance to another instances to continue the task processing. Default value is 90 seconds (1,5 * WaitPeriod).

### WaitPeriod

Indicates the wait period of the instance after it performs the reads and the heartbeat update to the cluster. Default value is 1 minute.

## Storage Options

The storage options can be set for a database and the container for the cluster to be created on the startup, and if exists, connect to it.

### Container

Indicates the CosmosDb container name. Default value is `TaskBox-Tasks-Clustering`.

### Database

Indicates the CosmosDb database name. Default value is `lithium`.

### DefaultTimeToLive

Indicates default time to live for all objects in the database. It overrides the previous Clustering options like `CanceledTasksTimeToLive`, `ClusteringInstanceTimeToLive`, `CompletedTasksTimeToLive` and `ErrorTasksTimeToLive`. It can be set to `-1` to turn on the time to live feature in the container and use the time to life defined on each object. Default value is `-1`.
