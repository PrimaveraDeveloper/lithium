# Primavera.Lithium.Meta.Specs

**Class library that contains types that provide specifications of micro services developed with the Lithium Framework.**

## Service Specification (`ServiceSpec`)

`ServiceSpec` allows describing a microservice and serializing that specification to JSON.

It includes the following information about a microservice:

- Description: contains the service description (including the identifier, the name, display name, etc.).
- Dependencies: a list containing the service dependencies (e.g. distributed cache, table storage, etc.).
- BackgroundServices: a list describing the background services contained in the microservice.
- AppSettings: a list containing the applications settings used by the microservice.
- ApiResource: describes the API resource (for IdentityManagement) for the microservice Web API.
- Clients: describes the clients (for Identity Management) that should exist to support the development of the microservice.
- Endpoints: describes the microservice endpoints (routes).
- Features: describes the features supported by the microservice.

### Serialization

To serialize a ServiceSpec:

```csharp
ServiceSpec spec = new ServiceSpec()
{
    // (...)
};
string json = spec.ToJson();
```

To deserialize:

```csharp
ServiceSpec spec1 = ServiceSpec.FromJson(new string[] { json1 });
ServiceSpec spec2 = ServiceSpec.FromJsonFile(new string[] { file1 });
```

NOTE: A microservice specification can be contained in multiple JSON files (to allow for part of the specification to be automatically generated and other parts to be specified by developers).

## Application Settings Specification (`AppSettingsSpec`)

`AppSettingsSpec` allows serializing a set of application settings for a microservice.

It is used by `ServiceSpec` but it is also used by the Lithium SDK code generation text templates to produce the application settings files (e.g. appsettings.json).