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
The purpose of this analysis is to determine how many beds I should allocate to tomatoes, carrots, and mesclun in order to maximize profit. It should also test whether the optimized allocation supports or contradicts the original hypothesis of 18/16/30 once marginal revenue, marginal cost, and the farm's bed and labor constraints are accounted for.

## Inputs — the named contract
| Name | Value | Unit | Source |
| --- | ---: | --- | ---|
| `SEASON_WEEKS` | 36 | Weeks per season | Case Scenario, farm table |
| `FIXED_COSTS` | 20000 |USD per season | Case Scenario, farm table |
| `TOTAL_BEDS_AVAILABLE` | 64 | Beds | Case Scenario, farm table |
| `FARMER_HOURS_AVAILABLE` | 720 | Field hours per season | Case Scenario, farm table |
| `FARMER_HOURLY_RATE` | 34.7222222222 | USD per field hour | Case scenario and Farm Profit Lab |
| `MAX_TEMP_WORKERS` | 4 | Workers per season | Case Scenario, farm table |
| `TEMP_HOURS_PER_WORKER` | 1440 | Field hours per worker season | Case Scenario, farm table |
| `TEMP_HOURLY_RATE` | 17.3611111111 | USD per field hour | Case scenario and Farm Profit Lab |
| `TOM_BED_CAP` | 20 | Beds | Case Scenario, crop table |
| `TOM_PRICE` | 8800 | USD per bed | Case scenario, crop table |
| `TOM_HRS`   | 2.5  | Hours per week per bed | Case scenario, crop table |
| `TOM_FERT_COST` | 880 | USD per bed | Case scenario, crop table |
| `TOM_DIM_PCT` | 10% | Percent per additional bed | Case scenario, crop table |
| `CAR_BED_CAP` | 20 | Beds | Case scenario, crop table |
| `CAR_PRICE` | 2094 | USD per bed | Case scenario, crop table |
| `CAR_HRS` | 0.8333333333 | Hours per week per bed | Case scenario and Farm Profit Lab |
| `CAR_FERT_COST` | 440 | USD per bed | Case scenario, crop table |
| `CAR_DIM_PCT` | 2.5% | Percent per additional bed | Case scenario, crop table |
| `MES_BED_CAP` | 30 | Beds | Case scenario, crop table |
| `MES_PRICE` | 2700 | USD per bed | Case scenario, crop table |
| `MES_HRS` | 1.25 | Hours per week per bed | Case scenario, crop table |
| `MES_FERT_COST` | 880 | USD per bed | Case scenario, crop table |
| `MES_DIM_PCT` | 1.25% | Percent per additional bed | Case scenario, crop table |

## Structure

The workbook must contain five worksheets named exactly `Inputs`, `Cost Structure`, `Marginal-Cost Schedules`, `Optimization`, and `Checks`.

- **Inputs:** The Inputs worksheet should contain all the given constants and assumptions, including bed revenue, maximum bed limits, and labor-increase rates for tomatoes, carrots, and mesclun, as well as the season length, fixed costs, total beds, and labor availability and rates. These values should be provided rather than calculated by formulas so they can be changed without editing the model's logic. Each input must clearly show its name, value, unit, and source.

- **Cost Structure:** The Cost Structure worksheet should calculate and display total labor and farm costs, including total labor hours, farmer hours, temporary-labor hours, labor cost, the blended labor rate, fertilizer cost, variable costs, fixed costs, and total costs.

- **Marginal-Cost Schedules:** The Marginal-Cost Schedules worksheet should show each crop's bed quantity, total labor hours required, standalone variable cost, marginal cost of the next bed, and crop price for comparison with marginal cost. Each schedule should cover every whole-bed quantity from zero through the crop's cap: 20 beds for tomatoes, 20 for carrots, and 30 for mesclun. Fixed costs are excluded because they do not change as bed quantity changes.

- **Optimization:** The Optimization worksheet should contain the three decision cells for tomato, carrot, and mesclun bed counts, the total-profit objective, the farm and crop constraints, and the Solver method and optimized result.

- **Checks:** The Checks worksheet should display the hand-calculation check for one tomato bed, the expected optimized mix and season profit, whether all constraints are satisfied, whether the workbook contains formula errors, and a clear pass/fail result for each check.

The completed workbook must be saved as `capabilities/marginal-analysis/model.xlsx`.

## Calculation logic

The decision variables are `TOM_BEDS`, `CAR_BEDS`, and `MES_BEDS`. Each must be a nonnegative whole number. 

Function notation such as `TOM_LABOR_HRS(q)` is conceptual named-range notation. The workbook may implement this logic using ordinary Excel formulas and named ranges; custom `LAMBDA` functions are not required.

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

Each schedule must display standalone variable cost rather than total cost. Fixed costs are excluded because they do not change as bed quantity changes.

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

For each crop, report the largest bed quantity `q` for which `MC(q) ≤ PRICE`, before the marginal cost of the next bed exceeds price.
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

- **Labor order and allocation:** The farmer's hours are consumed first (up to 720), with temporary labor covering only the remainder. Temporary labor is capped at four workers × 1,440 hours each. Since neither worker type is assigned to a specific crop, crop labor costs use the farm-wide blended rate.

