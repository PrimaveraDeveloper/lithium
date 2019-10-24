# Deploying Microservices

Microservices run on the Azure cloud using Web apps (in the future they will run on containers).

The build, release, and deployment processes are implemented on Azure DevOps and Release Manager.

## Source Code

The source code for each microservices is stored in 2 branches:

- **Development**: the source code for the version under development.
- **Mainline**: the source code for the version deployed in production.

## Environments

Each microservice can be deployed to the following environments:

- **Development (dv)**: used by the development to test versions under development.
- **Staging (st)**: used by the development for regression tests.
- **Preview (pr)**: used for fast swapping to production.
- **Production (pd)**: used by the consumers.

There is a release configured on the Development branch and another configured for the Mainline branch. The Development release can only be deployed to Dv and St. The Mainline release can be deployed to all the environments.
