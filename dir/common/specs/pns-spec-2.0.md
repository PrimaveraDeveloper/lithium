# Push Notifications Service (PNS) Specification v2.0

The Push Notifications Service allows to receive, store and broadcast real-time notifications to the connected clients. More specifically:

* Open a ```persistent``` connection to initiate groups registration and broadcast pending notifications.
* Receive notifications from the ```PushNotifications``` endpoint and process it (save to the ```CosmosDb``` database and broadcast to the clients via ```SignalR```);
* Listen to notifications published on ```EventBus``` and process it (save to the ```CosmosDb``` database and broadcast to the clients via ```SignalR```).

# Connect to PNS and listen to notifications

To have a persistent connection with the service, there are two client packages to install:

* _@primavera/services_ - allows to create a persistent connection to the Push Notifications Service and receive real time notifications.
* _@primavera/pushNotifications_ - allows to create a persistent connection to the Push Notifications Service and receive real time notifications with the [notifications central UI](_assets/notifications_v3.png).

The code location for this packages can be found [here](_details/pns-spec-2.0-details.md#How-to-establish-a-persistent-connection-and-listen-notifications-without-the-Notifications-Central-UI).

On the product configuration, the following settings should be added to the `Web.config`.

```xml
<configSections>
	<section name="pnsConfiguration" type="Primavera.Core.Lithium.PnsConfigurationSection, Primavera.Core, Version=3.0.0.0, Culture=neutral, PublicKeyToken=33086db60a481256" />
</configSections>

<pnsConfiguration>
	<elements>
		<add name="default" pnsUri="pnsUrlHere" clientCredentialsId="yourClientCredentials" clientCredentialsSecret="yourClientSecret" clientCredentialsScope="lithium-pushnotifications" />
	</elements>
</pnsConfiguration>
```

# Push a notification

## By API

To push notifications via API, it must have the following example structure:

```json
{
  "Message": [
    {
      "culture": "en-US",
      "key": "Title",
      "value": "This is the title."
    },
    {
      "culture": "en-US",
      "key": "Message",
      "value": "This is the message! <br> <br> <a href='https://www.w3schools.com/html/html_links.asp'>See more.</a>"
    },
    {
      "culture": "pt-PT",
      "key": "Title",
      "value": "Isto é o título."
    },
    {
      "culture": "pt-PT",
      "key": "Message",
      "value": "Isto é a mensagem! <br> <br> <a href='https://www.w3schools.com/html/html_links.asp'>Ver mais.</a>"
    },
    {
      "culture": "es-ES",
      "key": "Title",
      "value": "Este es el titulo."
    },
    {
      "culture": "es-ES",
      "key": "Message",
      "value": "¡Este es el mensaje! <br> <br> <a href='https://www.w3schools.com/html/html_links.asp'>Ver más.</a>"
    }
  ],
  "Context": [
    {
      "Key": "SomeContext",
      "Value": "SomeContextValue"
    }
  ],
  "NotificationType": 1,
  "ExpiresOn": "2021-12-31T23:59:59Z",
  "Source": "Alerts",
  "Targets": [
    "JasminPremium$800001$800001-0445$d9e7a021-9fae-4ee5-ad70-27764b2ae923"
  ]
}
```

<!-- markdown-link-check-disable -->
*Note:

* To correctly display the notifications in the [notifications central UI](_assets/notifications_v3.png), the message must have as key a ```Title``` and a ```Message```;

* The message payload can receive 1024 characters but is not recommended to exceed 254 characters. The 1024 payload is just to allow user to send embed html and big url's.

* The notificationType, if not provided, default value will be 0 (Information);

* The expiration date of a notification will be defined by time to live configuration or, if provided, the ExpiresOn parameter;

* Targets are constructed when the client is connected with the server. To see how targets are constructed to send notifications to correct target, read this [documentation (TFS)](https://tfs.primaverabss.com/tfs/P.TEC.Elevation/Lithium/_versionControl?path=%24%2FLithium%2FMicroservices%2FCommon%2Fpns%2Fmainline%2Fserver%2Freadme.md&version=T&_a=preview).
<!-- markdown-link-check-enable -->

The following example is the only required fields for a notification:

```json
{
  "Message": [
    {
      "Key": "Title",
      "Value": "Message for Sales users.",
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
    "JasminPremium$800001$800001-0445$d9e7a021-9fae-4ee5-ad70-27764b2ae923"
  ]
}
```

## By an event (EventBus)

The service is listening to eventBus events with specified filters that can be changed in the configuration. The notification must follow the same structure as the API, and the object sent to eventBus must be of type ```DataTransferObject```.

If there is the need to add a new listener to another notification event (external), the new listener can be added following [this howto example](../../../howto/howto-subscribe-events-from-eventbus.md#getting-started).

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
