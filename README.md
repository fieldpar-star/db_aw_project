# AdventureWorks Data Project

A cloud data engineering project using Azure Data Factory, Databricks, Synapse Analytics and Power BI.

## Architecture

```
┌─────────────┐     ┌──────────────────┐     ┌────────────────────────────────────┐     ┌──────────┐     ┌──────────┐
│ Data Source │────▶│  Data Ingestion   │────▶│           Transformation            │────▶│ Serving  │────▶│Reporting │
│   (HTTP)    │     │  (Data Factory)   │     │                                    │     │(Synapse) │     │(Power BI)│
└─────────────┘     └────────┬─────────┘     │  ┌────────────────────────────┐    │     └──────────┘     └──────────┘
                             │               │  │    Transformed Data         │    │
                             ▼               │  │     (Data Lake Gen2)        │    │
                    ┌─────────────────┐      │  └────────────────────────────┘    │
                    │  Raw Data Store │      │             ▲                       │
                    │ (Data Lake Gen2)│─────▶│        Databricks                  │
                    └─────────────────┘      │     (Transformation)               │
                                            └────────────────────────────────────┘
```

### Pipeline Flow

| Step | Service | Description |
|---|---|---|
| 1 | **HTTP Source** | Raw data ingested from HTTP endpoints |
| 2 | **Azure Data Factory** | Orchestrates data ingestion pipelines |
| 3 | **Raw Data Lake Gen2** | Stores raw/bronze layer data |
| 4 | **Azure Databricks** | Transforms and processes raw data (silver layer) |
| 5 | **Transformed Data Lake Gen2** | Stores cleaned/transformed data (gold layer) |
| 6 | **Azure Synapse** | Serves transformed data for analytics |
| 7 | **Power BI** | Reporting and visualisation |

## Repository Structure

```
db_aw_project/
├── infrastructure/            # ARM templates for Azure deployment
│   ├── template.json
│   └── parameters.json
├── synapse/                   # Synapse pipelines, notebooks, datasets
├── databricks/                # Databricks transformation notebooks
└── .github/
    └── workflows/
        └── deploy.yml         # GitHub Actions CI/CD pipeline
```

## Deployed Resources

| Resource | Name | Region |
|---|---|---|
| Synapse Workspace | `awprojectsynapseblooms` | West US 2 |
| Storage Account (ADLS Gen2) | `defaultsynapsestorblo3` | West US 2 |
| Blob Container | `defaultfilesystem` | West US 2 |

## CI/CD Pipeline

GitHub Actions automatically runs on every push to `main` or `db_feature`:

1. **Validate** — validates the ARM template against Azure
2. **Deploy** — deploys to Azure (only on pushes to `main`)

### Required GitHub Secrets

| Secret | Description |
|---|---|
| `AZURE_CREDENTIALS` | Service principal JSON for Azure login |
| `AZURE_SUBSCRIPTION_ID` | Azure subscription ID |
| `SQL_ADMIN_PASSWORD` | Synapse SQL admin password |

## Branches

| Branch | Purpose |
|---|---|
| `main` | Production — triggers full deployment |
| `db_feature` | Databricks feature development |

## Azure Details

- **Subscription:** Azure subscription 1
- **Resource Group:** AdventureWorksProject
- **Region:** West US 2
