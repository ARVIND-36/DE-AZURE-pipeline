# Bronze Layer - ADF Ingestion

Configuration-driven Azure Data Factory pipeline ingesting raw data from GitHub to ADLS Gen2.

## Pipeline Architecture

![Pipeline Design](./azure-templates/adf-pipeline.png)

**Activity Flow:**
1. **Lookup Activity** - Reads `git.json` config file from blob storage, returns array of dataset objects
2. **ForEach Activity** - Iterates through config array with parallel execution enabled
3. **Copy Activity** - Performs HTTP GET from GitHub raw URL, writes to ADLS Gen2 sink

**Dynamic Parameterization:**
```javascript
// Source URL construction
@concat('https://raw.githubusercontent.com/', item().p_rel_url)

// Sink path construction  
@concat('/data/raw/', item().p_sinkk_folder, '/', item().p_sink_file)
```

## Components

**ARM Templates** (`azure-templates/`)
- `dy_git_to_raw.json` - Pipeline definition with linked services and datasets
- Parameterized for environment-specific deployment

**Configuration** (`git.json`)
- JSON array defining source URLs and sink paths
- Each entry creates one copy operation in ForEach loop

**Sample Data** (`Data/`)
- 10 AdventureWorks CSV files for reference/testing

## Output Structure

![Storage Container](./azure-templates/data-lake-structure.png)

Data lands in bronz/ container with folder-per-dataset organization. Ready for downstream Silver layer transformations.

## Technical Details

**Linked Services:**
- HTTP (GitHub raw content API)
- ADLS Gen2 (hierarchical namespace enabled)
- Blob Storage (config file location)

**Datasets:**
- `ds_git_parameters` - JSON config source
- `ds_git_dynamic` - Parameterized HTTP source
- `ds_sink_dynamic` - Parameterized ADLS sink
