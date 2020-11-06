# Primavera.Lithium.RoseAsyncTasks

Provides asynchronous multi-task processing for ROSE applications or services that need to improve performance, scalability, and reusability. This component implements the asynchronous operation behavior in the PRIMAVERA Elevation Platform.

## RAT Architecture

The RAT microservice implements the Taskbox library to achieve the multi-task processing behavior. To know more about this feature see the [Primavera.Taskbox.](../../../ref/taskbox-1.0/Taskbox.md)

### Pipelines

#### Upload Saft

The product will publish an event to the EventBus Azure, then the RAT microservice using the EventTask, component provided by the `Primavera.Taskbox`, subscribes to this event and when this is raised this task will execute the corresponding pipeline.

This pipeline is responsible for processing the SAFT asynchronously. This is composed of 3 handlers.

- `SAFTUploader` - responsible for obtaining the SAFT by performing an HTTP request to the specified address where the SAFT is stored;
- `SAFTVerifier` - responsible for asking the DIS microservice about the SAFT state;
- `SAFTPublisher` - publishes the SAFT upload request state to the specified address.

The following image describes the full process for this scenario.

![Upload SAFT](_assets/uploadsaft.png)
