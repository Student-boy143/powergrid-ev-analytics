# EV Charging Network — Time-of-Day & Day-of-Week Analysis (Analysis 7)

## 1. Executive Summary

This report delivers a comprehensive empirical investigation into the temporal demand dynamics of the UK EV Charging Network across 1,096 calendar days (January 1, 2022 to December 31, 2024), analyzing 294,024 charging sessions delivering 13,808,685 kWh of electricity and generating £8,171,098 in total revenue.

### Key Empirical Findings:
- **Diurnal Load Curve & Plateau**: Charging demand exhibits a concentrated 5-hour peak plateau from **10:00 to 14:00**, capturing **40.97% of total daily volume** (120,450 sessions). The single peak hour occurs at **10:00 AM** with 24,214 sessions (mean 22.09 sessions/day; 8.24% of network volume).
- **Overnight Trough**: Demand drops into a deep overnight valley between **23:00 and 05:00**, where each hour accounts for less than 1% of daily volume. The absolute trough occurs at **03:00 AM** with only 2,633 sessions (2.40 sessions/day; 0.90% share), establishing a **9.20x peak-to-trough demand ratio**.
- **Significant Weekday vs. Weekend Disparity**: Weekdays (Monday–Friday) average **290.74 daily sessions** delivering **13,650.77 kWh/day**, compared to weekends (Saturday–Sunday) averaging **212.31 daily sessions** delivering **9,980.40 kWh/day**. Weekday demand is **36.94% higher** than weekend demand. Non-parametric hypothesis testing confirms this difference is statistically highly significant (**Mann-Whitney U = 164,097.0, p = 2.73 × 10⁻¹⁸**).
- **Day-of-Week Consistency**: Tuesday is the highest-demand day (45,922 sessions; 292.50 sessions/day), closely followed by Friday (291.85 sessions/day) and Monday (291.29 sessions/day). Saturday and Sunday record identical low volumes (212.32 and 212.29 sessions/day).
- **Invariance of Energy Delivered per Session**: Across all 24 hours of the day and all 7 days of the week, the average energy delivered per session remains remarkably constant between **46.7 kWh and 47.2 kWh** (overall mean: 46.96 kWh). Temporal variation in total power demand is driven entirely by arrival volume (fleet utilization), not by variations in vehicle battery intake.
- **Charger Type Throughput & Bay Utilization**: Each charger rating accounts for approximately one-third of total delivered energy (~32.7% for DC_50kW, 34.4% for DC_150kW, and 32.9% for DC_300kW). However, bay dwell times diverge substantially: DC_50kW sessions average **49.48 minutes**, DC_150kW average **24.50 minutes**, and DC_300kW average just **19.49 minutes**.

---

## 2. Project Objectives

1. Determine precise network-wide peak charging hours, ramp-up periods, and overnight low-demand troughs.
2. Establish whether weekend charging behavior differs significantly from weekday patterns using appropriate statistical testing.
3. Compare charging volume and energy intensity across Monday through Sunday.
4. Evaluate monthly and seasonal stability while accounting for historical network expansion from 8 stations (2022) to 33 stations (2024).
5. Compare hourly demand profiles across all nine UK regions to identify geographic synchronization or divergence.
6. Investigate temporal utilization and throughput disparities across DC_50kW, DC_150kW, and DC_300kW charger types.
7. Provide defensible operational recommendations for staffing, grid connection management, and time-of-use pricing frameworks.

---

## 3. Dataset Overview

The analysis is based on the primary `sessions`, `chargers`, and `stations` sheets from the raw Excel dataset (`data/raw/ev_charging_dataset.xlsx`):

| Dataset Table | Record Count | Unique Identifiers | Key Attributes |
|---|---:|---|---|
| `sessions` | 294,024 | `session_id` (294,024 unique) | Start/end timestamps, duration, energy (kWh), price/kWh, total cost, station region, charger type |
| `chargers` | 199 | `charger_id` (199 unique) | Station ID, charger type (50/150/300 kW), installation date |
| `stations` | 33 | `station_id` (33 unique) | Station name, UK region, activation year, charger count |

