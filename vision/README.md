# Vision and Architecture

## Objectives

The **Lithium Initiative** is an internal project - with implications on various other projects, both on Elevation and Windows technologies - that **aims the development, operation, and maintenance of a set of microservices, shared by multiple products, integrated among them, and integrated with other third-party services**.

Each microservice provides features - as simple and contained as possible, logically related - to consumers (applications and other services), in a way that makes it easier to maintain and evolve the microservice with a minimal and controlled impact on those consumers. This approach also facilitates the maintenance and evolution of those consumers, reducing the costs associated with their development by sharing and reusing the features provided by the microservices.

The integration of such microservices in the latest generation of cloud applications (like Jasmin and Rose) is paramount, although, whenever possible, the microservices should be developed to also support desktop applications (V10+ in particular).

## Motivations

A microservices architecture is a good solution for recurring problems that business applications, like PRIMAVERA's, face, both during their initial development and during their operation over the years. Problems such as:

- Distinct products often require similar functionality and it is not always easy to share business logic across different technologies (e.g. cloud and desktop), across different products (e.g. Jasmin and Rose), or even across different versions of the same application.
- It is common for applications to consume third-party services directly (e.g. tax authority services). When such third-party services become unavailable, the applications fail immediately. When such services change, all dependent products need to be updated, one at a time, usually with very demanding deadlines.
- There are no centralized mechanisms to manage or control the access of a given customer/user to the features shared by different products. Licensing is, as a result, more difficult and complex than it should be.
- Retrieving usage data and insights about what each customer effectively uses is too hard.
- Often features in the products depend on artifacts with lifetimes (e.g. certificates) but there is no way to manage them from a common store and update them automatically.

The Lithium Framework and the Lithium microservices are designed exactly to address this kind of problems and similar issues.

On the other hand, the microservices architecture put in place by Lithium embodies several other benefits that can be explored further. Such as:

- New business models driven, for example, by the productization of the microservices themselves.
- Improved independence from the technology stacks applied at any given moment to products and to the microservices.
- "Separation of concerns" and "single responsibility" applied to the design and the functional logic of the applications.
- Greater agility and faster response to new requirements and/or bug fixing, resulting from the inherent distributed deployment of the microservices that compose applications.
- Improved independence between development teams by defining their interactions over the services contracts, not to their concrete implementations.
- Easier testing.
- Improved scalability of applications.

## Next

> [What IS a Microservice?](./1-what-is-a-microservice.md)
