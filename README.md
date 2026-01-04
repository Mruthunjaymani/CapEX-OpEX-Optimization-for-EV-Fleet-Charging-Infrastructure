# CapEX-OpEX-Optimization-for-EV-Fleet-Charging-Infrastructure
MILP to minimize Capex and Opex over a period of 5 years for a fleet owner looking to expand their EV Trucks fleet size.
This repository contains a **Mixed Integer Linear Programming (MILP)** model for planning **EV fleet charging infrastructure** over a **5-year horizon (2025–2029)**.  
The model optimizes **CapEx, OpEx, and Export Revenue separately** to support infrastructure and energy planning decisions for an electric truck depot.

---

## Problem Summary

A fleet operator owns **10 EV trucks in 2025**, increasing by **5 trucks per year**, reaching **30 trucks by 2029**.  
Each truck is assigned to one of **four 6-hour charging slots** per day.

Each charging slot has:
- EV charging demand
- Baseload demand
- Grid electricity price
- Solar generation potential

Energy demand can be met using:
- Solar energy (direct use)
- Battery-stored solar energy
- Grid electricity

Excess solar or battery energy may be **exported to the grid** for revenue.

---

## Planning Horizon

- **Years:** 2025–2029  
- **Daily Slots (6 hours each):**
  - Slot 1: 12 AM – 6 AM  
  - Slot 2: 6 AM – 12 PM  
  - Slot 3: 12 PM – 6 PM  
  - Slot 4: 6 PM – 12 AM  

---

## Objective Function

The model minimizes **total net cost** (CapEX + OpEX - Export Revenue):


### CapEx
Year-wise investment cost for:
- Solar PV capacity (kWp)
- Battery storage capacity (kWh)
- Charging infrastructure (50 kW and 400 kW chargers)

### OpEx
- Cost of electricity purchased from the grid
- Calculated per slot, annualized over 365 days

### Export Revenue
- Revenue earned from exporting solar or battery energy to the grid
- Annualized over 365 days

---

## Export Revenue Scenarios

The model is solved under three export pricing assumptions:

| Export Ratio | Description |
|-------------|------------|
| 0.0 | No export revenue |
| 0.5 | Export revenue = 50% of grid price |
| 1.0 | Export revenue = 100% of grid price |

Export ratios are applied **slot-wise** to grid prices.

---

## Decision Variables

### CapEx Decisions
- Solar capacity added per year (kWp)
- Battery capacity added per year (kWh)
- Number of 50 kW chargers added per year
- Number of 400 kW chargers added per year

### Operational Decisions
For each year and slot:
- Solar energy used
- Solar energy stored to battery
- Solar energy exported
- Battery energy used
- Battery energy exported
- Grid energy purchased
- Battery energy carried between slots

---

## Key Constraints

- Energy demand (charging + baseload) must be met in every slot
- Solar expansion limited to **200 kWp**
- Battery capacity limited to **2000 kWh**
- Battery discharge limited by **C-rate = 0.5**
- Battery energy balance enforced across slots
- Grid purchase limited to **1750 kWh per slot**
- Charger capacity must satisfy charging demand within **6 hours**
- Annual CapEx budget limited to **$60,000**
- Solar generation depends on installed capacity and slot-specific solar factors

---

## Data Files

- `Vehicle_Data.xlsx` – Fleet growth and routing
- `Vehicle_Charging_Data.xlsx` – Battery capacity and energy use
- `Energy_Profile_Data.xlsx` – Solar generation, baseload, grid prices
- `Infra_Procurement_Costs.xlsx` – Year-wise infrastructure costs
- `Project_dataset.xlsx` – Processed optimization inputs

---

## Notebooks

- `FULL_OPTIMIZATION_PROJECT.ipynb` – Export ratio = 1
- `0.5 EXPORT REVENUE.ipynb` – Export ratio = 0.5
- `NO EXPORT REVENUE.ipynb` – Export ratio = 0

---

## Outputs


### Export Ratio = 1
- `optimization_results.csv`  
  - Year-wise CapEx, OpEx, Export Revenue, Total Cost
- `operational_details.csv`  
  - Slot-wise energy flows (solar, battery, grid, exports)

### Export Ratio = 0.5
- `optimization_results 0.5.csv`  
  - Year-wise CapEx, OpEx, Export Revenue, Total Cost
- `operational_details(0.5 export revenue).csv`  
  - Slot-wise energy flows (solar, battery, grid, exports)

### Export Ratio = 0
- `optimization_results_no_export.csv`  
  - Year-wise CapEx, OpEx, Export Revenue, Total Cost
- `operational_details(no export).csv`  
  - Slot-wise energy flows (solar, battery, grid, exports)


## Tools Used

- Python
- Gurobi Optimizer
- Mixed Integer Linear Programming (MILP)
- Excel
- Jupyter Notebook

---

## Conclusion

This project integrates **CapEx planning, OpEx optimization, and export revenue modeling** into a unified optimization framework for EV fleet charging infrastructure. The results highlight how export incentives influence operational decisions while infrastructure investments remain relatively stable over time.

