# Notifications Service (NS) Specification v2.0

The Notifications Service allows sending email notifications and SMS notifications, based on templates, to a set of receivers.

The templates should be defined previously and define the notification properties (the sender, priority, subject, body, etc.).

## Email Notifications

An email notification is sent to a set of receivers (each receiver is defined by an email address and an optional name) based on a template previously defined and registered with NS.

### Email Templates

The email template specifies the properties of the notification. These properties will be used by all notifications sent using the template. They include:

- The sender of the notification.
- The notification priority.
- The notification send priority.
- The notification subject.
- The notification body (in HTML).
- A value indicating whether the sender of the notification can be modified by individual notifications.

#### Sender

The sender of a notification is used to set the corresponding properties of the email message that will be sent for each notification. It can be defined using the following properties:

- `From`: the email address used in the email message `From` field. Required.
- `FromName`: the name using in the email message `From` field. Optional.
- `Sender`: the email address used in the email message `Sender` field. Optional.
- `SenderName`: the name using in the email message `Sender` field. Optional.
- `ReplyTo`: the email address used in the email message `ReplyTo` field. Optional.
- `ReplyToName`: the name using in the email message `ReplyTo` field. Optional.

#### Send Priority

All notifications are sent by the service asynchronously by a worker that executes from time to time and determines which notifications need to be sent. By default, all notifications will wait for the next execution of this worker to be sent.

This is true for notifications that have the send priority set to `Normal`.

Some scenarios - e.g. a notification required to sign-in a user - require the notifications to sent immediately (as soon as possible). This can be achieved by setting the send priority of the email template to `High`.

#### Parameters

Email templates support the notion of parameters, which are variables that can be set in the subject and/or body and that will be replaced at runtime, when the concrete notifications are sent, by the values provided by the service client.

These parameters are identified using the following notation: `{ParameterName}`.

### Sending an Email Notification

Sending a notification is straightforward:

```csharp
using NotificationsClient client = new NotificationsClient(...);

try
{
    EmailNotificationData notification = new EmailNotificationData()
    {
        Id = Guid.NewGuid(),
        EmailTemplateId = (...)
    };

    notification.Receivers.Add(
        new EmailReceiverData()
        {
            Address = (...),
            Name = (...)
        }
    );

    notification.Parameters.Add(
        new ParameterData()
        {
            Name = (...),
            Value = (...)
        }
    );

    ServiceOperationResult<Guid> response = await client.EmailNotifications.SendEmailNotificationAsync(notification).ConfigureAwait(false);

    // (...)
}
catch (ServiceException ex)
{
    // (...)
}
```

#### Overrides

When `AllowOverride` is set to true on an email template, individual notifications will be allowed to override some of the properties of the email template.

This is done by providing values in the `EmailNotificationData.Settings` property, like in the following example:

```csharp
notification.Settings = new EmailNotificationSettingsData()
{
    OverrideFrom = (...),
    OverrideFromName = (...),
    OverrideSender = (...),
    OverrideSenderName = (...),
    OverrideReplyTo = (...),
    OverrideReplyToName = (...)
};
```

### Other Operations on Email Notifications

The service provides additional operations on email notifications:

- `GetEmailNotification()` - allows retrieving the definition of an email notification.
- `GetEmailNotificationState()` - allows retrieving the state of an email notification (per receiver).
- `GetEmailNotificationsState()` - allows retrieving the state of a set of email notifications.
- `CancelEmailNotification()` - allows canceling an email notification (provided that the worker has not yet processed it).
- `RetryEmailNotification()` - allows retrying sending an email notification that failed for at least one receiver.

## SMS Notifications

A SMS notification is sent to a set of receivers (each receiver is defined by a phone number and an optional name) based on a template previously defined and registered with NS.

### SMS Templates

The SMS template specifies the properties of the notification. These properties will be used by all notifications sent using the template. They include:

- The sender of the notification.
- The notification priority.
- The notification send priority.
- The notification body (plain text).
- A value indicating whether the sender of the notification can be modified by individual notifications.

#### Sender

The sender of a notification is used to set the corresponding properties of the SMS message that will be sent for each notification. It can be defined using the following property:

