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
- **Inputs:** The Inputs worksheet should contain all the given constants and assumptions, including bed revenue, maximum bed limits, and labor-increase rates for tomatoes, carrots, and mesclun, as well as the season length, fixed costs, total beds, and labor availability and rates. These values should be provided rather than calculated by formulas so they can be changed without editing the model's logic. Each input must clearly show its name, value, unit, and source.

- **Cost Structure:** The Cost Structure worksheet should calculate and display total labor and farm costs, including total labor hours, farmer hours, temporary-labor hours, labor cost, the blended labor rate, fertilizer cost, variable costs, fixed costs, and total costs.

- **Marginal-Cost Schedules:** The Marginal-Cost Schedules worksheet should show each crop's bed quantity, total labor hours required, total cost, marginal cost of the next bed, and crop price for comparison with marginal cost. Each schedule should cover every whole-bed quantity from zero through the crop's cap: 20 beds for tomatoes, 20 for carrots, and 30 for mesclun.

- **Optimization:** The Optimization worksheet should contain the three decision cells for tomato, carrot, and mesclun bed counts, the total-profit objective, the farm and crop constraints, and the Solver method and optimized result.

- **Checks:** The Checks worksheet should display the hand-calculation check for one tomato bed, the expected optimized mix and season profit, whether all constraints are satisfied, whether the workbook contains formula errors, and a clear pass/fail result for each check.

## Calculation logic

The decision variables are `TOM_BEDS`, `CAR_BEDS`, and `MES_BEDS`. Each must be a nonnegative whole number.

### Crop labor requirements

For `q` beds of each crop:

```text
TOM_LABOR_HRS(q) =
q × TOM_HRS × SEASON_WEEKS × (1 + TOM_DIM_PCT)^q

CAR_LABOR_HRS(q) =
q × CAR_HRS × SEASON_WEEKS × (1 + CAR_DIM_PCT)^q

MES_LABOR_HRS(q) =
q × MES_HRS × SEASON_WEEKS × (1 + MES_DIM_PCT)^q
```

These formulas calculate total seasonal labor hours and capture how each additional bed increases the labor requirements across all beds already planted for that crop.

### Farm labor allocation and cost

```text
TOTAL_LABOR_HRS =
TOM_LABOR_HRS(TOM_BEDS)
+ CAR_LABOR_HRS(CAR_BEDS)
+ MES_LABOR_HRS(MES_BEDS)

FARMER_HOURS_USED =
MIN(TOTAL_LABOR_HRS, FARMER_HOURS_AVAILABLE)

TEMP_HOURS_USED =
MAX(TOTAL_LABOR_HRS - FARMER_HOURS_AVAILABLE, 0)

MAX_TEMP_HOURS =
MAX_TEMP_WORKERS × TEMP_HOURS_PER_WORKER

TEMP_WORKERS_NEEDED =
TEMP_HOURS_USED / TEMP_HOURS_PER_WORKER
```

The farmer's hours must be used first and are capped at 720. Temporary labor supplies the remaining hours, subject to the four-worker capacity of 5,760 hours.

```text
FARMER_LABOR_COST =
FARMER_HOURS_USED × FARMER_HOURLY_RATE

TEMP_LABOR_COST =
TEMP_HOURS_USED × TEMP_HOURLY_RATE

TOTAL_LABOR_COST =
FARMER_LABOR_COST + TEMP_LABOR_COST

BLENDED_LABOR_RATE =
IF(TOTAL_LABOR_HRS = 0, 0,
TOTAL_LABOR_COST / TOTAL_LABOR_HRS)
```

The farm-wide blended labor rate must be used to allocate labor cost to each crop because the farmer and temporary workers are not dedicated to individual crops.

```text
TOM_LABOR_COST =
TOM_LABOR_HRS(TOM_BEDS) × BLENDED_LABOR_RATE

CAR_LABOR_COST =
CAR_LABOR_HRS(CAR_BEDS) × BLENDED_LABOR_RATE

MES_LABOR_COST =
MES_LABOR_HRS(MES_BEDS) × BLENDED_LABOR_RATE
```

### Revenue, fertilizer, and profit

