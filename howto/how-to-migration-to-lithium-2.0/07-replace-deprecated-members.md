# How to Upgrade a Microservice from Lithium v1.0 to Lithium v2.0

## 7 - Replace Deprecated Members

### 7.1 - Replace the following deprecated properties:

| Property | Replace With | Obs. |
| - | - | - |
| `ApiVersions.DefaultVersion.Literal` | --- | Replace with the service API version literal (e.g. "1.0") |
| `OperationResult.IsNotFound` | `OperationResult.IsFailureWith(string)` | Parameter `errorCode` should be a specific error code of the operation being called that corresponds to a not found error (as passed by `OperationResult.Failure(OperationError)`. |
| `OperationResult<T>.IsNotFound` | `OperationResult<T>.IsFailureWith(string)` | Parameter `errorCode` should be a specific error code of the operation being called that corresponds to a not found error (as passed by `OperationResult<T>.Failure(OperationError)`.
| `OperationResult.IsSucess` | `OperationResult.IsSuccess` | |
| `OperationResult<T>.IsSucess` | `OperationResult<T>.IsSuccess` | |

### 7.2 - Replace the following deprecated methods:

| Method | Replace With | Obs. |
| - | - | - |
| `IServiceCollection.ResolveConfiguration<T>()` | `IServiceCollection.GetRequiredService<T>()` | |
| `IServiceCollection.ResolveService<T>(true)` | `IServiceCollection.GetRequiredService<T>()` | |
| `IServiceCollection.ResolveService<T>()` | `IServiceCollection.GetService<T>()` | |
| `OperationResult.NotFound()` | `OperationResult.Failure(OperationError)` | Parameter `error` should be a specific `OperationError` indicating a not found error. |
| `OperationResult<T>.NotFound()` | `OperationResult<T>.Failure(OperationError)` | Parameter `error` should be a specific `OperationError` indicating a not found error. |
| `OperationResult.ToServiceError()` | `ServiceError.FromOperationResult(OperationResult)` or `ServiceErrorDetail.FromOperationResult(OperationResult)` |  |
| `OperationResult<T>.ToServiceError()` | `ServiceError.FromOperationResult<T>(OperationResult<T>)` or `ServiceErrorDetail.FromOperationResult<T>(OperationResult<T>)` |  |
| `string.Format().With(object[])` | `string.Format(object[])` | |
| `string.EqualsAny()` | `string.EqualsAnyNoCase()` | |
| `string.EqualsWithoutCase(string)` | `string.EqualsNoCase(string)` | |

### 7.3 - Remove the following deprecated properties:

| Property | Obs. |
| - | - |
| `HttpResponseMessageSurrogate.Headers.Location` | `HttpResponseMessageSurrogate.Headers` is now a read-only dictionary with a `string key`. |

## Next

> [8 - Update and Compile Models](./08-update-compile-models.md)