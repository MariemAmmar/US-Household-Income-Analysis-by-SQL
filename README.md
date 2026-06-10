# 🏠 US Household Income — SQL Data Cleaning & Exploratory Analysis

An end-to-end SQL project that cleans and explores a large-scale US household income dataset, uncovering geographic and demographic income disparities across 54 states and territories using over 32,000 records.

---

## 📌 Project Overview

This project analyzes US household income data at the city and county level across all 50 states and territories. Using MySQL, the raw dataset is first cleaned to remove duplicates and fix data quality issues, then joined with an income statistics table to explore mean and median income patterns by state, city, and area type — providing insights that support socioeconomic research and policy planning.

---

## 📁 Repository Structure

```
├── USHouseholdIncome.csv                       # Raw geographic & demographic data
├── USHouseholdIncome_Statistics.csv            # Raw income statistics data
├── USHouseholdIncome.sql                       # Table creation & data import (income)
├── USHouseholdIncome_Statistics.sql            # Table creation & data import (statistics)
├── 13_3_US_Income_Data_Cleaning.sql            # Data cleaning queries
└── 13_4_US_Income_Exploratory_Data_Analysis.sql # EDA queries
```

---

## 🗄️ Dataset

### Table 1: USHouseholdIncome (~32,533 rows)

| Field | Description |
|---|---|
| `row_id` | Unique row identifier |
| `id` | Record ID (joins to statistics table) |
| `State_Code` | Numeric state code |
| `State_Name` | Full state name |
| `State_ab` | State abbreviation |
| `County` | County name |
| `City` | City name |
| `Place` | Census-designated place name |
| `Type` | Area type (City, Town, Track, CDP, Village, Borough, etc.) |
| `Primary` | Primary classification |
| `Zip_Code` | ZIP code |
| `Area_Code` | Phone area code |
| `ALand` | Land area (sq meters) |
| `AWater` | Water area (sq meters) |
| `Lat` / `Lon` | Geographic coordinates |

### Table 2: USHouseholdIncome_Statistics (~32,526 rows)

| Field | Description |
|---|---|
| `id` | Record ID (joins to income table) |
| `State_Name` | State name |
| `Mean` | Mean household income (USD) — ranges from $0 to $242,857 |
| `Median` | Median household income (USD) — ranges from $0 to $300,000 |
| `Stdev` | Standard deviation of income |
| `sum_w` | Statistical weight |

---

## 🧹 Part 1 — Data Cleaning

**File:** `13_3_US_Income_Data_Cleaning.sql`

### Steps performed:

**1. Initial Data Exploration**
- Queried both tables to understand structure, column types, and data distribution
- Identified quality issues including duplicates, blank fields, and inconsistent casing

**2. Duplicate Detection & Removal**
- Used `ROW_NUMBER() OVER (PARTITION BY id)` to detect duplicate records
- Removed duplicate `row_id` entries keeping only the first occurrence per ID

**3. Fixing Inconsistent State Name Casing**
- Identified records where `State_Name` was entered in lowercase (e.g., `'alabama'` instead of `'Alabama'`)
- Applied `UPPER()` / proper casing correction using `UPDATE` statements

**4. Handling Missing or Blank Values**
- Identified NULL and blank values across key fields
- Applied targeted fixes for blank `Place` and `Type` fields where recoverable
- Flagged unresolvable nulls for exclusion in analysis

**5. Data Type & Format Validation**
- Verified numeric fields (`ALand`, `AWater`, `Zip_Code`) were stored correctly
- Confirmed join key consistency between both tables on the `id` field

---

## 🔍 Part 2 — Exploratory Data Analysis

**File:** `13_4_US_Income_Exploratory_Data_Analysis.sql`

### Key analyses:

**Geographic Land & Water Distribution**
- Aggregated `ALand` and `AWater` by state using `SUM()`
- Identified the top 10 states by total land area and total water area
- Revealed significant geographic size disparities across US states

**Joining Income & Statistics Tables**
- Performed an `INNER JOIN` between both tables on `id`
- Combined geographic context (state, county, city, type) with income metrics (mean, median)

**State-Level Income Analysis**
- Calculated `AVG(Mean)` and `AVG(Median)` income grouped by state
- Ranked states from lowest to highest median income
- Highlighted stark income inequality between the wealthiest and least wealthy states

**Income by Area Type**
- Grouped income statistics by `Type` (City, Town, Track, CDP, Village, Borough)
- Used `HAVING COUNT(Type) > 100` to filter statistically significant area types
- Revealed that Cities consistently outperform Tracks and CDPs in average income

**City-Level Income Rankings**
- Identified the **lowest income cities** by averaging mean income per city per state
- Identified the **highest income cities** using `ORDER BY AVG(Mean) DESC LIMIT 10`
- Surfaced the specific cities and states at both extremes of the income spectrum

---

## 🛠️ Tools & Technologies

- **Database:** MySQL
- **Language:** SQL
- **Techniques:** `JOIN`, `GROUP BY`, `HAVING`, `ORDER BY`, `ROW_NUMBER()` window function, aggregate functions (`AVG`, `SUM`, `COUNT`), subqueries, `LIMIT`, data deduplication, string cleaning

---

## 💡 Key Findings

- **54 states and territories** represented across 32,500+ geographic records
- Mean household income ranges widely from **$0 to $242,857** across locations
- Median income ranges from **$0 to $300,000**, indicating extreme income inequality between regions
- **Cities** have significantly higher average incomes than rural area types like Tracks and CDPs
- The **top 10 highest-income cities** are concentrated in a small number of high-cost states
- States with the **largest land areas** do not necessarily correspond to the highest income levels

---

## 🚀 Getting Started

1. Clone this repository
2. Run `USHouseholdIncome.sql` and `USHouseholdIncome_Statistics.sql` to create and populate both tables
3. Run the data cleaning script: `13_3_US_Income_Data_Cleaning.sql`
4. Run the EDA script: `13_4_US_Income_Exploratory_Data_Analysis.sql`

> Tested on **MySQL 8.0+**. Window functions (`ROW_NUMBER`) require MySQL 8.0 or later.

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
