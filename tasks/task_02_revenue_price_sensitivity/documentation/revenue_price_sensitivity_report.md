# EV Charging Revenue & Price Sensitivity Analysis

## 1. Project Overview

This analysis examines revenue performance, pricing behaviour, charging demand, and future revenue for an EV charging network using charging-session data from 2022–2024.

The project combines exploratory data analysis, statistical modelling, time-series forecasting, scenario analysis, and Power BI visualization to understand how the charging network has evolved and to support data-driven revenue planning.

---

## 2. Business Objectives

The analysis focuses on five key questions:

1. How has EV charging revenue changed over time?
2. How does charging demand vary with pricing?
3. Can monthly revenue be forecast reliably?
4. What would happen under illustrative +5% and −10% price scenarios?
5. How effectively does the selected forecasting model perform compared with a simple baseline?

---

## 3. Dataset

The analysis uses the `sessions` sheet from the EV charging dataset.

The dataset contains **294,024 charging sessions** covering **2022–2024**.

Key variables used include:

- `start_timestamp`
- `energy_kwh`
- `price_per_kwh`
- `total_cost`
- `session_id`
- `year`
- `station_id`
- `charger_type`
- `user_type`
- `payment_method`

The data was aggregated at a monthly level for revenue, energy consumption, session volume, and pricing analysis.

---

## 4. Revenue Analysis

Monthly revenue was calculated by summing `total_cost` for all charging sessions within each month.

The resulting dataset contains **36 monthly observations** from January 2022 through December 2024.

### Annual Revenue

| Year | Revenue | Sessions |
|---|---:|---:|
| 2022 | £1.03M | 40,640 |
| 2023 | £2.59M | 86,132 |
| 2024 | £4.55M | 167,252 |

Revenue growth was:

- **2023:** +151.5% compared with 2022
- **2024:** +76.0% compared with 2023

Revenue therefore increased substantially across the three-year period, while the annual growth rate moderated in 2024.

The increase in revenue was accompanied by a substantial increase in charging-session volume.

---

## 5. Pricing Analysis

A weighted average price per kWh was calculated as:

`Total Revenue / Total Energy Consumed`

The approximate weighted price levels were:

- **2022:** £0.51/kWh
- **2023:** £0.64/kWh
- **2024:** £0.59/kWh

There was meaningful price variation within each year, allowing price sensitivity to be investigated.

---

## 6. Price Sensitivity Analysis

A log-log regression model was used to estimate price elasticity.

The model examined the relationship between:

- log of monthly energy demand
- log of weighted monthly price

Year indicators were included to control for the strong underlying growth trend between 2022 and 2024.

### Controlled Model Result

Estimated price elasticity:

**+0.75**

However, the estimated coefficient had a **p-value of 0.854**.

Therefore, the aggregated monthly data does **not provide strong statistical evidence of a reliable price-demand relationship**.

The high model R² should also be interpreted carefully because the underlying time trend explains much of the variation in the data.

### Interpretation

The analysis should not be interpreted as evidence that increasing prices causes charging demand to increase.

More detailed customer- or session-level modelling, with additional controls for factors such as charger type, station, user type, seasonality, and time of day, would be required to estimate price sensitivity more reliably.

---

## 7. Revenue Forecasting

A **Holt-Winters Exponential Smoothing** model was selected for monthly revenue forecasting.

The model was chosen because the monthly revenue series contains a strong growth trend and recurring yearly structure.

### Train/Test Setup

The data was divided chronologically:

- **Training period:** January 2022 – June 2024
- **Testing period:** July 2024 – December 2024
- **Test horizon:** 6 months

The forecasting model was evaluated only on the unseen test period.

---

## 8. Forecast Model Performance

Holt-Winters was compared against a simple naive baseline.

The naive baseline assumes that future revenue remains approximately equal to the most recently observed monthly revenue.

| Model | MAE | RMSE |
|---|---:|---:|
| Naive Baseline | £9,832.60 | £11,418.79 |
| Holt-Winters | £6,727.27 | £8,110.73 |

Compared with the naive baseline, Holt-Winters reduced:

- **MAE by 31.58%**
- **RMSE by 28.97%**

This indicates lower forecasting error for Holt-Winters on the held-out July–December 2024 test period.

---

## 9. 2025 Revenue Forecast

