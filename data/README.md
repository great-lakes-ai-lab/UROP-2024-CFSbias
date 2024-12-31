# Data Description: Climate Forecast System (CFS) version 2

Our project uses archived weather forecast data from the Climate Forecast System (CFS) version 2. These datasets contain a variety of meteorological variables critical for our analysis and model training. Below, we describe the specific variables we will use for this project and provide additional details on accessing and using these datasets on the U-M HPC.

## Specific Variables Used in the Project

| **Variable Name**                | **Unit**          | **Description**                               |
|----------------------------------|-------------------|-----------------------------------------------|
| SHTFL_surface                    | W/m<sup>2</sup>   | Sensible Heat Net Flux at surface             |
| LHTFL_surface                    | W/m<sup>2</sup>   | Latent Heat Net Flux at surface               |
| TMP_2maboveground                | K                 | Temperature 2m above ground                   |
| APCP_surface                     | kg/m<sup>2</sup>  | Total Precipitation at surface                |

## Data Structure on U-M HPC

### Directory Structure
The data is organized into directories by date, with each directory containing four forecasts produced at 0000, 0600, 1200, and 1800 hours. Each forecast extends nine months into the future. The filenames reflect the date and time when the forecast was produced.

Directory structure:
```
/nfs/turbo/seas-dannes/urop-2024-bias/cfs-forecasts/archive/
├── 20120101
│ ├── flxf.01.2012010100.allmonths.cnbs.nc
│ ├── flxf.01.2012010106.allmonths.cnbs.nc
│ ├── flxf.01.2012010112.allmonths.cnbs.nc
│ ├── flxf.01.2012010118.allmonths.cnbs.nc
│ ├── pgbf.01.2012010100.allmonths.cnbs.nc
│ ├── pgbf.01.2012010106.allmonths.cnbs.nc
│ ├── pgbf.01.2012010112.allmonths.cnbs.nc
│ ├── pgbf.01.2012010118.allmonths.cnbs.nc
├── 20120929
│ ├── flxf.01.2012092900.allmonths.cnbs.nc
│ ├── flxf.01.2012092906.allmonths.cnbs.nc
│ ├── flxf.01.2012092912.allmonths.cnbs.nc
│ ├── flxf.01.2012092918.allmonths.cnbs.nc
│ ├── pgbf.01.2012092900.allmonths.cnbs.nc
│ ├── pgbf.01.2012092906.allmonths.cnbs.nc
│ ├── pgbf.01.2012092912.allmonths.cnbs.nc
│ ├── pgbf.01.2012092918.allmonths.cnbs.nc
...
```

Each forecast directory follows this pattern and is named after the date on which the forecasts were produced.

#### Accessing Data on U-M HPC

#### Data Location
Our data is stored on the shared "turbo" drive in the following directory:
`/nfs/turbo/seas-dannes/urop-2024-bias/cfs-forecasts/archive/`

#### Example Python Script for Accessing and Visualizing Data
The following Python script provides an example of how to access, load, and visualize data using the `xarray` library, which is well-suited for handling multi-dimensional arrays and NetCDF files:

```python
# Install required packages
!pip install --upgrade pip
!pip install numpy cftime xarray

import xarray as xr
import os
import matplotlib.pyplot as plt

# Define the path to your forecast data directory
data_path = '/nfs/turbo/seas-dannes/urop-2024-bias/cfs-forecasts/archive/20120101/'

# List all files in the directory
files = os.listdir(data_path)
print("Files in directory:", files)

# Filter files to identify one `flxf` and one `pgbf` file
flxf_files = [f for f in files if 'flxf' in f]
pgbf_files = [f for f in files if 'pgbf' in f]

# Print identified files
print("FLX Files:", flxf_files)
print("PGP Files:", pgbf_files)

# Select one `flxf` and one `pgbf` file to inspect
flxf_file = os.path.join(data_path, flxf_files[0])
pgbf_file = os.path.join(data_path, pgbf_files[0])

print(f"Selected FLXF file: {flxf_file}")
print(f"Selected PGBF file: {pgbf_file}")

# Load the datasets
flxf_ds = xr.open_dataset(flxf_file)
pgbf_ds = xr.open_dataset(pgbf_file)

# Print dataset summaries
print(flxf_ds)
print(pgbf_ds)
```

