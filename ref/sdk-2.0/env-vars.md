# Lithium Environment Variables

When the Lithium SDK is installed on a machine the following environment variables will be set:

| Env. Variable | Description |
| - | - |
| `LithiumCE` | Specifies the directory where the Lithium Common Extension is installed |
| `LithiumSDE` | Specifies the directory where the Lithium Service Designer Extension is installed |
| `LITHIUM_LOCALDEVELOPMENT` | Identifies a special execution environment identified as "Local Development" |
| `Path` | This variable is augmented with the path of the directory where Lithium SDK Tools are installed |

## Local Development

This is a special environment that allows distinguishing a development environment (DV) on a server (on Azure for example) from a development environment on a developer's machine.

Some components of the framework use this environment variables to setup custom behavior when running in a developer's machine (where thinks like, for example, managed identity cannot be used). See `IHostEnvironmentExtensions` for more information.