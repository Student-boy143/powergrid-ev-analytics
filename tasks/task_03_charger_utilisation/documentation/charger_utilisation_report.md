# EV Charger Utilisation & Efficiency Analysis

## 1. Project Overview

This analysis evaluates the operational utilisation and energy performance of EV charging infrastructure across 33 charging stations and 199 chargers.

The analysis focuses on charger usage, charging throughput, observed time utilisation, capacity utilisation, charger-type performance, and station-level performance profiles.

---

## 2. Business Objectives

The analysis aims to answer:

- How frequently are chargers being used?
- How much energy is delivered by different charger types?
- How does observed utilisation vary across chargers and stations?
- Which stations deliver the highest energy throughput per charger?
- Which stations show potentially lower utilisation?
- Do different charger capacities exhibit different usage patterns?

---

## 3. Dataset

The analysis uses the EV charging dataset containing:

- 33 stations
- 199 chargers
- 294,024 charging sessions

The sessions dataset contains timestamps, charging duration, energy consumption, charger type, charger ID, station ID, and other session-level attributes.

---

## 4. Analytical Methodology

The raw session-level data was aggregated to two analytical grains:

### Charger level

One row represents one charger.

Metrics include:

- Total sessions
- Total charging hours
- Unique charging hours
- Total energy delivered
- Average delivered power
- Capacity utilisation
- Observed time utilisation

### Station level

One row represents one station.

Metrics include:

- Number of chargers
- Total sessions
- Total energy delivered
- Average time utilisation
- Average capacity utilisation
- Energy per charger
- Sessions per charger
- Performance profile

---

## 5. Time Utilisation Method

Initial utilisation calculations based on charger installation dates produced invalid results because a substantial number of sessions occurred before the recorded installation dates.

The analysis also identified overlapping sessions for the same charger. Simply summing session durations would therefore overestimate occupied time.

To address this, overlapping charging intervals were merged and unique charging time was calculated.

Observed time utilisation was then calculated as:

`Unique charging hours / Observed first-to-last session window`

This metric represents observed charging activity within the available session window. It is not a direct measure of physical charger availability or uptime because reliable outage and maintenance information was not available.

---

## 6. Capacity Utilisation

Average delivered charging power was calculated as:

`Total energy delivered / Total charging hours`

Capacity utilisation was then calculated as:

`Average delivered power / Rated charger capacity × 100`

This measures the proportion of rated charging power being delivered during charging activity.

It should not be interpreted as electrical efficiency.

---

## 7. Charger-Type Analysis

The analysis identified differences between charger types.

Average results:

| Charger Type | Chargers | Avg Sessions | Avg Energy (kWh) | Avg Time Utilisation | Avg Capacity Utilisation |
|---|---:|---:|---:|---:|---:|
| DC_50kW | 95 | 1,440 | 47,485 | 7.44% | 79.95% |
| DC_150kW | 69 | 1,405 | 68,818 | 3.66% | 79.96% |
| DC_300kW | 35 | 1,721 | 129,978 | 3.25% | 77.52% |

DC_300kW chargers delivered the highest average energy per charger, while DC_50kW chargers showed the highest observed time utilisation.

This demonstrates that occupancy and energy throughput are different dimensions of charger performance.

---

## 8. Station-Level Performance

Station performance was evaluated using both total energy and energy per charger.

Energy per charger was introduced to normalise station performance for differences in installed charger capacity.

For example, STN_006 delivered approximately 778,478 kWh in total and approximately 194,620 kWh per charger, while STN_002 delivered approximately 940,999 kWh overall but approximately 188,200 kWh per charger.

Therefore, total station energy alone does not fully describe infrastructure utilisation.

---

## 9. Station Performance Profiles

Stations were classified using the median observed time utilisation and median energy per charger.

The resulting distribution was:

| Performance Profile | Stations | Percentage |
|---|---:|---:|
| High Utilisation / High Throughput | 10 | 30.30% |
| Lower Utilisation / Lower Throughput | 9 | 27.27% |
| Lower Utilisation / High Throughput | 7 | 21.21% |
| High Utilisation / Lower Throughput | 7 | 21.21% |

The results show that station performance varies across multiple dimensions.

The lower-utilisation groups should be treated as candidates for further investigation rather than automatically classified as underperforming assets.

---

## 10. Key Findings

1. Charger performance differs substantially by charger type.
2. DC_300kW chargers have the highest average energy throughput per charger.
3. DC_50kW chargers have the highest observed time utilisation.
4. Capacity utilisation is relatively consistent across charger types at approximately 78–80%.
5. Station rankings change when performance is normalised by charger count.
6. Observed time utilisation and energy throughput measure different operational characteristics.
7. A single performance metric would therefore provide an incomplete view of charging infrastructure performance.

---

## 11. Power BI Dashboard

The Power BI dashboard provides:

- Network-level KPI metrics
- Energy delivered by charger type
- Observed time utilisation by charger type
- Top stations by energy per charger
- Station performance profile distribution

The dashboard is designed to support operational comparison and identify stations requiring further investigation.

---

## 12. Limitations

- Installation dates were inconsistent with observed session timestamps and were therefore not used as the availability denominator.
- Overlapping sessions were present and required interval merging to avoid double-counting occupied time.
- The dataset does not provide reliable charger uptime, outage, or maintenance information.
- Observed time utilisation should therefore not be interpreted as true physical availability utilisation.
- Performance profiles are relative classifications based on network medians and do not independently establish business underperformance.
- Higher energy throughput for higher-capacity chargers is partly expected because of their greater rated power.

---

## 13. Conclusion

The analysis demonstrates that EV charging infrastructure should be evaluated using multiple operational metrics rather than a single utilisation measure.

Energy throughput, observed time utilisation, capacity utilisation, and normalised station performance provide complementary perspectives on charger and station behaviour.

The resulting analytical datasets and Power BI dashboard provide a foundation for identifying utilisation patterns and prioritising stations for deeper operational investigation.