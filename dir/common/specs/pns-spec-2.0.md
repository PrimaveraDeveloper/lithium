# Push Notifications Service (PNS) Specification v2.0

The Push Notifications Service allows to receive, store and broadcast real-time notifications to the connected clients. More specifically:

* Open a ```WebSocket``` connection to initiate groups registration and broadcast pending notifications.
* Receive notifications from the ```PushNotifications``` endpoint and process it (save to the ```CosmosDb``` database and broadcast to the clients via ```SignalR```);
* Listen to notifications published on ```EventBus``` and process it (save to the ```CosmosDb``` database and broadcast to the clients via ```SignalR```).

# Connect to PNS and listen to notifications

To have a persistent connection with the service, there are two client packages to choose from:

* _@primavera/services_ - allows to create a persistent connection to the Push Notifications Service and receive real time notifications.
* _@primavera/pushNotifications_ - allows to create a persistent connection to the Push Notifications Service and receive real time notifications with the [notifications central UI](_assets/notifications_v3.png).

The packages localizations can be found [here](_details/pns-spec-2.0-details.md#How-to-establish-a-persistent-connection-and-listen-notifications-without-the-Notifications-Central-UI).

# Push a notification

## By API

To push notifications via API, it must have the following example structure:

```json
{
  "Message": [
    {
      "Key": "Title",
      "Value": "Message for Dummy User.",
      "Culture": "en-US"
    },
    {
      "Key": "Message",
      "Value": "This is a success message and it will expire on 12 of February at 6:00PM!",
      "Culture": "en-US"
    }
  ],
  "Context": [
    {
      "Key": "SomeContext",
      "Value": "SomeContextValue"
    }
  ],
  "NotificationType": 1,
  "ExpiresOn": "2020-02-12T18:00:00Z",
  "Source": "Alerts",
  "Targets": [
    "dummyuser@mail.com"
  ]
}
```

<!-- markdown-link-check-disable -->
> Note: 
> * To correctly display the notifications in the [notifications central UI](_assets/notifications_v3.png), the message must have as key a ```Title``` and a ```Message```;
> * The notificationType, if not provided, default value will be 0 (Information);
> * The expiration date of a notification will be defined by time to live configuration or, if provided, the ExpiresOn parameter;
> * Targets are constructed when the client is connected with the server. To see how targets are constructed to send notifications to correct target, read this [documentation (TFS)](https://tfs.primaverabss.com/tfs/P.TEC.Elevation/Lithium/_versionControl?path=%24%2FLithium%2FMicroservices%2Fcommon%2FPNS%2FMainline%2Freadme.md&version=T&_a=preview).
<!-- markdown-link-check-enable -->

The following example is the only required fields for a notification:

```json
{
  "Message": [
    {
      "Key": "Title",
      "Value": "Message for Dummy User.",
      "Culture": "en-US"
    },
    {
      "Key": "Message",
      "Value": "This is an info message!",
      "Culture": "en-US"
    }
  ],
  "Source": "Marketing",
  "Targets": [
    "dummyuser@mail.com"
  ]
}
```

<!-- markdown-link-check-disable -->
Tha available endpoints can be found [here](https://tfs.primaverabss.com/tfs/P.TEC.Elevation/Lithium/_versionControl?path=%24%2FLithium%2FMicroservices%2Fcommon%2FPNS%2FMainline%2Freadme.md&version=T&_a=preview).
<!-- markdown-link-check-enable -->

## By an event (EventBus)

The service is listening to eventBus events with specified filters that can be changed in the configuration. The notification must follow the same structure as the API, and the object sent to eventBus must be of type ```DataTransferObject```.

If there is the need to add a new listener to another notification event (external), the new listener can be added following [this howto example](../../../howto/howto-subscribe-new-notifications-events.md#getting-started).

# Configuration

## Notifications Time To Live

Each notification has an expiration date that is defined by NotificationType, or when it is provided by ExpiresOn parameter. It is possible to configure a ttl for each notificationType in the configuration such as:

```json
"ServiceConfiguration": {
    "NotificationTimeToLive": {
      "Info": 86400,
      "Success": 86400,
      "Warning": 691200,
      "Error": 2592000
    }
}
```

> Note: The ttl is defined in seconds. To turn off the ttl the value provided must be -1. More info about the ttl can be found [here](_details/pns-spec-2.0-details.md#NotificationType).

## Eventbus filters

To specify the filters for the eventBus events, it can be done in the ```SubscriptionFilters``` configuration like:

```json
"SubscriptionFilters": [
        {
          "Key": "Source",
          "Value": "TBX"
        },
        {
          "Key": "Version",
          "Value": "1.0"
        }
]
```

## CORS

In order to allow the browser to connect to the service using persistent connection and to SignalR to work properly, CORS must be correctly configured in the service such as:

```json
"CorsPolicyConfiguration": {
      "WithOrigins": [ "http://localhost:51913", "http://localhost:4200" ],
      "SetIsOriginAllowed": true
}
```

## SignalR

SignalR also can be configured in the settings, whenever it is needed. For example, if the client is using an old browser that does not support ```WebSocket``` connection, or the client timeout must be modified, it can be done in the SignalR configuration:

```json
"SignalR": {
      "ConnectionString": "someConnectionString",
      "ClientTimeoutInterval": 3600,
      "HandshakeTimeout": 30,
      "KeepAliveInterval": 15,
      "EnableDetailedErrors": true,
      "MaximumReceiveMessageSize": 32000,
      "StreamBufferCapacity": 10,
      "SupportedProtocols": ["WebSockets", "ServerSentEvents", "LongPolling", "ForeverFrame"],
      "ServerConnectionCount": 1
    }
```

# Details

Full details about all topics described above can be found in the [Details section](_details/pns-spec-2.0-details.md#Technologies-used).