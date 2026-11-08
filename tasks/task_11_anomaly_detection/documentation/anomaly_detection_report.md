# EV Charging Session Anomaly Detection (Analysis 11)

## 1. Executive Summary

This report establishes an unsupervised anomaly detection framework for the UK EV Charging Network across 294,024 charging sessions (2022–2024), analyzing 13.81 million kWh delivered across 33 stations and 199 chargers.

Using a multi-model architecture combining statistical baselines (IQR and Z-score), tree-based isolation (**Isolation Forest**), and deep neural reconstruction (**Neural Autoencoder** via MLP bottleneck architecture $16 \rightarrow 4 \rightarrow 16$), the framework isolates operational, electrical, and commercial outliers to support proactive maintenance, revenue assurance, and grid-demand monitoring.

### Key Analytical & Empirical Findings:
- **Baseline Statistical Outliers**: Univariate IQR screening flags **5,735 sessions (1.95%)**, primarily driven by pricing bounds (£0.80 ceiling, £0.45 floor) and duration boundaries (15 min and 60 min). Z-score analysis ($|Z| > 3.0$) flags **794 sessions (0.27%)**, constrained by non-normal skewness.
- **Isolation Forest Baseline**: At an operational contamination threshold of $\nu = 2.5\%$, Isolation Forest isolates **7,351 sessions** based on sparse feature partitioning across multi-dimensional energy-duration-power space.
- **Neural Autoencoder Reconstruction**: The $16 \rightarrow 4 \rightarrow 16$ bottleneck network trained on multivariate standardized features establishes an empirical 97.5th percentile MSE threshold at **0.4286**, flagging **7,351 sessions (2.50%)** exhibiting atypical feature inter-relationships.
- **Model Concordance & Overlap**: 
  - Overlap between Isolation Forest and Autoencoder: **1,240 sessions** (Jaccard similarity: 0.092).
  - Sessions flagged by at least 2 models: **4,233 sessions (1.44%)**.
  - Unanimous 4-model consensus: **137 extreme sessions (0.05%)**.
- **Charger-Type Disparities**: Anomalies are heavily concentrated on **DC_300kW ultra-rapid chargers**, which exhibit a consensus anomaly rate of **6.82%** (4,110 sessions), compared to only **0.06%** for DC_150kW and **0.05%** for DC_50kW. The primary operational driver is severe power utilization throttling (sessions drawing only 180–190 kW on a 300 kW dispenser due to vehicle onboard battery BMS limits).
- **User-Type Invariance**: Anomaly rates are remarkably uniform across user types (Public: 1.48%, Taxi: 1.45%, Fleet: 1.44%, Delivery: 1.39%), indicating that anomalies stem from vehicle-dispenser electrical dynamics rather than user segment driving behaviors.

> [!IMPORTANT]
> **Operational Disclaimer**: Anomaly detection identifies statistical rarity, NOT confirmed equipment defects, fraud, or billing malpractice. Flagged sessions represent prioritized candidates for engineering telemetry inspection and automated audit queues.

---

## 2. Business Objectives

1. Define what constitutes an unusual EV charging session from physical, commercial, and operational perspectives.
2. Establish robust univariate statistical outlier baselines (IQR and Z-score).
3. Develop, optimize, and compare unsupervised machine learning architectures (Isolation Forest vs. Neural Autoencoder).
4. Quantify model overlap, consensus, and score distributions without fabricating supervised metrics.
5. Perform deep-dive root cause investigations across charger types, user types, and individual station facilities.
6. Provide an operational alert triage framework for engineering dispatch and revenue protection.

---

## 3. Business Understanding: Anomalies vs. Faults

In public EV charging operations, confusing an anomaly with a fault or fraudulent act leads to customer friction, false technician dispatches, and misallocated CapEx.

