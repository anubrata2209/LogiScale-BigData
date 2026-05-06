# LogiScale Control Tower
### End-to-End Supply Chain Analytics Pipeline

---

## Overview :

LogiScale Control Tower is a big data supply chain analytics project built using **PySpark**, **Python**, **SQLite**, and **Power BI**.

The project takes raw logistics data from over 180,000 shipment records and transforms it into a fully working analytics pipeline — from data cleaning all the way to an interactive Power BI dashboard that a logistics manager can use every day.

It solves a real business problem: when delivery data is too large for normal tools to handle, the existing reporting systems crash. This project replaces that with a scalable PySpark-based solution that processes data beyond local memory limits, calculates delivery delay variances, forecasts warehouse demand, and displays everything in a live Control Tower dashboard.

---

## Project Architecture

```
Raw Dataset (DataCo Supply Chain — 180,000+ records)
        ↓
Data Cleaning & Feature Engineering   [data_cleaning.ipynb]
        ↓
Big Data Aggregations & Window Functions  [bigdata_processing.ipynb]
        ↓
Validation & Spark Optimization  [analytics_and_validation.ipynb]
        ↓
EDA Visualizations & Export  [Visualization.ipynb]
        ↓
SQLite Database (logiscale.db)
        ↓
Power BI Control Tower Dashboard
```

---

## Project Structure

```
LogiScale_BigData/
│
├── dashboard/
│   ├── powerbi.pbix                   ← Power BI dashboard file
│   └── PowerBI_Representation.pdf     ← Exported dashboard PDF
│
├── notebooks/
│   ├── data_cleaning.ipynb            ← Phase 1: Load, clean, benchmark
│   ├── bigdata_processing.ipynb       ← Phase 2: Aggregations, window functions
│   ├── analytics_and_validation.ipynb ← Phase 3: Validation, export to SQLite
│   └── Visualization.ipynb            ← Phase 4: EDA charts using Plotly
│
├── outputs/
│   └── logiscale.db                   ← SQLite database with all aggregated tables
│
├── requirements.txt
├── .gitignore
└── README.md
```

> **Note:** CSV output files are not tracked in this repository.
> They are generated automatically when the notebooks are run.
> All aggregated tables are stored inside `logiscale.db`.

---

## Dataset

**Source:** Kaggle — DataCo Smart Supply Chain Dataset

**Link:** https://www.kaggle.com/datasets/shashwatwork/dataco-smart-supply-chain-for-big-data-analysis

The dataset contains 180,000+ logistics records covering:
- Shipment orders and delivery timelines
- Estimated vs actual delivery dates
- Product categories and SKUs
- Customer segments and regions
- Sales, profit, and market data across 50+ global warehouses

---

## Technologies Used

| Tool | Purpose |
|---|---|
| PySpark | Distributed big data processing |
| Pandas | Data validation and cross-checks |
| SQLAlchemy + SQLite | Aggregated result storage |
| Plotly | EDA visualizations |
| Power BI | Interactive Control Tower dashboard |
| Jupyter Notebook | Development environment |

---

## How to Run

