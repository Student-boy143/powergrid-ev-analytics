# Customer Behaviour Segmentation

## 1. Objective

The objective of this analysis is to identify distinct customer behaviour patterns in the EV charging network using customer-level charging activity.

The analysis focuses on understanding:

- Customer charging frequency
- Energy consumption
- Session behaviour
- Customer spending
- Average charging characteristics
- Differences between customer segments

The resulting segments can support targeted customer engagement, retention strategies and network planning.

---

## 2. Dataset

The analysis uses the EV charging sessions dataset containing 294,024 charging sessions and 8,000 unique customers.

The `customer_id` field provides a persistent identifier, allowing customer-level behavioural profiles to be created across multiple charging sessions.

---

## 3. Customer-Level Feature Engineering

Customer profiles were created by aggregating charging sessions by `customer_id`.

The following features were used:

| Feature | Description |
|---|---|
| `total_sessions` | Total number of charging sessions |
| `total_energy_kwh` | Total energy consumed |
| `avg_energy_per_session` | Average energy consumed per session |
| `avg_session_duration` | Average charging duration |
| `total_spend` | Total customer spending |
| `avg_spend_per_session` | Average spending per session |
| `avg_price_per_kwh` | Average price paid per kWh |

This produced a customer-level dataset containing 8,000 customer profiles.

---

## 4. Data Preprocessing

Several behavioural features were positively skewed because a small number of customers had substantially more sessions, energy consumption and spending than typical customers.

Log transformation using `log1p()` was applied to:

- `total_sessions`
- `total_energy_kwh`
- `total_spend`
- `avg_energy_per_session`

The features were then standardized using `StandardScaler` so that variables with different numerical scales would contribute appropriately to clustering.

---

## 5. Clustering Method

K-Means clustering was selected to group customers according to similarities in their charging behaviour.

The number of clusters was evaluated using:

- Elbow method
- Silhouette score

Silhouette scores were:

| K | Silhouette Score |
|---:|---:|
| 2 | 0.588 |
| 3 | **0.653** |
| 4 | 0.484 |
| 5 | 0.512 |
| 6 | 0.468 |
| 7 | 0.487 |
| 8 | 0.466 |

K = 3 was selected because it produced the highest silhouette score and provided clear separation between customer behaviour groups.

---

## 6. Customer Segments

The final model produced three customer segments.

### Segment 1 — Frequent High-Value Users

Approximately 75% of customers belong to this segment.

Typical customer profile:

- ~48.70 sessions
- ~2,286 kWh total energy
- ~46.89 kWh per session
- ~35.10 minutes average session duration
- ~£1,354 average total spend
- ~£27.77 average spend per session

These customers are the most frequent and commercially significant users of the charging network.

### Segment 2 — Occasional Large-Charge Users

Approximately 15.3% of customers belong to this segment.

Typical customer profile:

- ~1 session
- ~60.47 kWh total energy
- ~59.99 kWh per session
- ~22.44 minutes average session duration
- ~£30.93 average total spend
- ~£30.66 average spend per session

These customers use the network infrequently but tend to consume relatively large amounts of energy during their charging sessions.

### Segment 3 — Occasional Standard Users

Approximately 9.7% of customers belong to this segment.

Typical customer profile:

- ~1 session
- ~33.85 kWh total energy
- ~33.15 kWh per session
- ~49.72 minutes average session duration
- ~£17.41 average total spend
- ~£17.01 average spend per session

These customers show occasional and comparatively smaller charging activity.

---

## 7. Validation Against Existing User Types

The resulting segments were compared with the existing `user_type` field.

The distribution of delivery, fleet, public and taxi users was relatively balanced across all three clusters.

This indicates that the K-Means segments are not simply reproducing the predefined `user_type` categories.

Instead, the clustering captures behavioural characteristics such as frequency, energy consumption and spending.

---

## 8. Business Insights

### Frequent High-Value Users

This segment represents the core customer base and contributes the majority of sessions, energy consumption and spending.

Potential business actions:

- Loyalty and retention programmes
- Subscription or membership plans
- Personalized charging offers
- Priority service or rewards

### Occasional Large-Charge Users

These customers have low frequency but relatively high energy consumption per session.

Potential business actions:

- Offers designed to encourage repeat visits
- Larger-session pricing incentives
- Targeted re-engagement campaigns

### Occasional Standard Users

These customers have limited charging activity and lower average energy consumption.

Potential business actions:

- First-to-repeat-session campaigns
- Introductory offers
- Customer education and engagement campaigns

---

## 9. Power BI Dashboard

The segmentation results were visualized using Power BI.

The dashboard includes:

- Total customers
- Total sessions
- Total energy consumption
- Total customer spending
- Customer distribution by segment
- Total sessions by customer segment
- Total spend by customer segment
- Total energy consumption by customer segment
- Average energy per session
- Customer segment profile comparison

The dashboard provides both an overall network view and a comparison of customer behaviour across segments.

---

## 10. Limitations

The clustering results describe behavioural patterns in the available historical dataset and should not be interpreted as causal relationships.

K-Means also requires the number of clusters to be selected in advance. The choice of K = 3 was supported by the silhouette analysis, but alternative clustering methods could produce different segment structures.

The analysis is based on historical charging behaviour and does not include additional customer attributes such as demographics, marketing interactions or customer satisfaction.

---

## 11. Conclusion

Customer-level K-Means clustering identified three distinct behavioural groups:

1. Frequent High-Value Users
2. Occasional Large-Charge Users
3. Occasional Standard Users

The results show that customer value is strongly differentiated by charging frequency and consumption behaviour.

The segmentation provides a practical foundation for targeted customer engagement, retention strategies and future customer analytics.