### Data Quality & Integrity Validation:
- Missing values: **0** across all 15 session columns.
- Duplicate records: **0** duplicate session IDs.
- Date range: **2022-01-01 01:30:00 to 2024-12-31 23:50:00** (exactly 1,096 consecutive calendar days).
- Session duration: Range **15.0 to 60.0 minutes** (mean: 35.09 min, median: 27.0 min).
- Energy delivered: Range **21.70 to 80.00 kWh** (mean: 46.96 kWh, median: 43.30 kWh).
- Total cost consistency: $\text{total\_cost} \approx \text{energy\_kwh} \times \text{price\_per\_kwh}$ with maximum rounding discrepancy of £0.005 (half-penny rounding).

---

## 4. Data Preparation & Feature Engineering

All timestamps were parsed to standardized datetime objects. From `start_timestamp`, the following features were constructed:
1. `hour`: Hour of arrival (integer 0–23).
2. `date`: Calendar date.
3. `day_of_week`: Day index (0 = Monday, 6 = Sunday).
4. `day_name`: Categorical day name (Monday through Sunday).
5. `is_weekend`: Boolean flag (True for Saturday and Sunday).
6. `day_type`: Categorical label (`Weekday` vs. `Weekend`).
7. `year_month`: Monthly period (e.g., `2024-05`).
8. `season`: Meteorological season:
   - Winter: December, January, February
   - Spring: March, April, May
   - Summer: June, July, August
   - Autumn: September, October, November

No observations were dropped or silently imputed, preserving complete analytical reproducibility.

---

## 5. Methodology

1. **Diurnal Aggregation**: Sessions, energy (kWh), and revenue were aggregated by hour of the day. To prevent confounding session counts across an unequal number of calendar days, hourly averages per day were computed by dividing by the exact number of observed calendar days (1,096 days).
2. **Distributional & Normality Testing**: Daily session volume series for weekdays ($n = 782$) and weekends ($n = 314$) were evaluated for normality using the **Shapiro-Wilk test**.
3. **Hypothesis Testing**: Because daily volume distributions were strongly non-normal ($p < 10^{-17}$), non-parametric **Mann-Whitney U tests** were applied as the primary defensible statistical test, supplemented by parametric **Welch's t-tests** for robustness.
4. **Calendar Day Normalization**: Total Monday through Sunday volumes were normalized by the exact occurrence count of each weekday in the 3-year calendar (157 Mondays, 157 Tuesdays, 156 Wednesdays, 156 Thursdays, 156 Fridays, 157 Saturdays, 157 Sundays).
5. **Cross-Dimensional Slicing**: Hourly demand profiles were cross-tabulated against geographic regions and charger power tiers.

---

## 6. Detailed Analytical Findings

### 6.1 Hourly Demand Profile (24-Hour Diurnal Curve)

