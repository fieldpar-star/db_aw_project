# AdventureWorks Data Project

A cloud data engineering project using Azure Synapse Analytics and Azure Databricks, deployed on Microsoft Azure.

## Architecture

```
AdventureWorksProject (Resource Group)
├── Azure Synapse Analytics    # Data integration, pipelines, SQL pools
├── Azure Databricks           # Data transformation and analytics
└── ADLS Gen2 Storage          # Data lake storage
```

## Repository Structure

```
db_aw_project/
├── infrastructure/            # ARM templates for Azure deployment
│   ├── template.json
│   └── parameters.json
├── synapse/                   # Synapse pipelines, notebooks, datasets
├── databricks/                # Databricks notebooks and jobs
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

## Getting Started

### Prerequisites
- Azure CLI
- Git
- Access to the `AdventureWorksProject` resource group

### Deploy Infrastructure

```bash
az deployment group create \
  --resource-group AdventureWorksProject \
  --template-file infrastructure/template.json \
  --parameters infrastructure/parameters.json \
  --parameters sqlAdministratorLoginPassword='<password>'
```

### Connect to Synapse

Open Synapse Studio at:
```
https://web.azuresynapse.net
```
Select workspace `awprojectsynapseblooms`.

## Azure Subscription

- **Subscription:** Azure subscription 1
- **Resource Group:** AdventureWorksProject
- **Region:** West US 2
