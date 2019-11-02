# How to Upgrade a Microservice from Lithium v1.0 to Lithium v2.0

## 6 - Replace Deprecated Types

Replace the following deprecated types:

| Type | Replace With | Obs. |
| - | - | - |
| `IBackgroundWorkerQueue<T>` | `IBackgroundWorkQueue<T>` | |
| `Duration` | `TimeSpan` | |
| `TableQueryOperation` | `TableQueryComparison` | |

## Next

[7 - Replace Deprecated Members](./07-replace-deprecated-members.md)