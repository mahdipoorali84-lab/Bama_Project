# Bama Car Market: Data Analysis, ML Imputation & Power BI Dashboard

## 📊 Project Overview
This project provides an end-to-end data analysis pipeline on a comprehensive real-world dataset of used car listings from **Bama** (Iranian automotive marketplace). The pipeline covers data cleaning, missing value imputation using Machine Learning (`HistGradientBoostingRegressor`), multi-threshold outlier handling, advanced feature engineering (storytelling features), Exploratory Data Analysis (EDA), and an interactive **Power BI dashboard** for market exploration and pricing insights.

---

## 🔍 Key Data Engineering & Processing Steps

1. **Data Cleaning & Type Casting:**
   - Stripped Persian measurement units (`لیتر`, `ثانیه`, `لیتر در صد کیلومتر`, `km`) and normalized text columns.
   - Handled ambiguous/negotiable price entries (`توافقی`) and near-zero/new vehicle mileage records (`صفر کیلومتر`).
   - Standardized Gregorian calendar years to Persian Jalali solar years.

2. **Advanced ML-Based Missing Value Imputation:**
   - Instead of dropping rows or simple mean/median filling, trained separate `HistGradientBoostingRegressor` models to predict missing technical specs (`engine_capacity`, `acceleration`, `combined_fuel_consumption`) based on remaining vehicle features, achieving exceptional accuracy:
     - **Engine Capacity Imputation:** R² ≈ 0.99
     - **Acceleration Imputation:** R² ≈ 0.98
     - **Fuel Consumption Imputation:** R² ≈ 0.98

3. **Multi-Threshold Outlier Capping & Filtering:**
   - **Price:** Filtered extreme right-skewed anomalies using the 97.5th percentile threshold.
   - **Mileage:** Capped at the 99th percentile.
   - **Engine Capacity:** Capped at the 98.5th percentile.
   - **Fuel Consumption:** Cleaned using the 3-sigma rule (mean ± 3σ).

4. **Deduplication:**
   - Identified and dropped 18,254 duplicate rows from an initial pool of 56,267 raw records, arriving at a clean, robust final dataset of **32,418 unique listings and 29 columns**.

---

## 🌟 Feature Engineering & Storytelling Features
Engineered custom features to extract deep market behavioral patterns:
- **`brand` & `model`**: Extracted cleanly from the raw vehicle title string.
- **`city` & `location`**: Extracted and standardized from raw Persian address strings.
- **`car_age`**: Calculated dynamically based on current Jalali year (`jdatetime`).
- **`annual_mileage` & `usage_intensity_ratio`**: Derived usage intensity normalized against standard driving baselines.
- **`price_per_mileage`**: Log-transformed ratio evaluating economic value relative to wear and tear.
- **`agility_index`**: Inverse acceleration-to-displacement ratio measuring vehicle agility.
- **`engine_class`**: Categorized engines into `Small` (≤ 1.5L), `Medium`, and `Large` (> 2.0L) using conditional numpy selectors.

---

## 📈 Exploratory Data Analysis (EDA) Highlights
- Analyzed price depreciation curves across vehicle age, grouped by engine class and transmission type.
- Generated correlation matrices and pairplots confirming expected economic relationships (e.g., strong negative correlation between car age/mileage and price, positive correlation for engine capacity and modern acceleration indexes).
- Exported the final processed dataset to `cleaned_bama.csv` for downstream BI reporting.

---

## 📊 Power BI Dashboard & Visualizations (`dashboard.pbix`)

The project features a multi-page, interactive Power BI dashboard designed with a modern dark-green theme (`Bama.ir` brand identity), featuring custom DAX measures, advanced data modeling, and dedicated navigation tabs (*Overview*, *Price*, and *Quality*).

### 1. Overview Page (`overview`)
- **Core KPI Cards:** Tracks total distinct car models (309), total market advertisements (1.484B), and the proportion of problem/damaged vehicles (22.3%).
- **Vehicle Condition Share:** Donut chart analyzing the ratio of clean/healthy vehicles vs. damaged/problematic listings (74.17% vs. 25.83%).
- **City-wise Average Mileage:** Bar chart comparing average vehicle usage/mileage across various Iranian cities (e.g., Tehran, Isfahan, Shiraz).
- **Price Distribution by Vehicle Age:** Area distribution chart mapping vehicle counts and age profiles against market pricing (up to 10bn Tomans).

### 2. Price Analytics Page (`price`)
- **Key Pricing Metrics:** Displays maximum market price (~10.55bn Tomans) and minimum listing price (~1M Tomans), alongside inventory counts.
- **Verification & Authentication Share:** Donut chart breaking down authenticated vs. unauthenticated listings (83.96% verified).
- **Liquidity & Agility Scatter Plot:** Evaluates the correlation between vehicle price and agility indexes, segmented by transmission type (Automatic vs. Manual).
- **Impact of Body Damage / Paint:** Horizontal bar breakdown showing valuation depreciation across body conditions (e.g., unpainted, multi-piece paint, replaced bonnet/doors).
- **Brand Pricing Matrix & Trends:** Interactive matrix showing average listing prices per brand (e.g., Pride, Peugeot, Dena) alongside multi-metric trends tracking mileage, annual usage, and price-per-mileage over model years.

### 3. Quality & Technical Insights Page (`quality`)
- **Fuel Efficiency KPI Gauge:** Radial gauge monitoring average combined fuel consumption (6.83 L/100km against a 20L ceiling).
- **Transmission Market Share:** Donut chart depicting market dominance between automatic and manual gearboxes (64.6% vs. 35.4%).
- **Fuel Type Breakdown:** Granular categorical breakdown of market preferences (gasoline 84.79%, hybrid, dual-fuel, electric).
- **Engine Class Performance Dynamics:** Multi-metric bar and line combo chart comparing acceleration, agility, and freshness scores across engine sizes (`Small` <= 1.5L, `Medium`, `Large` > 2.0L).
- **Powertrain & Engine Capacity Distribution:** Horizontal distribution charts mapping engine displacement across various cylinder and turbo configurations (e.g., M20D, A25, M20C).

