# Primavera.Hydrogen.Text.Composition

**Class library that contains types that support string composition and string formatting.**

For more information on string composition see:

- [https://docs.microsoft.com/en-us/dotnet/standard/base-types/composite-formatting](https://docs.microsoft.com/en-us/dotnet/standard/base-types/composite-formatting)
- [https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/tokens/interpolated](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/tokens/interpolated)

## TextTemplate

This class allows parsing and rendering text templates using variables defined by indexed arguments (like string.Format) and/or named arguments.

### Render vs Parse + Render

`TextTemplate` can be used in two different ways.

If you want to parse and render the text template in one step, you should call `Render` as in the following example:

```csharp
string output = TextTemplate.Render(CultureInfo.CurrentCulture, "This is a value {0}.", value);
```

> This form will configure `TextTemplate` to use the default options.

You can access reuse the input and customize options by first calling `Parse` and then `Render`:

```csharp
TextTemplate template = TextTemplate.Parse("This is a value {0}.");
string output = template.Render(CultureInfo.CurrentCulture, value);
```

### Using Indexed Arguments

Like `string.Format`, `TextTemplate` allows specifying parameters in the template using positional arguments:

```csharp
string output = TextTemplate.Render(CultureInfo.CurrentCulture, "First {0}, Second {1}, First again {0}", value1, value2);
```

### Using Named Arguments

You can also specify named parameters. In that case, the arguments can either be specified using a Dictionary or any arbitrary object.

```csharp
string output = TextTemplate.Render(
    CultureInfo.CurrentCulture,
    "First {First}, Second {Second}",
    new Dictionary<string, object>()
    {
        ["First"] = 1,
        ["Second"] = 2,
        ["Third"] = 3
    });

string output = TextTemplate.Render(
    CultureInfo.CurrentCulture,
    "First {First}, Second {Second}",
    new
    {
        First = 1,
        Second = 2,
        Third = 3
    });
```

When using objects, the parameter name can be nested in the object graph:

```csharp
string output = TextTemplate.Render(
    CultureInfo.CurrentCulture,
    "First {Person.FirstName}, Second {Person.Father.LastName}",
    myobject);
```

> When using named arguments, by default, the values will be retrieved from the first
argument in the call to `Render`.

### Mixing Indexed and Named Arguments

You can mix indexed and named arguments to control how parameters are resolved.

```csharp
string output = TextTemplate.Render(
    CultureInfo.CurrentCulture,
    "First {0}, Second {1:{Second}}, Third {1.Third}, Fourth {2.Fourth}",
    1,
    new Dictionary<string, object>()
    {
        ["Second"] = 2,
        ["Third"] = 3
    },
    new
    {
        Fourth = 4
    });
```

### Alignment and Formatting

`TextTemplate` supports alignment and formatting operators in the same way as `string.Format` does.

For alignment the operator is ',':

```csharp
string output = TextTemplate.Render(CultureInfo.CurrentCulture, "Value: {0,10}", value);
string output = TextTemplate.Render(CultureInfo.CurrentCulture, "Value: {Value,10}", new { Value = 10 });
```

For formatting the operator is ':":

```csharp
string output = TextTemplate.Render(CultureInfo.CurrentCulture, "Value: {0:t}", DateTime.Now);
string output = TextTemplate.Render(CultureInfo.CurrentCulture, "Value: {Value:t}", new { Value = DateTime.Now });
```

Both can be combined:

```csharp
string output = TextTemplate.Render(CultureInfo.CurrentCulture, "Value: {0,10:t}", DateTime.Now);
string output = TextTemplate.Render(CultureInfo.CurrentCulture, "Value: {Value,10:t}", new { Value = DateTime.Now });
```

### Options

`TextTemplate` allows the configuration of the following options:

- The character used to escape braces (double braces are used by default, e.g., `{{token}}`).
- How errors are handled when parsing input or formatting the output (by default the parameters in error will remain unmodified in the output).

These options can be use if `TextTemplate` is used with Parse+Render form via the following methods:

- `UseDefaultOptions()` - sets the default options.
- `UseAlternativeEscapeChar()` - sets the character that should be used to escape braces.
- `OnErrorMantainTokens()` - on error, the tokens will remain unmodified in the output.
- `OnErrorThrowException()` - on error, an exception of type `TextTemplateException` will be raised.
- `OnErrorOutputError()` - on error, the error message will be present in the output.
- `OnErrorIgnore()` - on error, the tokens in error will be ignored and removed from the output.

Example:

```csharp
string output = TextTemplate
    .Parse(...)
    .UseAlternativeEscapeChar()
    .OnErrorThrowException()
    .Render(...);
```