### Key Conceptual Distinctions:
1. **Statistical Anomaly**: An observation whose combination of duration, delivered energy, charging rate, and pricing deviates significantly from population norms.
2. **Physical/BMS Constraint (False Fault)**: A 300kW charger delivering an average of 185 kW over 20 minutes is a statistical outlier because its utilization ratio is ~61%, but physically normal: modern EV battery chemistry throttles current above 80% State of Charge (SoC) or during cold ambient temperatures to protect battery cells.
3. **Confirmed Hardware Fault**: A dispenser metering power while physical delivery is zero, or a communication timeout terminating sessions prematurely (e.g., duration < 1 minute, energy < 0.1 kWh).
4. **Billing Inconsistency**: Sessions where $\text{total\_cost} \neq \text{energy\_kwh} \times \text{price\_per\_kwh}$ beyond rounding tolerances.

---

## 4. Dataset Overview & Data Quality Audit

The analysis leverages the full 3-year charging dataset (`data/raw/ev_charging_dataset.xlsx`):
- Total records: **294,024 sessions**.
- Covered stations: **33 stations** across 9 UK regions.
- Covered chargers: **199 chargers** (92 DC_50kW, 66 DC_150kW, 41 DC_300kW).
- Covered customers: **8,000 customer IDs**.
- Missing values: **0** across all attributes.
- Duplicate records: **0** duplicate session IDs.
- Audit check: Zero sessions recorded average power exceeding the dispenser nameplate rating ($\text{Avg Power} \le \text{Max Power}$ across all records).

---

## 5. Feature Engineering

To capture physical, temporal, and commercial dimensions without label leakage or ID memorization, we developed 17 standardized features:

| Feature Category | Feature Name | Formula / Encoding | Analytical Rationale |
|---|---|---|---|
| **Continuous Operational** | `energy_kwh` | Raw continuous metric | Volume of electricity dispensed per session. |
| **Continuous Operational** | `session_duration_minutes` | Raw continuous metric | Dwell duration at the charging stall. |
| **Derived Electrical** | `avg_power_kw` | $(\text{energy\_kwh} / \text{duration}) \times 60$ | Effective rate of charging delivered. |
| **Derived Electrical** | `power_utilization_ratio` | $\text{avg\_power\_kw} / \text{max\_power\_kW}$ | Proportion of dispenser capacity effectively utilized. |
| **Commercial / Billing** | `price_per_kwh` | Tariff applied (£/kWh) | Unit price charged. |
| **Commercial / Billing** | `total_cost` | Total session spend (£) | Financial transaction size. |
| **Commercial / Billing** | `cost_per_minute` | $\text{total\_cost} / \text{duration}$ | Spend rate per minute of bay occupancy. |
| **Cyclical Temporal** | `hour_sin`, `hour_cos` | $\sin(2\pi h / 24), \cos(2\pi h / 24)$ | Preserves cyclical continuity (23:00 is close to 00:00). |
| **Cyclical Temporal** | `dow_sin`, `dow_cos` | $\sin(2\pi d / 7), \cos(2\pi d / 7)$ | Preserves cyclical continuity across Monday–Sunday. |
| **Categorical Technical** | `chg_DC_50kW`, `chg_DC_150kW`, `chg_DC_300kW` | One-hot binary indicators | Separates distinct hardware operating envelopes. |
| **Categorical Segment** | `usr_fleet`, `usr_delivery`, `usr_taxi`, `usr_public` | One-hot binary indicators | Accounts for segment-specific duty cycles. |

All features were standardized using `StandardScaler` ($\mu = 0, \sigma = 1$) to ensure distance-based models and neural networks are not biased by high-magnitude variables.

---

## 6. Model Development & Evaluation

### 6.1 Statistical Baselines
- **IQR Outlier Rule**: An observation is flagged if it falls outside $[Q_1 - 1.5 \times \text{IQR}, Q_3 + 1.5 \times \text{IQR}]$ on any continuous metric. Flags **5,735 sessions (1.95%)**.
- **Z-Score Rule**: An observation is flagged if $|Z| > 3.0$ on standardized continuous metrics. Flags **794 sessions (0.27%)**.

