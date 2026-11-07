# W6 Cash flow and investment decision: ReFeed

Lesson 6 (Capital Budgeting). Compares the three scope scenarios from [W5](w5-handover.md) against business as usual (BAU), where ReFeed continues coordinating pickups manually. Monetary values are in constant 2026 VND. This is a pre-tax planning appraisal built from workshop cost estimates and operating assumptions.

## 1. Assumptions

| Assumption | Value and basis |
|---|---|
| Evaluation horizon and discount rate | Five annual operating periods after Year 0. Use a 15% real planning hurdle from the ReFeed financial appraisal workbook. Finance approval is open. |
| Build cost (W5) | Must only 44.520.000 VND (162.4 p-h), Must + Should 60.760.000 VND (223.2 p-h), full in-scope ceiling 63.600.000 VND (234 p-h). Full scope excludes Won't rows. |
| Contingency | 4.452.000 VND in each scenario, equal to 10% of the Must-only estimate. |
| Adoption | 210 active digital partners in Year 1, rising by 110 each year up to a cap of 550. Modelled active partners by year are 210, 320, 430, 540 and 550. |
| UCO volume and margin | 20 litres per active site each week for 52 weeks. Assume 25% of that volume is incremental to BAU and earns 512 VND/litre in contribution margin, separate from fuel savings on baseline routes. |
| Fuel savings | Assume one eligible pickup per active site per week and 8 allocated baseline km per pickup. Use an 8% route-km reduction, 8 km/litre fuel efficiency and the appraisal's historical 29.717 VND/litre diesel proxy. |
| Annual running cost | 32.864.000 VND for compute, routing API, backups, monitoring and support. |
| Working capital | Fund seven days of incremental UCO cost at 25.082,4 VND/litre. Charge the change in the balance each year. The finite-horizon case assumes full recovery at the end of Year 5. |
| Other costs and benefits | Rollout and migration beyond the build estimate, supplier bonuses, onboarding and payment fees have zero placeholders pending estimates. Cashable staff savings, audit savings, taxes, financing and residual asset value are outside the quantified case. |

The ReFeed Cost Workshop's calculated `W5_Handover!B9:C11`, contingency in `B23` and `W2_Rollup!C27:D29` supply the cost inputs. The roll-up's narrative observations show different figures. In its calculated checks, the Must share (69.4%) exceeds the 60% ceiling and the Could pool (4.6%) falls below the 20% target.

The operating inputs come from `ReFeed_Financial_Appraisal_Updated (1) (1).xlsx`, sheets `Assumptions` and `Operating Build`. The workbook's driver note at `Operating Build!D7` gives a 550-active-partner cap. The displayed formula uses the broader 5,500-partner addressable estimate and produces 650 in Year 5. W6 uses the documented cap and recalculates the cash flows.

W5 defines three **release scopes**. The available model supplies one operating forecast and no benefit estimates by priority tier. The tables therefore vary build cost while holding operating cash flows constant. The operating downside and upside appear separately in the sensitivity table.

### Operating drivers

| Driver | Year 1 | Year 2 | Year 3 | Year 4 | Year 5 |
|---|---:|---:|---:|---:|---:|
| Active digital partners | 210 | 320 | 430 | 540 | 550 |
| Eligible baseline route km | 87.360 | 133.120 | 178.880 | 224.640 | 228.800 |
| Incremental qualified UCO (litres) | 54.600 | 83.200 | 111.800 | 140.400 | 143.000 |
| Working-capital balance before terminal recovery (VND) | 26.264.365 | 40.021.890 | 53.779.414 | 67.536.939 | 68.787.623 |

### Calculation

- Route fuel savings = eligible baseline km × route-km reduction × diesel price ÷ km per litre.
- Incremental contribution margin = incremental qualified litres × contribution margin per litre.
- Working-capital balance = incremental qualified litres × UCO cost per litre × 7 ÷ 365.
- Net cash flow = fuel savings + incremental contribution margin − running cost − increase in working capital + terminal recovery − build investment.
- NPV discounts each year-end net cash flow at 15% and includes Year 0 at face value. IRR is the rate that makes NPV zero.
- Payback uses cumulative net cash flow and interpolates within the recovery year. Discounted payback uses discounted flows. Profitability index = 1 + NPV ÷ Year 0 investment. Metrics use unrounded flows.

## Pessimistic: Must only

| Line item | Year 0 | Year 1 | Year 2 | Year 3 | Year 4 | Year 5 |
|---|---:|---:|---:|---:|---:|---:|
| Route fuel savings | 0 | 25.960.771 | 39.559.270 | 53.157.770 | 66.756.269 | 67.992.496 |
| Incremental UCO contribution margin | 0 | 27.955.200 | 42.598.400 | 57.241.600 | 71.884.800 | 73.216.000 |
| Running cost | 0 | -32.864.000 | -32.864.000 | -32.864.000 | -32.864.000 | -32.864.000 |
| Change in working capital | 0 | -26.264.365 | -13.757.525 | -13.757.525 | -13.757.525 | -1.250.684 |
| Terminal working-capital recovery | 0 | 0 | 0 | 0 | 0 | 68.787.623 |
| Investment (build + contingency) | -48.972.000 | 0 | 0 | 0 | 0 | 0 |
| Net cash flow | -48.972.000 | -5.212.394 | 35.536.146 | 63.777.845 | 92.019.544 | 175.881.435 |
| Cumulative net cash flow | -48.972.000 | -54.184.394 | -18.648.248 | 45.129.597 | 137.149.141 | 313.030.576 |
| PV of net cash flow | -48.972.000 | -4.532.516 | 26.870.432 | 41.934.968 | 52.612.473 | 87.444.158 |

