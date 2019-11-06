<a name='assembly'></a>
# Primavera.Lithium.Notifications

## Contents

- [EmailNotificationsOperations](#T-Primavera-Lithium-Notifications-EmailNotificationsOperations 'Primavera.Lithium.Notifications.EmailNotificationsOperations')
  - [#ctor(client,actions)](#M-Primavera-Lithium-Notifications-EmailNotificationsOperations-#ctor-Primavera-Lithium-Notifications-NotificationsClientBase,Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase}- 'Primavera.Lithium.Notifications.EmailNotificationsOperations.#ctor(Primavera.Lithium.Notifications.NotificationsClientBase,Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase})')
- [EmailNotificationsOperationsBase](#T-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase 'Primavera.Lithium.Notifications.EmailNotificationsOperationsBase')
  - [#ctor(client,actions)](#M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-#ctor-Primavera-Lithium-Notifications-NotificationsClientBase,Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase}- 'Primavera.Lithium.Notifications.EmailNotificationsOperationsBase.#ctor(Primavera.Lithium.Notifications.NotificationsClientBase,Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase})')
  - [Actions](#P-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-Actions 'Primavera.Lithium.Notifications.EmailNotificationsOperationsBase.Actions')
  - [Client](#P-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-Client 'Primavera.Lithium.Notifications.EmailNotificationsOperationsBase.Client')
  - [CancelEmailNotification()](#M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-CancelEmailNotification-System-Guid- 'Primavera.Lithium.Notifications.EmailNotificationsOperationsBase.CancelEmailNotification(System.Guid)')
  - [CancelEmailNotificationAsync()](#M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-CancelEmailNotificationAsync-System-Guid,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.EmailNotificationsOperationsBase.CancelEmailNotificationAsync(System.Guid,System.Threading.CancellationToken)')
  - [Dispose()](#M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-Dispose 'Primavera.Lithium.Notifications.EmailNotificationsOperationsBase.Dispose')
  - [Dispose(disposing)](#M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-Dispose-System-Boolean- 'Primavera.Lithium.Notifications.EmailNotificationsOperationsBase.Dispose(System.Boolean)')
  - [GetEmailNotification()](#M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-GetEmailNotification-System-Guid- 'Primavera.Lithium.Notifications.EmailNotificationsOperationsBase.GetEmailNotification(System.Guid)')
  - [GetEmailNotificationAsync()](#M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-GetEmailNotificationAsync-System-Guid,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.EmailNotificationsOperationsBase.GetEmailNotificationAsync(System.Guid,System.Threading.CancellationToken)')
  - [GetEmailNotificationState()](#M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-GetEmailNotificationState-System-Guid- 'Primavera.Lithium.Notifications.EmailNotificationsOperationsBase.GetEmailNotificationState(System.Guid)')
  - [GetEmailNotificationStateAsync()](#M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-GetEmailNotificationStateAsync-System-Guid,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.EmailNotificationsOperationsBase.GetEmailNotificationStateAsync(System.Guid,System.Threading.CancellationToken)')
  - [GetEmailNotificationsState()](#M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-GetEmailNotificationsState-System-Collections-Generic-IEnumerable{System-Guid}- 'Primavera.Lithium.Notifications.EmailNotificationsOperationsBase.GetEmailNotificationsState(System.Collections.Generic.IEnumerable{System.Guid})')
  - [GetEmailNotificationsStateAsync()](#M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-GetEmailNotificationsStateAsync-System-Collections-Generic-IEnumerable{System-Guid},System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.EmailNotificationsOperationsBase.GetEmailNotificationsStateAsync(System.Collections.Generic.IEnumerable{System.Guid},System.Threading.CancellationToken)')
  - [RetryEmailNotification()](#M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-RetryEmailNotification-System-Guid- 'Primavera.Lithium.Notifications.EmailNotificationsOperationsBase.RetryEmailNotification(System.Guid)')
  - [RetryEmailNotificationAsync()](#M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-RetryEmailNotificationAsync-System-Guid,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.EmailNotificationsOperationsBase.RetryEmailNotificationAsync(System.Guid,System.Threading.CancellationToken)')
  - [SendEmailNotification()](#M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-SendEmailNotification-Primavera-Lithium-Notifications-Models-EmailNotificationData- 'Primavera.Lithium.Notifications.EmailNotificationsOperationsBase.SendEmailNotification(Primavera.Lithium.Notifications.Models.EmailNotificationData)')
  - [SendEmailNotificationAsync()](#M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-SendEmailNotificationAsync-Primavera-Lithium-Notifications-Models-EmailNotificationData,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.EmailNotificationsOperationsBase.SendEmailNotificationAsync(Primavera.Lithium.Notifications.Models.EmailNotificationData,System.Threading.CancellationToken)')
- [EmailTemplatesOperations](#T-Primavera-Lithium-Notifications-EmailTemplatesOperations 'Primavera.Lithium.Notifications.EmailTemplatesOperations')
  - [#ctor(client,actions)](#M-Primavera-Lithium-Notifications-EmailTemplatesOperations-#ctor-Primavera-Lithium-Notifications-NotificationsClientBase,Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase}- 'Primavera.Lithium.Notifications.EmailTemplatesOperations.#ctor(Primavera.Lithium.Notifications.NotificationsClientBase,Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase})')
- [EmailTemplatesOperationsBase](#T-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase 'Primavera.Lithium.Notifications.EmailTemplatesOperationsBase')
  - [#ctor(client,actions)](#M-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-#ctor-Primavera-Lithium-Notifications-NotificationsClientBase,Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase}- 'Primavera.Lithium.Notifications.EmailTemplatesOperationsBase.#ctor(Primavera.Lithium.Notifications.NotificationsClientBase,Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase})')
  - [Actions](#P-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-Actions 'Primavera.Lithium.Notifications.EmailTemplatesOperationsBase.Actions')
  - [Client](#P-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-Client 'Primavera.Lithium.Notifications.EmailTemplatesOperationsBase.Client')
  - [CreateEmailTemplate()](#M-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-CreateEmailTemplate-Primavera-Lithium-Notifications-Models-EmailTemplateData- 'Primavera.Lithium.Notifications.EmailTemplatesOperationsBase.CreateEmailTemplate(Primavera.Lithium.Notifications.Models.EmailTemplateData)')
  - [CreateEmailTemplateAsync()](#M-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-CreateEmailTemplateAsync-Primavera-Lithium-Notifications-Models-EmailTemplateData,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.EmailTemplatesOperationsBase.CreateEmailTemplateAsync(Primavera.Lithium.Notifications.Models.EmailTemplateData,System.Threading.CancellationToken)')
  - [DeleteEmailTemplate()](#M-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-DeleteEmailTemplate-System-String- 'Primavera.Lithium.Notifications.EmailTemplatesOperationsBase.DeleteEmailTemplate(System.String)')
  - [DeleteEmailTemplateAsync()](#M-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-DeleteEmailTemplateAsync-System-String,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.EmailTemplatesOperationsBase.DeleteEmailTemplateAsync(System.String,System.Threading.CancellationToken)')
  - [Dispose()](#M-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-Dispose 'Primavera.Lithium.Notifications.EmailTemplatesOperationsBase.Dispose')
  - [Dispose(disposing)](#M-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-Dispose-System-Boolean- 'Primavera.Lithium.Notifications.EmailTemplatesOperationsBase.Dispose(System.Boolean)')
  - [GetEmailTemplate()](#M-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-GetEmailTemplate-System-String- 'Primavera.Lithium.Notifications.EmailTemplatesOperationsBase.GetEmailTemplate(System.String)')
  - [GetEmailTemplateAsync()](#M-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-GetEmailTemplateAsync-System-String,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.EmailTemplatesOperationsBase.GetEmailTemplateAsync(System.String,System.Threading.CancellationToken)')
  - [GetEmailTemplates()](#M-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-GetEmailTemplates 'Primavera.Lithium.Notifications.EmailTemplatesOperationsBase.GetEmailTemplates')
  - [GetEmailTemplatesAsync()](#M-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-GetEmailTemplatesAsync-System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.EmailTemplatesOperationsBase.GetEmailTemplatesAsync(System.Threading.CancellationToken)')
  - [UpdateEmailTemplate()](#M-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-UpdateEmailTemplate-Primavera-Lithium-Notifications-Models-EmailTemplateData- 'Primavera.Lithium.Notifications.EmailTemplatesOperationsBase.UpdateEmailTemplate(Primavera.Lithium.Notifications.Models.EmailTemplateData)')
  - [UpdateEmailTemplateAsync()](#M-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-UpdateEmailTemplateAsync-Primavera-Lithium-Notifications-Models-EmailTemplateData,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.EmailTemplatesOperationsBase.UpdateEmailTemplateAsync(Primavera.Lithium.Notifications.Models.EmailTemplateData,System.Threading.CancellationToken)')
- [IEmailNotificationsOperations](#T-Primavera-Lithium-Notifications-IEmailNotificationsOperations 'Primavera.Lithium.Notifications.IEmailNotificationsOperations')
  - [CancelEmailNotification(id)](#M-Primavera-Lithium-Notifications-IEmailNotificationsOperations-CancelEmailNotification-System-Guid- 'Primavera.Lithium.Notifications.IEmailNotificationsOperations.CancelEmailNotification(System.Guid)')
  - [CancelEmailNotificationAsync(id,cancellationToken)](#M-Primavera-Lithium-Notifications-IEmailNotificationsOperations-CancelEmailNotificationAsync-System-Guid,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.IEmailNotificationsOperations.CancelEmailNotificationAsync(System.Guid,System.Threading.CancellationToken)')
  - [GetEmailNotification(id)](#M-Primavera-Lithium-Notifications-IEmailNotificationsOperations-GetEmailNotification-System-Guid- 'Primavera.Lithium.Notifications.IEmailNotificationsOperations.GetEmailNotification(System.Guid)')
  - [GetEmailNotificationAsync(id,cancellationToken)](#M-Primavera-Lithium-Notifications-IEmailNotificationsOperations-GetEmailNotificationAsync-System-Guid,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.IEmailNotificationsOperations.GetEmailNotificationAsync(System.Guid,System.Threading.CancellationToken)')
  - [GetEmailNotificationState(id)](#M-Primavera-Lithium-Notifications-IEmailNotificationsOperations-GetEmailNotificationState-System-Guid- 'Primavera.Lithium.Notifications.IEmailNotificationsOperations.GetEmailNotificationState(System.Guid)')
  - [GetEmailNotificationStateAsync(id,cancellationToken)](#M-Primavera-Lithium-Notifications-IEmailNotificationsOperations-GetEmailNotificationStateAsync-System-Guid,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.IEmailNotificationsOperations.GetEmailNotificationStateAsync(System.Guid,System.Threading.CancellationToken)')
  - [GetEmailNotificationsState(ids)](#M-Primavera-Lithium-Notifications-IEmailNotificationsOperations-GetEmailNotificationsState-System-Collections-Generic-IEnumerable{System-Guid}- 'Primavera.Lithium.Notifications.IEmailNotificationsOperations.GetEmailNotificationsState(System.Collections.Generic.IEnumerable{System.Guid})')
  - [GetEmailNotificationsStateAsync(ids,cancellationToken)](#M-Primavera-Lithium-Notifications-IEmailNotificationsOperations-GetEmailNotificationsStateAsync-System-Collections-Generic-IEnumerable{System-Guid},System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.IEmailNotificationsOperations.GetEmailNotificationsStateAsync(System.Collections.Generic.IEnumerable{System.Guid},System.Threading.CancellationToken)')
  - [RetryEmailNotification(id)](#M-Primavera-Lithium-Notifications-IEmailNotificationsOperations-RetryEmailNotification-System-Guid- 'Primavera.Lithium.Notifications.IEmailNotificationsOperations.RetryEmailNotification(System.Guid)')
  - [RetryEmailNotificationAsync(id,cancellationToken)](#M-Primavera-Lithium-Notifications-IEmailNotificationsOperations-RetryEmailNotificationAsync-System-Guid,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.IEmailNotificationsOperations.RetryEmailNotificationAsync(System.Guid,System.Threading.CancellationToken)')
  - [SendEmailNotification(notification)](#M-Primavera-Lithium-Notifications-IEmailNotificationsOperations-SendEmailNotification-Primavera-Lithium-Notifications-Models-EmailNotificationData- 'Primavera.Lithium.Notifications.IEmailNotificationsOperations.SendEmailNotification(Primavera.Lithium.Notifications.Models.EmailNotificationData)')
  - [SendEmailNotificationAsync(notification,cancellationToken)](#M-Primavera-Lithium-Notifications-IEmailNotificationsOperations-SendEmailNotificationAsync-Primavera-Lithium-Notifications-Models-EmailNotificationData,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.IEmailNotificationsOperations.SendEmailNotificationAsync(Primavera.Lithium.Notifications.Models.EmailNotificationData,System.Threading.CancellationToken)')
- [IEmailTemplatesOperations](#T-Primavera-Lithium-Notifications-IEmailTemplatesOperations 'Primavera.Lithium.Notifications.IEmailTemplatesOperations')
  - [CreateEmailTemplate(emailTemplate)](#M-Primavera-Lithium-Notifications-IEmailTemplatesOperations-CreateEmailTemplate-Primavera-Lithium-Notifications-Models-EmailTemplateData- 'Primavera.Lithium.Notifications.IEmailTemplatesOperations.CreateEmailTemplate(Primavera.Lithium.Notifications.Models.EmailTemplateData)')
  - [CreateEmailTemplateAsync(emailTemplate,cancellationToken)](#M-Primavera-Lithium-Notifications-IEmailTemplatesOperations-CreateEmailTemplateAsync-Primavera-Lithium-Notifications-Models-EmailTemplateData,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.IEmailTemplatesOperations.CreateEmailTemplateAsync(Primavera.Lithium.Notifications.Models.EmailTemplateData,System.Threading.CancellationToken)')
  - [DeleteEmailTemplate(id)](#M-Primavera-Lithium-Notifications-IEmailTemplatesOperations-DeleteEmailTemplate-System-String- 'Primavera.Lithium.Notifications.IEmailTemplatesOperations.DeleteEmailTemplate(System.String)')
  - [DeleteEmailTemplateAsync(id,cancellationToken)](#M-Primavera-Lithium-Notifications-IEmailTemplatesOperations-DeleteEmailTemplateAsync-System-String,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.IEmailTemplatesOperations.DeleteEmailTemplateAsync(System.String,System.Threading.CancellationToken)')
  - [GetEmailTemplate(id)](#M-Primavera-Lithium-Notifications-IEmailTemplatesOperations-GetEmailTemplate-System-String- 'Primavera.Lithium.Notifications.IEmailTemplatesOperations.GetEmailTemplate(System.String)')
  - [GetEmailTemplateAsync(id,cancellationToken)](#M-Primavera-Lithium-Notifications-IEmailTemplatesOperations-GetEmailTemplateAsync-System-String,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.IEmailTemplatesOperations.GetEmailTemplateAsync(System.String,System.Threading.CancellationToken)')
  - [GetEmailTemplates()](#M-Primavera-Lithium-Notifications-IEmailTemplatesOperations-GetEmailTemplates 'Primavera.Lithium.Notifications.IEmailTemplatesOperations.GetEmailTemplates')
  - [GetEmailTemplatesAsync(cancellationToken)](#M-Primavera-Lithium-Notifications-IEmailTemplatesOperations-GetEmailTemplatesAsync-System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.IEmailTemplatesOperations.GetEmailTemplatesAsync(System.Threading.CancellationToken)')
  - [UpdateEmailTemplate(emailTemplate)](#M-Primavera-Lithium-Notifications-IEmailTemplatesOperations-UpdateEmailTemplate-Primavera-Lithium-Notifications-Models-EmailTemplateData- 'Primavera.Lithium.Notifications.IEmailTemplatesOperations.UpdateEmailTemplate(Primavera.Lithium.Notifications.Models.EmailTemplateData)')
  - [UpdateEmailTemplateAsync(emailTemplate,cancellationToken)](#M-Primavera-Lithium-Notifications-IEmailTemplatesOperations-UpdateEmailTemplateAsync-Primavera-Lithium-Notifications-Models-EmailTemplateData,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.IEmailTemplatesOperations.UpdateEmailTemplateAsync(Primavera.Lithium.Notifications.Models.EmailTemplateData,System.Threading.CancellationToken)')
- [IMonitoringOperations](#T-Primavera-Lithium-Notifications-IMonitoringOperations 'Primavera.Lithium.Notifications.IMonitoringOperations')
  - [DiagnosticsAsync(cancellationToken)](#M-Primavera-Lithium-Notifications-IMonitoringOperations-DiagnosticsAsync-System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.IMonitoringOperations.DiagnosticsAsync(System.Threading.CancellationToken)')
  - [ProbeAsync(cancellationToken)](#M-Primavera-Lithium-Notifications-IMonitoringOperations-ProbeAsync-System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.IMonitoringOperations.ProbeAsync(System.Threading.CancellationToken)')
- [INotificationsClient](#T-Primavera-Lithium-Notifications-INotificationsClient 'Primavera.Lithium.Notifications.INotificationsClient')
  - [EmailNotifications](#P-Primavera-Lithium-Notifications-INotificationsClient-EmailNotifications 'Primavera.Lithium.Notifications.INotificationsClient.EmailNotifications')
  - [EmailTemplates](#P-Primavera-Lithium-Notifications-INotificationsClient-EmailTemplates 'Primavera.Lithium.Notifications.INotificationsClient.EmailTemplates')
  - [Monitoring](#P-Primavera-Lithium-Notifications-INotificationsClient-Monitoring 'Primavera.Lithium.Notifications.INotificationsClient.Monitoring')
  - [SmsNotifications](#P-Primavera-Lithium-Notifications-INotificationsClient-SmsNotifications 'Primavera.Lithium.Notifications.INotificationsClient.SmsNotifications')
  - [SmsTemplates](#P-Primavera-Lithium-Notifications-INotificationsClient-SmsTemplates 'Primavera.Lithium.Notifications.INotificationsClient.SmsTemplates')
- [ISmsNotificationsOperations](#T-Primavera-Lithium-Notifications-ISmsNotificationsOperations 'Primavera.Lithium.Notifications.ISmsNotificationsOperations')
  - [CancelSmsNotification(id)](#M-Primavera-Lithium-Notifications-ISmsNotificationsOperations-CancelSmsNotification-System-Guid- 'Primavera.Lithium.Notifications.ISmsNotificationsOperations.CancelSmsNotification(System.Guid)')
  - [CancelSmsNotificationAsync(id,cancellationToken)](#M-Primavera-Lithium-Notifications-ISmsNotificationsOperations-CancelSmsNotificationAsync-System-Guid,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.ISmsNotificationsOperations.CancelSmsNotificationAsync(System.Guid,System.Threading.CancellationToken)')
  - [GetSmsNotification(id)](#M-Primavera-Lithium-Notifications-ISmsNotificationsOperations-GetSmsNotification-System-Guid- 'Primavera.Lithium.Notifications.ISmsNotificationsOperations.GetSmsNotification(System.Guid)')
  - [GetSmsNotificationAsync(id,cancellationToken)](#M-Primavera-Lithium-Notifications-ISmsNotificationsOperations-GetSmsNotificationAsync-System-Guid,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.ISmsNotificationsOperations.GetSmsNotificationAsync(System.Guid,System.Threading.CancellationToken)')
  - [GetSmsNotificationState(id)](#M-Primavera-Lithium-Notifications-ISmsNotificationsOperations-GetSmsNotificationState-System-Guid- 'Primavera.Lithium.Notifications.ISmsNotificationsOperations.GetSmsNotificationState(System.Guid)')
  - [GetSmsNotificationStateAsync(id,cancellationToken)](#M-Primavera-Lithium-Notifications-ISmsNotificationsOperations-GetSmsNotificationStateAsync-System-Guid,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.ISmsNotificationsOperations.GetSmsNotificationStateAsync(System.Guid,System.Threading.CancellationToken)')
  - [GetSmsNotificationsState(ids)](#M-Primavera-Lithium-Notifications-ISmsNotificationsOperations-GetSmsNotificationsState-System-Collections-Generic-IEnumerable{System-Guid}- 'Primavera.Lithium.Notifications.ISmsNotificationsOperations.GetSmsNotificationsState(System.Collections.Generic.IEnumerable{System.Guid})')
  - [GetSmsNotificationsStateAsync(ids,cancellationToken)](#M-Primavera-Lithium-Notifications-ISmsNotificationsOperations-GetSmsNotificationsStateAsync-System-Collections-Generic-IEnumerable{System-Guid},System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.ISmsNotificationsOperations.GetSmsNotificationsStateAsync(System.Collections.Generic.IEnumerable{System.Guid},System.Threading.CancellationToken)')
  - [RetrySmsNotification(id)](#M-Primavera-Lithium-Notifications-ISmsNotificationsOperations-RetrySmsNotification-System-Guid- 'Primavera.Lithium.Notifications.ISmsNotificationsOperations.RetrySmsNotification(System.Guid)')
  - [RetrySmsNotificationAsync(id,cancellationToken)](#M-Primavera-Lithium-Notifications-ISmsNotificationsOperations-RetrySmsNotificationAsync-System-Guid,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.ISmsNotificationsOperations.RetrySmsNotificationAsync(System.Guid,System.Threading.CancellationToken)')
  - [SendSmsNotification(notification)](#M-Primavera-Lithium-Notifications-ISmsNotificationsOperations-SendSmsNotification-Primavera-Lithium-Notifications-Models-SmsNotificationData- 'Primavera.Lithium.Notifications.ISmsNotificationsOperations.SendSmsNotification(Primavera.Lithium.Notifications.Models.SmsNotificationData)')
  - [SendSmsNotificationAsync(notification,cancellationToken)](#M-Primavera-Lithium-Notifications-ISmsNotificationsOperations-SendSmsNotificationAsync-Primavera-Lithium-Notifications-Models-SmsNotificationData,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.ISmsNotificationsOperations.SendSmsNotificationAsync(Primavera.Lithium.Notifications.Models.SmsNotificationData,System.Threading.CancellationToken)')
- [ISmsTemplatesOperations](#T-Primavera-Lithium-Notifications-ISmsTemplatesOperations 'Primavera.Lithium.Notifications.ISmsTemplatesOperations')
  - [CreateSmsTemplate(smsTemplate)](#M-Primavera-Lithium-Notifications-ISmsTemplatesOperations-CreateSmsTemplate-Primavera-Lithium-Notifications-Models-SmsTemplateData- 'Primavera.Lithium.Notifications.ISmsTemplatesOperations.CreateSmsTemplate(Primavera.Lithium.Notifications.Models.SmsTemplateData)')
  - [CreateSmsTemplateAsync(smsTemplate,cancellationToken)](#M-Primavera-Lithium-Notifications-ISmsTemplatesOperations-CreateSmsTemplateAsync-Primavera-Lithium-Notifications-Models-SmsTemplateData,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.ISmsTemplatesOperations.CreateSmsTemplateAsync(Primavera.Lithium.Notifications.Models.SmsTemplateData,System.Threading.CancellationToken)')
  - [DeleteSmsTemplate(id)](#M-Primavera-Lithium-Notifications-ISmsTemplatesOperations-DeleteSmsTemplate-System-String- 'Primavera.Lithium.Notifications.ISmsTemplatesOperations.DeleteSmsTemplate(System.String)')
  - [DeleteSmsTemplateAsync(id,cancellationToken)](#M-Primavera-Lithium-Notifications-ISmsTemplatesOperations-DeleteSmsTemplateAsync-System-String,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.ISmsTemplatesOperations.DeleteSmsTemplateAsync(System.String,System.Threading.CancellationToken)')
  - [GetSmsTemplate(id)](#M-Primavera-Lithium-Notifications-ISmsTemplatesOperations-GetSmsTemplate-System-String- 'Primavera.Lithium.Notifications.ISmsTemplatesOperations.GetSmsTemplate(System.String)')
  - [GetSmsTemplateAsync(id,cancellationToken)](#M-Primavera-Lithium-Notifications-ISmsTemplatesOperations-GetSmsTemplateAsync-System-String,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.ISmsTemplatesOperations.GetSmsTemplateAsync(System.String,System.Threading.CancellationToken)')
  - [GetSmsTemplates()](#M-Primavera-Lithium-Notifications-ISmsTemplatesOperations-GetSmsTemplates 'Primavera.Lithium.Notifications.ISmsTemplatesOperations.GetSmsTemplates')
  - [GetSmsTemplatesAsync(cancellationToken)](#M-Primavera-Lithium-Notifications-ISmsTemplatesOperations-GetSmsTemplatesAsync-System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.ISmsTemplatesOperations.GetSmsTemplatesAsync(System.Threading.CancellationToken)')
  - [UpdateSmsTemplate(smsTemplate)](#M-Primavera-Lithium-Notifications-ISmsTemplatesOperations-UpdateSmsTemplate-Primavera-Lithium-Notifications-Models-SmsTemplateData- 'Primavera.Lithium.Notifications.ISmsTemplatesOperations.UpdateSmsTemplate(Primavera.Lithium.Notifications.Models.SmsTemplateData)')
  - [UpdateSmsTemplateAsync(smsTemplate,cancellationToken)](#M-Primavera-Lithium-Notifications-ISmsTemplatesOperations-UpdateSmsTemplateAsync-Primavera-Lithium-Notifications-Models-SmsTemplateData,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.ISmsTemplatesOperations.UpdateSmsTemplateAsync(Primavera.Lithium.Notifications.Models.SmsTemplateData,System.Threading.CancellationToken)')
- [MonitoringOperations](#T-Primavera-Lithium-Notifications-MonitoringOperations 'Primavera.Lithium.Notifications.MonitoringOperations')
  - [#ctor(client,actions)](#M-Primavera-Lithium-Notifications-MonitoringOperations-#ctor-Primavera-Lithium-Notifications-NotificationsClientBase,Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase}- 'Primavera.Lithium.Notifications.MonitoringOperations.#ctor(Primavera.Lithium.Notifications.NotificationsClientBase,Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase})')
- [MonitoringOperationsBase](#T-Primavera-Lithium-Notifications-MonitoringOperationsBase 'Primavera.Lithium.Notifications.MonitoringOperationsBase')
  - [#ctor(client,actions)](#M-Primavera-Lithium-Notifications-MonitoringOperationsBase-#ctor-Primavera-Lithium-Notifications-NotificationsClientBase,Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase}- 'Primavera.Lithium.Notifications.MonitoringOperationsBase.#ctor(Primavera.Lithium.Notifications.NotificationsClientBase,Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase})')
  - [Actions](#P-Primavera-Lithium-Notifications-MonitoringOperationsBase-Actions 'Primavera.Lithium.Notifications.MonitoringOperationsBase.Actions')
  - [Client](#P-Primavera-Lithium-Notifications-MonitoringOperationsBase-Client 'Primavera.Lithium.Notifications.MonitoringOperationsBase.Client')
  - [Diagnostics()](#M-Primavera-Lithium-Notifications-MonitoringOperationsBase-Diagnostics 'Primavera.Lithium.Notifications.MonitoringOperationsBase.Diagnostics')
  - [DiagnosticsAsync()](#M-Primavera-Lithium-Notifications-MonitoringOperationsBase-DiagnosticsAsync-System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.MonitoringOperationsBase.DiagnosticsAsync(System.Threading.CancellationToken)')
  - [Dispose()](#M-Primavera-Lithium-Notifications-MonitoringOperationsBase-Dispose 'Primavera.Lithium.Notifications.MonitoringOperationsBase.Dispose')
  - [Dispose(disposing)](#M-Primavera-Lithium-Notifications-MonitoringOperationsBase-Dispose-System-Boolean- 'Primavera.Lithium.Notifications.MonitoringOperationsBase.Dispose(System.Boolean)')
  - [Probe()](#M-Primavera-Lithium-Notifications-MonitoringOperationsBase-Probe 'Primavera.Lithium.Notifications.MonitoringOperationsBase.Probe')
  - [ProbeAsync()](#M-Primavera-Lithium-Notifications-MonitoringOperationsBase-ProbeAsync-System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.MonitoringOperationsBase.ProbeAsync(System.Threading.CancellationToken)')
- [NotificationsClient](#T-Primavera-Lithium-Notifications-NotificationsClient 'Primavera.Lithium.Notifications.NotificationsClient')
  - [#ctor(baseUri,credentials)](#M-Primavera-Lithium-Notifications-NotificationsClient-#ctor-System-Uri,Primavera-Hydrogen-Rest-Client-ServiceClientCredentials- 'Primavera.Lithium.Notifications.NotificationsClient.#ctor(System.Uri,Primavera.Hydrogen.Rest.Client.ServiceClientCredentials)')
  - [#ctor(baseUri,credentials,handler)](#M-Primavera-Lithium-Notifications-NotificationsClient-#ctor-System-Uri,Primavera-Hydrogen-Rest-Client-ServiceClientCredentials,System-Net-Http-HttpMessageHandler- 'Primavera.Lithium.Notifications.NotificationsClient.#ctor(System.Uri,Primavera.Hydrogen.Rest.Client.ServiceClientCredentials,System.Net.Http.HttpMessageHandler)')
  - [#ctor(baseUri,credentials,handler,disposeHandler)](#M-Primavera-Lithium-Notifications-NotificationsClient-#ctor-System-Uri,Primavera-Hydrogen-Rest-Client-ServiceClientCredentials,System-Net-Http-HttpMessageHandler,System-Boolean- 'Primavera.Lithium.Notifications.NotificationsClient.#ctor(System.Uri,Primavera.Hydrogen.Rest.Client.ServiceClientCredentials,System.Net.Http.HttpMessageHandler,System.Boolean)')
  - [#ctor(baseUri,callback)](#M-Primavera-Lithium-Notifications-NotificationsClient-#ctor-System-Uri,Primavera-Hydrogen-Rest-Client-AuthenticationCallback- 'Primavera.Lithium.Notifications.NotificationsClient.#ctor(System.Uri,Primavera.Hydrogen.Rest.Client.AuthenticationCallback)')
  - [#ctor(baseUri,callback,handler)](#M-Primavera-Lithium-Notifications-NotificationsClient-#ctor-System-Uri,Primavera-Hydrogen-Rest-Client-AuthenticationCallback,System-Net-Http-HttpMessageHandler- 'Primavera.Lithium.Notifications.NotificationsClient.#ctor(System.Uri,Primavera.Hydrogen.Rest.Client.AuthenticationCallback,System.Net.Http.HttpMessageHandler)')
  - [#ctor(baseUri,callback,handler,disposeHandler)](#M-Primavera-Lithium-Notifications-NotificationsClient-#ctor-System-Uri,Primavera-Hydrogen-Rest-Client-AuthenticationCallback,System-Net-Http-HttpMessageHandler,System-Boolean- 'Primavera.Lithium.Notifications.NotificationsClient.#ctor(System.Uri,Primavera.Hydrogen.Rest.Client.AuthenticationCallback,System.Net.Http.HttpMessageHandler,System.Boolean)')
- [NotificationsClientBase](#T-Primavera-Lithium-Notifications-NotificationsClientBase 'Primavera.Lithium.Notifications.NotificationsClientBase')
  - [#ctor(baseUri,credentials)](#M-Primavera-Lithium-Notifications-NotificationsClientBase-#ctor-System-Uri,Primavera-Hydrogen-Rest-Client-ServiceClientCredentials- 'Primavera.Lithium.Notifications.NotificationsClientBase.#ctor(System.Uri,Primavera.Hydrogen.Rest.Client.ServiceClientCredentials)')
  - [#ctor(baseUri,credentials,handler)](#M-Primavera-Lithium-Notifications-NotificationsClientBase-#ctor-System-Uri,Primavera-Hydrogen-Rest-Client-ServiceClientCredentials,System-Net-Http-HttpMessageHandler- 'Primavera.Lithium.Notifications.NotificationsClientBase.#ctor(System.Uri,Primavera.Hydrogen.Rest.Client.ServiceClientCredentials,System.Net.Http.HttpMessageHandler)')
  - [#ctor(baseUri,credentials,handler,disposeHandler)](#M-Primavera-Lithium-Notifications-NotificationsClientBase-#ctor-System-Uri,Primavera-Hydrogen-Rest-Client-ServiceClientCredentials,System-Net-Http-HttpMessageHandler,System-Boolean- 'Primavera.Lithium.Notifications.NotificationsClientBase.#ctor(System.Uri,Primavera.Hydrogen.Rest.Client.ServiceClientCredentials,System.Net.Http.HttpMessageHandler,System.Boolean)')
  - [#ctor(baseUri,callback)](#M-Primavera-Lithium-Notifications-NotificationsClientBase-#ctor-System-Uri,Primavera-Hydrogen-Rest-Client-AuthenticationCallback- 'Primavera.Lithium.Notifications.NotificationsClientBase.#ctor(System.Uri,Primavera.Hydrogen.Rest.Client.AuthenticationCallback)')
  - [#ctor(baseUri,callback,handler)](#M-Primavera-Lithium-Notifications-NotificationsClientBase-#ctor-System-Uri,Primavera-Hydrogen-Rest-Client-AuthenticationCallback,System-Net-Http-HttpMessageHandler- 'Primavera.Lithium.Notifications.NotificationsClientBase.#ctor(System.Uri,Primavera.Hydrogen.Rest.Client.AuthenticationCallback,System.Net.Http.HttpMessageHandler)')
  - [#ctor(baseUri,callback,handler,disposeHandler)](#M-Primavera-Lithium-Notifications-NotificationsClientBase-#ctor-System-Uri,Primavera-Hydrogen-Rest-Client-AuthenticationCallback,System-Net-Http-HttpMessageHandler,System-Boolean- 'Primavera.Lithium.Notifications.NotificationsClientBase.#ctor(System.Uri,Primavera.Hydrogen.Rest.Client.AuthenticationCallback,System.Net.Http.HttpMessageHandler,System.Boolean)')
  - [EmailNotifications](#P-Primavera-Lithium-Notifications-NotificationsClientBase-EmailNotifications 'Primavera.Lithium.Notifications.NotificationsClientBase.EmailNotifications')
  - [EmailTemplates](#P-Primavera-Lithium-Notifications-NotificationsClientBase-EmailTemplates 'Primavera.Lithium.Notifications.NotificationsClientBase.EmailTemplates')
  - [Monitoring](#P-Primavera-Lithium-Notifications-NotificationsClientBase-Monitoring 'Primavera.Lithium.Notifications.NotificationsClientBase.Monitoring')
  - [SmsNotifications](#P-Primavera-Lithium-Notifications-NotificationsClientBase-SmsNotifications 'Primavera.Lithium.Notifications.NotificationsClientBase.SmsNotifications')
  - [SmsTemplates](#P-Primavera-Lithium-Notifications-NotificationsClientBase-SmsTemplates 'Primavera.Lithium.Notifications.NotificationsClientBase.SmsTemplates')
  - [Dispose()](#M-Primavera-Lithium-Notifications-NotificationsClientBase-Dispose-System-Boolean- 'Primavera.Lithium.Notifications.NotificationsClientBase.Dispose(System.Boolean)')
  - [InitializeApiVersion()](#M-Primavera-Lithium-Notifications-NotificationsClientBase-InitializeApiVersion 'Primavera.Lithium.Notifications.NotificationsClientBase.InitializeApiVersion')
- [SmsNotificationsOperations](#T-Primavera-Lithium-Notifications-SmsNotificationsOperations 'Primavera.Lithium.Notifications.SmsNotificationsOperations')
  - [#ctor(client,actions)](#M-Primavera-Lithium-Notifications-SmsNotificationsOperations-#ctor-Primavera-Lithium-Notifications-NotificationsClientBase,Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase}- 'Primavera.Lithium.Notifications.SmsNotificationsOperations.#ctor(Primavera.Lithium.Notifications.NotificationsClientBase,Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase})')
- [SmsNotificationsOperationsBase](#T-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase 'Primavera.Lithium.Notifications.SmsNotificationsOperationsBase')
  - [#ctor(client,actions)](#M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-#ctor-Primavera-Lithium-Notifications-NotificationsClientBase,Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase}- 'Primavera.Lithium.Notifications.SmsNotificationsOperationsBase.#ctor(Primavera.Lithium.Notifications.NotificationsClientBase,Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase})')
  - [Actions](#P-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-Actions 'Primavera.Lithium.Notifications.SmsNotificationsOperationsBase.Actions')
  - [Client](#P-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-Client 'Primavera.Lithium.Notifications.SmsNotificationsOperationsBase.Client')
  - [CancelSmsNotification()](#M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-CancelSmsNotification-System-Guid- 'Primavera.Lithium.Notifications.SmsNotificationsOperationsBase.CancelSmsNotification(System.Guid)')
  - [CancelSmsNotificationAsync()](#M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-CancelSmsNotificationAsync-System-Guid,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.SmsNotificationsOperationsBase.CancelSmsNotificationAsync(System.Guid,System.Threading.CancellationToken)')
  - [Dispose()](#M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-Dispose 'Primavera.Lithium.Notifications.SmsNotificationsOperationsBase.Dispose')
  - [Dispose(disposing)](#M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-Dispose-System-Boolean- 'Primavera.Lithium.Notifications.SmsNotificationsOperationsBase.Dispose(System.Boolean)')
  - [GetSmsNotification()](#M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-GetSmsNotification-System-Guid- 'Primavera.Lithium.Notifications.SmsNotificationsOperationsBase.GetSmsNotification(System.Guid)')
  - [GetSmsNotificationAsync()](#M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-GetSmsNotificationAsync-System-Guid,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.SmsNotificationsOperationsBase.GetSmsNotificationAsync(System.Guid,System.Threading.CancellationToken)')
  - [GetSmsNotificationState()](#M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-GetSmsNotificationState-System-Guid- 'Primavera.Lithium.Notifications.SmsNotificationsOperationsBase.GetSmsNotificationState(System.Guid)')
  - [GetSmsNotificationStateAsync()](#M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-GetSmsNotificationStateAsync-System-Guid,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.SmsNotificationsOperationsBase.GetSmsNotificationStateAsync(System.Guid,System.Threading.CancellationToken)')
  - [GetSmsNotificationsState()](#M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-GetSmsNotificationsState-System-Collections-Generic-IEnumerable{System-Guid}- 'Primavera.Lithium.Notifications.SmsNotificationsOperationsBase.GetSmsNotificationsState(System.Collections.Generic.IEnumerable{System.Guid})')
  - [GetSmsNotificationsStateAsync()](#M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-GetSmsNotificationsStateAsync-System-Collections-Generic-IEnumerable{System-Guid},System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.SmsNotificationsOperationsBase.GetSmsNotificationsStateAsync(System.Collections.Generic.IEnumerable{System.Guid},System.Threading.CancellationToken)')
  - [RetrySmsNotification()](#M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-RetrySmsNotification-System-Guid- 'Primavera.Lithium.Notifications.SmsNotificationsOperationsBase.RetrySmsNotification(System.Guid)')
  - [RetrySmsNotificationAsync()](#M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-RetrySmsNotificationAsync-System-Guid,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.SmsNotificationsOperationsBase.RetrySmsNotificationAsync(System.Guid,System.Threading.CancellationToken)')
  - [SendSmsNotification()](#M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-SendSmsNotification-Primavera-Lithium-Notifications-Models-SmsNotificationData- 'Primavera.Lithium.Notifications.SmsNotificationsOperationsBase.SendSmsNotification(Primavera.Lithium.Notifications.Models.SmsNotificationData)')
  - [SendSmsNotificationAsync()](#M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-SendSmsNotificationAsync-Primavera-Lithium-Notifications-Models-SmsNotificationData,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.SmsNotificationsOperationsBase.SendSmsNotificationAsync(Primavera.Lithium.Notifications.Models.SmsNotificationData,System.Threading.CancellationToken)')
- [SmsTemplatesOperations](#T-Primavera-Lithium-Notifications-SmsTemplatesOperations 'Primavera.Lithium.Notifications.SmsTemplatesOperations')
  - [#ctor(client,actions)](#M-Primavera-Lithium-Notifications-SmsTemplatesOperations-#ctor-Primavera-Lithium-Notifications-NotificationsClientBase,Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase}- 'Primavera.Lithium.Notifications.SmsTemplatesOperations.#ctor(Primavera.Lithium.Notifications.NotificationsClientBase,Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase})')
- [SmsTemplatesOperationsBase](#T-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase 'Primavera.Lithium.Notifications.SmsTemplatesOperationsBase')
  - [#ctor(client,actions)](#M-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-#ctor-Primavera-Lithium-Notifications-NotificationsClientBase,Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase}- 'Primavera.Lithium.Notifications.SmsTemplatesOperationsBase.#ctor(Primavera.Lithium.Notifications.NotificationsClientBase,Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase})')
  - [Actions](#P-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-Actions 'Primavera.Lithium.Notifications.SmsTemplatesOperationsBase.Actions')
  - [Client](#P-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-Client 'Primavera.Lithium.Notifications.SmsTemplatesOperationsBase.Client')
  - [CreateSmsTemplate()](#M-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-CreateSmsTemplate-Primavera-Lithium-Notifications-Models-SmsTemplateData- 'Primavera.Lithium.Notifications.SmsTemplatesOperationsBase.CreateSmsTemplate(Primavera.Lithium.Notifications.Models.SmsTemplateData)')
  - [CreateSmsTemplateAsync()](#M-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-CreateSmsTemplateAsync-Primavera-Lithium-Notifications-Models-SmsTemplateData,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.SmsTemplatesOperationsBase.CreateSmsTemplateAsync(Primavera.Lithium.Notifications.Models.SmsTemplateData,System.Threading.CancellationToken)')
  - [DeleteSmsTemplate()](#M-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-DeleteSmsTemplate-System-String- 'Primavera.Lithium.Notifications.SmsTemplatesOperationsBase.DeleteSmsTemplate(System.String)')
  - [DeleteSmsTemplateAsync()](#M-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-DeleteSmsTemplateAsync-System-String,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.SmsTemplatesOperationsBase.DeleteSmsTemplateAsync(System.String,System.Threading.CancellationToken)')
  - [Dispose()](#M-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-Dispose 'Primavera.Lithium.Notifications.SmsTemplatesOperationsBase.Dispose')
  - [Dispose(disposing)](#M-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-Dispose-System-Boolean- 'Primavera.Lithium.Notifications.SmsTemplatesOperationsBase.Dispose(System.Boolean)')
  - [GetSmsTemplate()](#M-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-GetSmsTemplate-System-String- 'Primavera.Lithium.Notifications.SmsTemplatesOperationsBase.GetSmsTemplate(System.String)')
  - [GetSmsTemplateAsync()](#M-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-GetSmsTemplateAsync-System-String,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.SmsTemplatesOperationsBase.GetSmsTemplateAsync(System.String,System.Threading.CancellationToken)')
  - [GetSmsTemplates()](#M-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-GetSmsTemplates 'Primavera.Lithium.Notifications.SmsTemplatesOperationsBase.GetSmsTemplates')
  - [GetSmsTemplatesAsync()](#M-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-GetSmsTemplatesAsync-System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.SmsTemplatesOperationsBase.GetSmsTemplatesAsync(System.Threading.CancellationToken)')
  - [UpdateSmsTemplate()](#M-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-UpdateSmsTemplate-Primavera-Lithium-Notifications-Models-SmsTemplateData- 'Primavera.Lithium.Notifications.SmsTemplatesOperationsBase.UpdateSmsTemplate(Primavera.Lithium.Notifications.Models.SmsTemplateData)')
  - [UpdateSmsTemplateAsync()](#M-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-UpdateSmsTemplateAsync-Primavera-Lithium-Notifications-Models-SmsTemplateData,System-Threading-CancellationToken- 'Primavera.Lithium.Notifications.SmsTemplatesOperationsBase.UpdateSmsTemplateAsync(Primavera.Lithium.Notifications.Models.SmsTemplateData,System.Threading.CancellationToken)')

<a name='T-Primavera-Lithium-Notifications-EmailNotificationsOperations'></a>
## EmailNotificationsOperations `type`

##### Namespace

Primavera.Lithium.Notifications

##### Summary

Provides actions to send email notifications.

##### See Also

- [Primavera.Lithium.Notifications.EmailNotificationsOperationsBase](#T-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase 'Primavera.Lithium.Notifications.EmailNotificationsOperationsBase')

<a name='M-Primavera-Lithium-Notifications-EmailNotificationsOperations-#ctor-Primavera-Lithium-Notifications-NotificationsClientBase,Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase}-'></a>
### #ctor(client,actions) `constructor`

##### Summary

Initializes a new instance of the [EmailNotificationsOperations](#T-Primavera-Lithium-Notifications-EmailNotificationsOperations 'Primavera.Lithium.Notifications.EmailNotificationsOperations') class.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| client | [Primavera.Lithium.Notifications.NotificationsClientBase](#T-Primavera-Lithium-Notifications-NotificationsClientBase 'Primavera.Lithium.Notifications.NotificationsClientBase') | The service client. |
| actions | [Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase}](#T-Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase} 'Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase}') | The service client actions. |

<a name='T-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase'></a>
## EmailNotificationsOperationsBase `type`

##### Namespace

Primavera.Lithium.Notifications

##### Summary

Provides actions to send email notifications.

##### See Also

- [Primavera.Lithium.Notifications.IEmailNotificationsOperations](#T-Primavera-Lithium-Notifications-IEmailNotificationsOperations 'Primavera.Lithium.Notifications.IEmailNotificationsOperations')
- [System.IDisposable](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.IDisposable 'System.IDisposable')

<a name='M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-#ctor-Primavera-Lithium-Notifications-NotificationsClientBase,Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase}-'></a>
### #ctor(client,actions) `constructor`

##### Summary

Initializes a new instance of the [EmailNotificationsOperationsBase](#T-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase 'Primavera.Lithium.Notifications.EmailNotificationsOperationsBase') class.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| client | [Primavera.Lithium.Notifications.NotificationsClientBase](#T-Primavera-Lithium-Notifications-NotificationsClientBase 'Primavera.Lithium.Notifications.NotificationsClientBase') | The service client. |
| actions | [Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase}](#T-Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase} 'Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase}') | The service client actions. |

<a name='P-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-Actions'></a>
### Actions `property`

##### Summary

Gets or sets the service client actions.

<a name='P-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-Client'></a>
### Client `property`

##### Summary

Gets or sets the service client.

<a name='M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-CancelEmailNotification-System-Guid-'></a>
### CancelEmailNotification() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-CancelEmailNotificationAsync-System-Guid,System-Threading-CancellationToken-'></a>
### CancelEmailNotificationAsync() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-Dispose'></a>
### Dispose() `method`

##### Summary

Performs application-defined tasks associated with freeing, releasing, or resetting unmanaged resources.

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-Dispose-System-Boolean-'></a>
### Dispose(disposing) `method`

##### Summary

Releases unmanaged and - optionally - managed resources.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| disposing | [System.Boolean](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Boolean 'System.Boolean') | `true` to release both managed and unmanaged resources; `false` to release only unmanaged resources. |

<a name='M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-GetEmailNotification-System-Guid-'></a>
### GetEmailNotification() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-GetEmailNotificationAsync-System-Guid,System-Threading-CancellationToken-'></a>
### GetEmailNotificationAsync() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-GetEmailNotificationState-System-Guid-'></a>
### GetEmailNotificationState() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-GetEmailNotificationStateAsync-System-Guid,System-Threading-CancellationToken-'></a>
### GetEmailNotificationStateAsync() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-GetEmailNotificationsState-System-Collections-Generic-IEnumerable{System-Guid}-'></a>
### GetEmailNotificationsState() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-GetEmailNotificationsStateAsync-System-Collections-Generic-IEnumerable{System-Guid},System-Threading-CancellationToken-'></a>
### GetEmailNotificationsStateAsync() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-RetryEmailNotification-System-Guid-'></a>
### RetryEmailNotification() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-RetryEmailNotificationAsync-System-Guid,System-Threading-CancellationToken-'></a>
### RetryEmailNotificationAsync() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-SendEmailNotification-Primavera-Lithium-Notifications-Models-EmailNotificationData-'></a>
### SendEmailNotification() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-EmailNotificationsOperationsBase-SendEmailNotificationAsync-Primavera-Lithium-Notifications-Models-EmailNotificationData,System-Threading-CancellationToken-'></a>
### SendEmailNotificationAsync() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='T-Primavera-Lithium-Notifications-EmailTemplatesOperations'></a>
## EmailTemplatesOperations `type`

##### Namespace

Primavera.Lithium.Notifications

##### Summary

Provides actions to create, update, and delete email templates.

##### See Also

- [Primavera.Lithium.Notifications.EmailTemplatesOperationsBase](#T-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase 'Primavera.Lithium.Notifications.EmailTemplatesOperationsBase')

<a name='M-Primavera-Lithium-Notifications-EmailTemplatesOperations-#ctor-Primavera-Lithium-Notifications-NotificationsClientBase,Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase}-'></a>
### #ctor(client,actions) `constructor`

##### Summary

Initializes a new instance of the [EmailTemplatesOperations](#T-Primavera-Lithium-Notifications-EmailTemplatesOperations 'Primavera.Lithium.Notifications.EmailTemplatesOperations') class.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| client | [Primavera.Lithium.Notifications.NotificationsClientBase](#T-Primavera-Lithium-Notifications-NotificationsClientBase 'Primavera.Lithium.Notifications.NotificationsClientBase') | The service client. |
| actions | [Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase}](#T-Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase} 'Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase}') | The service client actions. |

<a name='T-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase'></a>
## EmailTemplatesOperationsBase `type`

##### Namespace

Primavera.Lithium.Notifications

##### Summary

Provides actions to create, update, and delete email templates.

##### See Also

- [Primavera.Lithium.Notifications.IEmailTemplatesOperations](#T-Primavera-Lithium-Notifications-IEmailTemplatesOperations 'Primavera.Lithium.Notifications.IEmailTemplatesOperations')
- [System.IDisposable](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.IDisposable 'System.IDisposable')

<a name='M-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-#ctor-Primavera-Lithium-Notifications-NotificationsClientBase,Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase}-'></a>
### #ctor(client,actions) `constructor`

##### Summary

Initializes a new instance of the [EmailTemplatesOperationsBase](#T-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase 'Primavera.Lithium.Notifications.EmailTemplatesOperationsBase') class.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| client | [Primavera.Lithium.Notifications.NotificationsClientBase](#T-Primavera-Lithium-Notifications-NotificationsClientBase 'Primavera.Lithium.Notifications.NotificationsClientBase') | The service client. |
| actions | [Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase}](#T-Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase} 'Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase}') | The service client actions. |

<a name='P-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-Actions'></a>
### Actions `property`

##### Summary

Gets or sets the service client actions.

<a name='P-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-Client'></a>
### Client `property`

##### Summary

Gets or sets the service client.

<a name='M-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-CreateEmailTemplate-Primavera-Lithium-Notifications-Models-EmailTemplateData-'></a>
### CreateEmailTemplate() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-CreateEmailTemplateAsync-Primavera-Lithium-Notifications-Models-EmailTemplateData,System-Threading-CancellationToken-'></a>
### CreateEmailTemplateAsync() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-DeleteEmailTemplate-System-String-'></a>
### DeleteEmailTemplate() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-DeleteEmailTemplateAsync-System-String,System-Threading-CancellationToken-'></a>
### DeleteEmailTemplateAsync() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-Dispose'></a>
### Dispose() `method`

##### Summary

Performs application-defined tasks associated with freeing, releasing, or resetting unmanaged resources.

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-Dispose-System-Boolean-'></a>
### Dispose(disposing) `method`

##### Summary

Releases unmanaged and - optionally - managed resources.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| disposing | [System.Boolean](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Boolean 'System.Boolean') | `true` to release both managed and unmanaged resources; `false` to release only unmanaged resources. |

<a name='M-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-GetEmailTemplate-System-String-'></a>
### GetEmailTemplate() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-GetEmailTemplateAsync-System-String,System-Threading-CancellationToken-'></a>
### GetEmailTemplateAsync() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-GetEmailTemplates'></a>
### GetEmailTemplates() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-GetEmailTemplatesAsync-System-Threading-CancellationToken-'></a>
### GetEmailTemplatesAsync() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-UpdateEmailTemplate-Primavera-Lithium-Notifications-Models-EmailTemplateData-'></a>
### UpdateEmailTemplate() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-EmailTemplatesOperationsBase-UpdateEmailTemplateAsync-Primavera-Lithium-Notifications-Models-EmailTemplateData,System-Threading-CancellationToken-'></a>
### UpdateEmailTemplateAsync() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='T-Primavera-Lithium-Notifications-IEmailNotificationsOperations'></a>
## IEmailNotificationsOperations `type`

##### Namespace

Primavera.Lithium.Notifications

##### Summary

Provides actions to send email notifications.

##### See Also

- [System.IDisposable](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.IDisposable 'System.IDisposable')

<a name='M-Primavera-Lithium-Notifications-IEmailNotificationsOperations-CancelEmailNotification-System-Guid-'></a>
### CancelEmailNotification(id) `method`

##### Summary

Cancels the specified email notification.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | [System.Guid](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Guid 'System.Guid') | The identifier of the email notification that should be canceled. |

<a name='M-Primavera-Lithium-Notifications-IEmailNotificationsOperations-CancelEmailNotificationAsync-System-Guid,System-Threading-CancellationToken-'></a>
### CancelEmailNotificationAsync(id,cancellationToken) `method`

##### Summary

Cancels the specified email notification.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | [System.Guid](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Guid 'System.Guid') | The identifier of the email notification that should be canceled. |
| cancellationToken | [System.Threading.CancellationToken](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Threading.CancellationToken 'System.Threading.CancellationToken') | The cancellation token. |

<a name='M-Primavera-Lithium-Notifications-IEmailNotificationsOperations-GetEmailNotification-System-Guid-'></a>
### GetEmailNotification(id) `method`

##### Summary

Retrieves the specified email notification.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | [System.Guid](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Guid 'System.Guid') | The identifier of the email notification that should be retrieved. |

<a name='M-Primavera-Lithium-Notifications-IEmailNotificationsOperations-GetEmailNotificationAsync-System-Guid,System-Threading-CancellationToken-'></a>
### GetEmailNotificationAsync(id,cancellationToken) `method`

##### Summary

Retrieves the specified email notification.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | [System.Guid](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Guid 'System.Guid') | The identifier of the email notification that should be retrieved. |
| cancellationToken | [System.Threading.CancellationToken](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Threading.CancellationToken 'System.Threading.CancellationToken') | The cancellation token. |

<a name='M-Primavera-Lithium-Notifications-IEmailNotificationsOperations-GetEmailNotificationState-System-Guid-'></a>
### GetEmailNotificationState(id) `method`

##### Summary

Retrieves the state of the specified email notification.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | [System.Guid](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Guid 'System.Guid') | The identifier of the email notification whose state should be retrieved. |

<a name='M-Primavera-Lithium-Notifications-IEmailNotificationsOperations-GetEmailNotificationStateAsync-System-Guid,System-Threading-CancellationToken-'></a>
### GetEmailNotificationStateAsync(id,cancellationToken) `method`

##### Summary

Retrieves the state of the specified email notification.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | [System.Guid](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Guid 'System.Guid') | The identifier of the email notification whose state should be retrieved. |
| cancellationToken | [System.Threading.CancellationToken](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Threading.CancellationToken 'System.Threading.CancellationToken') | The cancellation token. |

<a name='M-Primavera-Lithium-Notifications-IEmailNotificationsOperations-GetEmailNotificationsState-System-Collections-Generic-IEnumerable{System-Guid}-'></a>
### GetEmailNotificationsState(ids) `method`

##### Summary

Retrieves the state of the specified email notifications.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| ids | [System.Collections.Generic.IEnumerable{System.Guid}](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Collections.Generic.IEnumerable 'System.Collections.Generic.IEnumerable{System.Guid}') | The identifiers of the email notifications whose state should be retrieved. |

<a name='M-Primavera-Lithium-Notifications-IEmailNotificationsOperations-GetEmailNotificationsStateAsync-System-Collections-Generic-IEnumerable{System-Guid},System-Threading-CancellationToken-'></a>
### GetEmailNotificationsStateAsync(ids,cancellationToken) `method`

##### Summary

Retrieves the state of the specified email notifications.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| ids | [System.Collections.Generic.IEnumerable{System.Guid}](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Collections.Generic.IEnumerable 'System.Collections.Generic.IEnumerable{System.Guid}') | The identifiers of the email notifications whose state should be retrieved. |
| cancellationToken | [System.Threading.CancellationToken](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Threading.CancellationToken 'System.Threading.CancellationToken') | The cancellation token. |

<a name='M-Primavera-Lithium-Notifications-IEmailNotificationsOperations-RetryEmailNotification-System-Guid-'></a>
### RetryEmailNotification(id) `method`

##### Summary

Retries to send an existing email notification to all the receivers that failed.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | [System.Guid](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Guid 'System.Guid') | The identifier of the email notification that should be retried. |

<a name='M-Primavera-Lithium-Notifications-IEmailNotificationsOperations-RetryEmailNotificationAsync-System-Guid,System-Threading-CancellationToken-'></a>
### RetryEmailNotificationAsync(id,cancellationToken) `method`

##### Summary

Retries to send an existing email notification to all the receivers that failed.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | [System.Guid](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Guid 'System.Guid') | The identifier of the email notification that should be retried. |
| cancellationToken | [System.Threading.CancellationToken](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Threading.CancellationToken 'System.Threading.CancellationToken') | The cancellation token. |

<a name='M-Primavera-Lithium-Notifications-IEmailNotificationsOperations-SendEmailNotification-Primavera-Lithium-Notifications-Models-EmailNotificationData-'></a>
### SendEmailNotification(notification) `method`

##### Summary

Sends a new email notification.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| notification | [Primavera.Lithium.Notifications.Models.EmailNotificationData](#T-Primavera-Lithium-Notifications-Models-EmailNotificationData 'Primavera.Lithium.Notifications.Models.EmailNotificationData') | The notification that should be sent. |

<a name='M-Primavera-Lithium-Notifications-IEmailNotificationsOperations-SendEmailNotificationAsync-Primavera-Lithium-Notifications-Models-EmailNotificationData,System-Threading-CancellationToken-'></a>
### SendEmailNotificationAsync(notification,cancellationToken) `method`

##### Summary

Sends a new email notification.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| notification | [Primavera.Lithium.Notifications.Models.EmailNotificationData](#T-Primavera-Lithium-Notifications-Models-EmailNotificationData 'Primavera.Lithium.Notifications.Models.EmailNotificationData') | The notification that should be sent. |
| cancellationToken | [System.Threading.CancellationToken](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Threading.CancellationToken 'System.Threading.CancellationToken') | The cancellation token. |

<a name='T-Primavera-Lithium-Notifications-IEmailTemplatesOperations'></a>
## IEmailTemplatesOperations `type`

##### Namespace

Primavera.Lithium.Notifications

##### Summary

Provides actions to create, update, and delete email templates.

##### See Also

- [System.IDisposable](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.IDisposable 'System.IDisposable')

<a name='M-Primavera-Lithium-Notifications-IEmailTemplatesOperations-CreateEmailTemplate-Primavera-Lithium-Notifications-Models-EmailTemplateData-'></a>
### CreateEmailTemplate(emailTemplate) `method`

##### Summary

Creates a new email template.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| emailTemplate | [Primavera.Lithium.Notifications.Models.EmailTemplateData](#T-Primavera-Lithium-Notifications-Models-EmailTemplateData 'Primavera.Lithium.Notifications.Models.EmailTemplateData') | The email template that should be created. |

<a name='M-Primavera-Lithium-Notifications-IEmailTemplatesOperations-CreateEmailTemplateAsync-Primavera-Lithium-Notifications-Models-EmailTemplateData,System-Threading-CancellationToken-'></a>
### CreateEmailTemplateAsync(emailTemplate,cancellationToken) `method`

##### Summary

Creates a new email template.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| emailTemplate | [Primavera.Lithium.Notifications.Models.EmailTemplateData](#T-Primavera-Lithium-Notifications-Models-EmailTemplateData 'Primavera.Lithium.Notifications.Models.EmailTemplateData') | The email template that should be created. |
| cancellationToken | [System.Threading.CancellationToken](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Threading.CancellationToken 'System.Threading.CancellationToken') | The cancellation token. |

<a name='M-Primavera-Lithium-Notifications-IEmailTemplatesOperations-DeleteEmailTemplate-System-String-'></a>
### DeleteEmailTemplate(id) `method`

##### Summary

Deletes an existing email template.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | [System.String](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.String 'System.String') | The identifier of the email template that should be deleted. |

<a name='M-Primavera-Lithium-Notifications-IEmailTemplatesOperations-DeleteEmailTemplateAsync-System-String,System-Threading-CancellationToken-'></a>
### DeleteEmailTemplateAsync(id,cancellationToken) `method`

##### Summary

Deletes an existing email template.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | [System.String](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.String 'System.String') | The identifier of the email template that should be deleted. |
| cancellationToken | [System.Threading.CancellationToken](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Threading.CancellationToken 'System.Threading.CancellationToken') | The cancellation token. |

<a name='M-Primavera-Lithium-Notifications-IEmailTemplatesOperations-GetEmailTemplate-System-String-'></a>
### GetEmailTemplate(id) `method`

##### Summary

Retrieves an existing email template.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | [System.String](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.String 'System.String') | The identifier of the email template that should be retrieved. |

<a name='M-Primavera-Lithium-Notifications-IEmailTemplatesOperations-GetEmailTemplateAsync-System-String,System-Threading-CancellationToken-'></a>
### GetEmailTemplateAsync(id,cancellationToken) `method`

##### Summary

Retrieves an existing email template.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | [System.String](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.String 'System.String') | The identifier of the email template that should be retrieved. |
| cancellationToken | [System.Threading.CancellationToken](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Threading.CancellationToken 'System.Threading.CancellationToken') | The cancellation token. |

<a name='M-Primavera-Lithium-Notifications-IEmailTemplatesOperations-GetEmailTemplates'></a>
### GetEmailTemplates() `method`

##### Summary

Retrieves all the email templates.

##### Returns

The operation result.

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-IEmailTemplatesOperations-GetEmailTemplatesAsync-System-Threading-CancellationToken-'></a>
### GetEmailTemplatesAsync(cancellationToken) `method`

##### Summary

Retrieves all the email templates.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| cancellationToken | [System.Threading.CancellationToken](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Threading.CancellationToken 'System.Threading.CancellationToken') | The cancellation token. |

<a name='M-Primavera-Lithium-Notifications-IEmailTemplatesOperations-UpdateEmailTemplate-Primavera-Lithium-Notifications-Models-EmailTemplateData-'></a>
### UpdateEmailTemplate(emailTemplate) `method`

##### Summary

Updates an existing email template.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| emailTemplate | [Primavera.Lithium.Notifications.Models.EmailTemplateData](#T-Primavera-Lithium-Notifications-Models-EmailTemplateData 'Primavera.Lithium.Notifications.Models.EmailTemplateData') | The email template that should be updated. |

<a name='M-Primavera-Lithium-Notifications-IEmailTemplatesOperations-UpdateEmailTemplateAsync-Primavera-Lithium-Notifications-Models-EmailTemplateData,System-Threading-CancellationToken-'></a>
### UpdateEmailTemplateAsync(emailTemplate,cancellationToken) `method`

##### Summary

Updates an existing email template.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| emailTemplate | [Primavera.Lithium.Notifications.Models.EmailTemplateData](#T-Primavera-Lithium-Notifications-Models-EmailTemplateData 'Primavera.Lithium.Notifications.Models.EmailTemplateData') | The email template that should be updated. |
| cancellationToken | [System.Threading.CancellationToken](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Threading.CancellationToken 'System.Threading.CancellationToken') | The cancellation token. |

<a name='T-Primavera-Lithium-Notifications-IMonitoringOperations'></a>
## IMonitoringOperations `type`

##### Namespace

Primavera.Lithium.Notifications

##### Summary

Provides monitoring operations on the service.

##### See Also

- [System.IDisposable](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.IDisposable 'System.IDisposable')

<a name='M-Primavera-Lithium-Notifications-IMonitoringOperations-DiagnosticsAsync-System-Threading-CancellationToken-'></a>
### DiagnosticsAsync(cancellationToken) `method`

##### Summary

Diagnoses the service.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| cancellationToken | [System.Threading.CancellationToken](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Threading.CancellationToken 'System.Threading.CancellationToken') | The cancellation token. |

<a name='M-Primavera-Lithium-Notifications-IMonitoringOperations-ProbeAsync-System-Threading-CancellationToken-'></a>
### ProbeAsync(cancellationToken) `method`

##### Summary

Probes the service.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| cancellationToken | [System.Threading.CancellationToken](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Threading.CancellationToken 'System.Threading.CancellationToken') | The cancellation token. |

<a name='T-Primavera-Lithium-Notifications-INotificationsClient'></a>
## INotificationsClient `type`

##### Namespace

Primavera.Lithium.Notifications

##### Summary

Defines the interface of the service client that allows accessing the Notifications Service Web API.

##### See Also

- [System.IDisposable](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.IDisposable 'System.IDisposable')

<a name='P-Primavera-Lithium-Notifications-INotificationsClient-EmailNotifications'></a>
### EmailNotifications `property`

##### Summary

Provides actions to send email notifications.

<a name='P-Primavera-Lithium-Notifications-INotificationsClient-EmailTemplates'></a>
### EmailTemplates `property`

##### Summary

Provides actions to create, update, and delete email templates.

<a name='P-Primavera-Lithium-Notifications-INotificationsClient-Monitoring'></a>
### Monitoring `property`

##### Summary

Provides monitoring operations on the service.

<a name='P-Primavera-Lithium-Notifications-INotificationsClient-SmsNotifications'></a>
### SmsNotifications `property`

##### Summary

Provides actions to send SMS notifications.

<a name='P-Primavera-Lithium-Notifications-INotificationsClient-SmsTemplates'></a>
### SmsTemplates `property`

##### Summary

Provides actions to create, update, and delete SMS templates.

<a name='T-Primavera-Lithium-Notifications-ISmsNotificationsOperations'></a>
## ISmsNotificationsOperations `type`

##### Namespace

Primavera.Lithium.Notifications

##### Summary

Provides actions to send SMS notifications.

##### See Also

- [System.IDisposable](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.IDisposable 'System.IDisposable')

<a name='M-Primavera-Lithium-Notifications-ISmsNotificationsOperations-CancelSmsNotification-System-Guid-'></a>
### CancelSmsNotification(id) `method`

##### Summary

Cancels the specified SMS notification.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | [System.Guid](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Guid 'System.Guid') | The identifier of the SMS notification that should be canceled. |

<a name='M-Primavera-Lithium-Notifications-ISmsNotificationsOperations-CancelSmsNotificationAsync-System-Guid,System-Threading-CancellationToken-'></a>
### CancelSmsNotificationAsync(id,cancellationToken) `method`

##### Summary

Cancels the specified SMS notification.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | [System.Guid](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Guid 'System.Guid') | The identifier of the SMS notification that should be canceled. |
| cancellationToken | [System.Threading.CancellationToken](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Threading.CancellationToken 'System.Threading.CancellationToken') | The cancellation token. |

<a name='M-Primavera-Lithium-Notifications-ISmsNotificationsOperations-GetSmsNotification-System-Guid-'></a>
### GetSmsNotification(id) `method`

##### Summary

Retrieves the specified SMS notification.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | [System.Guid](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Guid 'System.Guid') | The identifier of the SMS notification that should be retrieved. |

<a name='M-Primavera-Lithium-Notifications-ISmsNotificationsOperations-GetSmsNotificationAsync-System-Guid,System-Threading-CancellationToken-'></a>
### GetSmsNotificationAsync(id,cancellationToken) `method`

##### Summary

Retrieves the specified SMS notification.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | [System.Guid](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Guid 'System.Guid') | The identifier of the SMS notification that should be retrieved. |
| cancellationToken | [System.Threading.CancellationToken](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Threading.CancellationToken 'System.Threading.CancellationToken') | The cancellation token. |

<a name='M-Primavera-Lithium-Notifications-ISmsNotificationsOperations-GetSmsNotificationState-System-Guid-'></a>
### GetSmsNotificationState(id) `method`

##### Summary

Retrieves the state of the specified SMS notification.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | [System.Guid](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Guid 'System.Guid') | The identifier of the SMS notification whose state should be retrieved. |

<a name='M-Primavera-Lithium-Notifications-ISmsNotificationsOperations-GetSmsNotificationStateAsync-System-Guid,System-Threading-CancellationToken-'></a>
### GetSmsNotificationStateAsync(id,cancellationToken) `method`

##### Summary

Retrieves the state of the specified SMS notification.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | [System.Guid](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Guid 'System.Guid') | The identifier of the SMS notification whose state should be retrieved. |
| cancellationToken | [System.Threading.CancellationToken](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Threading.CancellationToken 'System.Threading.CancellationToken') | The cancellation token. |

<a name='M-Primavera-Lithium-Notifications-ISmsNotificationsOperations-GetSmsNotificationsState-System-Collections-Generic-IEnumerable{System-Guid}-'></a>
### GetSmsNotificationsState(ids) `method`

##### Summary

Retrieves the state of the specified SMS notifications.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| ids | [System.Collections.Generic.IEnumerable{System.Guid}](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Collections.Generic.IEnumerable 'System.Collections.Generic.IEnumerable{System.Guid}') | The identifiers of the SMS notifications whose state should be retrieved. |

<a name='M-Primavera-Lithium-Notifications-ISmsNotificationsOperations-GetSmsNotificationsStateAsync-System-Collections-Generic-IEnumerable{System-Guid},System-Threading-CancellationToken-'></a>
### GetSmsNotificationsStateAsync(ids,cancellationToken) `method`

##### Summary

Retrieves the state of the specified SMS notifications.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| ids | [System.Collections.Generic.IEnumerable{System.Guid}](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Collections.Generic.IEnumerable 'System.Collections.Generic.IEnumerable{System.Guid}') | The identifiers of the SMS notifications whose state should be retrieved. |
| cancellationToken | [System.Threading.CancellationToken](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Threading.CancellationToken 'System.Threading.CancellationToken') | The cancellation token. |

<a name='M-Primavera-Lithium-Notifications-ISmsNotificationsOperations-RetrySmsNotification-System-Guid-'></a>
### RetrySmsNotification(id) `method`

##### Summary

Retries to send an existing SMS notification to all the receivers that failed.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | [System.Guid](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Guid 'System.Guid') | The identifier of the SMS notification that should be retried. |

<a name='M-Primavera-Lithium-Notifications-ISmsNotificationsOperations-RetrySmsNotificationAsync-System-Guid,System-Threading-CancellationToken-'></a>
### RetrySmsNotificationAsync(id,cancellationToken) `method`

##### Summary

Retries to send an existing SMS notification to all the receivers that failed.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | [System.Guid](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Guid 'System.Guid') | The identifier of the SMS notification that should be retried. |
| cancellationToken | [System.Threading.CancellationToken](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Threading.CancellationToken 'System.Threading.CancellationToken') | The cancellation token. |

<a name='M-Primavera-Lithium-Notifications-ISmsNotificationsOperations-SendSmsNotification-Primavera-Lithium-Notifications-Models-SmsNotificationData-'></a>
### SendSmsNotification(notification) `method`

##### Summary

Sends a new SMS notification.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| notification | [Primavera.Lithium.Notifications.Models.SmsNotificationData](#T-Primavera-Lithium-Notifications-Models-SmsNotificationData 'Primavera.Lithium.Notifications.Models.SmsNotificationData') | The notification that should be sent. |

<a name='M-Primavera-Lithium-Notifications-ISmsNotificationsOperations-SendSmsNotificationAsync-Primavera-Lithium-Notifications-Models-SmsNotificationData,System-Threading-CancellationToken-'></a>
### SendSmsNotificationAsync(notification,cancellationToken) `method`

##### Summary

Sends a new SMS notification.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| notification | [Primavera.Lithium.Notifications.Models.SmsNotificationData](#T-Primavera-Lithium-Notifications-Models-SmsNotificationData 'Primavera.Lithium.Notifications.Models.SmsNotificationData') | The notification that should be sent. |
| cancellationToken | [System.Threading.CancellationToken](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Threading.CancellationToken 'System.Threading.CancellationToken') | The cancellation token. |

<a name='T-Primavera-Lithium-Notifications-ISmsTemplatesOperations'></a>
## ISmsTemplatesOperations `type`

##### Namespace

Primavera.Lithium.Notifications

##### Summary

Provides actions to create, update, and delete SMS templates.

##### See Also

- [System.IDisposable](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.IDisposable 'System.IDisposable')

<a name='M-Primavera-Lithium-Notifications-ISmsTemplatesOperations-CreateSmsTemplate-Primavera-Lithium-Notifications-Models-SmsTemplateData-'></a>
### CreateSmsTemplate(smsTemplate) `method`

##### Summary

Creates a new SMS template.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| smsTemplate | [Primavera.Lithium.Notifications.Models.SmsTemplateData](#T-Primavera-Lithium-Notifications-Models-SmsTemplateData 'Primavera.Lithium.Notifications.Models.SmsTemplateData') | The SMS template that should be created. |

<a name='M-Primavera-Lithium-Notifications-ISmsTemplatesOperations-CreateSmsTemplateAsync-Primavera-Lithium-Notifications-Models-SmsTemplateData,System-Threading-CancellationToken-'></a>
### CreateSmsTemplateAsync(smsTemplate,cancellationToken) `method`

##### Summary

Creates a new SMS template.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| smsTemplate | [Primavera.Lithium.Notifications.Models.SmsTemplateData](#T-Primavera-Lithium-Notifications-Models-SmsTemplateData 'Primavera.Lithium.Notifications.Models.SmsTemplateData') | The SMS template that should be created. |
| cancellationToken | [System.Threading.CancellationToken](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Threading.CancellationToken 'System.Threading.CancellationToken') | The cancellation token. |

<a name='M-Primavera-Lithium-Notifications-ISmsTemplatesOperations-DeleteSmsTemplate-System-String-'></a>
### DeleteSmsTemplate(id) `method`

##### Summary

Deletes an existing SMS template.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | [System.String](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.String 'System.String') | The identifier of the SMS template that should be deleted. |

<a name='M-Primavera-Lithium-Notifications-ISmsTemplatesOperations-DeleteSmsTemplateAsync-System-String,System-Threading-CancellationToken-'></a>
### DeleteSmsTemplateAsync(id,cancellationToken) `method`

##### Summary

Deletes an existing SMS template.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | [System.String](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.String 'System.String') | The identifier of the SMS template that should be deleted. |
| cancellationToken | [System.Threading.CancellationToken](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Threading.CancellationToken 'System.Threading.CancellationToken') | The cancellation token. |

<a name='M-Primavera-Lithium-Notifications-ISmsTemplatesOperations-GetSmsTemplate-System-String-'></a>
### GetSmsTemplate(id) `method`

##### Summary

Retrieves an existing SMS template.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | [System.String](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.String 'System.String') | The identifier of the SMS template that should be retrieved. |

<a name='M-Primavera-Lithium-Notifications-ISmsTemplatesOperations-GetSmsTemplateAsync-System-String,System-Threading-CancellationToken-'></a>
### GetSmsTemplateAsync(id,cancellationToken) `method`

##### Summary

Retrieves an existing SMS template.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | [System.String](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.String 'System.String') | The identifier of the SMS template that should be retrieved. |
| cancellationToken | [System.Threading.CancellationToken](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Threading.CancellationToken 'System.Threading.CancellationToken') | The cancellation token. |

<a name='M-Primavera-Lithium-Notifications-ISmsTemplatesOperations-GetSmsTemplates'></a>
### GetSmsTemplates() `method`

##### Summary

Retrieves all the SMS templates.

##### Returns

The operation result.

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-ISmsTemplatesOperations-GetSmsTemplatesAsync-System-Threading-CancellationToken-'></a>
### GetSmsTemplatesAsync(cancellationToken) `method`

##### Summary

Retrieves all the SMS templates.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| cancellationToken | [System.Threading.CancellationToken](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Threading.CancellationToken 'System.Threading.CancellationToken') | The cancellation token. |

<a name='M-Primavera-Lithium-Notifications-ISmsTemplatesOperations-UpdateSmsTemplate-Primavera-Lithium-Notifications-Models-SmsTemplateData-'></a>
### UpdateSmsTemplate(smsTemplate) `method`

##### Summary

Updates an existing SMS template.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| smsTemplate | [Primavera.Lithium.Notifications.Models.SmsTemplateData](#T-Primavera-Lithium-Notifications-Models-SmsTemplateData 'Primavera.Lithium.Notifications.Models.SmsTemplateData') | The SMS template that should be updated. |

<a name='M-Primavera-Lithium-Notifications-ISmsTemplatesOperations-UpdateSmsTemplateAsync-Primavera-Lithium-Notifications-Models-SmsTemplateData,System-Threading-CancellationToken-'></a>
### UpdateSmsTemplateAsync(smsTemplate,cancellationToken) `method`

##### Summary

Updates an existing SMS template.

##### Returns

The operation result.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| smsTemplate | [Primavera.Lithium.Notifications.Models.SmsTemplateData](#T-Primavera-Lithium-Notifications-Models-SmsTemplateData 'Primavera.Lithium.Notifications.Models.SmsTemplateData') | The SMS template that should be updated. |
| cancellationToken | [System.Threading.CancellationToken](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Threading.CancellationToken 'System.Threading.CancellationToken') | The cancellation token. |

<a name='T-Primavera-Lithium-Notifications-MonitoringOperations'></a>
## MonitoringOperations `type`

##### Namespace

Primavera.Lithium.Notifications

##### Summary

Provides monitoring operations on the service.

##### See Also

- [Primavera.Lithium.Notifications.MonitoringOperationsBase](#T-Primavera-Lithium-Notifications-MonitoringOperationsBase 'Primavera.Lithium.Notifications.MonitoringOperationsBase')

<a name='M-Primavera-Lithium-Notifications-MonitoringOperations-#ctor-Primavera-Lithium-Notifications-NotificationsClientBase,Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase}-'></a>
### #ctor(client,actions) `constructor`

##### Summary

Initializes a new instance of the [MonitoringOperations](#T-Primavera-Lithium-Notifications-MonitoringOperations 'Primavera.Lithium.Notifications.MonitoringOperations') class.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| client | [Primavera.Lithium.Notifications.NotificationsClientBase](#T-Primavera-Lithium-Notifications-NotificationsClientBase 'Primavera.Lithium.Notifications.NotificationsClientBase') | The service client. |
| actions | [Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase}](#T-Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase} 'Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase}') | The service client actions. |

<a name='T-Primavera-Lithium-Notifications-MonitoringOperationsBase'></a>
## MonitoringOperationsBase `type`

##### Namespace

Primavera.Lithium.Notifications

##### Summary

Defines the base class for the type that provides monitoring operations on the service.

##### See Also

- [Primavera.Lithium.Notifications.IMonitoringOperations](#T-Primavera-Lithium-Notifications-IMonitoringOperations 'Primavera.Lithium.Notifications.IMonitoringOperations')
- [System.IDisposable](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.IDisposable 'System.IDisposable')

<a name='M-Primavera-Lithium-Notifications-MonitoringOperationsBase-#ctor-Primavera-Lithium-Notifications-NotificationsClientBase,Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase}-'></a>
### #ctor(client,actions) `constructor`

##### Summary

Initializes a new instance of the [MonitoringOperationsBase](#T-Primavera-Lithium-Notifications-MonitoringOperationsBase 'Primavera.Lithium.Notifications.MonitoringOperationsBase') class.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| client | [Primavera.Lithium.Notifications.NotificationsClientBase](#T-Primavera-Lithium-Notifications-NotificationsClientBase 'Primavera.Lithium.Notifications.NotificationsClientBase') | The service client. |
| actions | [Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase}](#T-Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase} 'Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase}') | The service client actions. |

<a name='P-Primavera-Lithium-Notifications-MonitoringOperationsBase-Actions'></a>
### Actions `property`

##### Summary

Gets or sets the service client actions.

<a name='P-Primavera-Lithium-Notifications-MonitoringOperationsBase-Client'></a>
### Client `property`

##### Summary

Gets or sets the service client.

<a name='M-Primavera-Lithium-Notifications-MonitoringOperationsBase-Diagnostics'></a>
### Diagnostics() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-MonitoringOperationsBase-DiagnosticsAsync-System-Threading-CancellationToken-'></a>
### DiagnosticsAsync() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-MonitoringOperationsBase-Dispose'></a>
### Dispose() `method`

##### Summary

Performs application-defined tasks associated with freeing, releasing, or resetting unmanaged resources.

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-MonitoringOperationsBase-Dispose-System-Boolean-'></a>
### Dispose(disposing) `method`

##### Summary

Releases unmanaged and - optionally - managed resources.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| disposing | [System.Boolean](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Boolean 'System.Boolean') | `true` to release both managed and unmanaged resources; `false` to release only unmanaged resources. |

<a name='M-Primavera-Lithium-Notifications-MonitoringOperationsBase-Probe'></a>
### Probe() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-MonitoringOperationsBase-ProbeAsync-System-Threading-CancellationToken-'></a>
### ProbeAsync() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='T-Primavera-Lithium-Notifications-NotificationsClient'></a>
## NotificationsClient `type`

##### Namespace

Primavera.Lithium.Notifications

##### Summary

Defines the service client that allows accessing the Notifications Service API.

<a name='M-Primavera-Lithium-Notifications-NotificationsClient-#ctor-System-Uri,Primavera-Hydrogen-Rest-Client-ServiceClientCredentials-'></a>
### #ctor(baseUri,credentials) `constructor`

##### Summary

Initializes a new instance of the [NotificationsClient](#T-Primavera-Lithium-Notifications-NotificationsClient 'Primavera.Lithium.Notifications.NotificationsClient') class.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| baseUri | [System.Uri](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Uri 'System.Uri') | The base URI of the service. |
| credentials | [Primavera.Hydrogen.Rest.Client.ServiceClientCredentials](#T-Primavera-Hydrogen-Rest-Client-ServiceClientCredentials 'Primavera.Hydrogen.Rest.Client.ServiceClientCredentials') | The credentials that should be used to access the service. |

<a name='M-Primavera-Lithium-Notifications-NotificationsClient-#ctor-System-Uri,Primavera-Hydrogen-Rest-Client-ServiceClientCredentials,System-Net-Http-HttpMessageHandler-'></a>
### #ctor(baseUri,credentials,handler) `constructor`

##### Summary

Initializes a new instance of the [NotificationsClient](#T-Primavera-Lithium-Notifications-NotificationsClient 'Primavera.Lithium.Notifications.NotificationsClient') class.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| baseUri | [System.Uri](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Uri 'System.Uri') | The base URI of the service. |
| credentials | [Primavera.Hydrogen.Rest.Client.ServiceClientCredentials](#T-Primavera-Hydrogen-Rest-Client-ServiceClientCredentials 'Primavera.Hydrogen.Rest.Client.ServiceClientCredentials') | The credentials that should be used to access the service. |
| handler | [System.Net.Http.HttpMessageHandler](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Net.Http.HttpMessageHandler 'System.Net.Http.HttpMessageHandler') | The root message handler. |

<a name='M-Primavera-Lithium-Notifications-NotificationsClient-#ctor-System-Uri,Primavera-Hydrogen-Rest-Client-ServiceClientCredentials,System-Net-Http-HttpMessageHandler,System-Boolean-'></a>
### #ctor(baseUri,credentials,handler,disposeHandler) `constructor`

##### Summary

Initializes a new instance of the [NotificationsClient](#T-Primavera-Lithium-Notifications-NotificationsClient 'Primavera.Lithium.Notifications.NotificationsClient') class.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| baseUri | [System.Uri](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Uri 'System.Uri') | The base URI of the service. |
| credentials | [Primavera.Hydrogen.Rest.Client.ServiceClientCredentials](#T-Primavera-Hydrogen-Rest-Client-ServiceClientCredentials 'Primavera.Hydrogen.Rest.Client.ServiceClientCredentials') | The credentials that should be used to access the service. |
| handler | [System.Net.Http.HttpMessageHandler](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Net.Http.HttpMessageHandler 'System.Net.Http.HttpMessageHandler') | The root message handler. |
| disposeHandler | [System.Boolean](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Boolean 'System.Boolean') | True if the inner handler should be disposed of, false if the inner handler is intended to be reused. |

<a name='M-Primavera-Lithium-Notifications-NotificationsClient-#ctor-System-Uri,Primavera-Hydrogen-Rest-Client-AuthenticationCallback-'></a>
### #ctor(baseUri,callback) `constructor`

##### Summary

Initializes a new instance of the [NotificationsClient](#T-Primavera-Lithium-Notifications-NotificationsClient 'Primavera.Lithium.Notifications.NotificationsClient') class.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| baseUri | [System.Uri](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Uri 'System.Uri') | The base URI of the service. |
| callback | [Primavera.Hydrogen.Rest.Client.AuthenticationCallback](#T-Primavera-Hydrogen-Rest-Client-AuthenticationCallback 'Primavera.Hydrogen.Rest.Client.AuthenticationCallback') | The callback that will be invoked during authentication to get the access token. |

<a name='M-Primavera-Lithium-Notifications-NotificationsClient-#ctor-System-Uri,Primavera-Hydrogen-Rest-Client-AuthenticationCallback,System-Net-Http-HttpMessageHandler-'></a>
### #ctor(baseUri,callback,handler) `constructor`

##### Summary

Initializes a new instance of the [NotificationsClient](#T-Primavera-Lithium-Notifications-NotificationsClient 'Primavera.Lithium.Notifications.NotificationsClient') class.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| baseUri | [System.Uri](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Uri 'System.Uri') | The base URI of the service. |
| callback | [Primavera.Hydrogen.Rest.Client.AuthenticationCallback](#T-Primavera-Hydrogen-Rest-Client-AuthenticationCallback 'Primavera.Hydrogen.Rest.Client.AuthenticationCallback') | The callback that will be invoked during authentication to get the access token. |
| handler | [System.Net.Http.HttpMessageHandler](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Net.Http.HttpMessageHandler 'System.Net.Http.HttpMessageHandler') | The root message handler. |

<a name='M-Primavera-Lithium-Notifications-NotificationsClient-#ctor-System-Uri,Primavera-Hydrogen-Rest-Client-AuthenticationCallback,System-Net-Http-HttpMessageHandler,System-Boolean-'></a>
### #ctor(baseUri,callback,handler,disposeHandler) `constructor`

##### Summary

Initializes a new instance of the [NotificationsClient](#T-Primavera-Lithium-Notifications-NotificationsClient 'Primavera.Lithium.Notifications.NotificationsClient') class.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| baseUri | [System.Uri](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Uri 'System.Uri') | The base URI of the service. |
| callback | [Primavera.Hydrogen.Rest.Client.AuthenticationCallback](#T-Primavera-Hydrogen-Rest-Client-AuthenticationCallback 'Primavera.Hydrogen.Rest.Client.AuthenticationCallback') | The callback that will be invoked during authentication to get the access token. |
| handler | [System.Net.Http.HttpMessageHandler](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Net.Http.HttpMessageHandler 'System.Net.Http.HttpMessageHandler') | The root message handler. |
| disposeHandler | [System.Boolean](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Boolean 'System.Boolean') | True if the inner handler should be disposed of, false if the inner handler is intended to be reused. |

<a name='T-Primavera-Lithium-Notifications-NotificationsClientBase'></a>
## NotificationsClientBase `type`

##### Namespace

Primavera.Lithium.Notifications

##### Summary

Defines the base class for the service client that allows accessing the Notifications Service API.

<a name='M-Primavera-Lithium-Notifications-NotificationsClientBase-#ctor-System-Uri,Primavera-Hydrogen-Rest-Client-ServiceClientCredentials-'></a>
### #ctor(baseUri,credentials) `constructor`

##### Summary

Initializes a new instance of the [NotificationsClientBase](#T-Primavera-Lithium-Notifications-NotificationsClientBase 'Primavera.Lithium.Notifications.NotificationsClientBase') class.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| baseUri | [System.Uri](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Uri 'System.Uri') | The base URI of the service. |
| credentials | [Primavera.Hydrogen.Rest.Client.ServiceClientCredentials](#T-Primavera-Hydrogen-Rest-Client-ServiceClientCredentials 'Primavera.Hydrogen.Rest.Client.ServiceClientCredentials') | The credentials that should be used to access the service. |

<a name='M-Primavera-Lithium-Notifications-NotificationsClientBase-#ctor-System-Uri,Primavera-Hydrogen-Rest-Client-ServiceClientCredentials,System-Net-Http-HttpMessageHandler-'></a>
### #ctor(baseUri,credentials,handler) `constructor`

##### Summary

Initializes a new instance of the [NotificationsClientBase](#T-Primavera-Lithium-Notifications-NotificationsClientBase 'Primavera.Lithium.Notifications.NotificationsClientBase') class.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| baseUri | [System.Uri](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Uri 'System.Uri') | The base URI of the service. |
| credentials | [Primavera.Hydrogen.Rest.Client.ServiceClientCredentials](#T-Primavera-Hydrogen-Rest-Client-ServiceClientCredentials 'Primavera.Hydrogen.Rest.Client.ServiceClientCredentials') | The credentials that should be used to access the service. |
| handler | [System.Net.Http.HttpMessageHandler](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Net.Http.HttpMessageHandler 'System.Net.Http.HttpMessageHandler') | The root message handler. |

<a name='M-Primavera-Lithium-Notifications-NotificationsClientBase-#ctor-System-Uri,Primavera-Hydrogen-Rest-Client-ServiceClientCredentials,System-Net-Http-HttpMessageHandler,System-Boolean-'></a>
### #ctor(baseUri,credentials,handler,disposeHandler) `constructor`

##### Summary

Initializes a new instance of the [NotificationsClientBase](#T-Primavera-Lithium-Notifications-NotificationsClientBase 'Primavera.Lithium.Notifications.NotificationsClientBase') class.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| baseUri | [System.Uri](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Uri 'System.Uri') | The base URI of the service. |
| credentials | [Primavera.Hydrogen.Rest.Client.ServiceClientCredentials](#T-Primavera-Hydrogen-Rest-Client-ServiceClientCredentials 'Primavera.Hydrogen.Rest.Client.ServiceClientCredentials') | The credentials that should be used to access the service. |
| handler | [System.Net.Http.HttpMessageHandler](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Net.Http.HttpMessageHandler 'System.Net.Http.HttpMessageHandler') | The root message handler. |
| disposeHandler | [System.Boolean](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Boolean 'System.Boolean') | True if the inner handler should be disposed of, false if the inner handler is intended to be reused. |

<a name='M-Primavera-Lithium-Notifications-NotificationsClientBase-#ctor-System-Uri,Primavera-Hydrogen-Rest-Client-AuthenticationCallback-'></a>
### #ctor(baseUri,callback) `constructor`

##### Summary

Initializes a new instance of the [NotificationsClientBase](#T-Primavera-Lithium-Notifications-NotificationsClientBase 'Primavera.Lithium.Notifications.NotificationsClientBase') class.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| baseUri | [System.Uri](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Uri 'System.Uri') | The base URI of the service. |
| callback | [Primavera.Hydrogen.Rest.Client.AuthenticationCallback](#T-Primavera-Hydrogen-Rest-Client-AuthenticationCallback 'Primavera.Hydrogen.Rest.Client.AuthenticationCallback') | The callback that will be invoked during authentication to get the access token. |

<a name='M-Primavera-Lithium-Notifications-NotificationsClientBase-#ctor-System-Uri,Primavera-Hydrogen-Rest-Client-AuthenticationCallback,System-Net-Http-HttpMessageHandler-'></a>
### #ctor(baseUri,callback,handler) `constructor`

##### Summary

Initializes a new instance of the [NotificationsClientBase](#T-Primavera-Lithium-Notifications-NotificationsClientBase 'Primavera.Lithium.Notifications.NotificationsClientBase') class.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| baseUri | [System.Uri](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Uri 'System.Uri') | The base URI of the service. |
| callback | [Primavera.Hydrogen.Rest.Client.AuthenticationCallback](#T-Primavera-Hydrogen-Rest-Client-AuthenticationCallback 'Primavera.Hydrogen.Rest.Client.AuthenticationCallback') | The callback that will be invoked during authentication to get the access token. |
| handler | [System.Net.Http.HttpMessageHandler](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Net.Http.HttpMessageHandler 'System.Net.Http.HttpMessageHandler') | The root message handler. |

<a name='M-Primavera-Lithium-Notifications-NotificationsClientBase-#ctor-System-Uri,Primavera-Hydrogen-Rest-Client-AuthenticationCallback,System-Net-Http-HttpMessageHandler,System-Boolean-'></a>
### #ctor(baseUri,callback,handler,disposeHandler) `constructor`

##### Summary

Initializes a new instance of the [NotificationsClientBase](#T-Primavera-Lithium-Notifications-NotificationsClientBase 'Primavera.Lithium.Notifications.NotificationsClientBase') class.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| baseUri | [System.Uri](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Uri 'System.Uri') | The base URI of the service. |
| callback | [Primavera.Hydrogen.Rest.Client.AuthenticationCallback](#T-Primavera-Hydrogen-Rest-Client-AuthenticationCallback 'Primavera.Hydrogen.Rest.Client.AuthenticationCallback') | The callback that will be invoked during authentication to get the access token. |
| handler | [System.Net.Http.HttpMessageHandler](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Net.Http.HttpMessageHandler 'System.Net.Http.HttpMessageHandler') | The root message handler. |
| disposeHandler | [System.Boolean](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Boolean 'System.Boolean') | True if the inner handler should be disposed of, false if the inner handler is intended to be reused. |

<a name='P-Primavera-Lithium-Notifications-NotificationsClientBase-EmailNotifications'></a>
### EmailNotifications `property`

##### Summary

*Inherit from parent.*

<a name='P-Primavera-Lithium-Notifications-NotificationsClientBase-EmailTemplates'></a>
### EmailTemplates `property`

##### Summary

*Inherit from parent.*

<a name='P-Primavera-Lithium-Notifications-NotificationsClientBase-Monitoring'></a>
### Monitoring `property`

##### Summary

*Inherit from parent.*

<a name='P-Primavera-Lithium-Notifications-NotificationsClientBase-SmsNotifications'></a>
### SmsNotifications `property`

##### Summary

*Inherit from parent.*

<a name='P-Primavera-Lithium-Notifications-NotificationsClientBase-SmsTemplates'></a>
### SmsTemplates `property`

##### Summary

*Inherit from parent.*

<a name='M-Primavera-Lithium-Notifications-NotificationsClientBase-Dispose-System-Boolean-'></a>
### Dispose() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-NotificationsClientBase-InitializeApiVersion'></a>
### InitializeApiVersion() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='T-Primavera-Lithium-Notifications-SmsNotificationsOperations'></a>
## SmsNotificationsOperations `type`

##### Namespace

Primavera.Lithium.Notifications

##### Summary

Provides actions to send SMS notifications.

##### See Also

- [Primavera.Lithium.Notifications.SmsNotificationsOperationsBase](#T-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase 'Primavera.Lithium.Notifications.SmsNotificationsOperationsBase')

<a name='M-Primavera-Lithium-Notifications-SmsNotificationsOperations-#ctor-Primavera-Lithium-Notifications-NotificationsClientBase,Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase}-'></a>
### #ctor(client,actions) `constructor`

##### Summary

Initializes a new instance of the [SmsNotificationsOperations](#T-Primavera-Lithium-Notifications-SmsNotificationsOperations 'Primavera.Lithium.Notifications.SmsNotificationsOperations') class.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| client | [Primavera.Lithium.Notifications.NotificationsClientBase](#T-Primavera-Lithium-Notifications-NotificationsClientBase 'Primavera.Lithium.Notifications.NotificationsClientBase') | The service client. |
| actions | [Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase}](#T-Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase} 'Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase}') | The service client actions. |

<a name='T-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase'></a>
## SmsNotificationsOperationsBase `type`

##### Namespace

Primavera.Lithium.Notifications

##### Summary

Provides actions to send SMS notifications.

##### See Also

- [Primavera.Lithium.Notifications.ISmsNotificationsOperations](#T-Primavera-Lithium-Notifications-ISmsNotificationsOperations 'Primavera.Lithium.Notifications.ISmsNotificationsOperations')
- [System.IDisposable](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.IDisposable 'System.IDisposable')

<a name='M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-#ctor-Primavera-Lithium-Notifications-NotificationsClientBase,Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase}-'></a>
### #ctor(client,actions) `constructor`

##### Summary

Initializes a new instance of the [SmsNotificationsOperationsBase](#T-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase 'Primavera.Lithium.Notifications.SmsNotificationsOperationsBase') class.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| client | [Primavera.Lithium.Notifications.NotificationsClientBase](#T-Primavera-Lithium-Notifications-NotificationsClientBase 'Primavera.Lithium.Notifications.NotificationsClientBase') | The service client. |
| actions | [Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase}](#T-Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase} 'Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase}') | The service client actions. |

<a name='P-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-Actions'></a>
### Actions `property`

##### Summary

Gets or sets the service client actions.

<a name='P-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-Client'></a>
### Client `property`

##### Summary

Gets or sets the service client.

<a name='M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-CancelSmsNotification-System-Guid-'></a>
### CancelSmsNotification() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-CancelSmsNotificationAsync-System-Guid,System-Threading-CancellationToken-'></a>
### CancelSmsNotificationAsync() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-Dispose'></a>
### Dispose() `method`

##### Summary

Performs application-defined tasks associated with freeing, releasing, or resetting unmanaged resources.

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-Dispose-System-Boolean-'></a>
### Dispose(disposing) `method`

##### Summary

Releases unmanaged and - optionally - managed resources.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| disposing | [System.Boolean](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Boolean 'System.Boolean') | `true` to release both managed and unmanaged resources; `false` to release only unmanaged resources. |

<a name='M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-GetSmsNotification-System-Guid-'></a>
### GetSmsNotification() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-GetSmsNotificationAsync-System-Guid,System-Threading-CancellationToken-'></a>
### GetSmsNotificationAsync() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-GetSmsNotificationState-System-Guid-'></a>
### GetSmsNotificationState() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-GetSmsNotificationStateAsync-System-Guid,System-Threading-CancellationToken-'></a>
### GetSmsNotificationStateAsync() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-GetSmsNotificationsState-System-Collections-Generic-IEnumerable{System-Guid}-'></a>
### GetSmsNotificationsState() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-GetSmsNotificationsStateAsync-System-Collections-Generic-IEnumerable{System-Guid},System-Threading-CancellationToken-'></a>
### GetSmsNotificationsStateAsync() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-RetrySmsNotification-System-Guid-'></a>
### RetrySmsNotification() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-RetrySmsNotificationAsync-System-Guid,System-Threading-CancellationToken-'></a>
### RetrySmsNotificationAsync() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-SendSmsNotification-Primavera-Lithium-Notifications-Models-SmsNotificationData-'></a>
### SendSmsNotification() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-SmsNotificationsOperationsBase-SendSmsNotificationAsync-Primavera-Lithium-Notifications-Models-SmsNotificationData,System-Threading-CancellationToken-'></a>
### SendSmsNotificationAsync() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='T-Primavera-Lithium-Notifications-SmsTemplatesOperations'></a>
## SmsTemplatesOperations `type`

##### Namespace

Primavera.Lithium.Notifications

##### Summary

Provides actions to create, update, and delete SMS templates.

##### See Also

- [Primavera.Lithium.Notifications.SmsTemplatesOperationsBase](#T-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase 'Primavera.Lithium.Notifications.SmsTemplatesOperationsBase')

<a name='M-Primavera-Lithium-Notifications-SmsTemplatesOperations-#ctor-Primavera-Lithium-Notifications-NotificationsClientBase,Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase}-'></a>
### #ctor(client,actions) `constructor`

##### Summary

Initializes a new instance of the [SmsTemplatesOperations](#T-Primavera-Lithium-Notifications-SmsTemplatesOperations 'Primavera.Lithium.Notifications.SmsTemplatesOperations') class.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| client | [Primavera.Lithium.Notifications.NotificationsClientBase](#T-Primavera-Lithium-Notifications-NotificationsClientBase 'Primavera.Lithium.Notifications.NotificationsClientBase') | The service client. |
| actions | [Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase}](#T-Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase} 'Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase}') | The service client actions. |

<a name='T-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase'></a>
## SmsTemplatesOperationsBase `type`

##### Namespace

Primavera.Lithium.Notifications

##### Summary

Provides actions to create, update, and delete SMS templates.

##### See Also

- [Primavera.Lithium.Notifications.ISmsTemplatesOperations](#T-Primavera-Lithium-Notifications-ISmsTemplatesOperations 'Primavera.Lithium.Notifications.ISmsTemplatesOperations')
- [System.IDisposable](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.IDisposable 'System.IDisposable')

<a name='M-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-#ctor-Primavera-Lithium-Notifications-NotificationsClientBase,Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase}-'></a>
### #ctor(client,actions) `constructor`

##### Summary

Initializes a new instance of the [SmsTemplatesOperationsBase](#T-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase 'Primavera.Lithium.Notifications.SmsTemplatesOperationsBase') class.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| client | [Primavera.Lithium.Notifications.NotificationsClientBase](#T-Primavera-Lithium-Notifications-NotificationsClientBase 'Primavera.Lithium.Notifications.NotificationsClientBase') | The service client. |
| actions | [Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase}](#T-Primavera-Hydrogen-Rest-Client-ServiceClientActions{Primavera-Lithium-Notifications-NotificationsClientBase} 'Primavera.Hydrogen.Rest.Client.ServiceClientActions{Primavera.Lithium.Notifications.NotificationsClientBase}') | The service client actions. |

<a name='P-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-Actions'></a>
### Actions `property`

##### Summary

Gets or sets the service client actions.

<a name='P-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-Client'></a>
### Client `property`

##### Summary

Gets or sets the service client.

<a name='M-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-CreateSmsTemplate-Primavera-Lithium-Notifications-Models-SmsTemplateData-'></a>
### CreateSmsTemplate() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-CreateSmsTemplateAsync-Primavera-Lithium-Notifications-Models-SmsTemplateData,System-Threading-CancellationToken-'></a>
### CreateSmsTemplateAsync() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-DeleteSmsTemplate-System-String-'></a>
### DeleteSmsTemplate() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-DeleteSmsTemplateAsync-System-String,System-Threading-CancellationToken-'></a>
### DeleteSmsTemplateAsync() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-Dispose'></a>
### Dispose() `method`

##### Summary

Performs application-defined tasks associated with freeing, releasing, or resetting unmanaged resources.

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-Dispose-System-Boolean-'></a>
### Dispose(disposing) `method`

##### Summary

Releases unmanaged and - optionally - managed resources.

##### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| disposing | [System.Boolean](http://msdn.microsoft.com/query/dev14.query?appId=Dev14IDEF1&l=EN-US&k=k:System.Boolean 'System.Boolean') | `true` to release both managed and unmanaged resources; `false` to release only unmanaged resources. |

<a name='M-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-GetSmsTemplate-System-String-'></a>
### GetSmsTemplate() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-GetSmsTemplateAsync-System-String,System-Threading-CancellationToken-'></a>
### GetSmsTemplateAsync() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-GetSmsTemplates'></a>
### GetSmsTemplates() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-GetSmsTemplatesAsync-System-Threading-CancellationToken-'></a>
### GetSmsTemplatesAsync() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-UpdateSmsTemplate-Primavera-Lithium-Notifications-Models-SmsTemplateData-'></a>
### UpdateSmsTemplate() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.

<a name='M-Primavera-Lithium-Notifications-SmsTemplatesOperationsBase-UpdateSmsTemplateAsync-Primavera-Lithium-Notifications-Models-SmsTemplateData,System-Threading-CancellationToken-'></a>
### UpdateSmsTemplateAsync() `method`

##### Summary

*Inherit from parent.*

##### Parameters

This method has no parameters.