### 6.2 Machine Learning Architecture 1: Isolation Forest
- **Ensemble Setup**: 100 isolation trees with random split selection.
- **Contamination Sensitivity**:
  - Contamination 1.0%: 2,941 sessions flagged.
  - Contamination 2.5%: **7,351 sessions flagged** (Primary operational baseline).
  - Contamination 5.0%: 14,702 sessions flagged.
- **Score Distribution**: Mean anomaly score: -0.124; 97.5th percentile score: **-0.038**.

### 6.3 Machine Learning Architecture 2: Neural Autoencoder
- **Architecture**: Multi-Layer Perceptron (MLP) with symmetric bottleneck structure ($17 \rightarrow 16 \rightarrow 4 \rightarrow 16 \rightarrow 17$).
- **Objective**: Minimize mean squared reconstruction error: $\text{MSE}_i = \frac{1}{D} \sum_{j=1}^D (X_{i,j} - \hat{X}_{i,j})^2$.
- **Training**: Trained on a representative sample of 60,000 sessions using the Adam optimizer with early stopping.
- **Reconstruction Error Threshold**: Empirical 97.5th percentile established at **0.4286**. Flags **7,351 sessions (2.50%)**.

---

## 7. Model Comparison & Consensus Analysis

Because unsupervised datasets lack true anomaly ground truth, supervised metrics (Precision, Recall, F1, ROC-AUC) cannot be computed without fabricating labels. Instead, we evaluate models through detection concordance and cross-model agreement:

| Method | Model Category | Flagged Sessions | Flagged Share (%) | Overlap with Isolation Forest | Overlap Share (%) |
|---|---|---:|---:|---:|---:|
| **IQR Statistical Rule** | Statistical Rule | 5,735 | 1.95% | 3,502 | 61.06% |
| **Z-Score (|Z| > 3.0)** | Statistical Rule | 794 | 0.27% | 527 | 66.37% |
| **Isolation Forest (2.5%)** | Machine Learning (Ensemble) | 7,351 | 2.50% | 7,351 | 100.00% |
| **Neural Autoencoder (97.5%)** | Deep Neural Architecture | 7,351 | 2.50% | 1,240 | 16.87% |

### Consensus Diagnostics:
- **Consensus Flags $\ge 2$**: **4,233 sessions (1.44%)** are flagged by at least two distinct detection methodologies.
- **Consensus Flags $\ge 3$**: **1,073 sessions (0.37%)** exhibit high isolation and elevated reconstruction errors.
- **Unanimous Consensus (Score = 4)**: **137 sessions (0.05%)** are flagged simultaneously by IQR, Z-Score, Isolation Forest, and Neural Autoencoder, representing extreme multi-dimensional outliers.

---

## 8. Deep-Dive Anomaly Investigation

### 8.1 Charger-Type Breakdown: The Ultra-Rapid Paradox

| Charger Type | Total Sessions | Isolation Forest Anomalies | Autoencoder Anomalies | Consensus Anomalies ($\ge 2$) | Consensus Rate (%) |
|---|---:|---:|---:|---:|---:|
| **DC_50kW** | 136,821 | 2,746 | 13 | 62 | 0.05% |
| **DC_150kW** | 96,977 | 338 | 2,525 | 61 | 0.06% |
| **DC_300kW** | 60,226 | 4,267 | 4,813 | **4,110** | **6.82%** |

**Root Cause Finding**:
Over **97.1% of consensus anomalies** occur on `DC_300kW` chargers. 
- On DC_50kW and DC_150kW chargers, vehicle battery acceptance curves closely track charger capacity (utilization ratios average ~80.0%).
- On DC_300kW chargers, many current EV models cannot sustain 300 kW charging, tapering rapidly to 180–230 kW. The models flag these sessions because the multi-dimensional correlation between duration (15–20 min), energy delivered (55–65 kWh), and nameplate power (300 kW) exhibits lower utilization (60%–75%), reflecting vehicle BMS limitations rather than dispenser failure.

### 8.2 User-Type Anomaly Breakdown