- `From`: the phone number or alpha numeric identifier used in the SMS message. Required.

#### Send Priority

As with email notifications, the send priority of a SMS message can also be set to make the make message be sent faster.

#### Parameters

SMS notifications also support parameters.

### Sending a SMS Notification

Sending a SMS notification is pretty much like sending an email notification:

```csharp
using NotificationsClient client = new NotificationsClient(...);

try
{
    SmsNotificationData notification = new SmsNotificationData()
    {
        Id = Guid.NewGuid(),
        SmsTemplateId = (...)
    };

    notification.Receivers.Add(
        new SmsReceiverData()
        {
            PhoneNumber = (...),
            Name = (...)
        }
    );

    notification.Parameters.Add(
        new ParameterData()
        {
            Name = (...),
            Value = (...)
        }
    );

    ServiceOperationResult<Guid> response = await client.SmsNotifications.SendSmsNotificationAsync(notification).ConfigureAwait(false);

    // (...)
}
catch (ServiceException ex)
{
    // (...)
}
```

#### Overrides

When `AllowOverride` is set to true on a SMS template, individual notifications will be allowed to override some of the properties of the SMS template.

This is done by providing values in the `SmsNotificationData.Settings` property, like in the following example:

```csharp
notification.Settings = new SmsNotificationSettingsData()
{
    OverrideFrom = (...)
};
```

### Other Operations on SMS Notifications

The service provides additional operations on SMS notifications:

- `GetSmsNotification()` - allows retrieving the definition of a SMS notification.
- `GetSmsNotificationState()` - allows retrieving the state of a SMS notification (per receiver).
- `GetSmsNotificationsState()` - allows retrieving the state of a set of SMS notifications.
- `CancelSmsNotification()` - allows canceling a SMS notification (provided that the worker has not yet processed it).
- `RetrySmsNotification()` - allows retrying sending a SMS notification that failed for at least one receiver.

## Background Worker

All notifications are sent by a background worker that periodically inspects a queue to determine the notifications that should be sent.

This worker first sends email notifications using a SMTP client.

An email notification may have the following states:

- `Queued` - the notification is queued to be sent.
- `Sent` - the notification has been sent to the receiver.
- `Failed` - the notification could not be sent to the receiver.
- `Canceled` - the notification was canceled.

Then it sends SMS notifications using a SMS gateway.

Given the way these gateways work, the SMS notifications are first sent to the gateway using an API. This means that the actual SMS message may take a few more seconds to be actually be sent by the gateway. The NS worker keeps track of the message status in the gateway to provide feedback to the client applications.

A SMS notification may have the following states:

- `Queued` - the notification is queued to be sent.
- `Sending` - the notification has been transmitted to the SMS gateway but there is no information yet about if it has been sent to the receiver.
- `Sent` - the notification has been sent to the receiver by the SMS gateway.
- `Failed` - the notification could not be sent to the receiver.
- `Canceled` - the notification was canceled.

## Configuration

The service provides the following configuration options to customize its behavior:

- `WorkerEnabled`: a value indicating whether the notifications worker is enabled. The default value is true.
- `WorkerWaitInterval`: the period of time between executions of the background worker. The default value is 30 seconds.
- `DefaultManagers` : a list of default managers (the email addresses of these users) (see back-office).
- `EmailNotificationsTimeout`: the timeout (in minutes) for email notifications. The default value is 180 minutes.
- `SmsNotificationsTimeout`: the timeout (in minutes) for SMS notifications. The default value is 180 minutes.

> Any notification that is not processed by the background worker before the corresponding timeout (`EmailNotificationsTimeout` or `SmsNotificationsTimeout`) as elapsed is abandoned and marked as failed.

## Back-office

The Notifications Service also provides a back-office that allows some users - known as managers - to manage the service.

This back-office is accessible via the /management route and allows:

- Creating, editing, and deleting email templates.
- Creating, editing, and deleting SMS templates.
- Creating, and deleting managers.
- Viewing an overview of the notifications processed by the service (a set of counters).

The back-office does not provide operations to oversee the actual notifications sent. This kind of monitoring is only available in App Insights.