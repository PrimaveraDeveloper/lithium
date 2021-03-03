# Services Directory - Common

These services are core to the Lithium Framework and provide common features, applicable to any kind of application.

| Microservice | Name | Description | Scopes | Client Lib | Web API | Spec |
| - | - | - | - | - | - | - |
| **CS** | Certificates Service | Allows distributing and automatically update certificates used by applications | `lithium-certificates` | [2.0](https://cs.lithium.primaverabss.com/.doc/clientlib) | [2.0](https://cs.lithium.primaverabss.com/.doc/webapi) | [2.0](./specs/cs-spec-2.0.md) |
| **DLS** | Data Lookup Service | Allows searching and updating reference data (e.g. VAT numbers) | `lithium-datalookup-vatnumber` `lithium-datalookup-exchrate` | [2.0](https://dls.lithium.primaverabss.com/.doc/clientlib) | [2.0](https://dls.lithium.primaverabss.com/.doc/webapi) | [2.0](./specs/dls-spec-2.0.md) |
| **ECCO** | eCommerce Connect Service | Provides a middleware to integrate products and eCommerce platforms | `lithium-ecco` | [2.0](https://ecco.lithium.primaverabss.com/.doc/clientlib) | [2.0](https://ecco.lithium.primaverabss.com/.doc/webapi) | [2.0](./specs/ecco-spec-2.0.md) |
| **FRS** | Fiscal Reports Service | Allows producing fiscal reports from predefined models | `lithium-fiscalreports` `lithium-reportmanagement` | n/a | n/a | n/a |
| **FS** | File Store Service | Allows sharing and automatically update files used by applications | `lithium-filestore` | [2.0](https://fs.lithium.primaverabss.com/.doc/clientlib) | [2.0](https://lithium-filestore.primaverabss.com/.doc/webapi) | [2.0](./specs/fs-spec-2.0.md) |
| **HUB** | Hub Service | Nanoservices that act as proxies for third-party services | `lithium-hub` | [2.0](https://hub.lithium.primaverabss.com/.doc/clientlib) | [2.0](https://hub.lithium.primaverabss.com/.doc/webapi) | n/a |
| **NS** | Notifications Service | Allows sending email and SMS notifications using templates | `lithium-notifications` | [2-0](https://ns.lithium.primaverabss.com/.doc/clientlib) | [2.0](https://ns.lithium.primaverabss.com/.doc/webapi) | [2.0](./specs/ns-spec-2.0.md) |
| **NTR** | Nitrogen Service | Provides features to support the App Stores | `lithium-nitrogen` | n/a | n/a | n/a |
| **OBS** | Open Banking Service | Provides integration with third-party open banking services and other operations related with banking | `lithium-obs` | n/a | n/a | [1.0](./specs/obs-spec-1.0.md) |
| **PNS** | Push Notifications Service | Allows storing and pushing real-time notifications to connected clients | `lithium-pushnotifications` `lithium-pushnotifications-hub` | [2.0](https://pns.lithium.primaverabss.com/.doc/clientlib) | [2.0](https://pns.lithium.primaverabss.com/.doc/webapi) | [2.0](./specs/pns-spec-2.0.md) |
| **SS** | Settings Service | Allows storing settings (per user, per product, and per product/user) | `lithium-settings` | [2.0](https://ss.lithium.primaverabss.com/.doc/clientlib) | [2.0](https://ss.lithium.primaverabss.com/.doc/webapi) | [2.0](./specs/ss-spec-2.0.md) |
| **TAXPT** | Tax Authority (Portugal) Service | Provides features to integrate with the Portuguese Tax Authority services | `lithium-taxauthority-portugal` | [3.0](https://taxpt.lithium.primaverabss.com/.doc/clientlib) | [3.0](https://taxpt.lithium.primaverabss.com/.doc/webapi) | [3.0](./specs/taxpt-spec-3.0.md) [2.0](./specs/taxpt-spec-2.0.md) |
| **TBX** | Taskbox Service | Allows executing asynchronous work in batch implemented as pipelines | `lithium-taskbox` | [2.0](https://tbx.lithium.primaverabss.com/.doc/clientlib) | [2.0](https://tbx.lithium.primaverabss.com/.doc/webapi) | [2.0](./specs/tbx-spec-2.0.md) |

## Endpoints

| Service | Development | Staging | Preview | Production |
| - | - | - | - | - |
| **CS** | [dv-cs](https://dv-cs.lithium.primaverabss.com/) | [st-cs](https://st-cs.lithium.primaverabss.com/) | [cs-preview](https://cs-preview.lithium.primaverabss.com/) | [cs](https://cs.lithium.primaverabss.com/) |
| **DLS** | [dv-dls](https://dv-dls.lithium.primaverabss.com/) | [st-dls](https://st-dls.lithium.primaverabss.com/) | [dls-preview](https://dls-preview.lithium.primaverabss.com/) | [dls](https://dls.lithium.primaverabss.com/) |
| **ECCO** | [dv-ecco](https://dv-ecco.lithium.primaverabss.com/) | [st-ecco](https://st-ecco.lithium.primaverabss.com/) | [ecco-preview](https://ecco-preview.lithium.primaverabss.com/) | [ecco](https://ecco.lithium.primaverabss.com/) |
| **FRS** | [dv-frs](https://dv-frs.lithium.primaverabss.com/) | [st-frs](https://st-frs.lithium.primaverabss.com/) | ??? | [frs](https://lithium-frs.primaverabss.com/) |
| **FS** | [dv-fs](https://dv-fs.lithium.primaverabss.com/) | [st-fs](https://st-fs.lithium.primaverabss.com/) | [fs-preview](https://fs-preview.lithium.primaverabss.com/) | [fs](https://fs.lithium.primaverabss.com/) |
| **HUB** | [dv-hub](https://dv-hub.lithium.primaverabss.com/) | [st-hub](https://st-hub.lithium.primaverabss.com/) | [hub-preview](https://hub-preview.lithium.primaverabss.com/) | [hub](https://hub.lithium.primaverabss.com/) |
| **NS** | [dv-ns](https://dv-ns.lithium.primaverabss.com/) | [st-ns](https://st-ns.lithium.primaverabss.com/) | [ns-preview](https://ns-preview.lithium.primaverabss.com/) | [ns](https://ns.lithium.primaverabss.com/) |
| **NTR** | ??? | ??? | ??? | ??? |
| **OBS** | ??? | ??? | ??? | ??? |
| **PNS** | [dv-pns](https://dv-pns.lithium.primaverabss.com/) | [st-pns](https://st-pns.lithium.primaverabss.com/) | [pns-preview](https://pns-preview.lithium.primaverabss.com/) | [pns](https://pns.lithium.primaverabss.com/) |
| **SS** | [dv-ss](https://dv-ss.lithium.primaverabss.com/) | [st-ss](https://st-ss.lithium.primaverabss.com/) | [ss-preview](https://ss-preview.lithium.primaverabss.com/) | [ss](https://ss.lithium.primaverabss.com/) |
| **TAXPT** | [dv-taxpt](https://dv-taxpt.lithium.primaverabss.com/) | [st-taxpt](https://st-taxpt.lithium.primaverabss.com/) | [taxpt-preview](https://taxpt-preview.lithium.primaverabss.com/) | [taxpt](https://taxpt.lithium.primaverabss.com/) |
| **TBX** | [dv-tbx](https://dv-tbx.lithium.primaverabss.com/) | [st-tbx](https://st-tbx.lithium.primaverabss.com/) | [tbx-preview](https://tbx-preview.lithium.primaverabss.com/) | [tbx](https://tbx.lithium.primaverabss.com/) |