| Hour | Total Sessions | Session Share (%) | Total Energy (kWh) | Avg Sessions / Day | Avg Energy / Day (kWh) | Avg kWh / Session |
|---:|---:|---:|---:|---:|---:|---:|
| 00:00 | 2,754 | 0.94% | 128,740.8 | 2.51 | 117.46 | 46.75 |
| 01:00 | 2,677 | 0.91% | 125,309.7 | 2.44 | 114.33 | 46.81 |
| 02:00 | 2,719 | 0.92% | 127,569.8 | 2.48 | 116.40 | 46.92 |
| 03:00 | 2,633 | 0.90% | 124,141.6 | 2.40 | 113.27 | 47.15 |
| 04:00 | 2,705 | 0.92% | 127,238.3 | 2.47 | 116.09 | 47.04 |
| 05:00 | 5,360 | 1.82% | 250,529.5 | 4.89 | 228.59 | 46.74 |
| 06:00 | 7,985 | 2.72% | 375,601.5 | 7.29 | 342.70 | 47.04 |
| 07:00 | 10,648 | 3.62% | 498,779.7 | 9.72 | 455.09 | 46.84 |
| 08:00 | 15,967 | 5.43% | 750,802.3 | 14.57 | 685.04 | 47.02 |
| 09:00 | 21,370 | 7.27% | 1,004,475.5 | 19.50 | 916.49 | 47.00 |
| **10:00** | **24,214** | **8.24%** | **1,134,524.0** | **22.09** | **1,035.15** | **46.85** |
| 11:00 | 24,048 | 8.18% | 1,129,683.7 | 21.94 | 1,030.73 | 46.98 |
| 12:00 | 23,945 | 8.14% | 1,123,218.3 | 21.85 | 1,024.83 | 46.91 |
| 13:00 | 24,123 | 8.20% | 1,131,742.8 | 22.01 | 1,032.61 | 46.92 |
| 14:00 | 24,120 | 8.20% | 1,131,890.0 | 22.01 | 1,032.75 | 46.93 |
| 15:00 | 21,251 | 7.23% | 1,004,073.7 | 19.39 | 916.13 | 47.25 |
| 16:00 | 18,750 | 6.38% | 881,564.0 | 17.11 | 804.35 | 47.02 |
| 17:00 | 15,992 | 5.44% | 748,783.4 | 14.59 | 683.20 | 46.82 |
| 18:00 | 13,480 | 4.58% | 634,131.4 | 12.30 | 578.59 | 47.04 |
| 19:00 | 10,628 | 3.61% | 496,195.6 | 9.70 | 452.73 | 46.69 |
| 20:00 | 7,959 | 2.71% | 375,641.0 | 7.26 | 342.74 | 47.20 |
| 21:00 | 5,353 | 1.82% | 252,607.5 | 4.88 | 230.48 | 47.19 |
| 22:00 | 2,677 | 0.91% | 125,868.9 | 2.44 | 114.84 | 47.02 |
| 23:00 | 2,666 | 0.91% | 125,632.8 | 2.43 | 114.63 | 47.12 |
| **Total** | **294,024** | **100.00%** | **13,808,685.0** | **268.27** | **12,599.16** | **46.96** |

**Diurnal Curve Structure**:
1. *Overnight Baseline (22:00–04:00)*: Demand remains virtually flat at ~2.4–2.5 sessions per day per hour.
2. *Morning Incline (05:00–09:00)*: Rapid acceleration from 4.89 sessions/day at 05:00 to 19.50 sessions/day at 09:00 (+299% growth).
3. *Midday Peak Plateau (10:00–14:00)*: Demand reaches steady capacity saturation, fluctuating by less than 1.1% across the 5 hours (21.85 to 22.09 sessions/day).
4. *Evening De-escalation (15:00–21:00)*: Steady, linear descent from 19.39 sessions/day at 15:00 down to 4.88 at 21:00.

---

### 6.2 Weekday versus Weekend Statistical Analysis

| Day Type | Days Observed | Total Sessions | Mean Daily Sessions | Std Daily Sessions | Mean Daily Energy (kWh) | Avg Energy / Session (kWh) |
|---|---:|---:|---:|---:|---:|---:|
| **Weekday** | 782 | 227,360 | **290.74** | 155.85 | 13,650.77 | 46.95 |
| **Weekend** | 314 | 66,664 | **212.31** | 113.02 | 9,980.40 | 47.01 |
| **Difference** | — | — | **+78.43 (+36.94%)** | — | **+3,670.37 (+36.78%)** | -0.06 (-0.13%) |

