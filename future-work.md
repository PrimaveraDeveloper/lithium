# Future Work

The following list describes epics that are part of the roadmap for future versions of the Lithium Framework.

## Technology

| Feature | Description | Roadmap | |
| - | - | - | - |
| .NET 5 | All packages (Hydrogen, etc.) that are not .NET Standard should target .NET 5 | v3 | |

## SDK

| Feature | Description | Roadmap | |
| - | - | - | - |
| **Solution templates (Server)** | Rename the WebApi project to Server, to clarify services that require back-office and/or complex infrastructure | v3 | |
| **Solution templates (Server Models)** | Add a new server-side project to hold models, constants, etc. available only server-side, to improve support for services that require back-office and/or complex infrastructure | v3 | |
| **Solution templates (Clean Architecture)** | New template to create services according to Clean Architecture | | |

## Service Designer

| Feature | Description | Roadmap | |
| - | - | - | - 
| **Configuration options** | New domain entity to allow modeling and generating configuration options | v3 | |
| **Storage abstractions** | Abstractions on storage to make decisions for the developer on what services to use and how to use them | | |

## Code Generation

| Feature | Description | Roadmap | |
| - | - | - | - |
| **Configuration** | Refactor the code generated for configuration to support types modeled in the designer and services with complex infrastructure | v3 | |
| **Mediators** | Refactor the code generated for controllers to add a new layer of mediators | v3 | |
| **gRPC** | Generate gRPC services |  | |
| **Source generators** | Apply source generators to simplify code generation templates | | |

## Hydrogen

| Feature | Description | Roadmap | |
| - | - | - | - |
| **OperationResult** | Refactor `OperationResult` to support complex results (NotFound, Duplicate, etc.) to avoid/simplify ceremony around `OperationResult.IsFailure` | v3 |
| **ServiceClient authentication** | Refactor authentication on service clients to simplify it, remove authentication callback and support chained credentials (like Azure.Core) | v3 | |
| **ServiceClient logging** | Support client-side logging out of the box in service clients (including the option to send to server telemetry service) | v3 |
| **ServiceError** | Refactor or replace `ServiceError` to adhere to the `ProblemDetails` implementation (RFC7807 section 3) | | |
| **Named services** | Refactor dependency injection for services (like `IBlobStorageService`) to use factories (like `IHttpClientFactory`) and support named services (and multiple instances of the same service) | | |
| **Extended background work** | Integration with third-party service to support background and asynchronous work that cannot be implemented easily with background services | | |
| **Webhooks event-driven** | Implementation of the webhooks services relying on a event-driven messaging framework (e.g. Service Bus) | | |

## Pipeline

| Feature | Description | Roadmap | |
| - | - | - | - |
| **Default packages repositories** | Move away from local `_packages` folder (set in `nuget.config`) and let packages use the standard repositories and caches | v2 | Done |
| **Semantic version packaging** | All NuGet packages should implement semantic version strictly (e.g. 3.0.0 instead of 3.0.0.0) | v3 | |