### 1. Clone the repository
```bash
git clone https://github.com/anubrata2209/LogiScale-BigData.git
cd LogiScale_BigData
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

> **Important:** PySpark requires Java JDK 8 or higher.
> Download from java.com and restart your machine after installing.

### 3. Download the dataset

Download `DataCoSupplyChainDataset.csv` from the Kaggle link above
and place it in the **project root folder** (same level as README.md).

### 4. Run the notebooks in this exact order

```
1. data_cleaning.ipynb
2. bigdata_processing.ipynb
3. analytics_and_validation.ipynb
4. Visualization.ipynb
```

### 5. Open the dashboard

- Open `dashboard/powerbi.pbix` in Power BI Desktop
- Click **Refresh** to load the latest data from logiscale.db

---

## Phase Breakdown

### Phase 1 — Data Cleaning (`data_cleaning.ipynb`)
- Loaded 180,000+ records using PySpark
- Dropped 23 irrelevant columns (personal info, unused fields)
- Removed duplicate rows and null values in critical columns
- Renamed all columns to clean project-standard names
- Parsed date columns and calculated `delay_days` (actual − scheduled)
- Added `delivery_flag` column (Late / On Time / Early)
- Ran a Pandas vs PySpark performance benchmark
- Exported cleaned data as `dataco_cleaned.csv`

### Phase 2 — Big Data Processing (`bigdata_processing.ipynb`)
- **Route Efficiency Analysis** — avg delay, stddev, on-time %, late % grouped by route and shipping mode
- **Warehouse Performance** — total sales, profit, units sold, risk % per warehouse
- **Daily Warehouse Metrics** — day-level aggregations per warehouse
- **Demand Forecasting** — 4-week moving average of units sold per category per warehouse using PySpark window functions
- **Delay Trend** — 7-day rolling average of delivery delay per route
- **Spark Optimization** — Explain Plan analysis, shuffle reduction, caching, partition tuning

### Phase 3 — Validation & Export (`analytics_and_validation.ipynb`)
- Row count integrity check (raw vs cleaned)
- Null check across all key columns
- Aggregation cross-check: PySpark results vs Pandas manual recalculation
- Outlier and range validation
- Exported all aggregated tables to `logiscale.db`
- Printed full validation report confirming all checks passed

### Phase 4 — Visualization (`Visualization.ipynb`)
Built 15 interactive Plotly charts across 5 analysis areas:
- Delivery status distribution and breakdown by route
- Top 10 customers by orders and profit
- Customer segment analysis
- Orders and sales by product category
- Geographic analysis by region and country
- Sales breakdown by product, category, delivery status, and region

---

## Tables Inside logiscale.db

| Table | Description |
|---|---|
| `delay_summary` | Avg delay, on-time %, late % by route and shipping mode |
| `warehouse_summary` | Sales, profit, units, risk % per warehouse |
| `daily_warehouse_metrics` | Day-level orders, sales, profit per warehouse |
| `demand_forecast` | 4-week moving average of units sold per category |
| `moving_avg_delay` | 7-day rolling average of delay per route |

---

## Power BI Dashboard — Control Tower

The final dashboard gives logistics managers a single-screen view of global operations.

**KPI Cards:**
- Total Shipments
- Average Delay (Days)
- On-Time Delivery %
- Total Revenue

**Charts:**
- Route efficiency — avg delay by region
- Shipment volume by region
- Warehouse revenue vs profit
- 7-day delay trend over time
- 4-week demand forecast by product category
- Shipping mode performance

**Filters:** Market, Shipping Mode, Date Range

---

## Key Business Insights

- Certain regions consistently show higher average delivery delays
- Some shipping modes carry significantly higher late-delivery risk
- High-volume routes do not always correlate with high profitability
- Demand forecasting reveals clear seasonal patterns per product category
- Warehouse performance varies significantly across markets

---

## Spark Optimization Summary

| Technique | What It Did |
|---|---|
| Explain Plan analysis | Identified unnecessary Exchange (shuffle) operations |
| Partition tuning | Reduced shuffle partitions from 200 to `2 × CPU cores` |
| DataFrame caching | Prevented re-reading CSV on every aggregation |
| Combined aggregations | Merged multiple groupBy calls into one to reduce shuffles |

---

## Requirements

```
Requirements
pyspark
pandas
sqlalchemy
plotly>=6.1.1
kaleido==1.0.0
jupyter
notebook
numpy
matplotlib
openpyxl
```

Install all with:
```bash
pip install -r requirements.txt
```

---

## Learning Outcomes

- Big data processing using Apache PySpark
- Distributed computing and lazy evaluation concepts
- Data pipeline design and development
- Spark performance tuning and optimization
- Business analytics and KPI generation
- Dashboard design in Power BI
- Data validation and quality assurance

---

## Conclusion

LogiScale Control Tower demonstrates how huge number of raw logistics data can be transformed into meaningful operational insights through a scalable, end-to-end analytics pipeline.

The project combines **Data Engineering**, **Big Data Analytics**, **Demand Forecasting**, **Data Validation**, and **Interactive Visualization** into a complete supply chain solution — proving that PySpark is the right tool when data size exceeds what traditional tools can handle.

---

*Dataset: DataCo Smart Supply Chain — Kaggle*
*Stack: PySpark → SQLite → Power BI*
