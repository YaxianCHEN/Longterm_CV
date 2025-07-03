# Longterm_CV

# Soil Respiration Trend Detectability Under Observational Uncertainty

This repository provides R scripts and functions for quantifying how observational errors affect the detectability of significant long-term trends in soil respiration (Rs) and heterotrophic respiration (Rh) across global gridded datasets (e.g., Hashimoto et al. 2015).

## Key Features

- **Automated grid-wise linear trend analysis** for global Rs/Rh datasets.
- **Stepwise fitting**: For each grid cell, fits time series progressively to identify the minimum number of years required for a statistically significant trend.
- **Error propagation**: Simulates observational errors (coefficient of variation, CV) and quantifies their impact on spatial detectability of significant increases.
- **Batch plotting functions**: Includes standardized functions for mapping, pie chart summarization, and sensitivity curves (e.g., CV vs. percentage of significant area).
- **Reproducible outputs**: All code modular, facilitating adaptation to other large-scale biogeochemical datasets.

## Workflow Overview

1. **Data Preparation**  
   - Read gridded respiration data (NetCDF format) for Rs or Rh.
   - Extract longitude, latitude, and annual time series.

2. **Trend Analysis with Error Simulation**  
   - For each grid cell, simulate observation error via random multiplicative noise (`fuzz_seq`).
   - Apply progressive linear fitting; record when a significant trend (p < 0.05) is first detected.
   - Repeat for a range of CVs (e.g., 0–0.19).

3. **Result Aggregation & Visualization**  
   - Calculate the spatial ratio of significantly increasing areas under each CV.
   - Generate summary plots:  
     - Sensitivity curves (CV vs. detection ratio)  
     - Spatial maps and pie charts of trend types  
     - Histograms of first significant year distributions

## Quick Start

### 1. Environment

- R version ≥ 4.1
- Key packages: `tidyverse`, `ncdf4`, `sf`, `cowplot`, `scales`, `tibble`, `dplyr`, `ggplot2`

### 2. Example Usage

```r
# Load and preprocess NetCDF data
# nc_RH <- nc_open("data/extdata/RH_yr_Hashimoto2015.nc")
# co2_RH <- ncvar_get(nc_RH, "co2", start = c(1,1,1,1), count = c(-1,-1,1,112))
# lon_RH <- ncvar_get(nc_RH, "lon")
# lat_RH <- ncvar_get(nc_RH, "lat")
# nc_close(nc_RH)

# Run main analysis
results_df <- process_and_analyze_data(co2_RH, lon_RH, lat_RH)
write.csv(results_df, "RH_ratios.csv", row.names = FALSE)

# Plot sensitivity curve
df_RH <- read.csv("RH_ratios.csv")
plot_inc_sig_ratio_curve(df_RH)