#### Rigorous Hypothesis Testing:
1. **Normality Test (Shapiro-Wilk)**:
   - Weekday daily sessions: $W = 0.8332, p = 8.59 \times 10^{-28}$ (Reject null hypothesis of normality).
   - Weekend daily sessions: $W = 0.8313, p = 7.88 \times 10^{-18}$ (Reject null hypothesis of normality).
   - *Interpretation*: The multi-year expansion trend from 2022 to 2024 creates a right-skewed, multi-modal distribution of daily totals. Standard t-test assumptions of normality are strictly violated.
2. **Primary Test — Non-Parametric Mann-Whitney U Test**:
   - $U = 164,097.0$
   - $p\text{-value} = 2.73 \times 10^{-18}$
   - *Conclusion*: We reject the null hypothesis of equal distributions at $p < 0.001$. Weekday session demand is significantly higher than weekend demand.
3. **Energy Demand Test**:
   - Mann-Whitney U on daily energy: $U = 163,958.0, p = 3.55 \times 10^{-18}$. Confirms daily electricity draw is also significantly higher on weekdays.
4. **Parametric Welch's t-test (Robustness Check)**:
   - $t = 9.2607, \text{df} = 789.09, p = 1.88 \times 10^{-19}$.

---

### 6.3 Day-of-Week Distribution (Monday through Sunday)

| Day Name | Days in Dataset | Total Sessions | Session Share (%) | Total Energy (kWh) | Avg Sessions / Day | Avg Energy / Day (kWh) | Avg kWh / Session | Avg Duration (min) |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| **Monday** | 157 | 45,733 | 15.55% | 2,142,123.0 | 291.29 | 13,644.10 | 46.84 | 35.21 |
| **Tuesday** | 157 | **45,922** | **15.62%** | **2,150,639.0** | **292.50** | **13,698.34** | 46.83 | 35.16 |
| **Wednesday** | 156 | 45,106 | 15.34% | 2,124,099.9 | 289.14 | 13,616.03 | 47.09 | 35.02 |
| **Thursday** | 156 | 45,070 | 15.33% | 2,122,028.4 | 288.91 | 13,602.75 | 47.08 | 35.01 |
| **Friday** | 156 | 45,529 | 15.48% | 2,136,010.7 | 291.85 | 13,692.38 | 46.92 | 35.14 |
| **Saturday** | 157 | 33,335 | 11.34% | 1,564,654.5 | 212.32 | 9,965.95 | 46.94 | 35.11 |
| **Sunday** | 157 | 33,329 | 11.34% | 1,569,190.3 | 212.29 | 9,994.84 | 47.08 | 34.98 |

**Day-of-Week Takeaway**:
- Tuesdays record the highest absolute volume (45,922 sessions) and daily average (292.50 sessions/day).
- All five weekdays cluster within 1.2% of each other (288.91 to 292.50 sessions/day).
- Saturday and Sunday show virtually identical usage (212.32 vs. 212.29 sessions/day), a 27.4% drop relative to Tuesday.
- Commercial fleet and commuter activity during business days are the primary drivers of network load.

---

### 6.4 Monthly & Seasonal Trends

| Year | Season | Sessions | Total Energy (kWh) | Revenue (£) | Avg Energy / Session (kWh) | Active Regions | Active Stations |
|---|---|---:|---:|---:|---:|---:|---:|
| 2022 | Spring | 10,343 | 510,543.5 | £261,200.50 | 49.36 | 5 | 8 |
| 2022 | Summer | 10,271 | 509,367.6 | £260,321.83 | 49.59 | 5 | 8 |
| 2022 | Autumn | 10,074 | 499,811.1 | £255,849.29 | 49.61 | 5 | 8 |
| 2022 | Winter | 9,952 | 492,105.8 | £251,400.18 | 49.45 | 5 | 8 |
| 2023 | Spring | 21,658 | 1,018,094.8 | £651,064.59 | 47.01 | 9 | 18 |
| 2023 | Summer | 21,765 | 1,025,241.7 | £654,701.04 | 47.11 | 9 | 18 |
| 2023 | Autumn | 21,407 | 1,004,102.6 | £641,998.73 | 46.91 | 9 | 18 |
| 2023 | Winter | 21,302 | 1,001,685.4 | £640,009.56 | 47.02 | 9 | 18 |
| 2024 | Spring | 41,886 | 1,939,338.8 | £1,140,597.67 | 46.30 | 9 | 33 |
| 2024 | Summer | 41,852 | 1,940,814.2 | £1,141,057.22 | 46.37 | 9 | 33 |
| 2024 | Autumn | 41,566 | 1,924,468.1 | £1,130,743.44 | 46.30 | 9 | 33 |
| 2024 | Winter | 41,948 | 1,943,172.2 | £1,142,156.07 | 46.32 | 9 | 33 |

