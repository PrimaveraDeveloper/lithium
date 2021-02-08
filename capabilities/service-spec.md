# Capability - Service Specification

> Tags: Canvas

The Lithium Framework provides, as part of the artifacts generated for a microservice, the service specification, which has a purpose similar to that of a [microservice canvas](https://chrisrichardson.net/post/microservices/general/2019/02/27/microservice-canvas.html).

This file, generated in the `DesignTime` project, is called `Service.gen.lsspec.json`.

It includes the specification of the service that can be directly inferred from the service model:

- Identifier, name, description, etc.
- Dependencies.
- App settings (configuration options) required.
- Features used.
- Endpoints provided.
- Etc.

This generated file can be combined with its developer-tailored pair (`Service.lsspec.json`), which should include the custom specification that cannot be inferred from the model - like dependencies added in custom code, additional configuration options defined in custom code, etc. - to provide the full service specification.

Being a machine-readable file (JSON), the specification can then be used to automate tasks in the CI/CD pipelines of the microservice. Apart from being a very powerful tool to document the service behavior.