# Data Description: Climate Forecast System (CFS) version 2

Our project uses archived weather forecast data from the **Climate Forecast System (CFS) version 2**. These datasets contain a variety of meteorological variables critical for our analysis and model training. Below, we describe the specific variables we use for this project and provide details on accessing and using these datasets on both the U-M HPC and Google Cloud Storage (GCS).

-----

## Specific Variables Used in the Project

| **Variable Name** | **Unit** | **Description** |
| :--- | :--- | :--- |
| **SHTFL\_surface** | W/m^2 | **Sensible Heat Net Flux** at surface |
| **LHTFL\_surface** | W/m^2 | **Latent Heat Net Flux** at surface |
| **TMP\_2maboveground** | K | **Temperature** 2m above ground |
| **APCP\_surface** | kg/m^2 | **Total Precipitation** at surface |

-----

## Data Locations and Structure

The project data is available in two distinct storage environments: a restricted **U-M HPC** mount for internal processing and a public **Google Cloud Storage (GCS)** bucket optimized for cloud-native workflows.

### 1\. U-M HPC Storage (Restricted Access) 🔒

This location is for users with **direct access to the U-M computing environment** and the necessary permissions for the shared "turbo" drive. The data is organized into forecasts by date.

#### Data Location

Our data is stored on the shared "turbo" drive in the following directory:
**`/nfs/turbo/seas-dannes/urop-2024-bias/cfs-forecasts/archive/`**

#### Directory Structure (U-M HPC)

The data is organized into directories by date (e.g., `20120101`), with each directory containing four forecasts (`0000`, `0600`, `1200`, and `1800` hours).

  - **`flxf` files** contain: `SHTFL_surface`, `LHTFL_surface`, and `TMP_2maboveground`.
  - **`pgbf` files** contain: `APCP_surface`.

### 2\. Google Cloud Storage (GCS) Bucket (Public Access) 🌐

This is the recommended location for users accessing the data **without U-M HPC access** or for cloud-native, scalable analysis (e.g., Dask, Zarr).

#### Data Location

The entire dataset is housed in the following bucket:
**`gs://great-lakes-osd/`**

#### Directory Structure (GCS)

The GCS data is optimized for cloud-reading. Instead of being grouped by forecast initialization date, the files are grouped by **variable** and further split by their native coordinate dimensions.

**Root Path:** `gs://great-lakes-osd/cfs-data/`

| Variable Group | Variable | Full GCS Path |
| :--- | :--- | :--- |
| **Precipitation** (`precip/`) | `APCP_surface` | `gs://great-lakes-osd/cfs-data/precip/APCP_surface/` |
| **Temperature/Flux** (`temp-vars/`) | `LHTFL_surface` | `gs://great-lakes-osd/cfs-data/temp-vars/LHTFL_surface/` |
| **Temperature/Flux** (`temp-vars/`) | `SHTFL_surface` | `gs://great-lakes-osd/cfs-data/temp-vars/SHTFL_surface/` |
| **Temperature/Flux** (`temp-vars/`) | `TMP_2maboveground` | `gs://great-lakes-osd/cfs-data/temp-vars/TMP_2maboveground/` |

**Path Structure within each Variable Directory (Optimized for Cloud Reading):**

```
.../APCP_surface/
├── latitude/
├── lead/
├── longitude/
└── time/
```

-----

## Accessing Data

### Accessing Data on U-M HPC

Use the standard path provided above in your scripts.

#### Example Python Script for Accessing and Visualizing Data (U-M HPC)

*(The rest of your existing HPC-specific Python code block goes here)*

-----

### Accessing Data on Google Cloud Storage (GCS) with Zarr

The data in the GCS bucket is structured to be read efficiently using the **Zarr** format, which is a specification for storing N-dimensional arrays in a cloud-optimized way. It allows you to **read small chunks of a large dataset without downloading the entire file**.

#### Key Concept: Zarr for Cloud-Native Analysis

Unlike the NetCDF files on the HPC, which must be read sequentially, Zarr breaks the data into smaller, compressed "chunks." When you access a variable (e.g., `SHTFL_surface`) in the GCS path, you are accessing a **Zarr store**, which acts like a single, virtual NetCDF file that can be accessed in parallel.

#### Example using Python to Load a Zarr Store

To read the Zarr data directly from GCS, you typically use `gcsfs` (to handle the cloud file system) and `xarray` (to open the data).

```python
# Installation (if needed):
# !pip install xarray gcsfs

import xarray as xr
import gcsfs

# Initialize the GCS File System
fs = gcsfs.GCSFileSystem()

# Define the full path to one of the variable-specific Zarr stores
zarr_store_path = 'gs://great-lakes-osd/cfs-data/temp-vars/TMP_2maboveground/'

# Open the Zarr store directly as an xarray Dataset
# 'r' mode means read-only
# The zarr_store_path points to the root of the Zarr directory
try:
    ds_tmp = xr.open_zarr(fs.get_mapper(zarr_store_path), consolidated=True)
    print("Successfully opened 2m Temperature Zarr store.")
    print(ds_tmp)
except Exception as e:
    print(f"Error opening Zarr store: {e}")
```

This method allows you to load only the required data chunks, making it highly efficient for massive climate datasets.
