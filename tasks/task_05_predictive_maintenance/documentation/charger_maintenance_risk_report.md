# EV Charger Maintenance Risk Screening

## 1. Objective

The objective of this analysis is to identify EV chargers that may warrant further inspection based on their charging-performance patterns over time.

The analysis uses historical charging sessions to examine capacity utilisation, identify negative performance trends, and develop a rule-based maintenance screening approach.

**Important:** The dataset does not contain verified charger failure records or maintenance history. Therefore, this project performs exploratory risk screening rather than validated failure prediction.

## 2. Dataset Overview

Three datasets were used:

| Dataset | Records | Purpose |
|---|---:|---|
| Stations | 33 | Station-level information |
| Chargers | 199 | Charger specifications and installation dates |
| Sessions | 294,024 | Historical charging activity |

The analysis covers 36 calendar months, from January 2022 to December 2024.

## 3. Feature Engineering

Average delivered power was calculated for each charging session:

Average Delivered Power (kW) = Energy Delivered (kWh) / Charging Duration (hours)

Capacity utilisation was calculated as:

Capacity Utilisation (%) = Average Delivered Power / Rated Charger Power × 100

Monthly charger-level summaries were created using session counts, average delivered power, average energy consumption, average session duration, and capacity utilisation.

Linear regression was used to estimate the monthly trend in capacity utilisation for each charger.

## 4. Data Quality Findings

A comparison of session timestamps with recorded charger installation dates identified 88,466 sessions (30.09%) occurring before the corresponding installation date.

This inconsistency could affect the reliability of installation-age analysis. The cause could not be established from the available data, so installation age was not used as a maintenance-risk feature.

The dataset also lacks confirmed failures, repairs, downtime, and maintenance records. These limitations prevent direct validation of a failure-prediction model.

## 5. Screening Methodology

A rule-based screening score was developed using two indicators:

1. **Low relative utilisation:** average capacity utilisation more than one percentage point below the median for chargers with the same rated power.
2. **Declining utilisation trend:** estimated linear trend below -0.05 percentage points per month.

Each indicator contributes one point to the screening score.

| Score | Screening category |
|---:|---|
| 0 | Routine Monitoring |
| 1 | Review Recommended |
| 2 | Higher Review Priority |

These thresholds are exploratory choices for this portfolio analysis, not established industry standards.

## 6. Results

The peer-adjusted screening classified the 199 chargers as follows:

| Screening category | Chargers | Percentage |
|---|---:|---:|
| Routine Monitoring | 180 | 90.45% |
| Review Recommended | 19 | 9.55% |
| Higher Review Priority | 0 | 0% |

The screening identified 19 chargers for further review. No charger triggered both screening indicators.

The charger with the steepest negative estimated trend was not necessarily the charger with the lowest average utilisation. This demonstrates the value of evaluating multiple indicators rather than relying on a single metric.

## 7. Candidate Investigation

Charger STN_025_CH_03 was initially classified as a higher-priority candidate under the network-wide utilisation threshold.

After comparing it with chargers of the same rated power, its average capacity utilisation was approximately 0.02 percentage points below the peer median. Its classification changed to Review Recommended because of its negative estimated trend.

This example illustrates why charger comparisons should account for rated power and why screening results require further investigation.

## 8. Power BI Dashboard

The Power BI dashboard, titled **EV Charger Maintenance Risk Screening**, contains:

- KPI cards for total chargers, routine monitoring, and review recommendations.
- A donut chart showing the screening-category distribution.
- A bar chart ranking chargers by declining utilisation trends.
- A monthly capacity-utilisation line chart.
- A charger slicer for interactive exploration.

The dashboard is intended to support investigation and prioritisation rather than automatically diagnose charger faults.

## 9. Limitations

- No verified failure labels or maintenance history are available.
- Installation dates are inconsistent with a substantial proportion of session timestamps.
- Capacity utilisation is influenced by vehicle characteristics, battery state, charging conditions, and other factors.
- Linear trends can be affected by noise and changing operating conditions.
- The screening thresholds have not been validated against actual maintenance outcomes.
- The analysis does not establish that low utilisation or a negative trend is caused by a faulty charger.

## 10. Future Improvements

Future work could incorporate verified fault logs, repair records, downtime, error codes, environmental conditions, and charger telemetry.

With sufficient labelled data, the project could progress to supervised failure prediction, time-to-failure modelling, and evaluation using precision, recall, and maintenance-cost metrics.

## 11. Conclusion

This project developed an exploratory, peer-adjusted approach for identifying chargers that may warrant further investigation.

The analysis classified 19 of 199 chargers as Review Recommended and 180 as Routine Monitoring. These results provide a starting point for maintenance prioritisation, but they do not represent confirmed failures or validated predictions.

The project demonstrates data-quality investigation, feature engineering, time-series trend analysis, peer benchmarking, rule-based screening, and business dashboard development.