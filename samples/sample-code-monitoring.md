# Sample - Monitoring Controller

This sample shows an implementation of the Monitoring Controller endpoints (Probe and Diagnostics).

## Health Checks (Monitoring)

A good design pattern for microservices is to have Web API endpoints to check the health (monitor) of the microservice.

The Lithium Framework defines two such endpoints as part of the monitoring controller. The service will be monitored in the production environment using these endpoints to trigger healing actions (scaling, reboots, etc.).

### Probe

This is the most basic monitoring endpoint.

It should only verify if the service minimum features are up and running by performing operations on the most important dependencies.

> The probe should not return only "OK" without performing any real operation on the service dependencies. There are other (prior) infrastructure health checks that verify that the service infrastructure is running. A service may be up and running (itself) but one his dependencies may be down or, worst, responding inconsistently. If the probe endpoint does not check those dependencies it will not be useful at all.

### Diagnostics

This monitoring endpoint should verify dependencies but it should also verify additional business logic specific to the service.

The objective of the diagnostics endpoint is to identify situations where the services are running and responding to requests without errors but producing incorrect responses for some reason related to internal business logic or related to inconsistencies in the dependencies that are too expensive to verify on every call to the probe endpoint.

> The diagnostics endpoint is called with a much lesser frequency than the probe endpoint.

## Example

This Notifications Service is a service that sends notifications by email and or SMS to users. It relies on some dependencies that need to be verifying in the probe endpoint (cache, table storage).

The probe and diagnostics endpoint are implemented like this in NS:

```csharp
using System;
using System.Diagnostics.CodeAnalysis;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;
using Primavera.Hydrogen;
using Primavera.Hydrogen.Rest;
using Primavera.Lithium.Notifications.Models;
using Primavera.Lithium.Notifications.WebApi.Managers.Interfaces;

namespace Primavera.Lithium.Notifications.WebApi.Controllers
{
    [SuppressMessage("StyleCop.CSharp.DocumentationRules", "SA1601: PartialElementsMustBeDocumented")]
    public partial class MonitoringController
    {
        // (...)

        #region Public Methods

        /// <inheritdoc />
        public override async Task<IActionResult> ProbeAsync()
        {
            // NOTE:
            // The probe endpoint tests the following dependencies byt
            // saving, retrieving, and deleting an email template:
            // - Distributed Caching
            // - Table Storage
            // - Managers

            string id = Guid.NewGuid().ToString().Transform().ToUpperCase();

            EmailTemplateData emailTemplate = new EmailTemplateData()
            {
                Id = id,
                Description = "Probe email template",
                ReplyTo = "no-reply@primaverabss.com",
                From = "no-reply@primaverabss.com",
                Subject = "Probe Email Template",
                BodyHtml = "<p>Probe email template</p>"
            };

            // Create an email template

            OperationResult<string> result1 = await this.EmailTemplatesManager
                .CreateAsync(emailTemplate, CancellationToken.None)
                .ConfigureAwait(false);
            
            if (result1.IsFailureWith(ErrorCodes.EmailTemplateAlreadyExists))
            {
                // Logging

                this.Logger.LogDebug($"The email template was not created because it already exists.");
            }
            else if (result1.IsFailure)
            {
                // Something went wrong

                ServiceError error = new ServiceError(result1.Error.Code, result1.Error.Description);
                return this.BadRequest(error);
            }
            else
            {
                // Logging

                this.Logger.LogDebug($"The email template was created.");
            }

            // Get the email template

            OperationResult<EmailTemplateData> result2 = await this.EmailTemplatesManager
                .GetAsync(emailTemplate.Id, CancellationToken.None)
                .ConfigureAwait(false);
            
            if (result2.IsFailure)
            {
                // Something went wrong

               ServiceError error = new ServiceError(result2.Error.Code, result2.Error.Description);
                return this.BadRequest(error);
            }

            // Logging

            this.Logger.LogDebug($"The email template was retrieved.");

            // Delete the email template

            OperationResult result3 = await this.EmailTemplatesManager
                .DeleteAsync(emailTemplate.Id, CancellationToken.None)
                .ConfigureAwait(false);
            
            if (result3.IsFailure)
            {
                // Something went wrong

                ServiceError error = new ServiceError(result3.Error.Code, result3.Error.Description);
                return this.BadRequest(error);
            }

            // Logging

            this.Logger.LogDebug($"The email template was deleted.");

            // Result OK

            return this.Ok("OK");
        }

        /// <inheritdoc />
        public override async Task<IActionResult> DiagnosticsAsync()
        {
            // NOTE:
            // The diagnostics endpoint tests the same as probe but also tests that
            // SMS notifications are not getting queueing.

            // Use probe

            IActionResult probeResult = await this
                .ProbeAsync()
                .ConfigureAwit(false);
            
            if (probeResult.StatusCode != 200)
            {
                // Something went wrong

                return probeResult;
            }

            // Get the email template

            OperationResult<int> result = await this.SmsNotificationsManager
                .CountQueuedAsync()
                .ConfigureAwait(false);
            
            if (result.IsFailure)
            {
                // Something went wrong

                ServiceError error = new ServiceError(result.Error.Code, result.Error.Description);
                return this.BadRequest(error);
            }

            if (result2.Body > 10)
            {
                // Something went wrong

                ServiceError error = new ServiceError("TooManyQueuedSmsNotifications", "There are more than 10 SMS notifications queued.");
                return this.BadRequest(error);
            }

            // Result OK

            return this.Ok("OK");
        }

        #endregion
    }
}
```