**Critical Methodological Insight on Seasonality**:
Within any given calendar year (e.g., 2024), seasonal demand fluctuates by less than 0.9% (Spring: 41,886 vs. Autumn: 41,566). EV charging demand on this network is not seasonal. The apparent annual growth (+111.9% in 2023 and +94.2% in 2024) is entirely structural, driven by:
1. Physical network expansion: 8 stations in 2022 $\rightarrow$ 18 in 2023 $\rightarrow$ 33 in 2024.
2. Regional rollout: 5 active regions in 2022 expanding to all 9 regions in 2023–2024.
3. Market-wide EV fleet penetration.

---

### 6.5 Regional Temporal Patterns

| Region | Total Sessions | Peak Hour | Peak Sessions | Peak Regional Share | Weekday Sessions | Weekend Sessions | Weekday Share (%) |
|---|---:|---:|---:|---:|---:|---:|---:|
| **North West** | 65,789 | 11:00 | 5,446 | 8.28% | 50,776 | 15,013 | 77.18% |
| **Midlands** | 50,598 | 13:00 | 4,228 | 8.36% | 39,091 | 11,507 | 77.26% |
| **London** | 40,760 | 10:00 | 3,436 | 8.43% | 31,644 | 9,116 | 77.63% |
| **North East** | 30,439 | 13:00 | 2,532 | 8.32% | 23,492 | 6,947 | 77.18% |
| **South West** | 30,401 | 14:00 | 2,552 | 8.39% | 23,456 | 6,945 | 77.16% |
| **South East** | 25,168 | 10:00 | 2,142 | 8.51% | 19,454 | 5,714 | 77.30% |
| **Yorkshire & Humber** | 20,468 | 12:00 | 1,727 | 8.44% | 15,856 | 4,612 | 77.47% |
| **Scotland** | 20,242 | 14:00 | 1,677 | 8.28% | 15,700 | 4,542 | 77.56% |
| **Wales** | 10,159 | 10:00 | 847 | 8.34% | 7,891 | 2,268 | 77.67% |

**Regional Findings**:
- Peak hour synchronization: Across all 9 regions, the peak hour invariably falls within the 10:00–14:00 plateau window (London, South East, Wales peak at 10:00; North West at 11:00; Yorkshire at 12:00; Midlands and North East at 13:00; Scotland and South West at 14:00).
- Weekday share consistency: Every region exhibits an almost identical weekday volume share of **77.15% to 77.67%**.

---

### 6.6 Charger-Type Temporal Behavior

| Charger Type | Total Sessions | Session Share (%) | Total Energy (kWh) | Energy Share (%) | Total Revenue (£) | Mean Duration (min) | Mean Energy / Session (kWh) | Mean Price (£/kWh) |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| **DC_50kW** | 136,821 | 46.53% | 4,511,045.8 | 32.67% | £2,680,348.57 | **49.48** | 32.97 | £0.5942 |
| **DC_150kW** | 96,977 | 32.98% | 4,748,473.8 | 34.39% | £2,807,082.52 | **24.50** | 48.96 | £0.5912 |
| **DC_300kW** | 60,226 | 20.48% | 4,549,226.2 | 32.94% | £2,683,669.03 | **19.49** | **75.54** | £0.5900 |

