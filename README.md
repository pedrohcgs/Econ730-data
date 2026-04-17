# Econ 730 — Data

Raw data files for the PhD course **Econ 730: Causal Inference with Panel Data** (Emory University, Pedro H.C. Sant'Anna).

The code that uses these datasets lives in the main course repo:
[pedrohcgs/Econ730](https://github.com/pedrohcgs/Econ730).

This repo is **storage only** — no analysis code. Scripts in the main repo
fetch files from here at *pinned commits* so reproduction is deterministic
years from now, even if data files are later revised.

## Layout

```text
Econ730-data/
├── README.md                                  (this file)
├── Lecture6/
│   └── dias_fontes_final_dataset.dta          (Brazil CAPS panel, 2002-2016)
└── Lecture8/
    └── ehec_data.dta                          (ACA Medicaid expansion, 2008-2019)
```

Future lectures will add subfolders (`Lecture9/`, etc.) as the course grows.

## Access pattern

R scripts in `pedrohcgs/Econ730` inline a compact `fetch_data()` helper so
each script is standalone (a student can download just one `.R` file from
the course schedule page and run it without cloning the full repo):

```r
# --- inlined in each user-facing script ---
.ECON730_DATA_COMMIT <- "<pinned_commit_sha>"
fetch_data <- function(path, commit = .ECON730_DATA_COMMIT) {
  local_path <- file.path(Sys.getenv("HOME"), ".cache",
                          "econ730-data", commit, path)
  if (file.exists(local_path)) return(local_path)
  dir.create(dirname(local_path), recursive = TRUE, showWarnings = FALSE)
  url <- sprintf("https://raw.githubusercontent.com/pedrohcgs/Econ730-data/%s/%s",
                 commit, path)
  utils::download.file(url, destfile = local_path, mode = "wb", quiet = TRUE)
  local_path
}
```

Files are fetched via
`https://raw.githubusercontent.com/pedrohcgs/Econ730-data/<pinned_commit>/<path>`
and cached in `~/.cache/econ730-data/<commit>/…` so subsequent runs are offline.

The canonical (non-inlined) helper lives at
[`scripts/R/utils/fetch_data.R`](https://github.com/pedrohcgs/Econ730/blob/main/scripts/R/utils/fetch_data.R)
in the main repo for reference.

## Data provenance

### `Lecture6/dias_fontes_final_dataset.dta`

- **Topic:** Brazil psychiatric reform (CAPS — Centros de Atenção Psicossocial)
  staggered rollout, 2002–2016
- **Units:** Brazilian municipalities × years (~5,475 units × 15 years =
  **82,140 observations**, 117 variables)
- **Key variables:** `cod` (municipality code), `ano` (year),
  `ca` (CAPS present indicator), `sim_agressao` (homicide rate),
  `pibpc_` (GDP per capita), plus 110 covariates for robustness analyses
- **Source:** Dias, F. L. de A., & Fontes, L. F. C. (2024).
  *The Effects of a Large-Scale Mental Health Reform: Evidence from Brazil*.
  American Economic Journal: Economic Policy, 16(3), 257–289.
- **Original file:** `data/final/final_dataset.dta` in the authors'
  replication package (AEJ:EP supplement). The file in this repo is a
  verbatim copy. Variable labels and Stata codebook preserved.

### `Lecture8/ehec_data.dta`

- **Topic:** ACA Medicaid expansion and insurance coverage
- **Units:** 51 states × 12 years (2008–2019) = **552 observations**
- **Key variables:** `stfips` (state FIPS), `year`, `dins` (insurance rate
  among low-income childless adults 25–64), `W` (sampling weights),
  `yexp2` (expansion year; missing if never expanded)
- **Source:** Derived from American Community Survey (ACS) public-use
  microdata, aggregated to state-year.

Large external replication packages (e.g., Baker et al. 2025 JEL-DiD county
mortality) remain at their upstream locations; scripts pin those commits
separately.
