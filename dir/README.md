# Services Directory

This chapter includes reference a directory of all the Lithium microservices available.

Microservices are classified, for the purpose of logical organization, in categories.

## Categories

### Common

These services are core to the Lithium Framework and provide common features, applicable to any kind of application.

| Microservice | Name | Description | Spec |
| - | - | - | - |
| [CS](./common/cs.md) | Certificates Service | Allows distributing and automatically update certificates used by applications | --- |
| [DLS](./common/dls.md) | Data Lookup Service | Allows searching and updating reference data (e.g. VAT numbers) | --- |
| [FRS](./common/frs.md) | Fiscal Reports Service | Allows producing fiscal reports (for Tax Authorities) from predefined models | --- |
| [FS](./common/fs.md) | File Store Service | Allows sharing and automatically update files used by applications | --- |
| [HUB](./common/hub.md) | Hub Service | Nanoservices that act as proxies to third-party APIs | --- |
| [IDS](./common/ids.md) | Identity Server | Provides identity management (user authentication and client applications authorization) for applications and microservices | --- |
| [NS](./common/ns.md) | Notifications Service | Allows sending email and SMS notifications using templates | --- |
| [NTR](./common/ntr.md) | Nitrogen Service | Supports the app store for cloud products | --- |
| [PNS](./common/pns.md) | Push Notifications Service | Allows receive, store and broadcast notifications to the connected clients | --- |
| [SM](./common/sm.md) | Service Management Service | Provides a proxy for the Service Management Service | --- |
| [SS](./common/ss.md) | Settings Service | Allows storing settings (per user, per product, and per product/user) | --- |
| [TAXES](./common/taxes.md) | Tax Authority (Spain) Service | Provides integration with the Spanish tax authority services | --- |
| [TAXPT](./common/taxpt.md) | Tax Authority (Portugal) Service | Provides integration with the Portuguese tax authority services | --- |
| [TBX](./common/tbx.md) | Taskbox Service | Supports asynchronous behavior in the PRIMAVERA Elevation Platform | --- |
| [TRG](./common/trg.md)| Triggers Service | Allows executing Web actions based on schedules | [2.0](./common/trg-spec-2.0.md) |

### Carbon

These services are part of the Carbon Framework (Intelligent ERP).

| Microservice | Name | Description | Spec |
| - | - | - | - |
| [MTR](./carbon/mtr.md) | Metrics Service | Allows storing and producing metrics | --- |

### Elevation

These services provide features that are part of the Elevation Framework.

| Microservice | Name | Description | Spec |
| - | - | - | - |
| [PMS](./elevation/pms.md) | Postman Service | Allows presenting reports, built from templates, to users | --- |
| [PUS](./elevation/pus.md) | Product Upgrade Service | Provides operations to upgrade cloud product versions | --- |

### ERP

These services were designed to integrate in the ERP.

| Microservice | Name | Description | Spec |
| - | - | - | - |
| [LIC](./erp/lic.md) | Licensing Service | Features to support licensing in the ERP | --- |
| [MV10](./erp/mv10.md) | Mobile V10 Service | Features to integrate the ERP with the Mobile V10 application | --- |

### PAA

These services provide features related with PRIMAVERA Accounting Automation.

| Microservice | Name | Description | Spec |
| - | - | - | - |
| [AAS](./paa/aas.md) | Accounting Automation Service | Features to integrate PAA with cloud products (Jasmin) | --- |

## Endpoints

A directory of the all the services endpoints is available [here](https://tfs.primaverabss.com/tfs/P.TEC.Elevation/Lithium/_versionControl?path=%24%2FLithium%2F_doc%2Flithium-endpoints.md).