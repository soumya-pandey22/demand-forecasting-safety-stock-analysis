# 📦 Demand Forecasting & Statistical Safety Stock Analysis

## 🔎 Overview
This project extends the analysis from my earlier
[Inventory Optimization: EOQ Analysis](https://github.com/soumya-pandey22/inventory-optimization-eoq)
project. It evaluates the accuracy of the dataset's built-in demand forecasts and
replaces the earlier project's simple max-min Safety Stock heuristic with a proper
statistical method based on demand variability and service levels.

## 🎯 Objective
To move from a rough heuristic to a statistically grounded approach for sizing
Safety Stock and Reorder Point, and to evaluate whether the improvement is
meaningful in practice.

## 🗂️ Dataset
- **File**: `retail_store_inventory.csv`
- **Size**: 73,100 rows, 15 columns
- **Date range**: 2022-01-01 to 2024-01-01 (2 years)
- **Key columns used**: Date, Category, Units Sold, Demand Forecast, Discount,
  Holiday/Promotion, Weather Condition

## 🛠️ Methodology
1. **Rebuilt Part 1 Results** — annual demand, average/max daily demand, old cost
   assumptions, old Safety Stock and Reorder Point (heuristic method).
2. **Forecast Accuracy Check** — compared the dataset's `Demand Forecast` column
   against actual `Units Sold` to measure forecast error per category.
3. **Demand Distribution Check** — visualized daily demand per category and ran a
   Shapiro-Wilk normality test (`scipy.stats`) to check the statistical assumption
   behind the Z-score Safety Stock method.
4. **Demand Variability** — calculated standard deviation of daily demand per
   category.
5. **Service Level & Z-score** — chose a 95% service level and converted it to a
   Z-score using `scipy.stats.norm.ppf`.
6. **New Safety Stock**:
   `Safety Stock = Z × std_dev × sqrt(lead_time)`
7. **New Reorder Point**:
   `ROP = (avg_daily_demand × lead_time) + New Safety Stock`
8. **Old vs New Comparison** — compared both metrics against Part 1's heuristic
   results, with grouped bar charts.
9. **Demand Drivers** — tested whether Discount, Holiday/Promotion, or Weather
   Condition correlate with demand, using `scipy.stats.pearsonr` and group
   comparisons.

## 📊 Key Findings
- 📉 **Forecast Accuracy**: The dataset's forecast slightly over-predicted actual
  sales by ~5 units per row on average, consistently across all categories.
- 📐 **Normality**: No category strictly passed the normality test (p < 0.05 for
  all), though the Z-score method was still applied as a standard, widely-used
  simplification.
- 🔑 **Old vs New Safety Stock**: The statistical method reduced Safety Stock by
  roughly **5–6x** across every category (e.g. Clothing: ~17,445 → ~3,177 units),
  and Reorder Point dropped by roughly **35–40%** — showing the original heuristic
  was substantially over-buffering inventory.
- 🌦️ **Demand Drivers**: Discount, Holiday/Promotion, and Weather Condition all
  showed negligible or statistically insignificant relationships with demand —
  a pattern that, combined with earlier observations, suggests this dataset may
  be synthetically generated rather than reflecting real retail behavior.

## ⚠️ Limitations
- Daily demand does not pass a strict statistical normality test, even though the
  Z-score method assumes approximate normality.
- The 95% service level is a chosen assumption, not derived from real business
  cost tradeoffs.
- Findings around demand drivers suggest the dataset may not fully reflect
  real-world retail dynamics.

## 🧰 Tech Stack
Python, Pandas, NumPy, Matplotlib, Seaborn, SciPy

## 🔗 Related Project
[Inventory Optimization: EOQ Analysis](https://github.com/soumya-pandey22/inventory-optimization-eoq)
— the original project this one builds on.
