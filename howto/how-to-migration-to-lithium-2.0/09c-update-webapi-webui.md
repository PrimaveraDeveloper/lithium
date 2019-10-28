# How to Upgrade a Microservice from Lithium v1.0 to Lithium v2.0

## 9c - Update Web UI Custom Code

### 9c.1 - Controllers

Remove controller actions (and associated constants) that respond to the following routes because they are already setup in the generated code:

- Index (route = `""`)
- AccessDenied (route = `"/accessdenied"`)
- Error (route = `"/error"`)
- Signout (route = `"/signout"`)

If the behavior you implemented is different from the one generated, override the actions of the generated `HomeController`.

### 9c.2 - Views

If you have defined a view named `Error.cshtml`, delete it.

### 9c.3 - ErrorViewModel

If you have defined classes named `ErrorViewModel` and/or `ErrorMessageViewModel`, delete them.

### 9c.4 - Localization

If you have created a custom partial for class `Constants`, make sure it is public.

> NOTE: Do not advance to the next step before this project is compiling without any error or warning.

## Next

> [9d - Update References to Table Storage](./09d-update-webapi-table-storage.md)