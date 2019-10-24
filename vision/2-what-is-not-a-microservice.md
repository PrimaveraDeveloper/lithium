# What IS NOT a Microservice?

The microservices architecture is just another style for designing applications and integrating solutions. It is not a magic solution for any kind of software design problem.

Although this architecture aims for simplicity, it also introduces additional efforts on developments that need to be considered. There are multiple situations where this architecture will not be the appropriate solution and will not produce the desired results.

Just as guide, when any of these conditions is found, probably using a microservice is not the best solution:

- The features do not require a Web API.
- The Web API cannot be implemented using REST.
- The Web API does not require or cannot apply authentication or authorization.
- The service cannot be deployed automatically.
- The features can only be implemented using a traditional SQL database.
- Communication with on-premises components/services is required.

## Next

> [Lithium Architecture](3-lithium-architecture.md)
