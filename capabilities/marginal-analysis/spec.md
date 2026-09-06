---
type: spec
capability: marginal-analysis
engagement: perfect-competition
date: 2026-09-05
status: draft            # draft | built | audited
built_with: "pending"
---

# Marginal Analysis — model specification

## Purpose
The purpose of this analysis is to determine how many beds I should allocate to tomatoes, carrots, and mesclun in order to maximize profit. It should also test whether the optimized allocation supports or contradicts the original hypothesis of 18/16/30 once marginal revenue, marginal cost, and the farm's bed constraints are accounted for.

## Inputs — the named contract
| Name | Value | Unit | Source |
| --- | ---: | --- | ---|
| `SEASON_WEEKS` | 36 | Weeks per season | Case Scenario, farm table |
| `FIXED_COSTS` | 20000 |USD per season | Case Scenario, farm table |
| `TOTAL_BEDS_AVAILABLE` | 64 | Beds | Case Scenario, farm table |
| `FARMER_HOURS_AVAILABLE` | 720 | Field hours per season | Case Scenario, farm table |
| `FARMER_HOURLY_RATE` | 34.72 | USD per field hour | Case Scenario, farm table |
| `MAX_TEMP_WORKERS` | 4 | Workers per season | Case Scenario, farm table |
| `TEMP_HOURS_PER_WORKER` | 1440 | Field hours per worker season | Case Scenario, farm table |
| `TEMP_HOURLY_RATE` | 17.36 | USD per field hour | Case Scenario, farm table |
| `TOM_BED_CAP` | 20 | Beds | Case Scenario, crop table |
| `TOM_PRICE` | 8800 | USD per bed | Case scenario, crop table |
| `TOM_HRS`   | 2.5  | Hours per week per bed | Case scenario, crop table |
| `TOM_FERT_COST` | 880 | USD per bed | Case scenario, crop table |
| `TOM_DIM_PCT` | 10% | Percent per additional bed | Case scenario, crop table |
| `CAR_BED_CAP` | 20 | Beds | Case scenario, crop table |
| `CAR_PRICE` | 2094 | USD per bed | Case scenario, crop table |
| `CAR_HRS` | 0.833 | Hours per week per bed | Case scenario, crop table |
| `CAR_FERT_COST` | 440 | USD per bed | Case scenario, crop table |
| `CAR_DIM_PCT` | 2.5% | Percent per additional bed | Case scenario, crop table |
| `MES_BED_CAP` | 30 | Beds | Case scenario, crop table |
| `MES_PRICE` | 2700 | USD per bed | Case scenario, crop table |
| `MES_HRS` | 1.25 | Hours per week per bed | Case scenario, crop table |
| `MES_FERT_COST` | 880 | USD per bed | Case scenario, crop table |
| `MES_DIM_PCT` | 1.25% | Percent per additional bed | Case scenario, crop table |

## Structure
Each sheet or region, and what it is for.

## Calculation logic
In named-range notation, never cell addresses:

  LABOR_HRS(q) = q x HRS_PER_BED x WEEKS x (1 + DIM_PCT)^q

"Column D times column E" is not a specification — it describes a spreadsheet
that does not exist yet.

## Conventions
The rules that are not visible in the formulas: costing order, allocation basis,
rounding, what happens at the boundaries. State all of them. A convention you
leave out is a convention the builder invents.

## Validation rules
The conditions the finished artifact must satisfy — check figures as acceptance
criteria, hand calculations, and structural rules ("every calculated cell
contains a formula", "no error cells").

## Outputs
Each result the model reports, by name.

## Audit findings
Added AFTER the build. For each check: what you checked, what you found, what
you did about it.
