# EV Charging Network — Year-Over-Year Growth Analytics (Analysis 12)

## 1. Executive Summary

This report delivers a comprehensive longitudinal investigation into the growth and performance evolution of the UK EV Charging Network across 2022, 2023, and 2024. Analyzing 294,024 charging sessions delivering 13.81 million kWh and generating £8.17M in gross revenue, this study disentangles physical infrastructure rollout from organic station productivity and macroeconomic tariff volatility.

### Key Analytical & Empirical Findings:
- **Top-Line Expansion Trajectory**:
  - **2022**: 40,640 sessions | 2,011,828 kWh | £1,028,772 revenue | 8 stations | 43 chargers.
  - **2023**: 86,132 sessions (**+111.9%**) | 4,049,125 kWh (**+101.3%**) | £2,587,774 revenue (**+151.5%**) | 17 stations | 102 chargers.
  - **2024**: 167,252 sessions (**+94.2%**) | 7,747,793 kWh (**+91.3%**) | £4,554,554 revenue (**+76.0%**) | 33 stations | 199 chargers.
  - Cumulative 2-year expansion: **+311.6% session volume** and **+342.7% revenue growth**.
- **The Like-for-Like (LFL) Station Reality**:
  - The mature 2022 station cohort (8 stations, 43 chargers) generated **40,640 sessions in 2022**, **40,393 sessions in 2023 (-0.61%)**, and **40,424 sessions in 2024 (+0.08%)**.
  - Like-for-like organic demand growth at existing stations was essentially **0.0%**. Existing stations operate at structural capacity ceilings determined by charger bay turnover and connection constraints.
  - Consequently, **100.2% of network session volume growth** was driven by physical footprint expansion (activating 9 new stations in 2023 and 16 new stations in 2024).
- **Macro Tariff Decoupling**:
  - In 2023, revenue growth (+151.5%) substantially outpaced session growth (+111.9%) due to a **+25.0% increase** in weighted electricity tariffs (£0.511/kWh -> £0.639/kWh) during the UK energy crisis.
  - In 2024, revenue growth (+76.0%) lagged session growth (+94.2%) as retail tariffs settled back to £0.588/kWh (-8.0% YoY).
- **Productivity per Charger Stability**: Network-wide charger utilization remained remarkably stable across all three years: **2.59 sessions/charger/day in 2022**, **2.31 in 2023**, and **2.30 in 2024**. Newly commissioned stations achieved mature network productivity almost instantaneously.
- **Regional Maturation**: The North West remains the largest volume hub (65,789 cumulative sessions; 35,544 in 2024), followed by the Midlands (50,598 sessions) and London (40,760 sessions).

---

## 2. Business Objectives

1. Quantify multi-year growth across sessions, delivered energy, revenue, and customer engagement.
2. Unpack monthly trajectories across the 36-month timeline to evaluate seasonality vs. step-change commissioning.
3. Conduct station activation cohort analysis to separate like-for-like station growth from geographic rollout.
4. Measure regional expansion dynamics across the 9 UK regions.
5. Track charger-type deployment and evaluate whether the network is shifting toward ultra-rapid infrastructure.
6. Perform multiplicative growth factor decomposition (Stations $\times$ Charger Density $\times$ Charger Utilization $\times$ Energy Intake $\times$ Unit Tariff).
7. Formulate actionable commercial and infrastructure recommendations for executive planning.

---

## 3. Business Understanding: Unpacking EV Network Growth

Evaluating EV charging network growth requires analyzing volume, energy, revenue, and infrastructure concurrently:
1. **Network Footprint vs. Organic Demand**: Adding 16 new stations will inevitably double session counts. However, declaring commercial success without examining like-for-like performance obscures whether existing assets are growing, stagnating, or losing market share to local competitors.
2. **Volume vs. Revenue Decoupling**: Because revenue equals $\text{Energy} \times \text{Tariff}$, top-line revenue can expand during volume stagnation if electricity prices rise (as occurred in 2023), or contract during volume expansion if tariffs decline (as occurred in 2024).
3. **Charger Productivity as a Health Benchmark**: True asset health is measured by **sessions per charger per day** and **kWh delivered per charger per day**. A growing network with collapsing charger productivity indicates capital misallocation and overbuilding.

---

## 4. Dataset Overview & Data Quality Audit

