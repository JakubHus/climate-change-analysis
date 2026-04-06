# 📈 Climate Change Analysis - Southern Białystok County, Poland (2014-2025) 🌍

Descriptive and diagnostic **analysis of climate** change based on **ERA5** reanalysis data (Copernicus Climate Data Store / ECMWF). 
The project covers **data exploration**, **temperature trend analysis**, **assessment of extreme weather events** and investigation of **relationships between climate variables**.

## Objectives 🎯
- Identify long-term annual and seasonal air temperature **trends**
- Assess **changes in the frequency and intensity** of extreme temperature events
- **Analyse relationships between** air temperature, surface energy balance, and soil moisture
- Evaluate the potential of **machine learning methods** for forecasting climate indicators

## Data 📁
| Parameter | Value |
|---|---|
| Source | ERA5 — Copernicus Climate Data Store (ECMWF) |
| Format | GRIB (`.grib`) |
| Period | 01 Jan 2014 – 05 Dec 2025 |
| Location | Southern Białystok County, NE Poland (~53°N, 23–23.25°E) |
| Spatial resolution | ~0.25° × 0.25° (2 grid points) |
| Temporal resolution | 2 synoptic steps per day: 05:00 and 12:00 UTC |

## Variables 🔢

**Instantaneous (`stepType=instant`):** 2 m air temperature (*t2m*), 2 m dew point temperature (*d2m*), skin temperature (*skt*), 10 m wind components (*u10*, *v10*), volumetric soil water layer 2 (*swvl2*)

**Accumulated (`stepType=accum`):** total precipitation (*tp*), surface solar radiation downwards (*ssr*), snowfall (*sf*)

## Methodology 🧪

### Data preparation
- Loading and separating instantaneous and accumulated variables (`xarray` + `cfgrib`)
- Unit conversion: K → °C, m → mm, J/m² → MJ/m²
- Wind speed calculated from U and V components
- Physical range validation for all variables
- Missing value removal

### Spatio-temporal aggregation
- Daily mean from two UTC steps → spatial mean across two grid points
- Monthly sum of precipitation and radiation → spatial mean

### Trend analysis
- OLS linear regression (scipy `linregress`) on annual and seasonal series
- Meteorological seasons: DJF / MAM / JJA / SON

### Temperature extremes
- Definition: very warm days (>P90) and very cold days (<P10) based on daily T2m series
- Trend analysis of frequency, mean temperature, and threshold exceedance intensity

### Dependency analysis
- Monthly anomalies (deviation from the multi-year monthly mean) to control for seasonality
- Pearson and Spearman correlations
- Multiple OLS regression with HAC-robust standard errors (`statsmodels`)

## Conclusions ✅

The analysis reveals a **marked warming of the winter season** in the study area. The association between **elevated temperatures** and **soil moisture** and **precipitation deficits** suggests *increased susceptibility of the region to **soil drought** episodes*. The lack of statistical significance for most trends is an inherent limitation of the **relatively short analysis period** (*11 years*) combined with the local scale of ERA5 reanalysis data.

## ML Potential 🤖

The stable relationships identified at the monthly anomaly level open the possibility of applying **predictive models** (e.g. *Random Forest*) to **forecast temperature anomalies or drought risk**. Proposed predictors include anomalies of SSR, soil moisture, dew point temperature, and precipitation.

## Stack 🛠️

| Library | Purpose |
|---|---|
| `xarray` + `cfgrib` | Loading and processing GRIB data |
| `pandas` + `numpy` | Data manipulation and aggregation |
| `geopandas` + `folium` | Spatial visualisation, interactive maps |
| `matplotlib` + `seaborn` | Charts and histograms |
| `scipy` | Linear regression, Pearson and Spearman correlations |
| `statsmodels` | Multiple OLS regression with HAC correction |
