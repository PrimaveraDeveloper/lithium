# Primavera.Hydrogen.Security.Abstractions

**Class library that contains types that define abstractions for generic security services.**

## Secrets Storage (ISecretsStorageService)

The `ISecretsStorageService` service allows retrieving data (secrets, keys, and certificates) from a secure storage in the cloud like the Azure KeyVault.

Instances of this service should be created via dependency injection (see concrete implementations for more information).

For example:

```csharp
ISecretsStorageService service = services.GetRequiredService<ISecretsStorageService>();

Secret result = await service.GetSecretAsync(
    new Uri(storageBaseAddress),
    "secretName")
    .ConfigureAwait(false);
```

## Password Strength Estimation (IPasswordStrengthEstimationService)

The `IPasswordStrengthEstimationService` service allows to score the strength of passwords and estimate crack times
and calculation times.

Instances of this service should be created via dependency injection.

```csharp
IPasswordStrengthEstimationService service = services.GetRequiredService<IPasswordStrengthEstimationService>();

int score = service.Score(password1);
double entropy = service.Entropy(password2);
double crackTime = service.CrackTime(password3);
long calculationTime = service.CalculationTime(password4);
```