The analysis leverages the primary tables of `data/raw/ev_charging_dataset.xlsx`:
- **Sessions**: 294,024 records spanning 1,096 consecutive days (2022–2024).
- **Stations**: 33 stations across 9 UK regions with recorded activation years (2022: 8, 2023: 9, 2024: 16).
- **Chargers**: 199 physical chargers (2022: 43, 2023: 59, 2024: 97).
- **Customers**: 8,000 distinct customer account tokens.
- **Audit Verification**: 0 missing values, 0 duplicate records, and exact calendar date alignment.

---

## 5. Overall Annual Network Performance

| Metric | Calendar Year 2022 | Calendar Year 2023 | Calendar Year 2024 | YoY Growth (2023) | YoY Growth (2024) | Cumulative Growth |
|---|---:|---:|---:|---:|---:|---:|
| **Total Charging Sessions** | 40,640 | 86,132 | 167,252 | **+111.94%** | **+94.18%** | **+311.55%** |
| **Total Energy Delivered (kWh)** | 2,011,828.0 | 4,049,124.5 | 7,747,793.3 | **+101.27%** | **+91.34%** | **+285.11%** |
| **Total Gross Revenue (£)** | £1,028,771.80 | £2,587,773.92 | £4,554,554.40 | **+151.54%** | **+76.00%** | **+342.72%** |
| **Average Energy / Session (kWh)** | 49.50 | 47.01 | 46.32 | -5.04% | -1.46% | -6.42% |
| **Average Revenue / Session (£)** | £25.31 | £30.04 | £27.23 | +18.68% | -9.36% | +7.57% |
| **Average Duration (Minutes)** | 33.14 | 35.23 | 35.50 | +6.31% | +0.77% | +7.13% |
| **Active Charging Stations** | 8 | 17 | 33 | **+112.50%** | **+94.12%** | **+312.50%** |
| **Active Chargers Installed** | 43 | 102 | 199 | **+137.21%** | **+95.10%** | **+362.79%** |
| **Distinct Customer Account IDs** | 7,669 | 5,977 | 6,000 | -22.06% | +0.38% | -21.76% |
| **Weighted Tariff (£/kWh)** | £0.5114 | £0.6391 | £0.5879 | **+24.98%** | **-8.02%** | **+14.96%** |
| **Days in Calendar Year** | 365 | 365 | 366 (Leap) | — | — | — |
| **Sessions / Charger / Day** | **2.59** | **2.31** | **2.30** | -10.65% | -0.74% | -11.32% |
| **Energy / Charger / Day (kWh)** | **128.18** | **108.76** | **106.38** | -15.15% | -2.19% | -17.01% |
| **Revenue / Charger / Day (£)** | **£65.55** | **£69.51** | **£62.53** | +6.04% | -10.03% | -4.60% |

---

## 6. Monthly Growth Dynamics (36-Month Timeline)

The 36-month monthly dataset reveals that growth occurs in **discrete step-changes** coinciding with annual network commissioning:
- **2022 Phase (Months 1–12)**: Monthly session volume ranged between 3,120 (Feb) and 3,518 (Aug), averaging **3,387 sessions/month** across the initial 8 stations.
- **2023 Phase (Months 13–24)**: Following the activation of 9 stations in January 2023, volume immediately stepped up to ~7,100–7,250 sessions/month, averaging **7,178 sessions/month**.
- **2024 Phase (Months 25–36)**: Following the activation of 16 stations in January 2024, volume doubled again to ~13,400–14,350 sessions/month, averaging **13,938 sessions/month**.
- **Seasonality Invariance**: Within each calendar year, month-over-month volume varies by less than 5% (adjusting for month length). Charging demand is non-seasonal, reflecting commuter, delivery, and fleet routine driving cycles rather than holiday tourism surges.

---

## 7. Station Cohort Tracking: Like-for-Like (LFL) vs. New Rollout

Partitioning stations by activation year isolates mature station organic growth from footprint expansion:

| Activation Cohort | Station Count | Charger Count | Calendar Year | Sessions Delivered | YoY Session Growth (%) | Energy Delivered (kWh) | Revenue Generated (£) |
|---|---:|---:|---:|---:|---:|---:|---:|
| **2022 Cohort** | 8 | 43 | 2022 | 40,640 | — | 2,011,828.0 | £1,028,771.80 |
| **2022 Cohort** | 8 | 43 | 2023 | 40,393 | **-0.61%** | 2,000,776.4 | £1,279,085.18 |
| **2022 Cohort** | 8 | 43 | 2024 | 40,424 | **+0.08%** | 2,005,263.6 | £1,177,776.69 |
| **2023 Cohort** | 9 | 59 | 2023 | 45,739 | — | 2,048,348.1 | £1,308,688.74 |
| **2023 Cohort** | 9 | 59 | 2024 | 45,738 | **-0.00%** | 2,047,482.1 | £1,203,559.49 |
| **2024 Cohort** | 16 | 97 | 2024 | 81,090 | — | 3,695,047.6 | £2,173,218.22 |