| User Type | Total Sessions | Isolation Forest Anomalies | Autoencoder Anomalies | Consensus Anomalies ($\ge 2$) | Consensus Rate (%) |
|---|---:|---:|---:|---:|---:|
| **Public** | 73,754 | 1,844 | 1,847 | 1,093 | 1.48% |
| **Taxi** | 73,235 | 1,833 | 1,835 | 1,061 | 1.45% |
| **Fleet** | 73,696 | 1,843 | 1,832 | 1,060 | 1.44% |
| **Delivery** | 73,339 | 1,831 | 1,837 | 1,019 | 1.39% |

**Insight**: User type does not drive anomalous behavior. Fleet and commercial drivers exhibit identical mechanical charging dynamics to general public drivers.

### 8.3 Station-Level Hotspots

Top 5 Stations with Highest Consensus Anomaly Rates:
1. **STN_007 (London)**: 9,077 sessions, 230 consensus anomalies (**2.53%**). High proportion of 300kW chargers and intense delivery turnaround.
2. **STN_031 (South East)**: 4,499 sessions, 114 consensus anomalies (**2.53%**).
3. **STN_002 (London)**: 9,067 sessions, 218 consensus anomalies (**2.40%**).
4. **STN_022 (Midlands)**: 8,767 sessions, 203 consensus anomalies (**2.32%**).
5. **STN_004 (North West)**: 14,798 sessions, 332 consensus anomalies (**2.24%**).

Stations with elevated anomaly rates correlate directly with the proportion of DC_300kW chargers installed at the facility.

---

## 9. Business & Operational Triage Framework

```
                              All 294,024 Charging Sessions
                                            │
                                ┌───────────┴───────────┐
                       Consensus Score < 2      Consensus Score >= 2
                      (289,791 sessions, 98.6%)  (4,233 sessions, 1.4%)
                                │                       │
                        Normal Operations       Triage Investigation Queue
                                                        │
                      ┌─────────────────────────────────┼─────────────────────────────────┐
                      │                                 │                                 │
             Tier 1: Unanimous (Score=4)       Tier 2: High (Score=3)            Tier 3: Moderate (Score=2)
                (137 sessions, 0.05%)             (936 sessions, 0.32%)            (3,160 sessions, 1.07%)
                      │                                 │                                 │
          Immediate Engineering Audit       BMS / Telemetry Review            Periodic Monthly Sampling
          • Billing reconciliation          • Compare vehicle model           • Verify pricing rules
          • Dispenser power meter check     • Check ambient temp log          • Track station wear trends
```

### Actionable Operator Recommendations:
1. **Automated Tier 1 Billing Reconciliation**: Automatically freeze invoices for sessions flagged by 4 models until metered energy matches payment gateway settlements.
2. **Dynamic BMS Profiling**: Adjust anomaly scoring baselines for DC_300kW chargers by factoring in vehicle make/model charging curves to reduce false-positive BMS throttling alerts.
3. **Preventive Maintenance Scheduling**: Prioritize field service inspections at STN_007, STN_031, and STN_002 where hardware strain and thermal throttling events are clustered.

---

## 10. Analytical Limitations

1. **Absence of Ground Truth Labels**: The network does not record verified fraud or confirmed hardware failure logs. Unsupervised models detect statistical outliers, not verified faults.
2. **Unobserved Vehicle Battery SoC**: Without battery State of Charge at plug-in, the model cannot verify whether a low charging rate is caused by battery cold-soaking, 85%+ SoC tapering, or dispenser power unit degradation.
3. **Contamination Parameter Selection**: The 2.5% contamination rate was selected based on standard utility triage capacities, but should be calibrated against operator field engineering resources.

---

## 11. Conclusion

Analysis 11 delivers a multi-model unsupervised anomaly detection system combining statistical screening, Isolation Forest, and an MLP Neural Autoencoder. The system identifies 4,233 high-priority consensus anomalies, revealing that charging irregularities in this network are predominantly driven by vehicle-dispenser power mismatch on 300kW ultra-rapid chargers rather than fraudulent customer behavior.

