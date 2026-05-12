# Sales Automation Pipeline



> **Portfolio project built for the MTO – Sales Automation role at PRAN-RFL Group.**
> An end-to-end Python automation pipeline that processes raw SPRO App field sales data
> and automatically delivers KPI reports, formatted Google Sheets dashboards, and
> intelligent field alerts — all from a **single Google Colab notebook.**


##  Project Overview

PRAN-RFL Group's field Sales Representatives use the **SPRO App** to log daily
shop visits, orders, GPS check-ins, and stock levels across routes in Bangladesh.
This project automates the entire data pipeline — from raw field CSV exports to
management-ready reports — with zero manual work.

**One notebook. One click. Everything runs automatically.**

| Step | What happens |
|---|---|
|  **Clean** | Ingest raw CSV, fix dates, calculate visit durations, validate totals, flag quality issues |
|  **Analyse** | Calculate 10+ KPI categories across 150 SRs, 10 routes, 5 product categories |
|  **Report** | Push a fully formatted 7-tab report to Google Sheets automatically |
|  **Alert** | Fire 215 intelligent field alerts across 5 alert types |
|  **Visualise** | Interactive HTML dashboard with 6 charts and 5 live filters |

---

##  Pipeline Architecture

```
┌─────────────────────────────────────────────────────────┐
│         SPRO App Field Data Export (CSV)                │
│      5,000 rows · 22 columns · March 2026               │
└────────────────────────┬────────────────────────────────┘
                         ↓
              Sales_Automation.ipynb
         ┌───────────────────────────────┐
         │                               │
         ▼                               │
  ┌─────────────────┐                    │
  │ SECTION 1       │                    │
  │ Data Cleaning   │                    │
  └────────┬────────┘                    │
           ↓                             │
  ┌─────────────────┐                    │
  │ SECTION 2       │                    │
  │ KPI Generation  │                    │
  └────────┬────────┘                    │
           ↓                             │
  ┌─────────────────┐  ┌──────────────┐  │
  │ SECTION 3       │  │ SECTION 4    │  │
  │ Google Sheets   │  │ Alert Engine │  │
  │ 7 formatted tabs│  │ 215 alerts   │  │
  └─────────────────┘  └──────────────┘  │
         │                               │
         └───────────────────────────────┘
                         ↓
         ┌───────────────────────────────┐
         │  Interactive HTML Dashboard   │
         │  index.html · 5 live filters  │
         └───────────────────────────────┘
```

---

##  Project Structure

```
pran-rfl-sales-automation/
│
├── Sales_Automation.ipynb     ← Single Colab notebook (entire pipeline)
│
├── dashboard/
│   └── index.html             ← Interactive HTML dashboard
│
├── data/
│   └── spro_sample.csv        ← Synthetic SPRO-style dataset (5,000 rows)
│
└── README.md
```

> The `output/` folder (cleaned CSV, KPI JSON, alert logs) is generated
> automatically inside Google Drive when you run the notebook.

---

##  Tech Stack

| Tool | Purpose |
|---|---|
| **Python 3.10+** | Core automation language |
| **pandas** | Data ingestion, cleaning, transformation |
| **numpy** | Numerical validation |
| **matplotlib** | Daily sales trend chart |
| **gspread** | Google Sheets API — push data via Python |
| **gspread-formatting** | Cell colors, headers, conditional formatting |
| **Google Colab** | Cloud notebook — no local setup needed |
| **Google Drive** | File storage between pipeline steps |
| **Chart.js** | Interactive dashboard charts |
| **HTML / CSS / JS** | Dashboard frontend |

---

## Dataset

| Property | Detail |
|---|---|
| Rows | 5,000 field transactions |
| Columns | 22 |
| Period | March 2026 — 31 working days |
| Routes | 10 (Dhanmondi, Savar, Mirpur-1, Uttara-3, Mirpur-10, Gazipur, Narayanganj, Chittagong, Uttara-10, Mohammadpur) |
| Sales Representatives | 150 SRs |
| Product Categories | Electronics, Kitchenware, Plastics, Household, Furniture |
| Payment Modes | Cash, bKash, Nagad, Credit |

