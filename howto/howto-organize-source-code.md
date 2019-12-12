# How to organize the source code of a microservice

This guide describes how the source code of a microservice solution should be organized and stored in source control to support development, maintenance and releases.

## Source Control Branches

Each microservice should be stored in source control in two branches with specific purposes:

- `Development`: this branch should contain the version of the microservice under development.
- `Mainline`: this branch should contain the version of the microservice that is deployed in production.

The general idea is that any new development should be always undertaken in the `Development` branch and then merged to the Mainline branch so it can be released in production.

The `Mainline`, since it reflects exactly the version in production, allows testing and debugging any scenario in the context of maintenance and provides a way to fix any bug found in the service while some other new feature is being developed in the `Development` branch (hence avoiding the risk of publishing any change that is still not fully tested).

> This organization means that merges will occur typically from the `Development` branch to the `Mainline` branch. Only hot fixes for bugs found in production will require merges from `Mainline` to `Development`.

## Solution

Microservices solutions should be named using the service identifier (e.g. `SS.sln`).

The solution should be stored according to the following organization (considering as an example the Settings Service):

```
Microservices
-- Common
---- SS
------ Development
-------- SS.sln
-------- Client.Console
---------- Client.Console.csproj
---------- (...)
-------- ClientLib
---------- ClientLib.csproj
---------- (...)
-------- Design
---------- Design.csproj
---------- (...)
-------- Models
---------- Models.csproj
---------- (...)
-------- WebApi
---------- WebApi.csproj
---------- (...)
-------- Tests
---------- ClientLib.Tests
------------ ClientLib.Tests.csproj
------------ (...)
---------- Integration.Tests
------------ Integration.Tests.csproj
------------ (...)
---------- Models.Tests
------------ Models.Tests.csproj
------------ (...)
---------- WebApi.Tests
------------ WebApi.Tests.csproj
------------ (...)
------ Mainline
-------- (...)
---- (...)
-- (...)
```

The root directory of the service (e.g.: `SS`) is placed under a directory named after the service category (e.g.: `Common`).

Under that directory that are two directories corresponding to the two branches (`Development` and `Mainline`).

Under each of these directories the is the solution (e.g.: `SS.sln`) and the directories corresponding to the solution organization (`Client.Console`, `ClientLib`, `Design`, `Models`, `WebApi` and `Tests`).