# Power BI Implementation Guide — Analysis 7: Time-Based Demand Analysis

## Dashboard Title
**EV Charging Network — Time-Based Demand Analysis**

---

## 1. Overview & Data Architecture

This guide details how to build and maintain the Power BI executive dashboard for Analysis 7 using the prepared, validated analytical datasets.

### Source Datasets (in `tasks/task_07_time_day_analysis/powerbi/`):
1. `hourly_demand.csv`: 24 hourly rows (00:00 to 23:00) with sessions, total energy, average energy per session, daily averages, and percentage shares.
2. `daily_demand.csv`: 1,096 calendar daily rows (2022–2024) with date, day name, weekday vs. weekend flag, sessions, energy, and revenue.
3. `weekday_weekend_comparison.csv`: 7 rows (Monday through Sunday) with day-of-week averages and shares.
4. `regional_time_patterns.csv`: 216 rows (9 regions × 24 hours) with regional hourly session distributions.
5. `charger_type_time_patterns.csv`: 72 rows (3 charger types × 24 hours) with dwell duration and throughput metrics.

---

## 2. Data Model & Relationships

- **Fact Table**: `daily_demand` (connected via Date to an automatic Power BI Date Table).
- **Dimension / Summary Tables**:
  - `hourly_demand` (Key: `hour` [0–23])
  - `weekday_weekend_comparison` (Key: `day_of_week` [0–6])
  - `regional_time_patterns` (Key: `station_region`, `hour`)
  - `charger_type_time_patterns` (Key: `charger_type`, `hour`)

---

## 3. Core DAX Measures

```dax
// Total Sessions Across Network
Total Sessions = SUM('daily_demand'[total_sessions])

// Total Energy Delivered (MWh)
Total Energy MWh = SUM('daily_demand'[total_energy_kwh]) / 1000

// Total Revenue (£M)
Total Revenue M = SUM('daily_demand'[total_revenue_gbp]) / 1000000

// Average Daily Sessions
Avg Daily Sessions = AVERAGE('daily_demand'[total_sessions])

// Weekday Mean Daily Sessions
Weekday Avg Sessions = 
CALCULATE(
    AVERAGE('daily_demand'[total_sessions]),
    'daily_demand'[day_type] = "Weekday"
)

// Weekend Mean Daily Sessions
Weekend Avg Sessions = 
CALCULATE(
    AVERAGE('daily_demand'[total_sessions]),
    'daily_demand'[day_type] = "Weekend"
)

// Weekday to Weekend Uplift %
Weekday Uplift Pct = 
DIVIDE([Weekday Avg Sessions] - [Weekend Avg Sessions], [Weekend Avg Sessions], 0)

// Average Energy per Session (kWh)
Avg Energy per Session = 
DIVIDE(SUM('daily_demand'[total_energy_kwh]), SUM('daily_demand'[total_sessions]), 0)

// Peak Hour Session Volume
Peak Hour Volume = 
MAXX('hourly_demand', 'hourly_demand'[total_sessions])
```

---

## 4. Visual Layout Specifications (1920 × 1080)

### Visual 1: Executive Banner (Top)
- **Visual Type**: Text Box / Shape Banner
- **Text**: `EV Charging Network — Time-Based Demand Analysis`
- **Background**: `#0078D4` (Segoe UI Bold, 22pt, White font).

### Visual 2: Executive KPI Cards (Row 1)
1. **Total Sessions**: Card visual (`294K` / 294,024 sessions). Subtitle: "Full 3-Year Network Total".
2. **Total Energy Delivered**: Card visual (`13.81M kWh`). Subtitle: "46.96 kWh Avg per Session".
3. **Peak Charging Hour**: Card visual (`10:00 AM`). Subtitle: "24,214 Sessions (22.1 / Day)".
4. **Highest-Demand Day**: Card visual (`Tuesday`). Subtitle: "292.5 Avg Daily Sessions (+37.8% vs Sun)".

### Visual 3: Diurnal Load Distribution (Middle Left)
- **Visual Type**: Clustered Column Chart
- **X-axis**: `hourly_demand[hour]` formatted as `hh:00`.
- **Y-axis**: `hourly_demand[total_sessions]` (in Thousands).
- **Formatting**: Default `#0078D4`, Data Colors rule for hours 10–14 set to `#107C41` (Green) to highlight the 41.0% Peak Plateau window.

### Visual 4: Day-of-Week Demand Profile (Middle Right)
- **Visual Type**: Clustered Column Chart
- **X-axis**: `weekday_weekend_comparison[day_name]` (Sorted Mon–Sun by `day_of_week`).
- **Y-axis**: `weekday_weekend_comparison[avg_sessions_per_day]`.
- **Data Callout**: Mon–Fri highlighted in Blue (`#0078D4`), Sat–Sun in Orange (`#D83B01`) demonstrating the -27.0% weekend dip.

### Visual 5: Regional Hourly Peaks (Bottom Left)
- **Visual Type**: Line Chart
- **X-axis**: `regional_time_patterns[hour]`.
- **Y-axis**: `regional_time_patterns[regional_session_share_pct]`.
- **Legend**: `regional_time_patterns[station_region]`.
- **Insight**: Demonstrates synchronized 10:00–14:00 peak plateau across all 9 UK regions.

### Visual 6: Charger Type Demand & Dwell Duration (Bottom Right)
- **Visual Type**: Line and Clustered Column Combo Chart
- **Shared Axis**: `charger_type_time_patterns[charger_type]`.
- **Column Values**: `total_sessions` (in Thousands).
- **Line Values**: `avg_duration_minutes` (Orange line on secondary axis).
- **Insight**: Highlights bay occupancy disparity (50kW = 49.5 min vs. 300kW = 19.5 min).

---

## 5. Verification & Export
A high-resolution visual capture of this exact Power BI report layout is rendered and verified at:
`tasks/task_07_time_day_analysis/powerbi/time_day_analysis_dashboard.png`.

