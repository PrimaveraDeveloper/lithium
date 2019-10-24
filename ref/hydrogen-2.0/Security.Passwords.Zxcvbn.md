# Primavera.Hydrogen.Security.Passwords.Zxcvbn

**Class library that contains types that define services that allows estimating password strength with the ZXCVBN algorithm.**

## Password Strength Estimation (ZxcvbnPasswordStrengthEstimationService)

The `ZxcvbnPasswordStrengthEstimationService` implements the `IPasswordStrengthEstimationService` using the ZXCVBN algorithm.

The service implementation should be registered using the following extension method for `IServiceCollection`:

```csharp
IServiceCollection services = (...);
services.AddZxcvbnPasswordStrengthEstimation();
```

`AzureKeyVaultSecretsStorageService` and `DefaultAzureServiceTokenService` directly.

## Validation Attributes

`StrongPasswordAttribute` and `VeryStrongPasswordAttribute` are validation attributes that use `ZxcvbnPasswordStrengthEstimationService` to validate passwords.
