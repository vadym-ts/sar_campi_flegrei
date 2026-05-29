# 🛰️ PyGMTSAR — SBAS + PSI Pipeline
## Campi Flegrei / Naples · Dual-Orbit Decomposition

Built on [Pechnikov's PyGMTSAR](https://github.com/AlexeyPechnikov/pygmtsar) (2025.x API).
*README auto-generated 2026-05-29.*

---

## Overview

Complete Sentinel-1 InSAR processing pipeline for the Campi Flegrei volcanic area (Naples, Italy):

- **SBAS** (Small Baseline Subset) time-series analysis
- **PSI** (Persistent Scatterer Interferometry) time-series analysis
- **Dual-orbit decomposition** — ascending + descending LOS → vertical + east-west displacement

![PSI Decomposed Displacement](images/psi_decomposition_last.png)

---

## Area of Interest

| Parameter | Value |
|-----------|-------|
| Location  | Campi Flegrei caldera, Naples, Italy |
| Min lon   | 14.0233° |
| Max lon   | 14.2155° |
| Min lat   | 40.7787° |
| Max lat   | 40.8624° |

---

## Input Data

### Sentinel-1 Scenes

| Orbit | Scenes | Pairs | Date range |
|-------|--------|-------|------------|
| Descending | 30 | 14 | 2022-01-10 → 2022-12-12 |
| Ascending  | 30 | 14 | 2022-01-11 → 2022-12-13 |

- **Data directory (desc)**: `data_naples_d`
- **Data directory (asc)**: `data_naples_a`
- **Reference date (desc)**: `2022-01-10`
- **Reference date (asc)**: `2022-01-11`
- **Reference point**: lon=14.1965, lat=40.8108
- **Subswath**: 1 (desc) / 1 (asc)

### DEM

- **File**: `data_naples_d/dem.nc`
- **Source**: Copernicus DEM (via PyGMTSAR Tiles)

### Landmask

- **File**: `data_naples_d/landmask.nc`
- **Source**: GSHHG via PyGMTSAR

---

## Processing Epochs

15 dates from 2022-01-10 to 2022-12-12:

2022-01-10, 2022-02-03, 2022-02-27, 2022-03-23, 2022-04-16, 2022-05-10, 2022-06-03, 2022-06-27, 2022-07-21, 2022-08-14, 2022-09-07, 2022-10-01, 2022-10-25, 2022-11-18, 2022-12-12

---

## Results

### PSI Velocity Range

| Orbit | p1% | p99% |
|-------|-----|------|
| Descending LOS | -67.1 mm/yr | 65.7 mm/yr |
| Ascending LOS  | -68.0 mm/yr | 67.1 mm/yr |

### Decomposed Displacement (last date: 2022-12-12)

| Component | p1% | p99% |
|-----------|-----|------|
| Vertical  | -50.7 mm | 55.8 mm |
| East-West | -69.1 mm   | 72.2 mm   |

![Displacement all dates](images/preview_displacement_all_dates.png)

---

## Outputs

| File | Size (MB) | Description |
|------|-----------|-------------|
| `.ipynb_checkpoints` | 0.0 | |
| `preview_correlation.png` | 0.9 | |
| `preview_displacement_all_dates.png` | 1.4 | |
| `preview_interferograms.png` | 0.5 | |
| `psi_decomposed_displacement.zip` | 541.7 | |
| `psi_eastwest_displacement.tif` | 271.1 | |
| `psi_vertical_displacement.tif` | 270.5 | |


- `psi_vertical_displacement.tif` — multi-band GeoTIFF, vertical displacement [mm], 15 bands (one per date), EPSG:4326, deflate compressed
- `psi_eastwest_displacement.tif` — multi-band GeoTIFF, east-west displacement [mm], 15 bands, EPSG:4326, deflate compressed
- Band order matches dates: 2022-01-10, 2022-02-03, 2022-02-27, 2022-03-23, 2022-04-16, 2022-05-10, 2022-06-03, 2022-06-27, 2022-07-21, 2022-08-14, 2022-09-07, 2022-10-01, 2022-10-25, 2022-11-18, 2022-12-12

---

## Notebook Structure

| Section | Description |
|---------|-------------|
| §1 Install | One-time package install (Colab compatible) |
| §2 Imports | Libraries and matplotlib patch |
| §3 Configuration | AOI, dates, ASF credentials — **change here for new area** |
| §4 Parameters | Processing thresholds, decimation, wavelengths |
| §5 Init | Stack objects, DEM, landmask |
| §6 Download | Sentinel-1 SLC download via ASF API (both orbits) |
| §7 SBAS | Interferogram formation, unwrapping, time-series |
| §8 PSI | Persistent scatterer selection, phase linking |
| §9 SBAS vs PSI | Cross-validation of descending velocities |
| §10 Decomposition | Dual-orbit LOS → vertical + east-west (Wright et al. 2004) |
| §11 Export | GeoTIFF export + preview PNGs + README |

---

## Usage

```bash
jupyter lab pygmtsar_naples_5.ipynb
```

To adapt to a new area — change only **§3** (AOI, dates, ASF credentials) and **§4** (parameters).

---

## Requirements

```bash
pip install pygmtsar xarray rioxarray numpy pandas geopandas scipy pyproj matplotlib ipywidgets dask distributed
```

---

## References

- Wright, T.J., Parsons, B.E., Lu, Z. (2004). *Toward mapping surface deformation in three
  dimensions using InSAR*. Geophysical Research Letters, 31(1).
- Pechnikov, A. (2025). *PyGMTSAR: Python package for satellite interferometry*.
  [GitHub](https://github.com/AlexeyPechnikov/pygmtsar)

---

## License

MIT
