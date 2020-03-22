# Capability - Serialization (JSON)

> Tags: Rest, Json.NET, System.Text.Json

REST Web API rely heavily in the serialization of data in JSON.

In .NET in general, this involved, most of the times, referencing [NewtonSoft.Json](https://www.newtonsoft.com/json), which resulted in various compatibility issues.

Lithium 2.0, as .NET Core 3.X, now relies only on [System.Text.Json](https://docs.microsoft.com/en-us/dotnet/standard/serialization/system-text-json-overview). Any reference to NewtonSoft should be avoided at all costs.