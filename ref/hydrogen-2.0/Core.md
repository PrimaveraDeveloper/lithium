# Primavera.Hydrogen.Core

**Class library that contains core types, helpers, and extension methods that can be used on all kinds of applications.**

This is the most basic library of PRIMAVERA Hydrogen and is designed to be intensively reused by all applications or libraries.

## Extension Methods

There is a set of extension methods for multiple purposes and on various types.

### `Byte[]`

`ByteExtensions` provides extension methods on `byte[]` such as:

- `GetString()`: returns the string corresponding to the byte array (using any Encoding).

### `DateTime`

`DateTimeExtensions` provides extension methods on `DateTime` and `DateTime?` such as:

- `Transform().ToHumanLike()`: returns the "human readable" string corresponding to the date/time value.

### `DateTimeOffset`

`DateTimeOffsetExtensions` provides extension methods on `DateTimeOffset` and `DateTimeOffset?` such as:

- `Transform().ToHumanLike()`: returns the "human readable" string corresponding to the date/time value.

### `IEnumerable<T>`

`IEnumerableExtensions` provides extension methods on `IEnumerable\<T\>` such as:

- `ForEach()`: allows iterating the enumerable to perform an action.

### `int`

`IntExtensions` provides extension methods on `int` such as:

- `Bits().SetBitOn()`: sets the bit of the value at a given position to 1.
- `Transform().ToWords()`: returns the words corresponding to the value (e.g.: 1 = one, 1000 = one thousand).

### `long`

`LongExtensions` provides extension methods on `long` such as:

- `Bits().SetBitOn()`: sets the bit of the value at a given position to 1.
- `Transform().ToWords()`: returns the words corresponding to the value (e.g.: 1 = one, 1000 = one thousand).

### `Socket`

`SocketsExtensions` provides extension methods on `System.Net.Sockets.Socket` such as:

- `ConnectAsync()`: connects to the specified endpoint with a timeout.
- `SendAsync()`: sends data to the specified socket with a timeout.
- `ReceiveAsync()`: receives data from the specified socket with a timeout.

### `string`

`StringExtensions` provides extension methods on `string` such as:

- `EqualsNoCase()`: compares two strings ignoring case.
- `Transform().ToSentenceCase()`: transforms a string to sentence case.

### Reflector

A generic extension method `Reflector()` is available for various types (like `object`, `Type`, etc.). This is an entry-point
for reflection on the object, like in the following example:

```csharp
Type myType = ...;
PropertyInfo property = myType.Reflector().GetProperty("Name");
```

## Caching

The `Primavera.Hydrogen.Caching` namespace includes types that support caching.

### `Memoizer`

`Memoizer` allows caching the result of an expensive function.

## Configuration

The `Primavera.Hydrogen.Configuration` namespace provides types to manipulate configuration.

### `ServiceCollectionOptionsExtensions`

`ServiceCollectionOptionsExtensions` provides extension methods for the `IServiceCollection` interface
that simplify the use of `IOptions<T>`.

For example, this extension method registers an instance of a options class that is configured using `IOptions<T>`. This allows
retrieving the instance directly without needing to use `IOptionsSnapshot<T>`.

```csharp
services.AddOptionsSnapshot<MyOptions>();
```

## Dependency Injection

The `Primavera.Hydrogen.DependencyInjection` namespace provides types related with dependency injection and IoC.

### Decorators

`DecoratorsServiceCollectionExtensions` provides an implementation of the Decorator design pattern applied to the registration of services.

Using the extension methods methods available, you can replace service registrations (decorated types) by new implementation types (decorators), while still accessing the original
implementation type in the decorator.

