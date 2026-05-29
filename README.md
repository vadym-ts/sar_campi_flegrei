# 🛰️ Sentinel-1 InSAR — Campi Flegrei Ground Deformation

Satellite radar processing pipeline for measuring ground deformation at Campi Flegrei, Naples.
Uses Sentinel-1 SAR data and PyGMTSAR to produce displacement time series and decomposed
vertical + east-west maps.

---

## What is InSAR

InSAR (Interferometric Synthetic Aperture Radar) measures ground deformation using radar
satellites. By comparing two radar images taken at different times, it detects millimeter-scale
changes in the distance between the satellite and the ground. Unlike optical sensors, radar
works through clouds and at night.

This notebook uses two InSAR methods:

- **SBAS** — averages many interferogram pairs for a stable velocity estimate
- **PSI** — tracks individual stable reflectors (buildings, rocks) over time

Combining ascending and descending satellite passes allows decomposition of the
line-of-sight measurements into true vertical and east-west displacement components.

---

## Study Area

Campi Flegrei is a 13km-wide volcanic caldera west of Naples, Italy. The ground there
rises and falls due to magmatic and hydrothermal activity — a process called bradyseism.
In 2022 the caldera was uplifting at 40–80 mm/year, which this analysis captures.

- **AOI**: lon 14.02–14.22, lat 40.78–40.86
- **Period**: January–December 2022
- **Epochs**: 15 Sentinel-1 acquisitions

---

## Stack

| Tool | Purpose |
|------|---------|
| [PyGMTSAR](https://github.com/AlexeyPechnikov/pygmtsar) | InSAR processing (coregistration, unwrapping, PSI, SBAS) |
| xarray / rioxarray | Gridded data handling and GeoTIFF export |
| numpy / scipy | Numerical computation, decomposition, statistics |
| geopandas | AOI handling |
| pyproj | Coordinate transformations |
| dask / distributed | Parallel processing |
| matplotlib | Visualization |
| JupyterLab | Interactive notebook environment |

---

## Datasets

| Dataset | Source | Access |
|---------|--------|--------|
| Sentinel-1 SLC (ascending + descending) | [ASF DAAC](https://asf.alaska.edu) | Free, requires registration |
| Copernicus DEM | Via PyGMTSAR Tiles | Automatic download |
| Landmask (GSHHG) | Via PyGMTSAR | Automatic download |

---

## Outputs

- `psi_vertical_displacement.tif` — vertical displacement time series, 15 bands, EPSG:4326
- `psi_eastwest_displacement.tif` — east-west displacement time series, 15 bands, EPSG:4326

---

## Usage

```bash
pip install pygmtsar xarray rioxarray numpy pandas geopandas scipy pyproj matplotlib ipywidgets dask distributed
jupyter lab sentinel1_campi_flegrei_psi_decomposition.ipynb
```

To apply to a different area — change only **§3** (AOI, dates) and **§4** (parameters).

---

## References

- Wright, T.J., Parsons, B.E., Lu, Z. (2004). *Toward mapping surface deformation in three dimensions using InSAR*. Geophysical Research Letters, 31(1).
- Pechnikov, A. (2025). *PyGMTSAR*. [GitHub](https://github.com/AlexeyPechnikov/pygmtsar)

---

## Acknowledgements

This work builds on [PyGMTSAR](https://github.com/AlexeyPechnikov/pygmtsar) developed by
Alexey Pechnikov. The processing API, canonical patterns, and Docker environment used in
this notebook are his work.

---

## License

MIT
