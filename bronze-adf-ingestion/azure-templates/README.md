# Azure Data Factory Pipeline Template

This folder contains the exported Azure Data Factory pipeline configuration.

## Files

- **dy_git_to_raw.json** - Main pipeline ARM template
- **manifest.json** - Pipeline metadata and visual representation

## Pipeline Components

### Activities
1. **LookupGit** - Reads git.json configuration from ADLS Gen2
2. **ForEachGit** - Iterates through each dataset
3. **Copy data1** - Copies data from GitHub HTTP source to ADLS Gen2 sink

### Datasets
- `ds_git_parameters` - Configuration JSON dataset
- `ds_git_dynamic` - Dynamic HTTP source dataset
- `ds_sink_dynamic` - Dynamic ADLS Gen2 sink dataset

### Parameters (Environment-Specific)
- `Data_lake_storage_1` - ADLS Gen2 Linked Service name
- `http_linked_service1` - HTTP Linked Service name

## Deployment

```bash
# Deploy using Azure CLI
az deployment group create \
  --resource-group <your-rg> \
  --template-file dy_git_to_raw.json \
  --parameters factoryName=<your-adf-name> \
               Data_lake_storage_1=<adls-linked-service> \
               http_linked_service1=<http-linked-service>
```

## Configuration

Update `.env` file with your Azure credentials before deployment.
