# 🚀 Inventory Rebalancing & Deployment Optimization for a Multi-DC Food Supply Network

---

## 🧠 Project Overview

This project builds a **multi-DC inventory risk detection and deployment recommendation engine** for a food supply chain network.

It answers a critical business question:

👉 When inventory shortages occur,
**should we rely on production, inbound supply, or rebalance across distribution centers?**

Using a combination of **DuckDB SQL and Python-based rolling projections**, the model simulates:

* Shortage timing (when gaps occur)
* Gap severity and duration
* Feasible replenishment sources
* Final deployment recommendations

The output is a set of **Power BI-ready tables** that support operational decision-making.

---

## 🧩 Project Architecture

```
Raw Data (CSV)
   ↓
DuckDB SQL (Risk Screening & WOS Calculation)
   ↓
Python Rolling Projection (4-week simulation)
   ↓
Gap Detection
   ↓
Replenishment Feasibility Engine
   ↓
Final Recommendation Engine
   ↓
Power BI Dashboard (Visualization Layer)
```

---

## 💼 Business Problem

In a multi-node supply chain, inventory shortages are not always caused by total supply constraints.

They may result from:

* uneven inventory distribution across DCs
* weak local production support
* delayed inbound shipments

This project models a realistic deployment planning scenario to determine:

1. Which SKU–DC combinations are at risk of shortage
2. When the shortage will occur
3. Whether it can be resolved by production or inbound supply
4. Whether inventory needs to be rebalanced across DCs
5. Whether the shortage is fundamentally unsolvable

---

## 📦 Network Scope

* **6 Distribution Centers:** PA, TX, FL, NV, WI, WA

* **3 Manufacturing Plants:**

  * Lehigh Valley (PA)
  * Henderson (NV)
  * Sulphur (TX)

* Weekly customer demand as forecast

* 4-week rolling planning horizon

* STO (Stock Transfer Orders) as inbound support

---

## 🧾 Data Model

The project uses the following datasets:

* `product_master.csv` → SKU master data
* `weekly_forecast.csv` → demand by SKU and DC
* `inventory_snapshot.csv` → on-hand inventory
* `production_plan.csv` → plant production schedule
* `sto_shipment_tracking.csv` → in-transit inventory
* `sku_plant_mapping.csv` → SKU to plant assignment

---

## ⚙️ Methodology

### Phase 1 — Data Generation

A simulated dataset was created to reflect realistic supply chain scenarios.

SKUs were intentionally grouped into:

* healthy inventory
* incoming supply coverage
* rebalancing-needed cases
* true supply-risk cases

---

### Phase 2 — Static Risk Screening (DuckDB SQL)

DuckDB SQL was used to:

* define analysis scope (Walmart 10oz SKUs)
* calculate 4-week average demand
* compute Weeks of Supply (WOS)
* identify shortage-risk nodes
* identify surplus donor DCs

---

### Phase 3 — Rolling Inventory Projection (Python)

A 4-week rolling projection model was built:

For each SKU–DC:

```
Beginning Inventory
+ Production arrivals
+ STO arrivals
- Forecast demand
= Projected Ending Inventory
```

This allowed identification of:

* first gap week
* gap duration
* cumulative shortage

---

### Phase 4 — Replenishment Feasibility

For each shortage case, the model evaluates whether the gap can be covered by:

* inbound STO
* local production
* inter-DC transfer (from surplus locations)

---

### Phase 5 — Recommendation Engine

Each SKU–DC case is classified into a final action type:

* ✅ Covered by STO
* ✅ Covered by Production
* 🔁 Transfer Required
* ❌ No Feasible Source

---

## 📊 Key Outputs

The model generates decision-ready datasets:

### 1. Weekly Inventory Projection

→ SKU-DC level projected inventory over time

### 2. Gap Summary Table

→ First shortage week, duration, severity

### 3. Replenishment Feasibility Table

→ Evaluates available supply sources

### 4. Final Recommendation Table

→ Action classification + recommended approach

### 5. Power BI Master Table

→ Combined dataset for dashboard visualization

---

## 🔍 Example Insights

* Some shortages are fully covered by inbound STO and require no action

* Some cases lack sufficient production support but can be solved via DC rebalancing

* Certain SKUs remain unresolved due to:

  * no inbound supply
  * weak production cadence
  * no available donor inventory

* Rolling projections reveal not just *if* a shortage occurs, but *when* and *how long it lasts*

---

## 🛠️ Tools & Technologies

* Python (Pandas, NumPy)
* DuckDB (SQL-based analytics engine)
* Jupyter Notebook
* Power BI (visualization layer)

---

## 🚀 Future Improvements

* Incorporate transport lead time into transfer feasibility
* Extend planning horizon (4 → 8–12 weeks)
* Add cost-based optimization for donor selection
* Build full Power BI dashboard for executive decision-making

---

## 📌 Author

**Jasmine Zhang**
Supply Chain Analytics | Data Analyst | BI & Optimization

---
