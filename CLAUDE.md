# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a UK economics research repository containing Jupyter notebooks and supporting Python modules for analysing business dynamism and productivity. Work is organised into project directories, each with its own notebooks, data, and charts.

## Project Structure

- **CEP Dynamism Project/** - Active project on UK business dynamism (entry/exit rates, job creation/destruction, firm-level data). Main scripts are in `Scripts/`.
- **dynamism_productivity/** - Analysis linking business dynamism to productivity, including ONS 2025 data (`ONS_2025/` subdirectory).
- **IndustrialStrategy/** - Industrial strategy sector analysis with geographic LAD-level mapping.
- **LDN_frontier_sectors/** - London frontier sector firms analysis using BSD data with MSOA/LAD geographic breakdowns.
- **BSD Dynamism [ARCHIVE]/** - Archived earlier BSD work; superseded by CEP Dynamism Project.
- **Other policy things/ISA/** - ISA investigation scoping work.

## Key Shared Module: `eco_style.py`

Every project directory contains a copy of `eco_style.py`. This module defines:
- A shared colour `pallete` dict with named colours for nominal categories, country codes (GBR, USA, DEU, FRA), and bar chart variants.
- Three Altair themes registered via `alt.themes.register(...)`: `report`, `light`, and `dark`. The `report` theme is the standard for publication charts.

Import pattern used across notebooks:
```python
import sys
sys.path.append('.')  # or the relevant project dir
from eco_style import pallete
import altair as alt
alt.themes.enable('report')
```

Note: `eco_style.py` is duplicated across directories rather than installed as a package. When making changes, check which copy a notebook actually imports.

## Data Sources

- **BSD** (Business Structure Database) - firm-level data on employment, entry, exit
- **ONS** data - business counts, labour productivity, workforce jobs
- **NOMIS API** - queried via notebooks (`bres_nomis_api_query.ipynb`, `localunit_nomis_api_query.ipynb`) for BRES employment data by sector/geography
- Shapefiles for LAD and MSOA boundaries (used with geopandas/altair for maps)

Data files are CSV and XLSX; large files are gitignored (see `.gitignore`). Output charts are saved as Altair JSON specs and PNGs.

## Workflows

**Running notebooks:** Open in Jupyter and run cells. No build step. Notebooks are self-contained per project.

**Querying NOMIS API:** See `IndustrialStrategy/bres_nomis_api_query.ipynb` or `CEP Dynamism Project/Scripts/localunit_nomis_api_query.ipynb` for the query pattern used to pull data in chunks.

**Saving charts:** Altair charts are saved as `.json` (Vega-Lite specs) for interactive use and `.png` for reports.

## Measure Definitions (BSD Dynamism Project)

All rates use the lagged (t-1) denominator. Disclosure-suppressed cells are marked '*' in the raw data and should be converted to NaN before analysis.

Entry rate = (n_entrants + n_entry_and_exit) / n_firms_{t-1}
Exit rate = (n_exiters + n_entry_and_exit) / n_firms_{t-1}

Note: n_entry_and_exit counts firms that both entered and exited within the same year; these are included in both numerators.

Job reallocation rate (JRR) = (JC_entrants + JC_incumbents + JD_exiters + JD_incumbents) / emp_{t-1}
Extensive margin = (JC_entrants + JD_exiters) / emp_{t-1}
Intensive margin = (JC_incumbents + JD_incumbents) / emp_{t-1}
Job creation rate (JCR) = (JC_entrants + JC_incumbents) / emp_{t-1}
Job destruction rate (JDR) = (JD_exiters + JD_incumbents) / emp_{t-1}

Survival: use kaplan_meier_rate column, not surv_prob.

DHS growth rate = (E_t - E_{t-1}) / ((E_t + E_{t-1}) / 2), bounded in [-2, 2]. Covers incumbent firms only.

High-growth firm (HGF): defined by the HGF threshold applied in the BSD output (n_hgf column in growth_cats sheet).
Stagnant firm: zero or near-zero DHS growth (n_stagnant column).

## Conventions

- Charts follow the CEP house style defined in `eco_style.py` — use `pallete` for colour assignments and enable the `report` theme for publication outputs.
- Geographic analysis uses ONS boundary shapefiles for LAD (Local Authority District) and MSOA levels.
- Data is kept in `Data/` subdirectories within each project; processed outputs go to `Charts/` or `Tables/`.