### Executive Insights from Cohort Tracking:
1. **Zero Like-for-Like Organic Demand Growth**:
   - The 8 stations in the 2022 cohort generated 40,640 sessions in 2022, 40,393 in 2023, and 40,424 in 2024.
   - The 9 stations in the 2023 cohort generated 45,739 sessions in 2023 and 45,738 in 2024.
   - **Mature charging stations operated at complete volume equilibrium.**
2. **Expansion Accounted for 100% of Volume Gains**:
   - In 2023: Total session growth was +45,492 sessions. The 2023 cohort contributed +45,739 sessions, while the 2022 cohort saw a slight dip of -247 sessions.
   - In 2024: Total session growth was +81,120 sessions. The 2024 cohort contributed +81,090 sessions, while existing cohorts contributed +30 sessions.
   - **Conclusion**: An operator cannot rely on organic adoption growth at existing sites; geographic expansion is the sole engine of volume expansion.

---

## 8. Regional Expansion Patterns

| UK Region | 2022 Stations | 2024 Stations | 2022 Sessions | 2024 Sessions | 2024 YoY Session Growth (%) | 2024 Revenue (£) | Regional Share (%) |
|---|---:|---:|---:|---:|---:|---:|---:|
| **North West** | 3 | 7 | 15,045 | 35,544 | +133.84% | £967,314.93 | 21.25% |
| **Midlands** | 2 | 5 | 10,295 | 25,180 | +66.50% | £685,389.96 | 15.06% |
| **London** | 1 | 4 | 5,133 | 20,554 | +103.44% | £561,003.58 | 12.29% |
| **North East** | 1 | 3 | 5,119 | 15,344 | +53.81% | £417,440.06 | 9.17% |
| **South West** | 1 | 3 | 5,048 | 15,263 | +51.27% | £415,860.83 | 9.13% |
| **South East** | 0 | 4 | 0 | 19,454 | +240.46% | £530,165.74 | 11.63% |
| **Yorkshire & Humber**| 0 | 3 | 0 | 15,856 | +243.80% | £432,605.15 | 9.48% |
| **Scotland** | 0 | 3 | 0 | 15,152 | +197.68% | £414,142.74 | 9.06% |
| **Wales** | 0 | 1 | 0 | 4,905 | -6.64% | £130,631.41 | 2.93% |

**Regional Findings**:
- The North West, Midlands, and London represent the backbone of network commercial returns, generating **48.60% of all 2024 revenue**.
- The South East and Yorkshire & Humber expanded rapidly in 2024 as new multi-charger hubs were commissioned.
- Wales recorded a slight session contraction in 2024 (-6.64%), operating only a single station (STN_021), illustrating the high volatility of single-station regional presence.

---

## 9. Customer Segment & Charger-Type Evolution

### 9.1 User-Type Dynamics
- Across all three years, session volume remained evenly balanced across all four user categories:
  - **Delivery**: 10,060 (2022) -> 21,308 (2023) -> 41,971 (2024) [25.09% share]
  - **Fleet**: 10,161 (2022) -> 21,687 (2023) -> 41,848 (2024) [25.02% share]
  - **Public**: 10,184 (2022) -> 21,631 (2023) -> 41,939 (2024) [25.08% share]
  - **Taxi**: 10,235 (2022) -> 21,506 (2023) -> 41,494 (2024) [24.81% share]
- Commercial users (Fleet, Delivery, Taxi) represent **74.92% of network volume**, emphasizing that B2B commercial operations are the foundational anchor of network throughput.

