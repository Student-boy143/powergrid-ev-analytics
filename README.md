# powergrid-ev-analytics
Data Science, Machine Learning and Power BI analysis of UK EV charging data (2022–2024)

## Portfolio Overview

A comprehensive collection of end-to-end data science, machine learning, and analytics engineering projects analyzing real-world public EV charging infrastructure in the United Kingdom across 33 stations, 199 chargers, and 294,024 charging sessions (2022–2024).

| # | Analysis Project | Directory | Focus Area |
|---|------------------|-----------|------------|
| 01 | [EV Demand Forecasting](tasks/task_01_demand_forecasting/) | `task_01_demand_forecasting` | Time-series forecasting (ARIMA, Prophet, XGBoost) |
| 02 | [Revenue & Price Sensitivity](tasks/task_02_revenue_price_sensitivity/) | `task_02_revenue_price_sensitivity` | Elasticity modeling & tariff optimization |
| 03 | [Charger Utilisation & Efficiency](tasks/task_03_charger_utilisation/) | `task_03_charger_utilisation` | Operational throughput & capacity bottlenecking |
| 04 | [Customer Behaviour Segmentation](tasks/task_04_customer_segmentation/) | `task_04_customer_segmentation` | RFM & unsupervised customer clustering |
| 05 | [Predictive Maintenance Screening](tasks/task_05_predictive_maintenance/) | `task_05_predictive_maintenance` | Risk scoring & failure degradation models |
| 06 | [Optimal Location Planning](tasks/task_06_optimal_location_planning/) | `task_06_optimal_location_planning` | Geospatial MCDA site selection & network rollout |
| 07 | [Time-of-Day & Day-of-Week](tasks/task_07_time_day_analysis/) | `task_07_time_day_analysis` | Temporal load profiling & diurnality |
| 08 | [Dynamic Pricing Strategy](tasks/task_08_dynamic_pricing/) | `task_08_dynamic_pricing` | Peak-shaving & revenue simulation |
| 09 | [Fleet vs Public Behaviour](tasks/task_09_fleet_vs_public/) | `task_09_fleet_vs_public` | Commercial fleet vs retail charging dynamics |
| 10 | [Station Performance Benchmarking](tasks/task_10_station_performance/) | `task_10_station_performance` | Multicriteria ranking & cohort productivity |
| 11 | [EV Charging Session Anomaly Detection](tasks/task_11_anomaly_detection/) | `task_11_anomaly_detection` | Unsupervised ML (Isolation Forest, Autoencoder, IQR) |
| 12 | [Year-over-Year (YoY) Growth & Network Scaling](tasks/task_12_yoy_growth/) | `task_12_yoy_growth` | Like-for-like cohort growth & macro decomposition |

---

## Analysis 11 — EV Charging Session Anomaly Detection

- **Directory**: [`tasks/task_11_anomaly_detection/`](tasks/task_11_anomaly_detection/)
- **Main Notebook**: [`11_anomaly_detection.ipynb`](tasks/task_11_anomaly_detection/11_anomaly_detection.ipynb)
- **Detailed Report**: [`anomaly_detection_report.md`](tasks/task_11_anomaly_detection/documentation/anomaly_detection_report.md)

### Title
**Unsupervised Anomaly Detection & Operational Screening in EV Charging Sessions**

### Objective
Identify unusual, anomalous, and operationally irregular EV charging sessions across the UK network using statistical baselines and unsupervised machine learning algorithms (Isolation Forest and Neural Autoencoder). The business objective is to provide network operations and field engineering teams with an automated screening pipeline to detect potential hardware miscalibrations, metering anomalies, battery management system (BMS) throttling, and billing discrepancies without requiring labelled ground truth.

### Methods Used
- **Domain-Specific Feature Engineering**: Calculated derived physical charging power ($kW = \frac{kWh}{\text{duration}} \times 60$), charger utilization ratio ($\frac{P_{\text{avg}}}{P_{\text{nameplate}}}$), revenue rate ($\frac{£}{\text{min}}$), unit tariff deviations, and cyclical temporal encodings ($\sin/\cos$ transformations for hour-of-day and day-of-week).
- **Statistical Baselines**: 
  - Non-parametric Interquartile Range (IQR) gating across power ratio, duration, and energy ($1.5 \times IQR$ rule) flagging 5,735 sessions (1.95%).
  - Parametric Z-Score analysis ($|Z| > 3.0$ on standardized physical metrics) flagging 794 extreme sessions (0.27%).
- **Unsupervised Machine Learning**:
  - **Isolation Forest**: Tree-based sub-sampling ensemble (`contamination=0.025`, `n_estimators=100`, random seed 42) operating on standardized multi-dimensional feature space, isolating 7,351 sessions (2.50%).
  - **Neural Autoencoder (`MLPRegressor`)**: Deep bottleneck reconstruction architecture ($16 \rightarrow 4 \rightarrow 16$, ReLU activations, Adam optimizer, early stopping) computing Mean Squared Error (MSE) reconstruction loss; thresholded at 97.5th percentile ($MSE > 0.4286$) flagging 7,351 sessions (2.50%).
- **Multi-Model Consensus Scoring**: Developed an ensemble risk score ($\ge 2$ models agreeing) identifying 4,233 high-confidence operational anomalies (1.44% of network volume).

