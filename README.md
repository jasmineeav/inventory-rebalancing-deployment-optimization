# Inventory Rebalancing & Deployment Optimization for a Multi-DC Food Supply Network

This project simulates inventory deployment planning in a 6-DC, 3-plant food supply network, using Walmart 10oz products as a customer-specific planning scope.

The analysis combines weekly forecast, inventory snapshots, 4-week production plans, and in-transit STO shipments to answer four core business questions:

1. Which SKU-DC combinations are at risk of shortage?
2. In which week does the shortage first appear?
3. Can the shortage be covered by inbound STO or local plant production?
4. If not, can inventory be rebalanced from another DC, or is the case truly supply constrained?

The project was built using **Python, DuckDB SQL, and Power BI-ready output tables**.


## Business Problem

In a multi-node supply chain, inventory shortages are not always caused by total supply constraints.  
They may result from uneven inventory distribution, weak local production support, or delayed inbound shipments.

This project models a realistic deployment planning scenario for a food supply network with:

- 6 distribution centers: PA, TX, FL, NV, WI, WA
- 3 manufacturing plants: LHV (PA), Henderson (NV), Sulphur (TX)
- weekly customer forecast as the demand signal
- a 4-week production horizon
- in-transit STO shipments as inbound support

The goal is to classify shortage cases into:
- Covered by STO
- Covered by Production
- Transfer Required
- No Feasible Source

## Scope

The analysis focuses on **Walmart 10oz products** to simulate customer-specific deployment logic.

Key assumptions:
- Weekly forecast is used as a proxy for demand.
- The next 4 weeks of production plan are treated as frozen and reliable.
- Same-state plant-to-DC replenishment is treated as fast local support.
- Cross-region support may require network rebalancing from surplus DCs.

## Data Model

The project uses the following datasets:

- `product_master.csv` – SKU master data
- `weekly_forecast.csv` – weekly customer demand by SKU and DC
- `inventory_snapshot.csv` – on-hand unrestricted, blocked, and quality inventory
- `production_plan.csv` – 12-week plant production schedule with cadence logic
- `sto_shipment_tracking.csv` – in-transit stock transfer orders
- `sku_plant_mapping.csv` – primary plant assignment by SKU

## Methodology

### Phase 1 – Data Generation
A simulated dataset was generated to reflect a realistic multi-DC food network.  
Walmart 10oz SKUs were intentionally grouped into:

- healthy
- incoming supply cover
- rebalancing needed
- true supply risk

This ensured the final model produced realistic shortage, donor, and no-solution cases.

### Phase 2 – Static Risk Screening
DuckDB SQL was used to:
- define the Walmart 10oz analysis scope
- calculate 4-week average weekly forecast
- calculate WOS (weeks of supply)
- identify low-WOS shortage nodes
- identify donor DC candidates with surplus inventory

### Phase 3 – Time-Phased Inventory Projection
A rolling 4-week projection model was built in Python:

Beginning Inventory  
+ STO arriving this week  
+ Production support arriving this week  
- Weekly forecast  
= Projected Ending Inventory

This allowed the model to identify:
- first gap week
- total gap duration
- cumulative shortage quantity

### Phase 4 – Replenishment Feasibility
For each shortage case, the model evaluated whether the gap could be resolved by:
- inbound STO
- local production support
- inter-DC transfer from a donor location

Each case was then classified into a final action type.


## Outputs

The project generates the following planning outputs:

- `weekly_inventory_projection.csv`
- `gap_summary.csv`
- `replenishment_feasibility.csv`
- `final_recommendation.csv`
- `powerbi_master_table.csv`

## Example Insights

- Some shortage cases were fully covered by in-transit STO and did not require deployment action.
- Some cases had insufficient local production within the frozen horizon, but could be mitigated through inter-DC transfer from surplus locations such as PA.
- A subset of cases remained unresolved due to the absence of timely STO, weak production cadence, and no feasible donor inventory.
- Rolling weekly projection made it possible to identify not just whether a shortage would occur, but exactly when it would begin and how long it would persist.

## Tools

- Python
- Pandas
- DuckDB SQL
- Jupyter Notebook
- Power BI (planned visualization layer)

## Future Improvements

- Add transport lane timing and transfer arrival logic directly into the recommendation engine
- Expand from 4-week horizon to 8–12 week scenario planning
- Add cost-based optimization for donor selection
- Build a Power BI dashboard for executive risk overview and rebalancing recommendations