After evaluation, the Holt-Winters model was refitted using the complete 2022–2024 monthly revenue series.

The resulting 12-month forecast for 2025 was:

| Month | Forecast Revenue |
|---|---:|
| January | £525,534.78 |
| February | £509,669.88 |
| March | £534,288.37 |
| April | £529,497.15 |
| May | £544,176.48 |
| June | £537,029.47 |
| July | £544,695.78 |
| August | £545,380.72 |
| September | £537,721.96 |
| October | £545,559.16 |
| November | £539,576.57 |
| December | £541,113.11 |

### Forecast Summary

- **Total 2025 forecast revenue:** £6.43M
- **Average monthly forecast:** £536,186.95
- **Minimum monthly forecast:** £509,669.88
- **Maximum monthly forecast:** £545,559.16

The forecast provides a relatively stable monthly revenue baseline around £0.51M–£0.55M.

---

## 10. Pricing Scenario Analysis

Illustrative scenarios were generated using the estimated elasticity from the controlled log-log model.

Two scenarios were evaluated:

- **Price increase of 5%**
- **Price decrease of 10%**

The estimated model-based effects were:

| Scenario | Estimated Demand Change | Estimated Revenue Change |
|---|---:|---:|
| Baseline | 0.00% | 0.00% |
| Price +5% | +3.73% | +8.91% |
| Price −10% | −7.60% | −16.84% |

These results are **illustrative model-based scenarios**, not statistically validated causal predictions.

Because the estimated elasticity was not statistically significant, these scenarios should be used for sensitivity exploration rather than direct pricing decisions.

---

## 11. Power BI Dashboard

A Power BI dashboard was developed to communicate the analysis interactively.

The dashboard contains:

### KPI Cards

- Total Revenue: **£8.17M**
- Total Energy: **13.81M kWh**
- Total Sessions: **294K**
- 2025 Forecast Revenue: **£6.43M**

### Visualizations

- Monthly Revenue Trend
- 2025 Revenue Forecast
- Monthly Price & Charging Demand
- Revenue Impact of Pricing Scenarios

The dashboard provides a consolidated view of historical performance, forecast revenue, pricing behaviour, and scenario analysis.

---

## 12. Key Insights

### Revenue Growth

Revenue increased from approximately **£1.03M in 2022 to £4.55M in 2024**.

### Session Growth

The number of charging sessions increased substantially over the same period, contributing to the overall revenue expansion.

### Forecasting

Holt-Winters achieved lower MAE and RMSE than the naive baseline on the held-out 2024 period.

### 2025 Outlook

The model forecasts approximately **£6.43M of revenue for 2025**, with relatively stable monthly revenue around £0.54M.

### Price Sensitivity

The controlled model did not provide statistically significant evidence of a reliable price-demand relationship using the aggregated monthly data.

### Scenario Analysis

The +5% and −10% pricing scenarios demonstrate how the estimated elasticity can be used to explore hypothetical outcomes, but they should not be interpreted as causal forecasts.

---

## 13. Limitations

Several limitations should be considered:

1. The price elasticity analysis uses aggregated monthly observations, resulting in only 36 data points.
2. The estimated elasticity was not statistically significant.
3. The positive elasticity estimate is counterintuitive and may reflect underlying trends or omitted variables rather than a genuine causal pricing effect.
4. The forecasting evaluation uses a six-month holdout period.
5. The 2025 forecast assumes that historical revenue patterns continue into the forecast horizon.
6. The pricing scenarios are model-based sensitivity estimates rather than experimentally validated pricing responses.

Future analysis could use session-level or station-level data and incorporate variables such as charger type, user type, station, time of day, day of week, seasonality, and regional effects.

---

## 14. Conclusion

The analysis demonstrates a substantial expansion in EV charging revenue between 2022 and 2024.

Holt-Winters forecasting provided lower prediction errors than a simple naive baseline and produced an estimated 2025 annual revenue of approximately **£6.43M**.

The pricing analysis highlights an important analytical limitation: the available aggregated monthly data does not provide strong statistical evidence for a reliable price-demand relationship. Consequently, the pricing scenarios are presented as illustrative sensitivity analyses rather than causal recommendations.

The combination of Python-based analysis, statistical modelling, forecasting, scenario analysis, and Power BI visualization provides an end-to-end view of EV charging revenue performance and future planning.