- **Rounding:** Formulas retain full precision; rounding is for display only. Currency and marginal costs display to the nearest dollar; hourly rates, labor hours, and temporary-worker equivalents to two decimals; bed counts as whole numbers.

- **Source precision:** Calculations use the unrounded course-model constants: `CAR_HRS = 2.5 / 3`, `FARMER_HOURLY_RATE = 50000 / (40 × 36)`, and `TEMP_HOURLY_RATE = 25000 / (40 × 36)`. Their workbook input values are provided at sufficient decimal precision and may display as `0.833`, `$34.72`, and `$17.36`.

- **Boundaries:** Bed counts are nonnegative integers, inclusive of their caps. The blended rate returns zero at zero labor hours instead of a division error. Marginal cost at `q = 0` is blank because no prior quantity exists. Each marginal-cost schedule stops at its crop's own bed cap.

- **Calculated names:** Create Excel named ranges for all scalar decision, cost, labor, revenue, profit, and constraint results referenced by name in this specification. Individual marginal-cost schedule rows do not require separate named ranges.

- **Fixed costs:** The $20,000 seasonal fixed cost is included once, in total cost and profit. It is excluded from crop allocations and standalone marginal-cost calculations because it does not change with bed count.

- **Temporary-worker equivalents:** `TEMP_WORKERS_NEEDED` equals temporary hours divided by 1,440 and may be fractional. Cost and capacity checks use actual hours, not a rounded-up worker count.

## Validation rules

1. **One-bed labor check:** At `q = 1`, `TOM_LABOR_HRS` equals 99 hours (`1 × 2.5 × 36 × 1.10`). The check passes if the calculated value is within 0.01 hour of 99.

2. **Optimized result:** Solver's optimal mix must equal 10 tomato, 20 carrot, and 30 mesclun beds (60 total). The optimized-profit check passes when the full-precision season profit rounds to $42,762 to the nearest dollar.

3. **Standalone P ≈ MC crossings:** Price must approximately equal marginal cost at 10 tomato, 10 carrot, and 6 mesclun beds on the standalone schedules.

4. **Two Solver starting points:** Run GRG Nonlinear with integer decisions from `0/0/0` and `20/0/0`. Record both final mixes and profits. Agreement is evidence against path dependence; disagreement requires reporting both, identifying the higher-profit result, and logging the disagreement as an audit finding.
   The `Checks` worksheet must contain a two-row audit table recording the starting point, final tomato/carrot/mesclun mix, total beds, total profit, and Solver status for each run. These results may be manually recorded after each Solver run.

5. **Farm Profit Lab cross-check:** Compare `TOM_MC(6)` (~$4,906) with the Farm Profit Lab's sixth-bed tomato marginal cost. Record both values and their difference. The check passes if they agree within $1 after rounding.

6. **Formulas and errors:** Every calculated cell must use a formula rather than a pasted value and must reference named inputs or other calculated cells. The workbook must contain no `#REF!`, `#DIV/0!`, `#NAME?`, `#VALUE!`, or `#N/A` errors. The formula-only requirement applies to calculated outputs. Input values, Solver decision cells, and manually recorded Solver-run audit evidence are permitted values and are exempt. Review all designated calculated cells in the `Cost Structure`, `Marginal-Cost Schedules`, `Optimization`, and `Checks` worksheets. Each must contain a formula, and the entire workbook must contain no Excel error values. The `Inputs` worksheet is excluded from the formula requirement but remains included in the error review.
 
7. **Constraints:** The Checks worksheet must show green PASS or red FAIL results for the total-bed limit, each crop's bed cap, temporary-worker capacity, nonnegative beds, and integer beds. Every constraint must pass for the optimized solution.

8. **Tomato marginal-cost dip:** Flag the decrease in tomato marginal cost from approximately $7,661 at bed 5 to approximately $4,906 at bed 6. Record where it occurs without explaining the economic cause at this stage. The `Checks` worksheet must use a formula to compare `TOM_MC(6)` with `TOM_MC(5)`, display `FLAGGED` when `TOM_MC(6) < TOM_MC(5)`, and record both marginal-cost values.
   
## Outputs

- **Decision results:** The model must report optimized tomato beds (`TOM_BEDS`), carrot beds (`CAR_BEDS`), mesclun beds (`MES_BEDS`), total beds planted (`TOTAL_BEDS_USED`), and total season profit (`TOTAL_PROFIT`).

- **Farm results:** The model must report total revenue (`TOTAL_REVENUE`), total labor hours (`TOTAL_LABOR_HRS`), farmer hours used (`FARMER_HOURS_USED`), temporary hours (`TEMP_HOURS_USED`), temporary-worker equivalents (`TEMP_WORKERS_NEEDED`), blended labor rate (`BLENDED_LABOR_RATE`), total labor cost (`TOTAL_LABOR_COST`), fertilizer cost (`TOTAL_FERT_COST`), variable cost (`TOTAL_VARIABLE_COST`), fixed costs (`FIXED_COSTS`), and total cost (`TOTAL_COST`).

- **Marginal Analysis:** For each crop, the model must report its standalone marginal-cost schedule, constant market price, and approximate quantity where price equals marginal cost. It must also identify the tomato marginal-cost dip.

- **Audit Evidence:** The model must report whether each validation and constraint check passed or failed, along with the final mix and profit from both Solver starting points.

## Audit Findings
