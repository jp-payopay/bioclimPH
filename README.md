# bioclimPH

19 bioclimatic variable (BCV) layers for the Philippine archipelago, derived from PAGASA CLIMAP v2.0 monthly rainfall and temperature grids (2001–2020).

> **Status: in preparation.** This dataset is not yet finalized. BIO3 and BIO4 is currently corrupted in this release (placeholder/export error) and should not be used until replaced. A versioned, citable release is planned via Zenodo once validation is complete.

## Overview

| | |
|---|---|
| Coverage | Philippine archipelago |
| Temporal window | 2001–2020 (climatological means) |
| Source data | PAGASA CLIMAP v2.0 monthly rainfall (rr) and temperature (tn/tx) grids |
| Derivation | [fastbioclim](https://github.com/nsauwald/fastbioclim) (R), following the standard BIO1–BIO19 variable set |
| CRS | EPSG:4326 (WGS84) |
| Spatial resolution | 0.01° (~1.1 km at the equator) |
| Extent | 114.275–126.605°E, 4.585–21.125°N |
| Raster dimensions | 1233 × 1654 pixels |
| Format | GeoTIFF, single band per variable, float32 |

## Variables

| Code | Variable | Unit |
|---|---|---|
| BIO1 | Annual Mean Temperature | °C |
| BIO2 | Mean Diurnal Range (mean of monthly max–min temp) | °C |
| BIO3 | Isothermality (BIO2/BIO7 × 100) | % |
| BIO4 | Temperature Seasonality (std. dev. × 100) | — |
| BIO5 | Max Temperature of Warmest Month | °C |
| BIO6 | Min Temperature of Coldest Month | °C |
| BIO7 | Temperature Annual Range (BIO5–BIO6) | °C |
| BIO8 | Mean Temperature of Wettest Quarter | °C |
| BIO9 | Mean Temperature of Driest Quarter | °C |
| BIO10 | Mean Temperature of Warmest Quarter | °C |
| BIO11 | Mean Temperature of Coldest Quarter | °C |
| BIO12 | Annual Precipitation | mm |
| BIO13 | Precipitation of Wettest Month | mm |
| BIO14 | Precipitation of Driest Month | mm |
| BIO15 | Precipitation Seasonality (CV) | % |
| BIO16 | Precipitation of Wettest Quarter | mm |
| BIO17 | Precipitation of Driest Quarter | mm |
| BIO18 | Precipitation of Warmest Quarter | mm |
| BIO19 | Precipitation of Coldest Quarter | mm |

Files are named `bio01.tif` … `bio19.tif` under `bioclim/`, following the WorldClim naming convention.

## Derivation notes

- CLIMAP rainfall is distributed as a mean daily rate (mm/day) and was converted to monthly totals before deriving the precipitation-based variables (BIO12–BIO19).
- Monthly climatologies (2001–2020) were computed from 20 years of daily/monthly source grids before variable derivation.

## Quality assurance

Two known issues in the PAGASA CLIMAP v2.0 source grids were identified and handled during derivation:

1. **tn/tx crossover and suppressed diurnal range.** A small fraction of pixels (~0.4%) have interpolated tx < tn in at least one month, concentrated on narrow mountainous terrain (notably the Palawan spine); a larger fraction (~5%) show implausibly suppressed diurnal range. Because these directly propagate into BIO2, BIO3, and BIO7, affected pixels were masked via a graded QA flag rather than silently left in the distributed layers. This is why **BIO2, BIO3, and BIO7 have a smaller valid-pixel footprint (240,701 px) than the other 16 variables.**
2. **Single-pixel precipitation spike.** One pixel in inland eastern Mindanao (Diwata Range) carried an inflated rainfall value traced to the raw CLIMAP input (likely a single gauge in the ClimGridPh network), which was the national maximum for five variables at once. It was identified via a 3×3 neighbourhood-median ratio screen and masked, affecting BIO12–BIO14 and BIO16–BIO17.

Valid-pixel counts by variable group:

| Variables | Valid pixels | Notes |
|---|---|---|
| BIO1, 4, 5, 6, 10, 11 | 245,345 | Temperature-only, unaffected by precipitation masking |
| BIO8, 9, 12–19 | 245,344 | Precipitation-linked; spike pixel masked |
| BIO2, 3, 7 | 240,701 | Crossover/diurnal-range QA masking applied |

The common footprint across all 19 layers is ~240,700 pixels (98.1% of the full domain).

## Known issues

- **BIO1 (Annual Mean Temperature) is corrupted in the current release** — the file contains all-zero values and does not match the expected footprint or data type of the other layers. Do not use until replaced.
- QA flag rasters (crossover/diurnal-range and precipitation-spike masks) are not yet included in this release as standalone layers.

## Citation

A formal citation (with DOI) will be added once this dataset is deposited on Zenodo. In the meantime, please contact the author before reuse.

## License

The derivation code and repository metadata are released under the MIT License (see `LICENSE`). Usage terms for the underlying PAGASA CLIMAP v2.0 source data are separate and are still being coordinated with PAGASA; please contact the author regarding appropriate attribution before redistributing the derived layers.

## Contact

John Paul Payopay — Center for Geoinformatics, Benguet State University
