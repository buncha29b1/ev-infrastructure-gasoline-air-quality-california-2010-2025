# Charging the Corridors

**A Distance-Weighted Difference-in-Differences Analysis of NEVI Interstate Fast-Charging Deployment, Gasoline Consumption, and Air Quality Across California Counties**

**Author:** Khoi Van

**Project Advisor:** Dr. John Conzelmann, Professor, Economics Department, Denison University

**Link to Paper:** You can access the paper [here](https://github.com/buncha29b1/ev-infrastructure-gasoline-air-quality-california-2010-2025/blob/main/Khoi%20Van%20-%20Summer%20Research%20Paper.pdf).

---

## Overview

California's 2022 **National Electric Vehicle Infrastructure (NEVI) Formula Program** directs federal
funds through the state to build direct-current fast-charging (DCFC) along designated Interstate
Alternative-Fuel Corridors *before* any other spending. Because corridor placement is fixed by the
federal interstate network and administered by the state — not chosen by counties — it provides a
plausibly exogenous policy shock for asking a causal question:

> **Does public fast-charging deployment reduce gasoline use and improve local air quality?**

This project assembles a California **county-year panel (58 counties, 2010–2025)** and estimates the
effect of NEVI corridor build-out on per-capita gasoline consumption and air quality, sharpening
identification with a distance-weighted design that concentrates weight on counties nearest the
corridors.

## Research design

- **Canonical two-way fixed-effects DiD** comparing corridor counties (treated, *n* = 23) with
  off-corridor counties (control, *n* = 35) around a **2023 treatment onset**, controlling for log
  median household income and the share of commuters who drive alone.
- **Distance-weighted DiD** that measures each county centroid's distance to the interstate corridor
  and up-weights corridor-adjacent counties using both an **exponential-decay kernel** and an
  **inverse-distance (1/d)** weight.
- **Event-study** specifications to trace dynamic effects year by year.

## Key findings

The estimated effect on log per-capita gasoline is **negative and grows stronger as weight is
concentrated on corridor-adjacent counties**. Median Air Quality Index shows no detectable effect
under any weighting. With only two post-treatment years and a small cross-section, estimates are
directionally consistent but not statistically significant at conventional levels.

| Specification | Effect on log per-capita gasoline | p-value |
|---|---|---|
| Unweighted DiD | −0.048 | 0.24 |
| Exponential kernel (h = 50 km) | −0.064 | 0.25 |
| Inverse-distance (1/d) | −0.089 | 0.10 |
| Event study (by 2024) | −0.17 | 0.055 |
| Median AQI (all weightings) | no detectable effect | — |

The pattern is read as **suggestive evidence that fuel displacement is concentrated near state-built
charging corridors**. The paper outlines how continuous-dose and causal-machine-learning designs could
formalize the result.

## Data sources

The panel is assembled from public data:

- **California Energy Commission (CEC)** — gasoline consumption and charging infrastructure
- **U.S. Department of Energy, Alternative Fuels Data Center (AFDC)** — EV charging stations
- **U.S. Environmental Protection Agency, Air Quality System (AQS)** — air quality (AQI, concentrations)
- **U.S. Census Bureau, American Community Survey (ACS)** — sociodemographic controls
- **State NEVI corridor GIS layer** — interstate corridor geometry and treatment assignment

See [`Data Sources.docx`](./Data%20Sources.docx) for full provenance.

## Repository structure

```
.
├── analysis/                        # Panel, model outputs, and figures
│   ├── analysis_panel.csv           # Final county-year analysis panel
│   ├── did_twfe_ols_results.csv     # Two-way fixed-effects DiD estimates
│   ├── did_weighted_results.csv     # Distance-weighted DiD estimates
│   ├── did_weighted_event_study.csv
│   ├── did_invdist_event_study.csv
│   ├── did_weighted_bandwidth.csv
│   ├── did_weight_scheme_comparison.csv
│   ├── poster_summary.csv
│   ├── nevi_corridors.geojson       # NEVI interstate corridor geometry
│   ├── figures/                     # Event-study and summary plots (PNG)
│   └── scripts/
│       └── models.ipynb             # Estimation notebook
├── data/                            # Raw and processed inputs by domain
│   ├── air_quality/                 # EPA AQS annual AQI and concentrations (2010–2025)
│   ├── ev_charging_infrastructure/  # AFDC EV stations by year (2010–2025)
│   ├── gasoline_consumption/        # CEC / EIA gasoline consumption
│   ├── zev_registrations_fleet_composition/
│   ├── sociodemographic/            # Census ACS by year (2010–2019+)
│   ├── county_spatial_data/         # California county geometry/centroids
│   └── rurality_classification/     # County rural/urban classification
├── Khoi Van - Summer Research Paper.pdf     # Full paper
├── NEVI_Corridor_DiD_Research_Paper.docx    # Working manuscript
├── Khoi Van - Summer Research Poster.pdf    # Conference poster
├── Khoi Van - Summer Research Poster.pptx
├── Khoi Van - 2026 Summer Research Proposal.pdf
├── Literature Review.docx
└── Data Sources.docx
```

## Documents

- **Paper:** `Khoi Van - Summer Research Paper.pdf` — the full write-up (abstract, methods, results, discussion).
- **Poster:** `Khoi Van - Summer Research Poster.pdf` / `.pptx` — visual summary.
- **Proposal & lit review:** background and motivation.

## Reproducing the analysis

The estimation lives in [`analysis/scripts/models.ipynb`](./analysis/scripts/models.ipynb). It reads the
assembled panel (`analysis/analysis_panel.csv`), fits the TWFE and distance-weighted DiD models, and
writes the result tables and figures in `analysis/`.

## Citation

> Van, K. (2026). *Charging the Corridors: A Distance-Weighted Difference-in-Differences Analysis of
> NEVI Interstate Fast-Charging Deployment, Gasoline Consumption, and Air Quality Across California
> Counties.* Summer Research 2026, Data Analytics Program, Denison University.

## Notes

This repository is a research data archive covering California counties, 2010–2025. Findings are
described by the author as suggestive rather than conclusive, given the short post-treatment window.
