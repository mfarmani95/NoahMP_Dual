
[![DOI](https://img.shields.io/badge/DOI-10.5281%2Fzenodo.14740700-blue)](https://doi.org/10.5281/zenodo.14740700)

# Noah-MP (Fortran) – Research Version

This repository contains a **Fortran-based implementation of the Noah-MP Land Surface Model (LSM)** used in the following studies:

- ESS Open Archive Preprint: https://essopenarchive.org/doi/full/10.22541/essoar.172286649.90332939/v1  
- Hydrology and Earth System Sciences (HESS, 2025): https://hess.copernicus.org/articles/29/547/2025/  

This version represents a **research-oriented modification of Noah-MP**, developed to improve the representation of key hydrological processes, particularly in **soil moisture dynamics, runoff generation, and subsurface flow**.

---

## 🔬 Key Model Developments

Compared to the standard Noah-MP implementation (e.g., NCAR version), this repository includes several important enhancements:

### 1. Mixed-Form Richards Equation Solver
- Implements a **mixed-form solution of Richards equation** for soil moisture dynamics  
- Improves numerical stability and physical consistency  
- Enhances representation of **unsaturated flow and soil moisture redistribution**

### 2. Van Genuchten (VG) Parameterization
- Uses **Van Genuchten soil hydraulic functions** for:
  - Soil water retention  
  - Hydraulic conductivity  
- Provides a more physically realistic description of soil hydraulic processes  
- *Note: availability of VG parameterization may differ across Noah-MP versions; this implementation ensures consistent use within the solver framework.*

### 3. Surface Ponding Representation
- Introduces **explicit surface ponding storage**  
- Allows accumulation of water when infiltration capacity is exceeded  
- Improves simulation of:
  - Saturation-excess runoff  
  - Surface water dynamics  

### 4. Macropore / Preferential Flow
- Includes representation of **macropore flow pathways**  
- Enables rapid vertical transport beyond matrix flow assumptions  
- Improves representation of:
  - Infiltration-excess runoff  
  - Deep percolation and groundwater recharge  

---

## ⚖️ Comparison with Standard Noah-MP (NCAR)

| Feature | NCAR Noah-MP (typical) | This Version |
|--------|------------------------|-------------|
| Richards Solver | Pressure-head / simplified | **Mixed-form solver** |
| Soil Hydraulics | Lookup / simplified | **Van Genuchten formulation** |
| Ponding | Limited / implicit | **Explicit ponding storage** |
| Preferential Flow | Not explicitly represented | **Macropore flow included** |
| Hydrologic Focus | General LSM | **Enhanced process-based hydrology** |

---

## 📁 Repository Structure

### `Noah_code/`
Core Noah LSM source code:
- `module_Noahlsm.F`
- `module_Noahlsm_param_init.F`
- `module_date_utilities.F`
- `module_Noahlsm_utility.F`

### `IO_code/`
Input/output handling modules:
- `module_Noah_NC_output.F`
- `module_Noahlsm_gridded_input.F`
- `Noahlsm_driver.F` – main model driver

### `Run/`
Execution configuration:
- `Noah_offline.namelist` – model configuration file
- Compiled executable (`./Noah`) is generated here

### `Noah_data/`

#### `forcings/`
- Forcing data reprocessed from GLDAS (NetCDF format)

#### `results/`
Simulation outputs:
- `exp1/hrly/` – hourly outputs (e.g., runoff)
- `exp1/hist/` – monthly mean outputs
- `exp1/ini/` – end-of-year states

#### `static/`
Static input datasets (1° resolution):
- `gvf.nc` – vegetation fraction
- `landmask.nc` – land-sea mask
- `plotmask.nc` – grid mapping
- `tbot.nc` – deep soil temperature (~8 m)
- `lon_lat.nc` – grid coordinates
- `soilcolor.nc` – soil color index (1–8)
- `veg_soil.nc` – vegetation type & soil texture index
- `rawdata/` – higher/lower resolution source datasets

---

## ⚙️ How to Run

### 1. Compile the Model
```bash
make
```
If compilation fails:

- Modify the `Makefile` for your system

- Ensure required compilers and libraries are available

---

### 2. Configure

Edit the namelist file:

`Run/Noah_offline.namelist`

Update settings such as:

- Simulation period  

- Input/output paths  

- Physics options  

- Experiment configuration  

---

### 3. Run

From the `Run/` directory:

```bash

./Noah

```
### 4. Output

Simulation results will be written to:
```bash

`Noah_data/results/`

```
---

## Requirements

This model is intended for use in a Unix/Linux environment and typically requires:

- A Fortran compiler (e.g., `gfortran` or `ifort`)

- Standard build tools such as `make`

- NetCDF libraries (if enabled in the build and I/O configuration)

---

## Notes

- This repository is a research archive and may differ from official Noah-MP releases, including the NCAR version.

- The modifications in this repository were developed for process-based hydrological investigations and scientific analysis, rather than operational forecasting.

- Users should carefully review the model structure, assumptions, and configuration before applying it to other studies.

---

## Acknowledgment

We acknowledge all contributors involved in the development of this code.

---

## In Memoriam

Our thoughts and prayers are with **Jetal Agnihotri**, who was also involved in the development of this code.

