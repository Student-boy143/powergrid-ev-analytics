# Power BI Implementation Guide — Analysis 8: Dynamic Pricing Strategy

## Dashboard Title
**EV Charging Network — Dynamic Pricing Strategy**

---

## 1. Overview & Data Architecture

This guide details how to build and maintain the Power BI executive dashboard for Analysis 8 using the prepared, validated dynamic pricing datasets.

### Source Datasets (in `tasks/task_08_dynamic_pricing/powerbi/`):
1. `pricing_baseline.csv`: 16 segment rows (Overall, Time Bands, Charger Types, Regions) with session volume, energy, total revenue, mean/median prices, and weighted prices.
2. `pricing_scenarios.csv`: 5 scenario definitions with mean proposed price, min/max prices, and % price changes vs baseline.
3. `revenue_impact.csv`: 20 rows evaluating 5 scenarios across 4 demand elasticity levels ($\epsilon = 0.0, -0.2, -0.5, -0.75$), detailing simulated energy, % energy change, simulated revenue, and revenue differences.
4. `pricing_recommendations.csv`: 5 strategic recommendations with priority, strategy pillar, target segment, recommended tariff, objective, evidence basis, and implementation type.

---

## 2. Data Model & Relationships

- **Fact Table**: `revenue_impact` (Primary fact table for scenario simulations).
- **Dimension Tables**:
  - `pricing_scenarios` (Key: `scenario_name`)
  - `pricing_baseline` (Key: `segment_name`)
  - `pricing_recommendations` (Key: `priority`)

---

## 3. Core DAX Measures

```dax
// Baseline Observed Revenue (£M)
Baseline Revenue M = 8.171

// Baseline Weighted Price (£/kWh)
Baseline Weighted Price = 0.5917

// Simulated Revenue Selected Scenario (£M)
Simulated Revenue M = 
DIVIDE(
    SELECTEDVALUE('revenue_impact'[simulated_revenue_gbp], 8171073.69),
    1000000,
    0
)

// Revenue Uplift (£)
Revenue Uplift GBP = 
SELECTEDVALUE('revenue_impact'[revenue_diff_gbp], 0)

// Revenue Uplift %
Revenue Uplift Pct = 
SELECTEDVALUE('revenue_impact'[revenue_diff_pct], 0)

// Simulated Energy Delivered (GWh)
Simulated Energy GWh = 
DIVIDE(
    SELECTEDVALUE('revenue_impact'[simulated_energy_kwh], 13808745.8),
    1000000,
    0
)

// Elasticity Scenario Slicer
Selected Elasticity = 
SELECTEDVALUE('revenue_impact'[elasticity_label], "Moderate Elasticity (e = -0.5)")
```

---

## 4. Visual Layout Specifications (1920 × 1080)

### Visual 1: Executive Banner (Top)
- **Visual Type**: Text Box / Shape Banner
- **Text**: `EV Charging Network — Dynamic Pricing Strategy`
- **Background**: `#0078D4` (Segoe UI Bold, 22pt, White font).

### Visual 2: Executive KPI Cards (Row 1)
1. **Baseline Weighted Price**: Card visual (`£0.59 / kWh`). Subtitle: "Historical Mean: £0.5923/kWh".
2. **Baseline Revenue**: Card visual (`£8.17M`). Subtitle: "13.81M kWh Across 294,024 Sessions".
3. **Primary Recommended Strategy**: Card visual (`Time-of-Use + Tier`). Subtitle: "TOU Peak/Off-Peak + Speed Velocity".
4. **Simulated Revenue Potential**: Card visual (`£8.32M – £8.67M`). Subtitle: "+1.8% to +6.1% Uplift vs Baseline".

### Visual 3: Scenario Revenue Comparison (Middle Left)
- **Visual Type**: Clustered Column Chart with Baseline Reference Line
- **X-axis**: `pricing_scenarios[scenario_name]`.
- **Y-axis**: `revenue_impact[simulated_revenue_gbp]` (in £ Millions).
- **Legend**: `revenue_impact[elasticity_label]`.
- **Constant Line**: At `£8.171M` styled with dashed Red line (`#D83B01`) labelled "Baseline (£8.17M)".

### Visual 4: Recommended Dynamic Tariff Matrix (Middle Right)
- **Visual Type**: Clustered Column Chart
- **X-axis**: Time Bands (Off-Peak 22:00–05:00, Daytime Normal 06:00–21:00, Midday Peak 10:00–14:00).
- **Y-axis**: Proposed Tariff (£/kWh).
- **Series**: DC_50kW (Purple `#5C2D91`), DC_150kW (Blue `#0078D4`), DC_300kW (Orange-Red `#D83B01`).
- **Reference Line**: Flat Baseline Price (£0.592/kWh).

### Visual 5: Baseline Price by Region & Infrastructure Pressure (Bottom Left)
- **Visual Type**: Column Chart
- **X-axis**: `pricing_baseline[segment_name]` (Filtered to `segment_type = "Region"`).
- **Y-axis**: `pricing_baseline[weighted_price_per_kwh]`.
- **Highlight**: Midlands, London, and North West highlighted in Green (`#107C41`) to represent high infrastructure utilization.

### Visual 6: Strategy Recommendations Table (Bottom Right)
- **Visual Type**: Formatted Table / Card visual
- **Columns**: Priority, Strategy Pillar, Recommended Tariff, Objective, Implementation Type.

---

## 5. Verification & Export
A high-resolution visual capture of this exact Power BI report layout is rendered and verified at:
`tasks/task_08_dynamic_pricing/powerbi/dynamic_pricing_dashboard.png`.

