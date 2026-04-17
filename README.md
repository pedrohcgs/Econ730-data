# Econ 730 — Data

Raw data files for the PhD course **Econ 730: Causal Inference with Panel Data** (Emory University, Pedro H.C. Sant'Anna).

The code that uses these datasets lives in the main course repo:
[pedrohcgs/Econ730](https://github.com/pedrohcgs/Econ730).

This repo is **storage only** — no analysis code. Scripts in the main repo
fetch files from here at *pinned commits* so reproduction is deterministic
years from now, even if data files are later revised.

## Layout

```
Econ730-data/
├── README.md          (this file)
└── Lecture8/
    └── ehec_data.dta  (ACA Medicaid expansion, 2008-2019; 51 states x 12 years)
```

Future lectures will add subfolders (`Lecture9/`, etc.) as the course grows.

## Access pattern

R scripts in `pedrohcgs/Econ730` use a helper like:

```r
source("scripts/R/utils/fetch_data.R")
ehec_path <- fetch_data("Lecture8/ehec_data.dta")   # cached locally
ehec <- haven::read_dta(ehec_path)
```

The helper fetches via
`https://raw.githubusercontent.com/pedrohcgs/Econ730-data/<pinned_commit>/<path>`
and caches the file in `~/.cache/econ730-data/` so subsequent runs are offline.

## Data provenance

### `Lecture8/ehec_data.dta`
- **Topic:** ACA Medicaid expansion and insurance coverage
- **Units:** 51 states × 12 years (2008-2019) = 552 observations
- **Key variables:** `stfips` (state FIPS), `year`, `dins` (insurance rate
  among low-income childless adults 25-64), `W` (sampling weights),
  `yexp2` (expansion year; missing if never expanded)
- **Source:** Derived from American Community Survey (ACS) public-use
  microdata, aggregated to state-year.

Large external replication packages (e.g., Baker et al. 2025 JEL-DiD county
mortality) remain at their upstream locations; scripts pin those commits
separately.
