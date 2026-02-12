# E-Commerce-Funnel-Analysis

## 🚀 Project Summary
This project analyzes an eCommerce user funnel **from Product View → Add to Cart → Begin Checkout → Purchase** using **Databricks, PySpark, and Delta Lake**.  
It ingests raw clickstream and transaction events, builds sessionized journeys, computes conversion metrics, and highlights where users drop off with actionable recommendations.

**Key outcomes**
- Stage-to-stage conversion rates and overall conversion
- Largest drop-off points with hypotheses and actions
- Reproducible notebooks and SQL/PySpark transformations
- Delta Lake optimizations for scale and speed

---

## 📈 Funnel Metrics (Sample Numbers)
> Replace these with your actual outputs if available.

- **Total Users Entering Funnel:** 500,000

- **Stage 1 — Product View**
  - Users: 500,000 (100.0% from previous)

- **Stage 2 — Add to Cart**
  - Users: 225,000 (45.0% from previous)
  - Drop-off from previous: 55.0%

- **Stage 3 — Begin Checkout**
  - Users: 112,000 (49.7% from previous)
  - Drop-off from previous: 50.3%

- **Stage 4 — Purchase**
  - Users: 51,000 (45.5% from previous)
  - Drop-off from previous: 54.5%

**Overall Funnel Conversion (Product View → Purchase): 10.2%**

### ASCII Funnel (Proportional)
Product View   |████████████████████████████████████████| 500,000

Add to Cart    |███████████████████                     | 225,000

Begin Checkout |███████████                              | 112,000

Purchase       |█████                                    | 51,000

---

## 🧩 Business Questions Answered
- What percentage of users convert at each stage?
- Where is the largest drop-off and what could be driving it?
- How do conversion rates vary by device, channel, and product category?
- What is the end-to-end time-to-conversion and sessions-to-conversion?
- Which acquisition sources drive the most qualified traffic?

---

## 🏗️ Architecture & Data Flow

**Sources**
- `events_raw`: clickstream events (`view_item`, `add_to_cart`, `begin_checkout`, `purchase`)
- `orders_raw`: transactional orders (header + line items)
- Optional enrichment: `products_dim`, `users_dim`, `sessions_dim`

**Processing Steps**
1. **Ingest** raw files (JSON/CSV/Parquet) into **Bronze (Delta)**  
2. **Clean & Standardize** into **Silver** (schema, timestamps, enums)  
3. **Sessionize** events by `user_id` with **30‑minute inactivity timeout**  
4. **Construct funnel** per user/session (ordered by timestamp)  
5. **Aggregate** stage counts, conversion rates, and segments into **Gold**  
6. **Visualize/Export** KPIs and charts

**Logical Diagram**

Raw Data --> Bronze (Delta) --> Silver (clean + sessionized) --> Gold (funnel KPIs)

ingest                standardize + joins             aggregates + BI

---

## 🧱 Data Model (Curated Tables)

- **`events_bronze_delta`**: Raw events near source format.  
- **`events_silver_delta`**: Cleaned events with columns like:  
  `event_time (ts), user_id, product_id, session_id, event_type, device, channel, country`
- **`funnel_paths_gold`**: One row per `user_id`/`session_id` with flags:  
  `has_view, has_add, has_checkout, has_purchase, stage_reached (0–4)`  
- **`funnel_kpis_gold`**: Aggregations (counts, conversion rates, segment breakdowns).

**Assumptions**
- Session timeout: **30 minutes** of inactivity  
- Count each stage at most once per session  
- Purchase ends the session’s funnel path  
- Late events within **24h** watermark still included (if streaming)

---

## 🛠️ Tech Stack
- **Databricks** (Notebooks, SQL, Workflows optional)
- **PySpark** (transformations/aggregations)
- **Delta Lake** (ACID storage + OPTIMIZE/Z-Order)
- **Visualization**: Databricks `display()`, optional matplotlib

---

