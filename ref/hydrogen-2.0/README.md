# PRIMAVERA Hydrogen v2.0

Hydrogen is the most fundamental part of the Lithium Framework as it includes a set of class libraries (for .NET Standard 2.0 and .NET Core 3.0) that provide common features used extensively by the framework and the Lithium microservices.

## Class Libraries

| Library | Description |
| - | - |
| [`Primavera.Hydrogen.AspNetCore`](./AspNetCore.md) | Contains types that support the development of Web applications with ASP.NET Core |
| [`Primavera.Hydrogen.AspNetCore.Authentication`](./AspNetCore.Authentication.md) | Contains types that support authentication of Web applications with ASP.NET Core |
| [`Primavera.Hydrogen.AspNetCore.Azure`](./AspNetCore.Azure.md) | Contains types that support the development of Web applications with ASP.NET Core hosted on Azure |
| [`Primavera.Hydrogen.AspNetCore.Extensions`](./AspNetCore.Extensions.md) | Contains types that provide extensions for Web applications developed with ASP.NET Core |
| [`Primavera.Hydrogen.AspNetCore.Throttling`](./AspNetCore.Throttling.md) | Contains services for implementing throttling in ASP.NET Core applications |
| [`Primavera.Hydrogen.AspNetCore.Webhooks`](./AspNetCore.Webhooks.md) | Contains types that define services for providing webhooks in ASP.NET Core applications |
| [`Primavera.Hydrogen.AspNetCore.Webhooks.Abstractions`](./AspNetCore.Webhooks.Abstractions.md) | Contains types that define abstractions for providing webhooks in ASP.NET Core applications |
| [`Primavera.Hydrogen.Caching.Distributed`](./Caching.Distributed.md) | Contains types that provided distributed caching services |
| [`Primavera.Hydrogen.Console`](./Console.md) | Contains types, helpers, and extension methods for console applications |
| [`Primavera.Hydrogen.Core`](./Core.md) | Contains core types, helpers, and extension methods that can be used on all kinds of applications |
| [`Primavera.Hydrogen.EventBus.Abstractions`](./EventBus.Abstractions.md) | Contains types that define abstractions for generic event bus services |
| [`Primavera.Hydrogen.EventBus.Azure`](./EventBus.Azure.md) | Contains types that define generic event bus services that use Microsoft Azure Service Bus |
| [`Primavera.Hydrogen.EventBus.InMemory`](./EventBus.InMemory.md) | Contains types that define generic event bus services that use an in-memory implementation |
| [`Primavera.Hydrogen.IdentityModel`](./IdentityModel.md) | Contains core types and helpers for OpenID Connect, OAuth and claims-based Identity |
| [`Primavera.Hydrogen.IdentityModel.Client`](./IdentityModel.Client.md) | Contains core types, helpers and client libraries for OpenID Connect, OAuth and claims-based Identity |
| [`Primavera.Hydrogen.Pipeboxes.Abstractions`](./Pipeboxes.Abstractions.md) | Contains types that define abstractions for the Pipeline design pattern with multiple execution flows |
| [`Primavera.Hydrogen.Pipeboxes`](./Pipeboxes.md) | Contains types that implement the Pipeline design pattern with multiple execution flows |
| [`Primavera.Hydrogen.Pipelines`](./Pipelines.md) | Contains types that implement the Pipeline design pattern |
| [`Primavera.Hydrogen.Primitives.PhoneNumbers`](./Primitives.PhoneNumbers.md) | Contains types that allow parsing, formatting, and validating international phone numbers |
| [`Primavera.Hydrogen.Rest`](./Rest.md) | Contains types for applications that define and/or use REST services |
| [`Primavera.Hydrogen.Rest.Client`](./Rest.Client.md) | Contains types that allow communication with REST services |
| [`Primavera.Hydrogen.Search.Abstractions`](./Search.Abstractions.md) | Contains types that define abstractions for generic search services |
| [`Primavera.Hydrogen.Search.Azure`](./Search.Azure.md) | Contains types that define generic search services that use Microsoft Azure search services |
| [`Primavera.Hydrogen.Security.Abstractions`](./Security.Abstractions.md) | Contains types that define abstractions for generic security services |
| [`Primavera.Hydrogen.Security.Azure`](./Security.Azure.md) | Contains types that define generic security services that use Windows Azure services |
| [`Primavera.Hydrogen.Security.Passwords.Zxcvbn`](./Security.Passwords.Zxcvbn.md) | Contains types that define services that allows estimating password strength with the ZXCVBN algorithm |
| [`Primavera.Hydrogen.Storage.Abstractions`](./Storage.Abstractions.md) | Contains types that define abstractions for generic storage services |
| [`Primavera.Hydrogen.Storage.Azure`](./Storage.Azure.md) | Contains types that define generic storage services that use Microsoft Azure storage services |
| [`Primavera.Hydrogen.Storage.Files`](./Storage.Files.md) | Contains types that define generic storage services that use local files |
| [`Primavera.Hydrogen.Telemetry.Abstractions`](./Telemetry.Abstractions.md) | Contains types that provide generic telemetry services |
| [`Primavera.Hydrogen.Telemetry.Azure`](./Telemetry.Azure.md) | Contains types that provide telemetry services that use Microsoft Azure Application Insights |
| [`Primavera.Hydrogen.Text.Composition`](./Text.Composition.md) | Contains types that support string composition and string formatting |
| [`Primavera.Hydrogen.TextMessaging.Abstractions`](./TextMessaging.Abstractions.md) | Contains types that define abstraction for text messages (SMS) |
| [`Primavera.Hydrogen.TextMessaging.Twilio`](./TextMessaging.Twilio.md) | Contains types that allow managing text messages (SMS) using the Twilio gateway |
| [`Primavera.Hydrogen.Wcf.Client`](./Wcf.Client.md) | Contains types that allow communication with WCF Web services |

## Release Notes

The release notes are available [here](release-notes.md).

## NuGet Packages

All these libraries are available as NuGet packages from the following feed: public-lithium-general.
