# EV Charging Infrastructure — Regional Expansion Planning

## 1. Executive Summary

This analysis evaluates regional EV charging demand and existing infrastructure across nine UK regions to identify areas that may warrant further investment in charging infrastructure.

The analysis combines regional charging demand, charger availability, demand growth and a weighted expansion priority scoring model.

The results identify **Midlands, London and North West** as the leading regions for further expansion investigation under the balanced scoring approach.

Midlands achieved the highest expansion priority score of **78.89/100**, followed by London (74.44) and North West (71.11).

These results represent regional investment screening priorities rather than verified optimal physical locations.

## 2. Project Objectives

- Compare EV charging demand across UK regions.
- Evaluate existing station and charger infrastructure.
- Identify regions with high energy consumption per charger.
- Analyse regional demand growth between 2023 and 2024.
- Develop a weighted regional expansion priority score.
- Test the sensitivity of recommendations to different scoring assumptions.

## 3. Dataset Overview

The analysis uses three datasets from the EV charging dataset.

| Dataset | Records | Description |
|---|---:|---|
| Stations | 33 | Station identifiers, regions, activation years and charger counts |
| Chargers | 199 | Charger identifiers, types and power ratings |
| Sessions | 294,024 | Charging sessions, energy consumption, timestamps and revenue |

The datasets were connected using station identifiers and regional information.

### Data Validation

- All 33 stations were matched successfully.
- No missing station matches were identified.
- No regional mismatches were found.
- All 33 stations had recorded charging sessions.
- No missing values were identified in the station dataset.

## 4. Regional Demand and Infrastructure Analysis

Charging sessions were aggregated by region to calculate:

- Total charging sessions.
- Total energy consumption.
- Total revenue.
- Average energy per session.
- Number of stations.
- Number of chargers.
- Charging sessions per charger.
- Energy consumption per charger.

### Key Findings

North West recorded the highest overall charging demand, with 65,789 sessions and approximately 3.01 million kWh of energy consumption across the complete dataset period.

However, South West had the highest 2024 energy consumption per charger at approximately 51,166 kWh.

Midlands also showed significant infrastructure pressure, with approximately 49,445 kWh per charger in 2024.

These results demonstrate that the region with the highest total demand is not necessarily the region with the highest demand per charger.

## 5. Regional Demand Growth

Monthly demand was analysed using charging session timestamps.

A data coverage check identified that:

- Five regions had records from January 2022 to December 2024.
- Four regions had records only from January 2023 to December 2024.

Therefore, the growth comparison was restricted to January–December 2023 versus January–December 2024, ensuring that all nine regions were compared over the same 12-month period.

### Regional Energy Demand Growth

| Region | Energy Growth |
|---|---:|
| South East | 333.48% |
| Scotland | 203.97% |
| North West | 140.94% |
| London | 102.88% |
| Midlands | 70.19% |
| North East | 59.52% |
| South West | 38.91% |
| Yorkshire & Humber | 0.04% |
| Wales | -1.22% |

South East recorded the fastest growth, while North West maintained the highest absolute 2024 energy demand.

## 6. Expansion Priority Scoring Model

A weighted scoring model was developed to combine three regional indicators.

| Indicator | Weight |
|---|---:|
| Infrastructure pressure | 40% |
| Energy demand growth | 30% |
| Total 2024 energy demand | 30% |

Each indicator was converted into a percentile rank across the nine regions.

The final score was calculated as:

**Expansion Priority Score =**

(0.40 × Infrastructure Pressure Score + 0.30 × Growth Score + 0.30 × Demand Score) × 100

A higher score indicates a stronger regional priority for further investigation.

### Regional Priority Results

| Rank | Region | Priority Score |
|---:|---|---:|
| 1 | Midlands | 78.89 |
| 2 | London | 74.44 |
| 3 | North West | 71.11 |
| 4 | South West | 66.67 |
| 5 | South East | 63.33 |
| 6 | North East | 53.33 |
| 7 | Scotland | 45.56 |
| 8 | Yorkshire & Humber | 35.56 |
| 9 | Wales | 11.11 |

## 7. Sensitivity Analysis

Three scoring scenarios were evaluated to understand how changing indicator weights affects regional rankings.

| Scenario | Infrastructure | Growth | Demand |
|---|---:|---:|---:|
| Infrastructure-focused | 50% | 25% | 25% |
| Balanced | 33.33% | 33.33% | 33.33% |
| Growth-focused | 25% | 50% | 25% |

### Key Observations

- Midlands ranked first under infrastructure-focused and balanced scoring.
- London remained among the leading regions across scenarios.
- North West ranked first under growth-focused scoring.
- South East moved from fifth under the balanced model to joint first under growth-focused scoring.
- Wales and Yorkshire & Humber remained at the bottom across all three scenarios.

The results demonstrate that expansion priorities depend partly on business objectives and scoring assumptions.

## 8. Regional Expansion Recommendations

### High-Priority Investigation

**Midlands:** Highest balanced expansion priority score, supported by high energy consumption per charger and strong overall demand.

**London:** High charging activity per charger and significant year-over-year energy growth.

**North West:** Highest overall demand in the complete dataset, with substantial growth between 2023 and 2024.

### Additional Monitoring

**South West:** Highest 2024 energy consumption per charger. Despite slower growth, its infrastructure pressure warrants further investigation.

**South East:** Highest demand growth, making it an important region for monitoring future infrastructure requirements.

**Scotland:** Strong demand growth, although its current energy consumption per charger is lower than several other regions.

### Lower Immediate Priority

North East, Yorkshire & Humber and Wales received lower balanced priority scores. This does not establish that additional infrastructure is unnecessary; further local demand and coverage analysis would be required.

## 9. Power BI Dashboard

The Power BI dashboard presents the findings through:

- Total stations KPI Card.
- Total chargers KPI Card.
- Total regions KPI Card.
- Top expansion priority score KPI Card.
- Regional expansion priority ranking.
- 2024 regional energy demand.
- 2024 energy consumption per charger.
- Regional energy demand growth.
- Scoring sensitivity comparison.

The dashboard provides a consolidated view of regional demand, infrastructure pressure and expansion priorities.

## 10. Limitations

1. **No geographic coordinates:** The dataset contains regional labels but no latitude or longitude. Exact physical sites cannot be identified.
2. **No waiting-time information:** The analysis cannot establish whether chargers experience queues or congestion.
3. **No failed charging requests:** Unmet demand cannot be measured directly.
4. **Incomplete historical coverage:** Four regions have no recorded sessions in 2022, so growth comparisons use 2023 and 2024.
5. **No installation cost information:** The financial feasibility of new charging locations cannot be evaluated.
6. **Exploratory scoring weights:** The 40%, 30% and 30% weights are business assumptions rather than statistically optimised parameters.
7. **Demand per charger is not utilisation:** Energy consumed per charger does not directly measure charger occupancy or downtime.

## 11. Conclusion

This analysis provides a regional screening framework for EV charging infrastructure expansion.

Midlands emerged as the leading region under the balanced scoring model, followed by London and North West. South West showed the highest energy consumption per charger, while South East demonstrated the fastest demand growth.

The combined analysis illustrates why infrastructure planning should consider current demand, demand growth and available charging capacity together.

The findings can guide further investigation, but precise site selection would require geographic coordinates, traffic patterns, local EV adoption, charger occupancy, waiting times, land availability and installation costs.

**Final recommendation:** Prioritise Midlands, London and North West for further regional expansion investigation, while monitoring South West and South East for infrastructure pressure and accelerating demand respectively.