### Example output from loading datasets with `xarray`:

**FLXF Dataset:**

```plaintext
<xarray.Dataset> Size: 8MB
Dimensions:            (time: 10, latitude: 181, longitude: 360)
Coordinates:
  * time               (time) datetime64[ns] 80B 2012-01-01T18:00:00 ... 2012...
  * latitude           (latitude) float64 1kB -90.0 -89.0 -88.0 ... 89.0 90.0
  * longitude          (longitude) float64 3kB 0.0 1.0 2.0 ... 357.0 358.0 359.0
Data variables:
    SHTFL_surface      (time, latitude, longitude) float32 3MB ...
    LHTFL_surface      (time, latitude, longitude) float32 3MB ...
    TMP_2maboveground  (time, latitude, longitude) float32 3MB ...
Attributes:
    Conventions:          COARDS
    History:              Tue Oct 22 13:00:20 2024: /usr/bin/ncrcat -O flxf.0...
    GRIB2_grid_template:  0
    NCO:                  netCDF Operators version 4.8.1 (Homepage = http://n...
```

**PGBF Dataset:**

```plaintext
<xarray.Dataset> Size: 3MB
Dimensions:       (time: 10, latitude: 181, longitude: 360)
Coordinates:
  * time          (time) datetime64[ns] 80B 2012-01-01 2012-02-01 ... 2012-10-01
  * latitude      (latitude) float64 1kB -90.0 -89.0 -88.0 ... 88.0 89.0 90.0
  * longitude     (longitude) float64 3kB 0.0 1.0 2.0 3.0 ... 357.0 358.0 359.0
Data variables:
    APCP_surface  (time, latitude, longitude) float32 3MB ...
Attributes:
    Conventions:          COARDS
    History:              Tue Oct 22 12:58:53 2024: /usr/bin/ncrcat -O pgbf.0...
    GRIB2_grid_template:  0
    NCO:                  netCDF Operators version 4.8.1 (Homepage = http://n...
```

### Visualizing Data
Here's how you can visualize specific variables in the datasets:

```python
import matplotlib.pyplot as plt

# Define function to plot data variable
def plot_variable(ds, var_name, time_index=0):
    if var_name in ds.variables:
        data_var = ds[var_name].isel(time=time_index)
        data_var.plot()
        plt.title(f'{var_name} at Time Step {time_index}')
        plt.show()
    else:
        print(f"{var_name} not found in dataset")

# Example to visualize sensible heat flux from FLXF file
plot_variable(flxf_ds, 'SHTFL_surface')

# Example to visualize latent heat flux from FLXF file
plot_variable(flxf_ds, 'LHTFL_surface')

# Example to visualize 2m temperature from FLXF file
plot_variable(flxf_ds, 'TMP_2maboveground')

# Example to visualize accumulated precipitation from PGBF file
plot_variable(pgbf_ds, 'APCP_surface')
```

You can also plot time series of a variable at specific locations:

```python
# Function to plot time series for a variable at a specific location
def plot_time_series(ds, var_name, latitude, longitude):
    if var_name in ds.variables:
        data_var = ds[var_name].sel(latitude=latitude, longitude=longitude, method='nearest')
        data_var.plot()
        plt.title(f'Time Series of {var_name} at Lat: {latitude}, Lon: {longitude}')
        plt.xlabel('Time')
        plt.ylabel(var_name)
        plt.show()
    else:
        print(f"{var_name} not found in dataset")

# Example time series for latent heat flux at specific location
lat, lon = 45, -90  # Replace with your desired latitude and longitude
plot_time_series(flxf_ds, 'LHTFL_surface', lat, lon)

# Example time series for 2m temperature at specific location
plot_time_series(flxf_ds, 'TMP_2maboveground', lat, lon)

# Example time series for accumulated precipitation at specific location
plot_time_series(pgbf_ds, 'APCP_surface', lat, lon)
```
