# Indonesia SPKLU Coverage Analysis

Spatial analysis of Indonesia's public EV charging network (*Stasiun Pengisian Kendaraan Listrik
Umum* — SPKLU): how stations are distributed across operators and regions, how coverage compares
per capita and per unit of economic output, and where spatial "charging deserts" appear.

**Interactive dashboard (Tableau Public):**
[P1M2 — SPKLU Coverage Dashboard](https://public.tableau.com/app/profile/ryan.pratama7647/viz/P1M2_ryan_farid/Dashboard1?publish=yes)

---

## Problem statement

Through Presidential Regulation No. 55/2019, Indonesia targets 15 million EVs on the road by 2030.
Reaching that depends on public charging infrastructure that is not only large enough but
*well distributed*. Rapid EV growth concentrated in big cities risks leaving regions outside the
main economic centres under-served ("charging deserts").

This project builds an objective, data-driven picture of SPKLU coverage inequality across
Indonesian provinces to support infrastructure planning by PT PLN and the Ministry of
Transportation.

## Key questions

1. **Operator market share** — how stations split between PT PLN, independent private CPOs,
   automaker/dealership networks, and others.
2. **Charging power by location type** — how power-tier mix varies across location categories.
3. **Provincial coverage gaps** — lowest-coverage provinces per 100k population *and* per trillion
   Rp GRDP (and whether the two rankings agree).
4. **Charging deserts** — share of stations isolated by more than 20 km of road from any neighbour.
5. **Descriptive statistics** — distribution of installed power and inter-station distance.
6. **Java vs. non-Java inequality** — inferential tests on power *and* distance, with effect size.

## Key findings

- **PT PLN dominates** the network with **65.5%** of stations (1,607 of 2,454); independent CPOs
  hold 19.2% and automaker/dealership networks 14.3%.
- **Regional gaps depend on how you measure.** The per-capita and per-GRDP lowest-coverage
  rankings disagree, but **Papua and Kepulauan Riau are under-served on both**. Coverage
  concentrates heavily in DKI Jakarta and Bali.
- **The Java vs. non-Java gap is about distance, not power.** Inter-station distance differs by
  ~20 km (Java 3.7 km vs non-Java 23.8 km; Cohen's d ≈ −0.73, p ≈ 4×10⁻³⁵ — a medium-to-large,
  statistically strong effect). The installed-power difference, though statistically significant,
  is practically negligible (d ≈ 0.14).
- **13.73% of stations (337)** are charging deserts, sitting more than 20 km by road from the
  nearest other station — concentrated outside Java.

**Takeaway:** the priority outside Java should be **new stations to close distance gaps and add
network redundancy**, rather than a blanket upgrade of existing stations' power.

## Dataset

- **Source:** Mendeley Data — *A Geospatial Dataset of Public Electric Vehicle Charging Stations
  in Indonesia*, [doi:10.17632/z2bvd5r574.2](https://data.mendeley.com/datasets/z2bvd5r574/2)
- **Contributors:** Richard Wiputra, Sherly Santiadi (BINUS University)
- **Scope:** 2,454 SPKLU stations with administrative, spatial, and power attributes, plus
  province-level demographic and economic indicators (2025/2026).

The analysis joins the station table (`spklu_stations`) with the provincial coverage table
(`spklu_province_coverage`) on the BPS province code. See
[`A Geospatial Dataset of Public Electric Vehicle/spklu_data_dictionary.csv`](A%20Geospatial%20Dataset%20of%20Public%20Electric%20Vehicle/spklu_data_dictionary.csv)
for full column definitions.

## Methodology

1. **Data loading & exploration** — inspect structure, types, missing values, duplicates.
2. **Data cleaning** — select key columns and join the two tables with a validated left join
   (row count preserved, no key fan-out).
3. **Analysis** — six key questions combining descriptive summaries, cross-tabs, and
   visualisations.
4. **Inferential statistics** — Welch two-sample t-tests (Java vs non-Java) on installed power and
   inter-station distance, reporting p-value, **Cohen's d effect size**, and **95% confidence
   interval** for the mean difference.
5. **Export** — write `spklu_curated.csv` (with derived `deserted` and `java` flags) for the
   Tableau dashboard.

## Repository structure

```
.
├── spklu_analysis.ipynb        # Main analysis notebook (English)
├── spklu_curated.csv           # Curated, joined dataset exported for Tableau
├── requirements.txt            # Python dependencies
├── LICENSE
└── A Geospatial Dataset of Public Electric Vehicle/
    ├── data/                   # Raw station & charger tables
    ├── reference/              # Province coverage, indicators, regency codes
    ├── spklu_data_dictionary.csv
    ├── PROCESSING_LOG.md
    └── README.md
```

## How to run

```bash
# 1. (optional) create a virtual environment
python -m venv .venv && source .venv/bin/activate

# 2. install dependencies
pip install -r requirements.txt

# 3. launch the notebook
jupyter notebook spklu_analysis.ipynb
```

Data paths in the notebook are relative to the repository root, so the notebook runs as-is after
cloning — no path editing required.

## Tools

Python (pandas, NumPy, SciPy, Matplotlib, Seaborn) for analysis; Tableau Public for the
interactive dashboard.

## Author

Ryan Farid Pratama — CODA-RMT-023
