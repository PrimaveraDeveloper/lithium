# Services Directory

This chapter includes a directory of all the Lithium microservices available.

## Categories

The microservices are classified, for the purpose of logical organization, in categories.

| Category | Description |
| - | - |
| [Common](./common/README.md) | These services are core to the Lithium Framework and provide common features, applicable to any kind of application. |
| [Carbon](./carbon/README.md) | These services are part of the Carbon Framework (Intelligent ERP). |
| [Elevation](./elevation/README.md) | These services provide features that are part of the Elevation Framework. |
| [ERP](./erp/README.md) | These services were designed to integrate in the ERP. |
| [Infrastructure](./infrastructure/README.md) | These services provide features for integration with the cloud infrastructure. |
| [PAA](./pass/README.md) | These services provide features related with PRIMAVERA Accounting Automation. |
| [ROSE](./rose/README.md) | These services provide features related with PRIMAVERA ROSE. |

## Microservices

This is the list of all managed microservices:

| Microservice | Name | Category | Description | Spec |
| - | - | - | - | - |
| [AAS](./paa/aas.md) | Accounting Automation Service | PAA | Provides features to integrate PAA with cloud products (Jasmin) | --- |
| [AMS](./elevation/ams.md) | Account Management Service | Elevation | Allows managing subscriptions and provides onboarding and subscription selection to products | --- |
| [CS](./common/cs.md) | Certificates Service | Common | Allows distributing and automatically update certificates used by applications | [2.0](./common/specs/cs-spec-2.0.md) |
| [DCS](./carbon/dcs.md) | Data Catalog Service | Carbon | Allows access to reference data like currencies, countries, postal codes and Spain economic activities codes | --- |
| [DE](./infrastructure/de.md)| Discount Engine Service | Infrastructure | --- | --- || [DIS](./carbon/dis.md)| Data Ingestion Service | Carbon | --- | --- |
| [DLS](./common/dls.md) | Data Lookup Service | Common | Allows searching and updating reference data (e.g. VAT numbers) | [2.0](./common/specs/dls-spec-2.0.md) |
| [FRS](./common/frs.md) | Fiscal Reports Service | Common | Allows producing fiscal reports (for Tax Authorities) from predefined models | --- |
| [FS](./common/fs.md) | File Store Service | Common | Allows sharing and automatically update files used by applications | [2.0](./common/specs/fs-spec-2.0.md) |
| [HUB](./common/hub.md) | Hub Service | Common | Nanoservices that act as proxies to third-party APIs | --- |
| [IDS](./common/ids.md) | Identity Server | Common | Provides identity management (user authentication and client applications authorization) for applications and microservices | --- |
| [LIC](./common/lic.md) | Licensing Service | Common | Provides features to support licensing for the ERP and ROSE | --- |
| [MTR](./carbon/mtr.md) | Metrics Service | Carbon | Allows storing and producing metrics | --- |
| [MV10](./erp/mv10.md) | Mobile V10 Service | ERP | Features to integrate the ERP with the Mobile V10 application | --- || [NS](./common/ns.md) | Notifications Service | Common | Allows sending email and SMS notifications using templates | [2.0](./common/specs/ns-spec-2.0.md) |
| [NTR](./common/ntr.md) | Nitrogen Service | Common | Supports the app store for cloud products | --- |
| [PCS](./infrastructure/pcs.md)| Provisioning Center Service | Infrastructure | --- | --- |
| [PMS](./elevation/pms.md) | Postman Service | Elevation | Allows presenting reports, built from templates, to users | --- |
| [PNS](./common/pns.md) | Push Notifications Service | Common | Allows to receive, store and broadcast real-time notifications to the connected clients | [2.0](./common/specs/pns-spec-2.0.md) |
| [PPS](./infrastructure/pps.md)| Pricing Policy Service | Infrastructure | --- | --- |
| [PUS](./elevation/pus.md) | Product Upgrade Service | Elevation | Provides operations to upgrade cloud product versions | --- |
| [RPS](./rose/rps.md) | ROSE People Service | ROSE | Provides features for ROSE People | [1.0](./rose/specs/rps-spec-1.0.md) |
| [SM](./common/sm.md) | Service Management Service | Common | Provides a proxy for the Service Management Service | [2.0](./common/specs/sm-spec-2.0.md) |
| [SS](./common/ss.md) | Settings Service | Common | Allows storing settings (per user, per product, and per product/user) | [2.0](./common/specs/ss-spec-2.0.md) |
| [TAXES](./common/taxes.md) | Tax Authority (Spain) Service | Common | Provides integration with the Spanish tax authority services | --- |
| [TAXPT](./common/taxpt.md) | Tax Authority (Portugal) Service | Common | Provides integration with the Portuguese tax authority services | [2.0](./common/specs/taxpt-spec-2.0.md) |
| [TBX](./common/tbx.md) | Taskbox Service | Common | Provides asynchronous multi-task processing for applications | --- |
| [TRG](./common/trg.md)| Triggers Service | Common | Allows executing Web actions based on schedules | [2.0](./common/specs/trg-spec-2.0.md) |

## Endpoints

<!-- markdown-link-check-disable -->
A directory of the all the services endpoints is available [here](https://tfs.primaverabss.com/tfs/P.TEC.Elevation/Lithium/_versionControl?path=%24%2FLithium%2F_doc%2Flithium-endpoints.md).
<!-- markdown-link-check-enable -->