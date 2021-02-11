# Primavera.Hydrogen.Azure.Client

**Contains types, helpers, and extension methods to integrate with Azure SDK client libraries**

The Azure SDK for .NET provides client libraries for most Azure services. These libraries use different approaches (although there is an ongoing effort from its development teams to unify them) for things like authorization, error handling, retry policies, etc.

`Primavera.Hydrogen.Azure.Client` provides helper types and methods to deal with these differences and some abstractions to simplify using the Azure libraries.

## Azure Identity

The `Azure.Identity` library provides a set of classes that allow defining credentials to access Azure resources. These allow integrating with Azure Activity Directory and support thinks features Managed Identity.

These token credentials can be use to authorize client applications and will be supported soon by all Azure services and the Hydrogen services that use them (like, for example, the Secret Storage service).

### `ChainedTokenCredentialBuilder`

`ChainedTokenCredentialBuilder` allows building a chain of `Azure.Identity.TokenCredential` instances that can be used to authorize calls to the Azure services.

The following Hydrogen services support this through configuration:

- [Blob Storage](./Storage.Azure.md)
- [Secrets Storage Service](/Security.Azure.md)

> More service will support this as new versions of the Azure SDK libraries become available.

Example:

```csharp
ChainedTokenCredential credential = ChainedTokenCredentialBuilder
    .AddManagedIdentity()
    .AddVisualStudio()
    .AddAzureCli()
    .AddClientSecret("tenantId", "clientId", "secretId");
```

### `ChainedTokenCredential`

This type is similar to `Azure.Identity.ChainedTokenCredential` and `Azure.Identity.DefaultAzureCredential` in the sense that it allows chaining a set of token credential instances that will be tried one at a time unit authorization succeeds.

> This type differs only in that it implements `GetHashCode()`, which is necessary to implement service client caching in some scenarios (like for the Secrets Storage service).

## Configuration

### `AzureCredentialsOptions`

`AzureCredentialsOptions` is a generic configuration type that is shared between various services so that the `ChainedTokenCredential` used can be created from configuration.

> Examples of configuration types that reuse `AzureCredentialsOptions` are AzureKeyVaultSecretsStorageOptions` and `AzureBlobStorageOptions`.

The following options are available:

| Option | Description |
| - | - |
| `ManagedIdentity.Enabled` | Indicates whether the `ManagedIdentityCredential` should be used in the chain. Required (default value is `true`). |
| `ManagedIdentity.ClientId` | The client identifier that should be used by `ManagedIdentityCredential`. Optional (no default value). |
| `VisualStudio.Enabled` | Indicates whether the `VisualStudioCredential` should be used in the chain. Required (default value is `false`). |
| `VisualStudio.TenantId` | The tenant identifier that should be used by `VisualStudioCredential`. Optional (no default value). |
| `VisualStudioCode.Enabled `| Indicates whether the `VisualStudioCodeCredential` should be used in the chain. Required (default value is `false`). |
| `VisualStudioCode.TenantId` | The tenant identifier that should be used by `VisualStudioCodeCredential`. Optional (no default value). |
| `AzureCli.Enabled` | Indicates whether the `AzureCliCredential` should be used in the chain. Required (default value is `false`). |
| `ClientSecret.Enabled` | Indicates whether the `ClientSecretCredential` should be used in the chain. Required (default value is `false`).  |
| `ClientSecret.TenantId` | The tenant identifier that should be used by `ClientSecretCredential`. Required (if enabled) (no default value). |
| `ClientSecret.ClientId` | The client identifier that should be used by `ClientSecretCredential`. Required (if enabled) (no default value). |
| `ClientSecret.ClientSecret` | The client secret that should be used by `ClientSecretCredential`. Required (if enabled) (no default value). |

The way these options are used to build the credential chain will depend on the service in question. Typically, however, the order will be: managed identity, Visual Studio, Visual Studio Code, Azure CLI, and finally client secret.