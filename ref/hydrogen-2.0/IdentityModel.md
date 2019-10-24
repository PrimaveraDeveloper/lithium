# Primavera.Hydrogen.IdentityModel

**Class library that contains core types and helpers for OpenID Connect, OAuth and claims-based Identity.**

## Constants

This library includes static classes that define constants useful in OIDC, OAuth, etc.

### OidcConstants

The `OidcConstants` defines constants using in requests and responses to/from OIDC endpoints, grant types, etc.

## Jwk

The `Primavera.Hydrogen.IdentityModel.Jwk` namespace provides types to manipulate JSON Web Keys.

## Jwt

The `Primavera.Hydrogen.IdentityModel.Jwt` namespace provides types to manipulate JWT tokens.

### JwtSecurityTokenDecoder

The `JwtSecurityTokenDecoder` class allows decoding a JWT token to obtain its properties (e.g. the client identifier) or evaluate them (e.g. to check if the token is expired).
