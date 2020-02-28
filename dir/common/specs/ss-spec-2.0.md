# Settings Service (SS) Specification v2.0

The Settings Service allows storing name/value settings per user, per product, and per product/user. It also allows storing user pictures.

## Product Settings

A product setting allows storing a key/value pair for a given product.

`ProductSettingData` represents such a setting:

- `ProductId` - the product identifier.
- `Key` - the setting key.
- `Value` - the setting value.
- `Properties` - a dictionary of properties associated with the setting.
- `ModifiedOn` - the moment when the setting was last modified.

### Saving a Product Setting

```csharp
using SettingsClient client = new SettingsClient(...);

try
{
    ProductSettingData setting = new ProductSettingData()
    {
        (...)
    };
    
    ServiceOperationResult result = await client
        .ProductSettings
            .SaveProductSettingAsync(
                setting)
        .ConfigureAwait(false);
}
catch (ServiceException ex)
{
    (...)
}
```

### Retrieving a Product Setting

```csharp
using SettingsClient client = new SettingsClient(...);

try
{
    string productId = (...);
    string key = (...);

    ServiceOperationResult<ProductSettingData> result = await client
        .ProductSettings
            .GetProductSettingAsync(
                productId,
                key)
        .ConfigureAwait(false);

    ProductSettingData setting = result.Body;
}
catch (ServiceException ex)
{
    (...)
}
```

### Deleting a Product Setting

```csharp
using SettingsClient client = new SettingsClient(...);

try
{
    string productId = (...);
    string key = (...);

    ServiceOperationResult result = await client
        .ProductSettings
            .DeleteProductSettingAsync(
                productId,
                key)
        .ConfigureAwait(false);
}
catch (ServiceException ex)
{
    (...)
}
```

### Retrieving All Settings for a Product

```csharp
using SettingsClient client = new SettingsClient(...);

try
{
    string productId = (...);
    string key = (...);

    ServiceOperationResult<IEnumerable<ProductSettingData>> result = await client
        .ProductSettings
            .GetProductSettingsAsync(
                productId)
        .ConfigureAwait(false);

    IEnumerable<ProductSettingData> settings = result.Body;
}
catch (ServiceException ex)
{
    (...)
}
```

## User Settings

A user setting allows storing a key/value pair for a given user.

`UserSettingData` represents such a setting:

- `UserId` - the user identifier.
- `Key` - the setting key.
- `Value` - the setting value.
- `Properties` - a dictionary of properties associated with the setting.
- `ModifiedOn` - the moment when the setting was last modified.

## Product/User Settings

A product/user setting allows storing a key/value pair for a given product and a given user.

`ProductUserSettingData` represents such a setting:

- `ProductId` - the product identifier.
- `UserId` - the user identifier.
- `Key` - the setting key.
- `Value` - the setting value.
- `Properties` - a dictionary of properties associated with the setting.
- `ModifiedOn` - the moment when the setting was last modified.

## User Pictures

You can also save user pictures (the image) using the Settings Service.

`UserPictureData` represents such the picture:

- `Bytes` - the image bytes.
- `MediaType` - the image media type.
- `PublicUri` - the user picture public URI (read-only).
- `Properties` - a dictionary of properties associated with the user picture.
- `ModifiedOn` - the moment when the user picture was last modified.

### Saving a User Picture

```csharp
using SettingsClient client = new SettingsClient(...);

try
{
    UserPictureData picture = new UserPictureData()
    {
        (...)
    }

    ServiceOperationResult result = await client
        .UserPicture
            .SaveUserPictureAsync(
                userId,
                picture)
        .ConfigureAwait(false);
}
catch (ServiceException ex)
{
    (...)
}
```

You can also save the picture by uploading a file from the file system:

```csharp
using SettingsClient client = new SettingsClient(...);

try
{
    string userId = (...);
    string fileName = (...);

    ServiceOperationResult result = await client
        .UserPicture
            .SaveUserPictureAsync(
                userId,
                fileName)
        .ConfigureAwait(false);
}
catch (ServiceException ex)
{
    (...)
}
```

### Retrieving a User Picture

```csharp
using SettingsClient client = new SettingsClient(...);

try
{
    string userId = (...);

    ServiceOperationResult<UserPictureData> result = await client
        .UserPicture
            .GetUserPictureAsync(
                userId)
        .ConfigureAwait(false);

    UserPictureData picture = result.Body;
}
catch (ServiceException ex)
{
    (...)
}
```

### Deleting a User Picture

```csharp
using SettingsClient client = new SettingsClient(...);

try
{
    string userId = (...);

    ServiceOperationResult result = await client
        .UserPicture
            .DeleteUserPictureAsync(
                userId)
        .ConfigureAwait(false);
}
catch (ServiceException ex)
{
    (...)
}
```

## Storage

Settings are storage in table storage and user pictures are stored in blob storage. The concrete storage accounts are specified in configuration:

```json
{
    "AzureTableStorageOptions": {
        "(...)"
    },
    "AzureBlobStorageOptions": {
        "(...)"
    }
}
```

## Caching

The service uses a REDIS cache to reduce the dependency on the table storage and blob storage. This cache is transparent for clients.