This is particularly useful when you want to replace a service implementation in such a way that is completely transparent to all the components that depend on that service. More info [here](https://andrewlock.net/adding-decorated-classes-to-the-asp.net-core-di-container-using-scrutor/).

The extension methods available for `IServiceCollection` are:

- `AddTransientDecorator()` - replaces a given service implementation by a new one and registers it as a transient (regardless of the lifetime of the original service).
- `AddSingletonDecorator()` - replaces a given service implementation by a new one and registers it as a singleton (regardless of the lifetime of the original service).
- `AddScopedDecorator()` - replaces a given service implementation by a new one and registers it as scoped (regardless of the lifetime of the original service).

Services are decorated like this:

```csharp
ServiceCollection services = new ServiceCollection();
services.AddSingleton<IMyService, OriginalImplementation>(); // this is the service that will be decorated
services.AddTransientDecorator<IMyService, NewImplementation>(); // this registers the new implementation
```

The following happens when you decorate a service like this:

- The original registration of `IMyService` (to `OriginalImplementation`) is removed from the service collection.
- A new registration of `IMyService` (to `NewImplementation`) is added with the specified lifetime (transient in the example).
- A new registration of `OriginalImplementation` (not adhering to IMyService) is added with the original lifetime (singleton in the example).
- A new registration of `Decorator<IMyService, OriginalImplementation>` is added with the same lifetime as the new implementation (transient).

The last registration allows the new service implementation to access the old one via dependency injection, like in the following example:

```csharp
public class NewImplementation : IMyService
{
    public NewImplementation(Decorator<IMyService, OriginalImplementation> decorator)
    {
        // Notice that decorator allows accessing the original instance of the implementation

        OriginalImplementation original = decorator.Instance;
    }
}
```

## Email

The `Primavera.Hydrogen.Net.Email` namespace provides types that allow sending email messages.

The `SmtpClient` provides an abstraction to send email via SMTP, like in the following example:

```csharp
EmailMessage message = new EmailMessage()
{
    Sender = new EmailAddress("i9@primaverabss.com", "i9"),
    From = new EmailAddress("webmaster@primaverabss.com", "Web master"),
    ReplyTo = new List<EmailAddress>()
    {
         new EmailAddress("no-reply@primaverabss.com", "No Reply")
    },
    To = new List<EmailAddress>()
    {
        new EmailAddress("hugo.quintela@hotmail.com")
    },
    CC = new List<EmailAddress>()
    {
        new EmailAddress("hugo.q.ribeiro@gmail.com")
    },
    BCC = new List<EmailAddress>()
    {
        new EmailAddress("hugo.ribeiro@primaverabss.com")
    },
    Subject = $"Hydrogen unit tests: {nameof(this.SmtpHostCredentials_Real_SendAsync_Complex)}",
    BodyHtml = "<b>This is the body in HTML</b>",
    BodyText = "This is the body in text",
    Priority = EmailMessagePriority.Low,
    Attachments = new List<EmailMessageAttachment>()
    {
        new EmailMessageAttachment("File1.txt", "This is file 1.".GetBytes()),
        new EmailMessageAttachment("File2.txt", "This is file 2.".GetBytes())
    }
};

SmtpHostAddress host = GetRealHost();
SmtpHostCredentials credentials = GetRealCredentials();

using (SmtpClientHandler handler = new SmtpClientHandler(host, credentials))
{
    using (SmtpClient client = new SmtpClient(handler))
    {
        await client.SendAsync(message).ConfigureAwait(false);
    }
}
```

`SmtpClient` can also be used like a service, instantiated via dependency injection. The following extension methods for `IServiceCollection` are available for that purpose:

- `services.AddSmtpClient()` - provides instances of `SmtpClient` using default configuration specified by `SmtpClientOptions` (e.g. in the `appsettings.json` file).
- `services.AddSmtpClient(Action<SmtpClientOptions>)` - provides instances of `SmtpClient` using the configuration provided.

## `OperationResult`

`OperationResult` and `OperationResult<T>` allow modeling the results of operations as success or failures.

While `OperationResult` simply allows modeling success and failure, `OperationResult<T>` allows storing additional data for the result.

You create instances of the result using the `Success()` and `Failure()` static methods:

```csharp
OperationResult result = OperationResult.Success();
OperationResult result = OperationResult.Failure();
OperationResult result = OperationResult.Failure(new OperationError(...));

OperationResult<MyResult> result = OperationResult.Success(new MyResult(...));
OperationResult<MyResult> result = OperationResult<MyResult>.Failure();
OperationResult<MyResult> result = OperationResult.Failure(new OperationError(...), new MyResult(...));
```

Then the operation result can be evaluate using the IsSuccess and IsFailure properties or the IsFailureWith() method:

```csharp
if (result.IsSuccess)
{
}

if (result.IsFailure)
{
}

if (result.IsFailureWith("ErrorCode"))
{
}
```

## JSON Serialization

The `Primavera.Hydrogen.Text.Json.Serialization` namespace provides converters for `System.Text.Json` (`JsonSerializer`):

- `InferredTypesObjectConverter`: allows deserializing for types that have properties of type `object` (like, for example: `AppSettingValueSpec`).

## Policies

### Retry

`RetryPolicy` provides a mechanism to execute actions and retry executions when transient conditions (exceptions) are raised. This is useful when the actions are unreliable like, for example,
invoking HTTP methods, which may return transient errors.

A retry policy is composed by defining a retry strategy - which determines if and how the action should be retried - and an error detection strategy - which evaluates exceptions to detect transient
conditions and trigger retries.

In its simpler form, a retry policy is created and executed like in the following example:

```csharp
RetryPolicy policy = new RetryPolicy(new MyRetryStrategy(), new MyErrorDetectionStrategy());
```

You can execute synchronous or asynchronous actions:

```csharp
policy.Execute(
    () =>
    {
        // The action code here
    });

MyValue result = policy.Execute(
    () =>
    {
        // The action code here

        return result;
    });

await policy.ExecuteAsync(
    () =>
    {
        // The action code here

        return Task.CompletedTask;
    })
    .ConfigureAwait(false);


MyValue result = await policy.ExecuteAsync(
    () =>
    {
        // The action code here

        return Task.FromResult(result);
    })
    .ConfigureAwait(false);
```

`RetryPolicy` includes a set of static methods that allow creating retry policies that use common retry strategies:

- `RetryPolicy.FixedInterval()`: initializes a retry policy that will use the `FixedIntervalRetryStrategy` strategy.
- `RetryPolicy.Incremental()`: initializes a retry policy that will use the `IncrementalRetryStrategy` strategy.
- `RetryPolicy.ExponentialBackoff()`: initializes a retry policy that will use the `ExponentialBackoffRetryStrategy` strategy.

The `RetryPolicy<T>` generic type allows to create instances that use a specific error detection strategy:

```csharp
RetryPolicy<MyErrorDetectionStrategy> policy = new RetryPolicy<MyErrorDetectionStrategy>(new MyRetryStrategy());
```

### Retry Strategies

The following retry strategies are available:

- `FixedIntervalRetryStrategy`: A strategy with a specified number of retry attempts and a default, fixed time interval between retries.
- `IncrementalRetryStrategy`: A strategy with a specified number of retry attempts and an incremental time interval between retries.
- `ExponentialBackoffRetryStrategy`: A strategy with back-off parameters for calculating the exponential delay between retries.

### Error Detection Strategies

The following error detection strategies are available:

- `IgnoreErrorDetectionStrategy`: A detection strategy that ignores all errors (considers that none are transient).
- `HttpStatusCodeErrorDetectionStrategy`: A detection strategy based on the HTTP status code. It considers the following status codes as transient: `RequestTimeout`, `BadGateway`, `ServiceUnavailable`, `GatewayTimeout`.
- `PredicateErrorDetectionStrategy`: A detection strategy that invokes a predicate to detect transient conditions.

### Retry Count

Note that retry strategies allow to specify the maximum number of times the action should be retried.

If, for example, that value is 3, the action can be executed up to 4 times. The first time and 3 retries.

## `SmartGuard`

This type allows validating arguments and throw `ArgumentException` when the specified condition is not met.
It includes multiple overloads for various types and supports many scenarios, such as:

- `SmartGuard.NotNull(() => parameter, parameter);`
- `SmartGuard.LessThan(() => parameter, parameter, 10);`
- `SmartGuard.IsValid(() => parameter, parameter);`

## Validation Attributes

The `Primavera.Hydrogen.ComponentModel.DataAnnotations` namespace provides a set of validation attributes that can be used
to validate objects using `ObjectValidator`, `ObjectGraphValidator`, and even `System.ComponentModel.DataAnnotations.Validator`.

Attributes like:

- `GreaterThanAttribute`
- `LessThanOrEqualToAttribute`
- `NotEmptyAttribute`
- `TextMinLengthAttribute`
- `ValidCharactersAttribute`

## Validation

Validating objects (models) can be performed with two types provided in this library. These classes should be used instead of `System.ComponentModel.DataAnnotations.Validator`.

### `ObjectValidator`

`ObjectValidator` validates an object pretty much as `Validator`, meaning that will only consider the main properties of the object and will not traverse complex object graphs (when a property of the project is itself an object that should be validated).

### `ObjectGraphValidator`

This type should be used when the entire object graph needs to be validated.

## Other Classes

### `Base64`

`Base64` provides methods to encode and decode values (strings or byte arrays) to and from base64.

### `ComputeHash` & `ComputeHashString`

`ComputeHash` provides methods to generate SHA256 and SHA512 hash values from an input (byte[] or string).

`ComputeHashString` is similar but it produces strings from the results of `ComputeHash` (encoded in Base64).

### `ExecutionTimer`

`ExecutionTimer` allows measuring the execution time of a function (piece of code).

```csharp
TimeSpan d = TimeSpan.Zero;
using (ExecutionTimer timer = new ExecutionTimer("name", logger, (duration) => d = duration))
{
    (...)
}
```

### `HashCodes`

`HashCodes` provides methods to combine hash code values.

```csharp
HashCodes.Combine(value1.GetHashCode(), value2.GetHashCode(), value3.GetHashCode(StringComparison.OrdinalIgnoreCase), value4.GetHashCode(), value5.GetHashCode());
HashCodes.Combine(value1, value2, value3, value4, value5);
```

### `LoggerWrapper`

There are scenarios where a given component uses an `ILogger` that may be null depending on how that component is created.

`LoggerWrapper` provides a way to avoid repeating code like this:

```csharp
if (this.Logger != null)
{
    this.Logger.LogInformation(...);
}
```

By allowing you to set the ILogger property to a `LoggerWrapper` instance.

```csharp
public MyComponent(ILogger<MyComponent> logger)
{
    this.Logger = LoggerWrapper.Wrap(logger);
}

internal MyComponent()
{
    this.Logger = LoggerWrapper.Wrap(null);
}

public void MyMethod()
{
    this.Logger.LogDebug(...);
    // ....
}
```

> `DefaultAzureServiceTokenService` uses `LoggerWrapper`.

### `NetworkTimeClient`

`NetworkTimeClient` allows retrieving accurate date and times from a NTP server.

```csharp
NetworkTimeClient client = new NetworkTimeClient("ntp02.oal.ul.pt");
DateTime time = await client.RetrieveLocalTimeAsync().ConfigureAwait(false);
```

## `SymmetricCipher`

This is the base class for types that allow encrypting and decrypting text using different symmetric algorithms.

The following algorithms are available:

- `AesCipher`
- `RijndaelCipher`

### TransientThreadCulture

`TransientThreadCulture` allows setting the current thread culture (`CurrentCulture` and `CurrentUICulture`) for a
specific piece of code (during the scope of the `TransientThreadCulture` instance):

```csharp
using (new TransientThreadCulture("fr-FR"))
{
    // This code runs in the specified culture
}
```

### `UriHelper`

`UriHelper` provides helper methods to manipulate URL addresses.

```csharp
bool isAbsolute = UriHelper.IsAbsoluteUri(uri);
bool result = UriHelper.TryCreateAbsoluteUri("http://localhost");
bool isBase = UriHelper.IsBaseAddress(uri);
string uri = UriHelper.Combine("http://www.foo.com/", "/too/", "/many/", "/slashes/", "too", "few", "one/two/");
string uri = UriHelper.Encode(input);
```

### `Disposable`

`Disposable` provides a base class for creating classes that implement the `IDisposable` interface.

See the [Disposable](./Core.Disposable.md) documentation for more information.
