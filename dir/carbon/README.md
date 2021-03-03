# Services Directory - Carbon

These services are part of the Carbon Framework (Intelligent ERP).

| Microservice | Name | Description | Scopes | Client Lib | Web API | Spec |
| - | - | - | - | - | - | - |
| **CDS** | Canonical Data Service | Allows retrieving data from the canonical data model (entities, documents, etc.) | `lithium-canonicaldataservice` | n/a | n/a | n/a |
| **DAS** | Digital Archive Service | Allows storing documents in the digital archive | `lithium-digitalarchiveservice` `lithium-digitalarchiveservice-crr` `lithium-digitalarchiveservice-drr` | n/a | n/a | [1.0](./specs/das-spec-1.0.md) |
| **DCS** | Data Catalog Service | Allows retrieving reference data (currencies, countries, etc.) | `lithium-datacatalogservice` | [2.0](https://dcs.lithium.primaverabss.com/.doc/clientlib) | [2.0](https://dcs.lithium.primaverabss.com/.doc/webapi) | n/a |
| **DIS** | Data Ingestion Service | Allows ingesting data files (e.g. SAF-T) to the Data Lake | `lithium-dataingestionservice` | [1.0](https://dis.lithium.primaverabss.com/.doc/clientlib) | [1.0](https://dis.lithium.primaverabss.com/.doc/clientlib) | [1.0](./specs/dis-spec-1.0.md) |
| **MTR** | Metrics Service | Allows storing and producing metrics | `lithium-metrics` | n/a | n/a | n/a |

## Endpoints

| Service | Development | Staging | Preview | Production |
| - | - | - | - | - |
| **CDS** | [dv-cds](https://dv-cds.lithium.primaverabss.com/) | [st-cds](https://st-cds.lithium.primaverabss.com/) | [cds-preview](https://cds-preview.lithium.primaverabss.com/) | [cds](https://cds.lithium.primaverabss.com/) |
| **DAS** | [dv-das](https://dv-das.lithium.primaverabss.com/) | [st-das](https://st-das.lithium.primaverabss.com/) | [das-preview](https://das-preview.lithium.primaverabss.com/) | [das](https://das.lithium.primaverabss.com/) |
| **DCS** | [dv-dcs](https://dv-dcs.lithium.primaverabss.com/) | [st-dcs](https://st-dcs.lithium.primaverabss.com/) | [dcs-preview](https://dcs-preview.lithium.primaverabss.com/) | [dcs](https://dcs.lithium.primaverabss.com/) |
| **DIS** | [dv-dis](https://dv-dis.lithium.primaverabss.com/) | [st-dis](https://st-dis.lithium.primaverabss.com/) | [dis-preview](https://dis-preview.lithium.primaverabss.com/) | [dis](https://dis.lithium.primaverabss.com/) |
| **MTR** | [dv-mtr](https://lithium-dv-mtr.azurewebsites.net/) | [st-mtr](https://st-lithium-carbonmtr.primaverabss.com/) | ??? | [mtr](https://lithium-carbonmtr.primaverabss.com/) |