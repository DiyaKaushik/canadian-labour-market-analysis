
# Canadian Labour Market Analysis 2021–2026

**Who got left behind in Canada's labour market recovery?**

An end-to-end data analysis project using Statistics Canada's Labour Force
Survey (LFS) Public Use Microdata Files, Consumer Price Index, and Average
Hourly Wages data — covering 6.7 million individual survey records from
January 2021 to April 2026.

---

## Project Overview

This project investigates whether Canada's post-2021 labour market recovery
was equitable across provinces, industries, immigration status, and education
levels. Built as a portfolio project targeting data analyst roles in Canada.

**Tools:** Python · pandas · scikit-learn · Power BI · Jupyter Notebook · Git

**Data:** Statistics Canada LFS PUMF · CPI Table 18-10-0004-01 · 
Average Hourly Wages 14-10-0063-01

---

## Key Findings

- **21.6% cumulative inflation** from January 2021 to April 2026
- Only **9 of 19 industries** achieved positive real wage growth after inflation
- **Professional services** gained +12.6% in real wages — the strongest performer
- **Information & culture** lost -10.5% in real wages — the worst performer
- **Healthcare workers** saw real wages decline -0.2% despite being essential
- **Newfoundland** had the strongest provincial employment recovery (+1.1pp)
- **Saskatchewan** was the only major province to decline (-0.3pp)
- The immigrant employment gap persisted at ~1pp across **all education levels**
- **Non-permanent residents** worked part-time at rates 3-4pp above Canadian-born
  workers throughout the entire period

---

## Project Structure

```
canadian-labour-market-analysis/
│
├── data/
│   ├── raw/                    # Original Statistics Canada files
│   │   ├── 2021-CSV/           # Monthly LFS PUMF files
│   │   ├── 2022-CSV/
│   │   ├── 2023-CSV/
│   │   ├── 2024-CSV/
│   │   ├── 2025-CSV/
│   │   ├── 2026-01-CSV/
│   │   ├── 2026-02-CSV/
│   │   ├── 2026-03-CSV/
│   │   ├── 2026-04-CSV/
│   │   ├── cpi/                # CPI table 18-10-0004-01
│   │   └── wages/              # Wages table 14-10-0063-01
│   └── processed/              # Cleaned parquet and CSV files
│
├── notebooks/
│   ├── 01_data_loading.ipynb   # Ingestion pipeline
│   ├── 02_cleaning.ipynb       # Cleaning and decoding
│   ├── 03_eda.ipynb            # Exploratory analysis and charts
│   └── 04_model.ipynb          # Predictive modelling
│
├── powerbi/
│   └── lfs_dashboard.pbix      # Interactive Power BI dashboard
│
├── docs/
│   ├── chart1_provincial_employment.png
│   ├── chart2_province_change.png
│   ├── chart3_parttime_trends.png
│   ├── chart4_parttime_education.png
│   ├── chart5_industry_recovery.png
│   ├── chart6_real_wages.png
│   ├── chart7_immigrant_gap.png
│   ├── chart8_immigrant_education_gap.png
│   ├── chart9_roc_curve.png
│   ├── chart10_feature_importance.png
│   └── chart11_actual_vs_predicted.png
│
└── README.md
```

---

## Notebooks

### Notebook 1 — Data Ingestion
- Dynamically loads 64 monthly LFS CSV files using glob pattern matching
- Reshapes CPI and wages data from wide to long format using pandas melt
- Implements incremental parquet writing to handle 6.7M rows on limited RAM
- Saves optimised parquet files for downstream notebooks

### Notebook 2 — Data Cleaning
- Decodes 8 categorical variables using the LFS PUMF codebook
- Identifies structured missingness — 49% missing in hourly earnings
  reflects self-employed workers, not data errors
- Converts hourly earnings from cents to dollars
- Creates binary analysis flags for employment, immigration, education
- Final cleaned dataset: 6.7M rows, 33 columns

### Notebook 3 — Exploratory Analysis
- Provincial employment rates by month 2021–2026
- Full-time vs part-time trends by immigration status and education
- Industry employment recovery index (January 2021 = 100)
- Real wage growth by industry after 21.6% cumulative inflation
- Immigrant employment gap analysis by education level

### Notebook 4 — Predictive Modelling
- Initial logistic regression on individual records: AUC 0.536
- Reframed to group-level linear regression predicting unemployment rates
- One-hot encoding improved R² from 4.4% to 28.8%
- MAE: 3.52 percentage points
- Seasonality (August +1.77pp) and age are strongest predictors

---

## Charts

### Provincial Employment Change 2021 vs 2025
![Provincial Change](docs/chart2_province_change.png)

### Real Wage Growth by Industry After Inflation
![Real Wages](docs/chart6_real_wages.png)

### Employment Rate by Immigration Status
![Immigration Gap](docs/chart7_immigrant_gap.png)

### Unemployment Risk Model — Feature Importance
![Feature Importance](docs/chart10_feature_importance.png)

---

## Power BI Dashboard

Interactive dashboard with four visuals and year/province slicers:
- Canada bubble map — provincial employment rates
- National employment rate trend 2021–2026
- Real wage growth by industry after inflation
- Employment rate by immigration status 2021–2026

![Dashboard](docs/dashboard.png)

---

## How to Run

### Requirements
```
pip install pandas numpy matplotlib seaborn plotly scikit-learn pyarrow
```

### Steps
1. Download LFS PUMF monthly files from Statistics Canada
2. Place files in `data/raw/` following the folder structure above
3. Run notebooks in order: 01 → 02 → 03 → 04
4. Open `powerbi/lfs_dashboard.pbix` in Power BI Desktop

### Data Sources
- [LFS PUMF](https://www150.statcan.gc.ca/n1/pub/71m0001x/71m0001x2021001-eng.htm)
- [CPI Table 18-10-0004-01](https://www150.statcan.gc.ca/t1/tbl1/en/table!18-10-0004-01)
- [Wages Table 14-10-0063-01](https://www150.statcan.gc.ca/t1/tbl1/en/table!14-10-0063-01)

---

## Limitations

- LFS PUMF is an anonymised public-use file — weights not applied,
  so estimates differ slightly from official Statistics Canada publications
- Group-level model R² of 28.8% reflects that unemployment is largely
  driven by macroeconomic conditions not captured by demographics alone
- Power BI dashboard requires Desktop — not published to Power BI Service

---

## Author

**Diya Kaushik**  
Aspiring Data Analyst | Python · SQL · Power BI  
[GitHub](https://github.com/DiyaKaushik)

---

*Data source: Statistics Canada. Labour Force Survey Public Use Microdata 
File. Released under Statistics Canada Open Licence.*
```
