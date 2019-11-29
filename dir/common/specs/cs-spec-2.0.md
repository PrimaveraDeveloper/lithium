# Certificates Service (CS) Specification v2.0

The Certificates Service provides a centralized store for certificates used by applications. This cloud store allows managing the certificates lifetimes and pushing updates automatically to the applications that use them without requiring to update those applications.

## Certificates

Certificates retrieved by the Certificates Service are served as X509 certificates in a model called `X509Certificate` data with an instance of `X509Certificate2` inside.

## Caching

Since certificates do not change frequently and retrieving them from the CS service - and the underlying secrets storage - is slower than retrieving them locally, the client library provides a two-level caching mechanism:

- 1st level is a memory cache with a short lifetime.
- 2nd level is a local (isolated) storage with a longer lifetime (e.g. 1 day).

When the certificate is not found in any of these cache instances, the client library will contact the CS service to retrieve it (and the store it the local cache).

### Configuration

This client caching mechanism can be configured by the client application using the `CertificatesClientConfiguration` instance available from the `Configuration` property of the service client. The configuration options are the following:

- `LocalMemoryCacheEnabled`: indicates whether the first-level client cache (in memory) is enabled. The default value is true.
- `LocalMemoryCacheAbsoluteExpiration`: sets the absolute expiration time (in seconds) of the first-level client cache. The default value is 1 hour.
- `LocalStorageCacheEnabled`: indicates whether the second-level client cache (in isolated storage) is enabled. The default value is true.
- `LocalStorageCacheAbsoluteExpiration` sets the absolute expiration time (in seconds) of the second-level client cache. The default value is 24 hours.

> The second-level cache should always be disabled when the client library is being used from Web server applications running on Azure (since isolated storage is not available).

## Retrieving a Certificate

```csharp
using CertificatesClient client = new CertificatesClient(...);

try
{
    ServiceOperationResult<X509CertificateData> result = await client.Certificates.GetCertificateAsync("MyCertificate").ConfigureAwait(false);
    X509Certificate2 certificate = result.Body.Certificate;
    // (...)
}
catch (ServiceException ex)
{
    if (ex.Response.StatusCode == HttpStatusCode.NotFound)
    {
        // (...)
    }
    else
    {
        // (...)
    }
}
```

> The `GetCertificateAsync()` operation accepts a parameter called `requiresPrivateKey` that allows specifying if the certificate is expected to include a private key. When set to true, if the certificate retrieved from the service does not include a private key, an exception will be raise (with error code `CertificateHasNoPrivateKey`).

## Storage

All the certificates managed by the service are stored in a secrets storage (Azure KeyVault).

By default the service retrieves all certificates from a single storage set in configuration:

```json
"HostConfiguration": {
    "SecretsStorageAddresses": [
        {
            "CertificateName": "*",
            "StorageUrl": "https://myvault.vault.azure.net/"
        }
    ],
}
```

This configuration allows configuring different storages based on the certificate name:

```json
"HostConfiguration": {
    "SecretsStorageAddresses": [
        {
            "CertificateName": "xpto*",
            "StorageUrl": "https://xptovault.vault.azure.net/"
        },
        {
            "CertificateName": "*",
            "StorageUrl": "https://myvault.vault.azure.net/"
        }
    ],
}
```

> The `CertificateName` property supports glob patterns. The order of items under `SecretsStorageAddresses` is obviously significant and the first match with the certificate name will determine the `StorageUrl` used.