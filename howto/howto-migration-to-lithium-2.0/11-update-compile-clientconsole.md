# How to Upgrade a Microservice from Lithium v1.0 to Lithium v2.0

## 11. Update and Compile Client.Console

Compile `Client.Console.csproj` individually.

Depending on the service design, fixes may be required in the custom code.

### `ApplicationConfiguration`

If you have defined a property named `IdentityServerBaseUri`, remove it because it is already generated in the base class.

> Do not advance to the next project before this one is compiling without any error or warning.

## Next

[12. Update and Compile Tests Projects](./12-update-compile-testprojects.md)