## Base: Must + Should

| Line item | Year 0 | Year 1 | Year 2 | Year 3 | Year 4 | Year 5 |
|---|---:|---:|---:|---:|---:|---:|
| Route fuel savings | 0 | 25.960.771 | 39.559.270 | 53.157.770 | 66.756.269 | 67.992.496 |
| Incremental UCO contribution margin | 0 | 27.955.200 | 42.598.400 | 57.241.600 | 71.884.800 | 73.216.000 |
| Running cost | 0 | -32.864.000 | -32.864.000 | -32.864.000 | -32.864.000 | -32.864.000 |
| Change in working capital | 0 | -26.264.365 | -13.757.525 | -13.757.525 | -13.757.525 | -1.250.684 |
| Terminal working-capital recovery | 0 | 0 | 0 | 0 | 0 | 68.787.623 |
| Investment (build + contingency) | -65.212.000 | 0 | 0 | 0 | 0 | 0 |
| Net cash flow | -65.212.000 | -5.212.394 | 35.536.146 | 63.777.845 | 92.019.544 | 175.881.435 |
| Cumulative net cash flow | -65.212.000 | -70.424.394 | -34.888.248 | 28.889.597 | 120.909.141 | 296.790.576 |
| PV of net cash flow | -65.212.000 | -4.532.516 | 26.870.432 | 41.934.968 | 52.612.473 | 87.444.158 |

## Optimistic: full scope

| Line item | Year 0 | Year 1 | Year 2 | Year 3 | Year 4 | Year 5 |
|---|---:|---:|---:|---:|---:|---:|
| Route fuel savings | 0 | 25.960.771 | 39.559.270 | 53.157.770 | 66.756.269 | 67.992.496 |
| Incremental UCO contribution margin | 0 | 27.955.200 | 42.598.400 | 57.241.600 | 71.884.800 | 73.216.000 |
| Running cost | 0 | -32.864.000 | -32.864.000 | -32.864.000 | -32.864.000 | -32.864.000 |
| Change in working capital | 0 | -26.264.365 | -13.757.525 | -13.757.525 | -13.757.525 | -1.250.684 |
| Terminal working-capital recovery | 0 | 0 | 0 | 0 | 0 | 68.787.623 |
| Investment (build + contingency) | -68.052.000 | 0 | 0 | 0 | 0 | 0 |
| Net cash flow | -68.052.000 | -5.212.394 | 35.536.146 | 63.777.845 | 92.019.544 | 175.881.435 |
| Cumulative net cash flow | -68.052.000 | -73.264.394 | -37.728.248 | 26.049.597 | 118.069.141 | 293.950.576 |
| PV of net cash flow | -68.052.000 | -4.532.516 | 26.870.432 | 41.934.968 | 52.612.473 | 87.444.158 |

## Results

| Metric | Pessimistic: Must only | Base: Must + Should | Optimistic: full scope |
|---|---:|---:|---:|
| Year 0 investment (VND) | 48.972.000 | 65.212.000 | 68.052.000 |
| NPV at 15% (VND) | 155.357.514 | 139.117.514 | 136.277.514 |
| IRR | 66.73% | 54.32% | 52.58% |
| Simple payback (years) | 2.29 | 2.55 | 2.59 |
| Discounted payback (years) | 2.64 | 3.02 | 3.07 |
| Profitability index | 4.17 | 3.13 | 3.00 |

### Base scope sensitivity

| Assumed reduction in eligible route km | NPV with Year 5 recovery (VND) | NPV with no Year 5 recovery (VND) |
|---|---:|---:|
| 4.8% | 75.352.885 | 41.153.279 |
| 8.0% | 139.117.514 | 104.917.908 |
| 13.0% | 238.749.747 | 204.550.141 |

Each row changes only route-km savings and the indicated terminal recovery treatment. The base scope has a modelled NPV of 139.117.514 VND at the 8% route assumption. The Must-only case is ahead by 16.240.000 VND because its initial cost is lower. Should work must add 16.240.000 VND of incremental discounted net benefit to match Must-only. Could work must add a further 2.840.000 VND to match the Base scope.

**Decision.** Keep Must + Should as the W5 planning case. Confirm adoption, route-km savings, realized contribution margin, cash operating costs and settlement timing in a pilot before committing funds. Check the omitted rollout and reward costs, and verify whether working capital would actually be released at the horizon.