**Columns:** `Date` `SR ID` `Route Name` `Shop Name` `Visit Start` `Visit End` `GPS Status`
`Product Category` `SKU Name` `Qty (Pcs)` `Unit Price` `Total Val` `Stock in Shop`
`Order Status` `Payment Mode` `Shop ID` `Order ID` `Distributor Name`
`Latitude` `Longitude` `Return Order` `SR Daily Target`

> **Dataset Notice:** Synthetically generated to mirror PRAN-RFL SPRO App
> field transaction structure. No real customer or employee data is used.

---

##  What the Notebook Does

###  Section 1 — Data Cleaning
Reads raw SPRO CSV and produces a validated, clean dataset.

- Parses `Date` column and extracts `Month`, `Day`, `DayOfWeek`
- Calculates `Visit Duration (min)` from `Visit Start` / `Visit End` timestamps
- Validates: `Total Val = Qty × Unit Price` — flags mismatches
- Removes duplicate `Order ID` entries
- Standardizes all text columns (strip, title-case)
- Assigns a `Data Quality Flag` to every row: `Clean` / `Invalid Date` / `Missing Duration` / `Value Mismatch` / `GPS Issue`

**Output →** `cleaned_data.csv` saved to Google Drive

---

### Section 2 — KPI Generation
Calculates all business metrics from the clean data.

- Top-level KPIs: Total Sales, Orders, Achievement %, Active SRs, Shops Visited
- Sales breakdown by Route, Category, SKU (Top 10)
- Daily sales trend chart (matplotlib) with highest/lowest day annotated
- SR performance table with tier classification:
  - 🟢 On Track (≥ 90%)
  - 🟡 Needs Attention (70–89%)
  - 🔴 Below Target (< 70%)
- Payment mode, Order status, GPS compliance breakdowns
- Low stock detection (Stock in Shop < 5 pcs)

**Output →** `kpi_output.json` + `daily_sales_trend.png` saved to Google Drive

---

### Section 3 — Google Sheets Automation
Pushes all KPIs into a formatted Google Sheets report automatically.

| Tab | Contents |
|---|---|
|  Executive Summary | KPI cards with title header and generation date |
|  Sales by Route | Top 10 routes ranked by revenue |
|  SR Performance | All SRs with color-coded performance tiers |
|  Daily Trend | 31-day sales data ready for Google Sheets chart |
|  Low Stock Alerts | Critical items highlighted in red |
|  Operations Breakdown | Payment / Order Status / GPS side by side |
|  Alerts | All 215 alerts with severity color coding |

**Formatting applied automatically:**
- Dark blue header rows with white bold text
- 🟢 Green / 🟡 Orange / 🔴 Red color on SR tiers
- Critical stock rows highlighted red
- All columns auto-resized, header rows frozen

**Output →** Live Google Sheets report pushed automatically

 **Live Report URL:**
```
https://docs.google.com/spreadsheets/d/1czkaf4Dg-ey-phXdUHHJiPoyYTAU2QNxj_MoUGVxnsM
```

---

###  Section 4 — Alert Engine
Scans the cleaned data and fires intelligent alerts for field issues.

| Alert Type | Threshold | Alerts | Severity |
|---|---|---|---|
|  GPS Mismatch | > 20% mismatch rate per SR | 3 SRs | 🔴 High |
|  High Cancellations | > 15% cancel rate per route | 9 routes | 🟡 Medium |
|  Below Target | < 60% achievement per SR | 10 SRs | 🔴 High |
|  Low Stock Critical | Stock = 0 pcs | 27 items | 🔴 Critical |
|  Low Stock Low | Stock 1–4 pcs | 166 items | 🟡 Low |
|  Sales Drop | > 30% day-over-day drop | 0 days | — |
| **TOTAL** | | **215 alerts** | |

Each alert contains: `Alert Type` · `Severity` · `Entity` · `Detail` · `Recommended Action`

**Output →** `alert_log_YYYY-MM-DD.csv` + pushed to  Alerts tab in Google Sheets

---

##  Key Results

```
╔══════════════════════════════════════════════════╗
║          MARCH 2026 — KPI SUMMARY               ║
╠══════════════════════════════════════════════════╣
║  Total Sales (BDT)    :  ৳202,268,682           ║
║  Total Target (BDT)   :  ৳211,575,000           ║
║  Target Achievement   :  95.6%                  ║
║  Total Orders         :  4,991                  ║
║  Active SRs           :  150                    ║
║  Shops Visited        :  3,847                  ║
║  Avg Visit Duration   :  15.5 min               ║
║  Confirmation Rate    :  59.8%                  ║
╚══════════════════════════════════════════════════╝
```