```text
TOM_REVENUE =
TOM_BEDS × TOM_PRICE

CAR_REVENUE =
CAR_BEDS × CAR_PRICE

MES_REVENUE =
MES_BEDS × MES_PRICE

TOTAL_REVENUE =
TOM_REVENUE + CAR_REVENUE + MES_REVENUE

TOTAL_FERT_COST =
(TOM_BEDS × TOM_FERT_COST)
+ (CAR_BEDS × CAR_FERT_COST)
+ (MES_BEDS × MES_FERT_COST)

TOTAL_VARIABLE_COST =
TOTAL_LABOR_COST + TOTAL_FERT_COST

TOTAL_COST =
TOTAL_VARIABLE_COST + FIXED_COSTS

TOTAL_PROFIT =
TOTAL_REVENUE - TOTAL_COST
```

### Standalone marginal-cost schedules

Each standalone schedule evaluates one crop at a time while applying the same farmer-first labor rule.

#### Tomatoes

```text
TOM_STANDALONE_FARMER_HOURS(q) =
MIN(TOM_LABOR_HRS(q), FARMER_HOURS_AVAILABLE)

TOM_STANDALONE_TEMP_HOURS(q) =
MAX(TOM_LABOR_HRS(q) - FARMER_HOURS_AVAILABLE, 0)

TOM_STANDALONE_VARIABLE_COST(q) =
(TOM_STANDALONE_FARMER_HOURS(q) × FARMER_HOURLY_RATE)
+ (TOM_STANDALONE_TEMP_HOURS(q) × TEMP_HOURLY_RATE)
+ (q × TOM_FERT_COST)

TOM_MC(q) =
TOM_STANDALONE_VARIABLE_COST(q)
- TOM_STANDALONE_VARIABLE_COST(q - 1)
```

#### Carrots

```text
CAR_STANDALONE_FARMER_HOURS(q) =
MIN(CAR_LABOR_HRS(q), FARMER_HOURS_AVAILABLE)

CAR_STANDALONE_TEMP_HOURS(q) =
MAX(CAR_LABOR_HRS(q) - FARMER_HOURS_AVAILABLE, 0)

CAR_STANDALONE_VARIABLE_COST(q) =
(CAR_STANDALONE_FARMER_HOURS(q) × FARMER_HOURLY_RATE)
+ (CAR_STANDALONE_TEMP_HOURS(q) × TEMP_HOURLY_RATE)
+ (q × CAR_FERT_COST)

CAR_MC(q) =
CAR_STANDALONE_VARIABLE_COST(q)
- CAR_STANDALONE_VARIABLE_COST(q - 1)
```

#### Mesclun

```text
MES_STANDALONE_FARMER_HOURS(q) =
MIN(MES_LABOR_HRS(q), FARMER_HOURS_AVAILABLE)

MES_STANDALONE_TEMP_HOURS(q) =
MAX(MES_LABOR_HRS(q) - FARMER_HOURS_AVAILABLE, 0)

MES_STANDALONE_VARIABLE_COST(q) =
(MES_STANDALONE_FARMER_HOURS(q) × FARMER_HOURLY_RATE)
+ (MES_STANDALONE_TEMP_HOURS(q) × TEMP_HOURLY_RATE)
+ (q × MES_FERT_COST)

MES_MC(q) =
MES_STANDALONE_VARIABLE_COST(q)
- MES_STANDALONE_VARIABLE_COST(q - 1)
```

Marginal cost at `q = 0` must be blank for all three crops because no previous quantity exists.

### Optimization

```text
TOTAL_BEDS_USED =
TOM_BEDS + CAR_BEDS + MES_BEDS
```

Solver must maximize `TOTAL_PROFIT` by changing `TOM_BEDS`, `CAR_BEDS`, and `MES_BEDS` using GRG Nonlinear with integer decisions.

The decision variables must satisfy:

```text
TOTAL_BEDS_USED <= TOTAL_BEDS_AVAILABLE
TOM_BEDS <= TOM_BED_CAP
CAR_BEDS <= CAR_BED_CAP
MES_BEDS <= MES_BED_CAP
TEMP_WORKERS_NEEDED <= MAX_TEMP_WORKERS
TOM_BEDS, CAR_BEDS, MES_BEDS >= 0
TOM_BEDS, CAR_BEDS, MES_BEDS = integers
```

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
