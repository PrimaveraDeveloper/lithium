# How to Upgrade a Microservice from Lithium v1.0 to Lithium v2.0

## 5 - Replace Deprecated Namespaces

### 5.1 - Remove all using statements that reference the following namespaces:

| Namespace |
| - |
| `Primavera.Hydrogen.Collections` |
| `Primavera.Hydrogen.Web` |

### 5.2 - Replace the references to the following namespaces:

| Namespace | Replace with |
| - | - |
| `Primavera.Hydrogen.Rest.ClientRuntime` | `Primavera.Hydrogen.Rest.Client` |
| `Primavera.Hydrogen.Rest.ClientRuntime.Authentication` | `Primavera.Hydrogen.Rest.Client.Authentication` |

## Next

> [6 - Replace Deprecated Types](./06-replace-deprecated-types.md)