**Operational Disparities**:
1. *Energy Delivery Symmetry*: Each tier accounts for nearly one-third of all electricity delivered.
2. *Dwell Time Bottlenecks*: DC_50kW chargers require **49.48 minutes** per session, consuming bay capacity 2.54x longer than DC_300kW units (19.49 minutes), yet delivering less than half the energy (32.97 vs. 75.54 kWh).
3. *Pricing Distortion*: DC_300kW chargers cost essentially the same per kWh (£0.5900) as DC_50kW units (£0.5942), creating no market mechanism to ration high-power bays or recover ultra-rapid capital expenditure.

---

## 7. Power BI Dashboard Architecture & Visualization

The executive dashboard **EV Charging Network — Time-Based Demand Analysis** was designed at 1920 × 1080 resolution:
- **KPI Row**: Total Sessions (294K), Total Energy Delivered (13.81M kWh), Peak Hour (10:00 AM), Highest-Demand Day (Tuesday).
- **Diurnal Load Curve**: Highlighting the 10:00–14:00 plateau window.
- **Weekday vs. Weekend**: Displaying the 36.94% weekday premium and statistical significance ($p < 0.001$).
- **Regional Profiles**: Comparing diurnal load across the 9 UK regions.
- **Charger Type Throughput & Duration**: Illustrating the dwell time contrast between 50kW and 300kW chargers.

Exported Dashboard Artifact:
`tasks/task_07_time_day_analysis/powerbi/time_day_analysis_dashboard.png`

---

## 8. Business Recommendations

### 8.1 Evidence-Backed Operational Insights (Supported Directly by Data)
1. **Grid Capacity Management During Peak Plateau (10:00–14:00)**: Over 40.97% of daily network energy is drawn during these 5 hours. Network operators should coordinate with Distribution Network Operators (DNOs) to implement automated demand response or on-site battery storage (BESS) to shave peak load and avoid capacity surcharge penalties.
2. **Staffing and Field Maintenance Scheduling**: Field maintenance, firmware updates, and preventive servicing should strictly be scheduled during the low-demand overnight window (23:00–05:00), where asset idle rates exceed 90%. Scheduling outages during 10:00–14:00 directly impairs customer throughput and network revenue.
3. **Overnight Off-Peak Incentivization**: Asset utilization falls below 1% per hour between 23:00 and 05:00. This presents a prime opportunity for off-peak commercial discounting to shift price-sensitive fleet and overnight charging.

### 8.2 Strategic & Future Hypotheses (Requiring Commercial Validation)
1. **Dynamic Time-of-Use (TOU) Pricing**: Implement peak surcharges during 10:00–14:00 and off-peak incentives overnight (to be modelled and evaluated in Analysis 8).
2. **Charger Speed Premium**: Introduce tiered pricing reflecting charging speed (e.g., premium for DC_300kW ultra-rapid bays) to manage queueing times and better align customer value with asset velocity.

---

## 9. Analytical Limitations

1. **Uncontrolled Vehicle Battery Limits**: The dataset records session duration and delivered energy, but does not record vehicle battery state-of-charge (SoC) at arrival. Differences in energy delivered per session may reflect vehicle onboard battery capacities rather than customer charging preference.
2. **Structural Expansion Confounding**: Network growth occurred in discrete steps (stations opening across 2022–2024). Longitudinal time trends must not be interpreted as organic EV market adoption alone.
3. **Absence of Rejection/Queueing Data**: We observe completed sessions, but cannot observe lost sales or drivers who arrived at a station and left due to long queues.

---

## 10. Conclusion

Analysis 7 establishes that UK EV charging network demand is heavily concentrated during midday hours (10:00–14:00) on weekdays, characterized by consistent energy intake per session and severe bay dwell disparities across charger types. These empirical findings provide the direct foundation for the dynamic pricing framework in **Analysis 8**.