### 9.2 Charger-Type Evolution
- **DC_50kW (Standard Rapid)**: 16,124 sessions (2022) -> 40,630 (2023) -> 80,067 (2024) [47.87% of 2024 sessions; 95 chargers installed].
- **DC_150kW (High-Power Rapid)**: 14,063 sessions (2022) -> 27,365 (2023) -> 55,549 (2024) [33.21% of 2024 sessions; 69 chargers installed].
- **DC_300kW (Ultra-Rapid)**: 10,453 sessions (2022) -> 18,137 (2023) -> 31,636 (2024) [18.92% of 2024 sessions; 35 chargers installed].
- Although DC_50kW chargers represent the largest session volume, DC_300kW chargers delivered 30.8% of all 2024 electricity (2.39M kWh) despite representing only 17.6% of the physical charger fleet.

---

## 10. Multiplicative Growth Factor Decomposition

We formalize the mathematical decomposition of network growth:
$$\text{Total Sessions} = \text{Stations} \times \left( \frac{\text{Chargers}}{\text{Station}} \right) \times \left( \frac{\text{Sessions}}{\text{Charger}} \right)$$
$$\text{Total Revenue} = \text{Sessions} \times \left( \frac{\text{Energy}}{\text{Session}} \right) \times \left( \frac{\text{Revenue}}{\text{Energy}} \right)$$

| Economic Component | 2022 to 2023 Growth (%) | 2023 to 2024 Growth (%) | Cumulative 2022–2024 Growth (%) | Primary Strategic Interpretation |
|---|---:|---:|---:|---|
| **Total Session Growth** | **+111.94%** | **+94.18%** | **+311.55%** | Network volume expanded more than 4-fold. |
| **Station Count Growth** | **+112.50%** | **+94.12%** | **+312.50%** | Exact 1:1 scaling with session growth. |
| **Charger Count Growth** | **+137.21%** | **+95.10%** | **+362.79%** | Average chargers per station rose from 5.38 to 6.03. |
| **Like-for-Like (LFL) Station Volume** | **-0.61%** | **+0.08%** | **-0.53%** | Zero organic volume growth at mature stations. |
| **New Station Contribution to Volume** | **100.54%** | **99.96%** | **100.17%** | Expansion accounted for entire network volume gain. |
| **Total Revenue Growth** | **+151.54%** | **+76.00%** | **+342.72%** | Revenue growth outpaced volume in 2023, lagged in 2024. |
| **Weighted Tariff Change (£/kWh)** | **+24.98%** | **-8.02%** | **+14.96%** | Macro energy price shock in 2023; normalization in 2024. |
| **Average Energy / Session** | **-5.04%** | **-1.46%** | **-6.42%** | Slight shift toward shorter top-up charges. |

---

## 11. Strategic Business Insights & Executive Takeaways

### 1. Capital Deployment is the Sole Growth Driver
Because mature stations operate at structural throughput capacity (~2.3 sessions/charger/day), **organic demand growth does not occur spontaneously**. Network operators cannot grow top-line volume through passive customer adoption alone; maintaining revenue momentum requires an active real estate and grid commissioning pipeline.

### 2. De-Risking Revenue from Wholesale Energy Shocks
The divergence between 2023 revenue growth (+151.5%) and 2024 revenue growth (+76.0%) underscores that top-line performance is heavily exposed to wholesale pass-through tariffs. Operators should introduce structured commercial contracts with fixed subscription tiers or dynamic time-of-use pricing (as formulated in Analysis 8) to decouple gross margin from wholesale electricity volatility.

### 3. High-Power Hub Strategy Validated
Increasing chargers per station from 5.38 to 6.03 did not dilute charger productivity (2.59 -> 2.30 sessions/charger/day). High-density hub stations attract fleet contracts and reduce driver queue anxiety, confirming that multi-bay charging hubs are operationally and commercially superior to isolated 1–2 charger sites.

---

## 12. Analytical Limitations

1. **Annual Station Activation Aggregation**: Station activations are recorded at annual resolution. Intrayear ramp-up curves (e.g. month 1 vs month 6 after opening) cannot be tracked without daily commissioning logs.
2. **Customer ID Accounting Tokens**: Customer IDs function as transaction account tokens rather than biometrically verified unique human beings. Longitudinal customer retention rates cannot be derived without contract-level CRM data.
3. **Macroeconomic Exogeneity**: Price movements in 2023 reflected UK wholesale gas and electricity crisis shocks rather than localized price elasticity testing.

---

## 13. Conclusion

Analysis 12 demonstrates that the rapid expansion of the UK EV Charging Network (+311.6% sessions, +342.7% revenue) was an **infrastructure rollout achievement**. Existing stations operate at stable capacity ceilings, establishing that capital deployment in high-density multi-charger hubs is the decisive driver of network enterprise value.