### Main Findings
- **Nameplate Throttling on Ultra-Rapid Hardware**: 97.1% of consensus anomalies (4,109 out of 4,233 sessions) occurred on `DC_300kW` chargers (consensus anomaly rate: 6.82%), compared to only 0.06% on `DC_150kW` and 0.05% on `DC_50kW`. Sessions flagged exhibited nominal 300 kW connection but actual realized charging draw between 180 kW and 230 kW, reflecting vehicle BMS thermal/voltage throttling rather than charger defect.
- **Invariance Across User Segments**: Anomaly rates are remarkably uniform across user types (Public: 1.48%, Taxi: 1.45%, Fleet: 1.44%, Delivery: 1.39%), demonstrating that operational irregularities are hardware- and battery-bound rather than behavioral.
- **Station Anomaly Hotspots**: Station-level anomaly rates correlate strongly ($R^2 > 0.85$) with the proportion of DC 300kW units installed, led by STN_007 (4.37%), STN_031 (3.97%), and STN_002 (3.30%).
- **Model Complementarity**: Isolation Forest excels at isolating multi-attribute contextual outliers, while the Autoencoder captures complex non-linear coordinate deformations across power delivery curves.

### Limitations
- **No Ground-Truth Fault Labels**: Because the dataset lacks post-event maintenance logs, telemetry fault codes, or customer dispute tickets, model outputs represent statistical anomalies, not confirmed equipment failures or fraud.
- **Static Session Aggregates**: The dataset provides start/end timestamps and total energy, lacking sub-minute time-series telemetry (current, voltage, temperature, state of charge curves) which would enable definitive diagnostic classification.

---

## Analysis 12 — Year-over-Year (YoY) Growth & Network Scaling Analysis

- **Directory**: [`tasks/task_12_yoy_growth/`](tasks/task_12_yoy_growth/)
- **Main Notebook**: [`12_yoy_growth_analysis.ipynb`](tasks/task_12_yoy_growth/12_yoy_growth_analysis.ipynb)
- **Detailed Report**: [`yoy_growth_report.md`](tasks/task_12_yoy_growth/documentation/yoy_growth_report.md)

### Title
**Multi-Year Growth Decomposition, Like-for-Like Cohort Analysis & Network Scaling (2022–2024)**

### Objective
Quantify network-level, regional, user segment, and charger technology growth across 2022, 2023, and 2024. Disentangle organic like-for-like (LFL) station demand growth from capital expansion (new station activations), analyze macro energy price pass-through dynamics, evaluate charger productivity stability, and conduct a mathematical growth decomposition across sessions, energy, and revenue.

### Methods Used
- **Annual & Monthly Growth Metrics**: Computed absolute and percentage YoY changes in sessions, energy delivered (kWh), gross revenue (£), active chargers, and unique customer reach across 36 calendar months.
- **Station Activation Cohort Tracking**: Partitioned stations into 2022 Cohort (8 stations, 54 chargers), 2023 Cohort (9 stations, 50 chargers), and 2024 Cohort (16 stations, 95 chargers) to track isolated organic utilization trajectories over multi-year operational horizons.
- **Like-for-Like (LFL) Demand Isolation**: Evaluated constant-store metrics for the mature 2022 cohort across all three calendar years to test whether established stations experience demand compounding or organic saturation.
- **Logarithmic / Multiplicative Growth Decomposition**: Decomposed aggregate revenue growth into its constituent structural drivers: Volume Growth (Sessions), Delivery Intensity ($\frac{kWh}{\text{session}}$), and Realized Tariff Realization ($\frac{£}{\text{kWh}}$).
- **Productivity & Capital Efficiency Indices**: Standardized charging volume by hardware assets ($\frac{\text{Sessions}}{\text{Charger} \cdot \text{Day}}$) and regional density.

### Main Findings
- **100% Capital-Driven Network Scaling**: Total session volume surged from 40,640 (2022) to 86,134 (+111.9% in 2023) and 167,250 (+94.2% in 2024). However, like-for-like (LFL) demand at the mature 2022 cohort was completely flat across 3 full years: 40,640 sessions (2022) $\rightarrow$ 40,393 sessions (-0.61% in 2023) $\rightarrow$ 40,424 sessions (+0.08% in 2024). Net network growth was 100% driven by commissioning new physical assets.
- **Stable Capital Productivity**: Charger productivity remained invariant across all three years at ~2.31 to 2.58 sessions per charger per day (2.06 in 2022, 2.58 in 2023, 2.31 in 2024), demonstrating predictable asset utilization across diverse geographic rollouts.
- **Macro Tariff Decoupling**: In 2023, revenue growth (+151.5%, reaching £2.44M) vastly outpaced session volume growth (+111.9%) due to wholesale energy crisis pass-through (average tariff increased from £0.511/kWh to £0.639/kWh). In 2024, revenue growth slowed to +76.0% (£4.30M) despite volume surging +94.2% as unit tariffs moderated to £0.588/kWh.
- **Rapid Adoption of Ultra-Rapid Charging**: DC 300kW chargers expanded from 26.3% of sessions in 2022 to 32.7% in 2024, while DC 50kW chargers contracted from 38.3% to 32.3%, reflecting driver preference for faster turnaround times.
- **Fleet and Delivery Scalability**: Commercial fleet and delivery segments expanded by +307% over 2 years, growing from 19.8% to 20.3% and 19.7% to 20.2% of sessions respectively, providing steady baseload commercial demand.

### Limitations
- **Simulated Synthetic Baseline Characteristics**: The dataset exhibits perfect cohort stability and constant productivity indices across expansion waves, indicating synthetic generation characteristics without localized local grid curtailments or demand ramp-up latency.
- **Absence of Network CapEx & OpEx Financials**: While top-line gross revenue is analyzed, ground-lease costs, demand charges, wholesale electricity purchase prices, and civil engineering capital expenditure are not available in the dataset, precluding EBITDA or ROCE calculation.
