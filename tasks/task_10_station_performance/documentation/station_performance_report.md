# EV Charging Station Performance Benchmarking

## 1. Executive Summary

This analysis benchmarks the performance of 33 EV charging stations using charging activity, energy delivery and revenue generation metrics.

The objective is to identify high-performing and underperforming stations, compare regional performance, and understand differences across station activation years.

The analysis covers 294,024 charging sessions, approximately 13.81 million kWh of energy delivered and £8.17 million in revenue.

## 2. Business Objectives

- Identify the highest- and lowest-performing charging stations.
- Compare station productivity after accounting for charger count and observed period.
- Evaluate regional performance differences.
- Examine performance differences across activation years.
- Develop a comparative station performance ranking.

## 3. Methodology

Station-level metrics were calculated by aggregating charging sessions.

Key productivity metrics:

- Sessions per charger per day.
- Energy per charger per day.
- Revenue per charger per day.

The observed period was calculated using the first and last recorded session timestamps for each station.

A composite performance score was developed using percentile ranks of the three productivity metrics, with equal weighting.

## 4. Key Findings

### Overall Station Performance

Top-performing stations:

- STN_014 and STN_028: 92.93
- STN_006: 91.92
- STN_018: 87.88
- STN_007: 83.84

Lowest-performing stations:

- STN_021: 5.05
- STN_023: 10.10
- STN_009: 13.13
- STN_026: 14.14
- STN_013: 21.21

### Regional Performance

- South West achieved the highest average performance score of 73.74.
- Midlands followed with 65.66.
- London achieved 65.45.
- Wales recorded the lowest average score of 13.13, based on only one station.
- North West had the largest regional sample, with seven stations.

### Regional Consistency

- North West had the lowest standard deviation among regions with multiple stations (18.23).
- South East had the highest standard deviation (34.94).
- South West combined a high average score with comparatively lower variation.

### Activation-Year Comparison

- 2022 stations had the highest average performance score: 64.27.
- 2024 stations averaged 49.31.
- 2023 stations averaged 44.11.

These are descriptive comparisons and do not establish that station age causes better performance.

## 5. Performance Score and Sensitivity Analysis

The composite score uses equal-weighted percentile rankings of:

1. Sessions per charger per day.
2. Energy per charger per day.
3. Revenue per charger per day.

Sensitivity analysis using balanced, demand-focused and revenue-focused weights showed that several leading and trailing stations retained relatively stable positions.

STN_028 and STN_014 consistently ranked near the top, while STN_021 remained last.

## 6. Business Recommendations

- Investigate operational and demand factors behind high-performing stations.
- Study the practices and characteristics of leading stations before considering expansion or replication.
- Investigate low-performing stations to identify potential demand, accessibility or infrastructure issues.
- Prioritise further investigation of regions with substantial station-level variation.
- Monitor station productivity regularly using comparable per-charger metrics.

## 7. Limitations

- Observed days represent the first-to-last recorded session span, not verified operational uptime.
- Station coordinates and detailed local demand information are unavailable.
- The composite score uses analyst-selected equal weights.
- The score is a relative benchmark, not a validated efficiency or profitability measure.
- Regional averages are affected by differences in station sample sizes.
- Wales has only one station, limiting regional conclusions.
- Activation-year comparisons may be influenced by regional demand and other differences between stations.

## 8. Deliverables

- Station performance benchmarking notebook.
- Validated station-level benchmarking CSV.
- Power BI dashboard.
- Dashboard screenshot.
- Analysis documentation.

## 9. Conclusion

The analysis demonstrates substantial variation in charging station productivity across the network.

The benchmarking framework provides a structured way to compare stations, identify potential improvement opportunities and support operational decision-making.

The results should be interpreted as descriptive performance comparisons rather than causal evidence of operational efficiency or profitability.