**Top 3 Routes by Sales:**
1. Dhanmondi — ৳21,875,918
2. Savar — ৳21,379,276
3. Mirpur-1 — ৳21,294,838

**Top 3 Products by Sales:**
1. Vision LED TV 43 — ৳14,923,469
2. Vision LED TV 32 — ৳14,617,475
3. RFL Food Carrier — ৳14,339,348

---

##  Alert Engine Output

```
============================================================
       SALES AUTOMATION — ALERT REPORT
       Generated: 12 May 2026
============================================================
  Total Alerts  :  215
  High          :  13
  Medium        :  202
============================================================

  GPS Mismatch  (3 alerts)
  SR-14  →  Mismatch rate: 20.8% (5 of 24 visits)
  SR-73  →  Mismatch rate: 22.2% (4 of 18 visits)
  SR-86  →  Mismatch rate: 21.2% (7 of 33 visits)

  High Cancellations  (9 routes)
  Chittagong    →  Cancel rate: 16.9% (84 of 498 orders)
  Dhanmondi     →  Cancel rate: 16.3% (84 of 515 orders)
  Gazipur       →  Cancel rate: 17.5% (86 of 491 orders)
  Uttara-10     →  Cancel rate: 19.0% (94 of 495 orders)
  ... 5 more routes

  Below Target  (10 SRs)
  SR-17   →  Achievement: 40.7%  |  Sales: ৳631,203
  SR-109  →  Achievement: 46.3%  |  Sales: ৳717,341
  SR-69   →  Achievement: 49.9%  |  Sales: ৳619,253
  ... 7 more SRs

  Low Stock  (193 alerts)
  Azad Store          →  RFL Food Carrier — 0 pcs  (Uttara-3)
  New Market Traders  →  Vision Fan — 0 pcs  (Chittagong)
  Bismillah Store     →  RFL Rice Container — 0 pcs  (Mohammadpur)
  ... 190 more items
```

---

## Google Sheets Report

The notebook automatically creates and formats a Google Sheets report with 7 tabs.
No manual formatting required — everything is pushed by Python.

 **View Live Report:**
`https://docs.google.com/spreadsheets/d/1czkaf4Dg-ey-phXdUHHJiPoyYTAU2QNxj_MoUGVxnsM`

---

##  Interactive Dashboard

`dashboard/index.html` is a standalone interactive dashboard.
Open it by double-clicking in any browser — Chrome, Firefox, Edge.
No server or installation needed.

**Charts included:**
- 6 KPI summary cards
- Sales by Route — horizontal bar chart
- Daily Sales Trend — line chart with average line
- Sales by Product Category — donut chart
- Top 10 Products — bar chart
- Sales vs Target — gauge chart
- SR Performance — table with color-coded tiers
- Payment Mode / Order Status / GPS — donut charts
- Low Stock Alerts — table

**5 Live Filters:** Route · Category · Payment Mode · Order Status · GPS Status

---

##  How to Run

### Step 1 — Set up Google Drive

Create this folder in your Google Drive and upload the dataset:

```
My Drive/
└── Sales_Automation/
    └── Data/
        └── Raw/
            └── spro_sample.csv
```

### Step 2 — Open in Google Colab

1. Go to [colab.research.google.com](https://colab.research.google.com)
2. **File → Upload notebook** → select `Sales_Automation.ipynb`
3. Click **Runtime → Run all**
4. When prompted, sign in with your Google account to mount Drive

### Step 3 — View your outputs

| Output | Location |
|---|---|
| Cleaned dataset | `My Drive/Sales_Automation/Data/Processed/cleaned_data.csv` |
| KPI data | `My Drive/Sales_Automation/Data/Processed/kpi_output.json` |
| Sales trend chart | `My Drive/Sales_Automation/Output/daily_sales_trend.png` |
| Alert log | `My Drive/Sales_Automation/Output/Alerts/alert_log_YYYY-MM-DD.csv` |
| Google Sheets report | URL printed in notebook output |

### Step 4 — Open the dashboard

Double-click `dashboard/index.html` — opens in any browser instantly.

*Dataset is synthetically generated to mirror PRAN-RFL SPRO field transaction
structure for demonstration purposes only.*
