# How to Upgrade a Microservice from Lithium v1.0 to Lithium v2.0

## 9g - Update Application Settings

### 9g.1 - Remove Deprecated Configuration Sections/Keys

Remove the following sections and keys from the application settings custom files (`appsettings.json` and `appsettings-development.json`):

| Section/Key | Obs. |
| - | - |
| `HostConfiguration::RedisCacheEnabled` | |
| `HostConfiguration::RedisCacheAbsoluteExpiration` | |
| `HostConfiguration::RedisCacheConnectionString` | This setting is defined in a new section named `RedisCacheOptions`. |

### 9g.2 - Update Modified Configuration Sections/Keys

Update the following sections and keys in the application settings custom files:

| Section/Key | Replace with | Obs. |
| - | - | - |
| `BlobStorageServiceConfiguration::*` | `AzureBlobStorageOptions::*` | |
| `TableStorageServiceConfiguration::*` | `AzureTableStorageOptions::*` | |
| `TelemetryConfiguration::*` | `AzureInsightsTelemetryOptions::*` | |

### 9g.3 - Add the New Configuration Sections/Keys

Add the following sections and keys to the application settings custom files:

| Section/Key | Obs. |
| - | - |
| `RedisCacheOptions::ConnectionString` | Defines the connection string to the REDIS cache instance | |

## Next

> [10 - Update and Compile ClientLib](./10-update-compile-clientlib.md)