# Capability - Caching

> Tags: Caching, Distributed Cache, Resilient Cache, REDIS

As performance is paramount in microservices and in the cloud in general, developers will need to implement caching mechanisms often.

The Lithium Framework supports adding a resilient distributed cache to the microservice simply by adding a specific dependency and configuring the connection string to a REDIS cache instance.

For more information see:

- [How to add support for distributed caching to a microservice](../howto/howto-add-distributed-cache.md)
- [Primavera.Hydrogen.Caching.Distributed](../ref/hydrogen-2.0/Caching.Distributed.md)

Most services include this distributed cache. The [Settings Service](../dir/common/ss.md) is one example.