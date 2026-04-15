# Inventory Rebalancing & Deployment Optimization for a Multi-DC Food Supply Network

This project simulates inventory deployment planning in a 6-DC, 3-plant food supply network, using Walmart 10oz products as a customer-specific planning scope.

The analysis combines weekly forecast, inventory snapshots, frozen production plans, and in-transit STO shipments to answer four core business questions:

1. Which SKU-DC combinations are at risk of shortage?
2. In which week does the shortage first appear?
3. Can the shortage be covered by inbound STO or local plant production?
4. If not, can inventory be rebalanced from another DC, or is the case truly supply constrained?

The project was built using **Python, DuckDB SQL, and Power BI-ready output tables**.
