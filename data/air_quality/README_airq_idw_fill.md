# airq_idw_fill.csv — air-quality gap-fill (provenance)

Produced by `twfe_did_models/00b_prepare_fills.ipynb` (or the all-in-one
`01b_build_panel_duckdb.py`). One row per California county-year (58 × 16 = 928).

| column | meaning |
|---|---|
| `fips`, `county_name`, `year` | county-year key |
| `pm25_idw` | leave-one-out IDW estimate of county-mean PM2.5 (ug/m3) |
| `o3_idw`   | leave-one-out IDW estimate of county-mean ozone 4th-max (ppm) |

## Method
For each year, each county's estimate is the inverse-distance-weighted average
(weight = 1 / d², d = straight-line distance between county **centroids**,
from `ca_counties_clean.csv`) of the values at **all other** counties that have
a real monitor reading that year (leave-one-out).

Monitored inputs mirror `01_build_panel.sql`:
- PM2.5 = param 88101, tiered duration (24 HOUR FRM/Y → 24 HOUR → 24-HR BLK AVG/Y → BLK AVG), county mean.
- Ozone = param 44201, county mean of monitor 4th-max.

## Use in the panel
The panel build joins this file and sets, per outcome:
`*_filled = COALESCE(observed monitor value, *_idw)` with
`*_source ∈ {monitor, idw_interpolated}`. **Observed monitor values are never
overwritten** — the IDW value is used only where no monitor exists. The raw
monitor columns `pm25` / `o3` are kept unchanged.

## Caveat
IDW is purely geographic (no elevation, terrain, wind, or local-emission
adjustment); rural fills carry more uncertainty. For a physically-fused
alternative, see the CDC/EPA Environmental Public Health Tracking Downscaler.
