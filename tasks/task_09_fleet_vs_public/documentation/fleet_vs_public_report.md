# Fleet vs Public EV Charging Behaviour Analysis

## 1. Project Objective

The objective of this analysis is to compare EV charging behaviour between Commercial and Public users across the charging network from 2022 to 2024.

The analysis examines session activity, energy consumption, revenue contribution, charging duration, regional demand, charger-type preferences, payment methods and usage trends.

For this comparison, the original `fleet`, `delivery` and `taxi` categories are grouped into **Commercial**, while `public` remains a separate **Public** segment.

## 2. Dataset Overview

The analysis uses the EV charging sessions dataset.

| Attribute | Details |
|---|---|
| Total sessions | 294,024 |
| Time period | 2022–2024 |
| Commercial sessions | 220,270 |
| Public sessions | 73,754 |
| Commercial share | 74.92% |
| Public share | 25.08% |
| Missing values | None |

The dataset includes charging timestamps, energy consumption, session duration, total cost, payment method, user type, customer ID, charger type and station region.

## 3. Methodology

The analysis followed these steps:

1. Grouped `fleet`, `delivery` and `taxi` into Commercial.
2. Retained Public as a separate customer segment.
3. Compared session counts, energy consumption, revenue and charging duration.
4. Examined regional charging activity.
5. Compared charger-type and payment-method preferences.
6. Analysed monthly and annual trends.
7. Examined customer-ID-level aggregates to understand repeated usage patterns.

Descriptive comparisons were used as the primary analytical approach. No causal relationship or statistically significant difference is claimed.

## 4. Key Findings

### 4.1 Overall Segment Comparison

| Metric | Commercial | Public |
|---|---:|---:|
| Sessions | 220,270 | 73,754 |
| Customer IDs | 7,510 | 6,452 |
| Energy consumed | 10.34M kWh | 3.46M kWh |
| Average energy/session | 46.96 kWh | 46.96 kWh |
| Total revenue | £6.12M | £2.05M |
| Average revenue/session | £27.79 | £27.80 |
| Average duration/session | 35.10 min | 35.08 min |
| Sessions per ID | 29.33 | 11.43 |

Commercial users account for approximately 74.92% of charging sessions and 74.90% of revenue.

Although Commercial users generate substantially more total demand, the average energy consumed, revenue and charging duration per session are almost identical between the two segments.

### 4.2 Regional Charging Behaviour

Commercial sessions exceed Public sessions in all nine regions.

- North West records the highest session volume for both segments.
- Wales records the lowest session volume for both segments.
- Midlands and North West also demonstrate substantial Commercial charging activity.

These are raw session totals and are not adjusted for the number of stations in each region.

### 4.3 Charger-Type Preferences

The distribution of charger types is almost identical between the two segments.

| Charger type | Commercial | Public |
|---|---:|---:|
| DC_50kW | 46.55% | 46.49% |
| DC_150kW | 32.97% | 33.02% |
| DC_300kW | 20.48% | 20.49% |

DC_50kW chargers account for the largest share of sessions in both segments.

### 4.4 Payment Method Preferences

Payment behaviour is also very similar.

| Payment method | Commercial | Public |
|---|---:|---:|
| App | 33.50% | 33.19% |
| Card | 33.28% | 33.29% |
| RFID | 33.22% | 33.52% |

No major difference in payment-method preference is evident.

### 4.5 Monthly and Annual Trends

Both segments experienced substantial growth in charging activity between 2022 and 2024.

| Year | Commercial sessions | Public sessions |
|---|---:|---:|
| 2022 | 30,456 | 10,184 |
| 2023 | 64,501 | 21,631 |
| 2024 | 125,313 | 41,939 |

Year-over-year session growth:

- Commercial: 111.78% in 2023 and 94.28% in 2024.
- Public: 112.40% in 2023 and 93.88% in 2024.

The growth patterns are very similar across the two segments.

### 4.6 Customer-ID-Level Usage

Commercial customer-ID records show higher average usage:

- Commercial: 29.33 sessions per ID.
- Public: 11.43 sessions per ID.

Commercial records also show higher average energy consumption and revenue per ID.

However, customer IDs are not reliable unique-person identifiers in this dataset. A large number of IDs appear under multiple original user types. Therefore, these figures represent ID-level aggregates, not confirmed individual customer behaviour.

## 5. Business Recommendations

1. **Commercial customer engagement:** Explore fleet-oriented packages and account-based services, given the larger Commercial share of charging activity.
2. **Regional planning:** Consider regional demand when planning capacity improvements, particularly in the North West and Midlands.
3. **Consistent charging experience:** Since charger-type preferences are similar, maintain a balanced charger portfolio that serves both segments.
4. **Flexible payment options:** Continue supporting app, card and RFID payments because usage is distributed almost equally.
5. **Further customer analysis:** Improve customer identification and segmentation to understand individual usage patterns more accurately.

These recommendations are exploratory and should be validated using operational and customer-level information.

## 6. Limitations

- Customer IDs appear across multiple user types and cannot be treated as unique individual customers.
- Regional session totals are not normalized by station count or installed charging capacity.
- The analysis is descriptive and does not establish causal relationships.
- The dataset does not provide detailed customer demographics or fleet-level business information.
- Session records may be correlated within customers, so they should not automatically be treated as independent observations for statistical testing.
- Results depend on the coverage and quality of the supplied dataset.

## 7. Conclusion

The analysis shows that Commercial users account for approximately three-quarters of total charging activity and revenue. However, Commercial and Public users demonstrate remarkably similar behaviour at the individual session level, including energy consumption, revenue and charging duration.

Both segments also show similar charger-type and payment-method preferences, as well as comparable growth patterns between 2022 and 2024.

The main distinction is therefore the volume and frequency of charging activity rather than the characteristics of an average charging session.

This analysis provides useful insights for EV charging network planning, customer engagement and infrastructure management.