# Task 1 — EV Charging Demand Forecasting

## 1. Objective

The objective of this task is to forecast EV charging energy demand using historical UK EV charging session data from 2022–2024.

The analysis focuses on daily total charging energy (`energy_kwh`) and evaluates forecasting models using RMSE and MAPE.

---

## 2. Dataset

The dataset contains EV charging activity from 2022 to 2024.

Relevant session fields used in this task include:

- `start_timestamp`
- `energy_kwh`
- `station_id`
- `charger_id`
- `charger_type`
- `user_type`
- `station_region`

The session dataset contains 294,024 charging sessions.

Daily demand was calculated by aggregating total `energy_kwh` for each calendar day.

This produced 1,096 daily observations covering 1 January 2022 to 31 December 2024.

---

## 3. Exploratory Analysis

The daily demand series showed:

- Strong year-over-year growth in charging demand.
- Noticeable weekly seasonality.
- Weekday demand was substantially higher than weekend demand.
- Daily demand showed considerable variation around the overall upward trend.

Average demand by day of week indicated that Monday–Friday demand was approximately 13.6–13.7 thousand kWh/day, while Saturday and Sunday demand was approximately 10 thousand kWh/day.

---

## 4. Train/Test Strategy

Because this is a time-series forecasting problem, the data was split chronologically rather than randomly.

- Training period: 1 January 2022 – 2 October 2024
- Testing period: 3 October 2024 – 31 December 2024
- Training observations: 1,006
- Testing observations: 90

The final 90 days were kept completely unseen during model training.

---

## 5. SARIMA Model

A SARIMA model was used as the statistical forecasting baseline.

Model:

SARIMA(1,1,1)(1,1,1,7)

The seasonal period of 7 represents weekly seasonality in the daily demand data.

### SARIMA Results

- RMSE: 1,856.88 kWh
- MAPE: 7.95%

The model provided a reasonable baseline and captured the general temporal structure of the demand series.

---

## 6. XGBoost Model

XGBoost was used as a machine-learning forecasting approach.

Feature engineering included:

- Day of week
- Month
- Day of year
- Year
- Lag 1 day
- Lag 7 days
- Lag 14 days
- Seven-day rolling average

Lagged and rolling features were constructed using only historical information to prevent data leakage.

### XGBoost Results

- RMSE: 1,123.90 kWh
- MAPE: 4.29%

---

## 7. Model Comparison

| Model | RMSE (kWh) | MAPE |
|---|---:|---:|
| SARIMA | 1,856.88 | 7.95% |
| XGBoost | 1,123.90 | 4.29% |

XGBoost produced lower RMSE and MAPE than SARIMA on the same unseen 90-day test period.

Therefore, XGBoost was selected as the preferred model for the forecasting stage.

---

## 8. Feature Importance

The XGBoost model identified `year` as the dominant feature, followed by lag and recent-demand features.

The importance ranking was approximately:

1. Year
2. Lag 1
3. Rolling 7-day average
4. Day of week
5. Lag 7
6. Day of year
7. Lag 14
8. Month

The strong importance of the year feature is consistent with the substantial increase in charging demand between 2022 and 2024.

---

## 9. Future Forecast

The selected XGBoost model was used to generate recursive forecasts for 30, 90 and 365 days.

| Forecast Horizon | Average Predicted Daily Demand |
|---|---:|
| 30 days | 21,334.91 kWh/day |
| 90 days | 21,148.30 kWh/day |
| 365 days | 21,310.52 kWh/day |

The forecasts indicate expected daily charging demand of approximately 21.3 MWh/day over the future forecast period.

The long-horizon forecast remains relatively stable. Therefore, the 365-day forecast should be interpreted as a baseline planning forecast rather than a highly dynamic long-term prediction.

---

## 10. Business Insights

### Increasing Demand

EV charging demand increased substantially between 2022 and 2024. This indicates increasing utilisation of the charging network and the need to plan future charging capacity accordingly.

### Weekly Demand Pattern

Weekday charging demand is significantly higher than weekend demand. Operators can use this pattern for capacity planning, staffing and maintenance scheduling.

### Capacity Planning

The increasing demand trend suggests that charging infrastructure should be monitored proactively to avoid future capacity constraints.

### Forecast-Based Planning

The XGBoost model provides a data-driven estimate of future charging demand that can support operational and infrastructure planning.

---

## 11. Recommendations

1. Use demand forecasts to support charging infrastructure expansion decisions.
2. Monitor weekday peak demand and station-level utilisation.
3. Schedule maintenance during lower-demand periods where operationally possible.
4. Continuously retrain the forecasting model as new charging data becomes available.
5. Monitor forecast accuracy using RMSE and MAPE.
6. Use the 30-day forecast for short-term operational planning and the longer forecasts for strategic capacity planning.

---

## 12. Conclusion

Two forecasting approaches were evaluated: SARIMA and XGBoost.

XGBoost achieved the better performance on the unseen 90-day test period, with an RMSE of 1,123.90 kWh and MAPE of 4.29%, compared with SARIMA's RMSE of 1,856.88 kWh and MAPE of 7.95%.

The selected XGBoost model was subsequently used to generate 30-day, 90-day and 365-day demand forecasts.

The analysis demonstrates that EV charging demand has a strong upward trend and a clear weekly usage pattern, providing useful information for capacity and operational planning.