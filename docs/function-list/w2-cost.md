# W2 Costing and roll-up: ReFeed

> ReFeed Group D. Updated 2026-11-27 against W0/W1 at repository revision `08e751f`.

Lesson 5 (block W2). Derived from all 125 L3 rows in [W1](w1-function-list.md), their EARS-precision flags, and the effective priorities in [W4](w4-priorities.md). [W0](w0-scope.md) supplies the six feature names and data-function points.

- Person-hours = base hours × AI leverage factor + base hours × verification factor.
- AI leverage: FE 0.35, BE 0.70, INT 1.00. Verification: precise 0.15; imprecise 0.40.
- Rates: FE Junior 200.000 VND/h; BE and INT Senior 300.000 VND/h. Cost = person-hours × rate.
- Retained workshop parameters: `Parameters!B9:B11`, `B15:C17`, `B21:B23`, `B27:B28`, `B38`, `B42:B54`. Calculations use unrounded values; hours display one decimal and VND displays whole units.
- W0 now lists 16 use cases. Withdrawal is absent from W0/W1 and has no task, effort or cost here. Current F-05 IDs follow their current behaviour: FR-05-07 = Green Profile; FR-05-08/09 = ESG report generation/provision. Previous withdrawal priorities are not copied onto these reused IDs.
- For this alignment, every function retained in W1 is estimated. Effective release priorities come from W4; W0's Needed/not needed inventory flags are preserved in W0. W3 records the need to clarify those flags if the team intends them to govern release scope.
- All-task totals include FR-04-11, the deferred trace query. Release totals and scenario shares exclude Won't rows. The source task ID `F-05-02-FE` is retained literally with parent `FR-05-02`; see W3 D-02.

## Cost per L3 work item

| L3 ID | Feature | Layer | Base h | AI-adj. h | Verif. h | Person-hours | Band | Cost (VND) | Effective priority |
|---|---|---|---|---|---|---|---|---|---|
| FR-01-01-FE | F-01 | FE | 8 | 2.8 | 3.2 | 6.0 | Junior | 1.200.000 | Must |
| FR-01-01-BE | F-01 | BE | 8 | 5.6 | 3.2 | 8.8 | Senior | 2.640.000 | Must |
| FR-01-02-BE | F-01 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-01-03-FE | F-01 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-01-04-FE | F-01 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-01-05-FE | F-01 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-01-06-BE | F-01 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-01-07-FE | F-01 | FE | 8 | 2.8 | 3.2 | 6.0 | Junior | 1.200.000 | Must |
| FR-01-07-BE | F-01 | BE | 4 | 2.8 | 1.6 | 4.4 | Senior | 1.320.000 | Must |
| FR-01-08-BE | F-01 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-01-09-FE | F-01 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-01-09-BE | F-01 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-01-10-FE | F-01 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-01-10-BE | F-01 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-01-11-FE | F-01 | FE | 8 | 2.8 | 3.2 | 6.0 | Junior | 1.200.000 | Must |
| FR-01-11-BE | F-01 | BE | 4 | 2.8 | 1.6 | 4.4 | Senior | 1.320.000 | Must |
| FR-01-12-FE | F-01 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-01-12-BE | F-01 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-01-13-BE | F-01 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-01-14-FE | F-01 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-01-14-BE | F-01 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-01-15-FE | F-01 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Should |
| FR-01-15-BE | F-01 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Should |
| FR-01-16-FE | F-01 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Should |
| FR-01-17-FE | F-01 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800.000 | Should |
| FR-01-17-BE | F-01 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Should |
| FR-01-18-BE | F-01 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Should |
| FR-01-19-BE | F-01 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Should |
| FR-01-20-FE | F-01 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Could |
| FR-01-20-BE | F-01 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Could |
| FR-02-01-FE | F-02 | FE | 4 | 1.4 | 1.6 | 3.0 | Junior | 600.000 | Must |
| FR-02-01-BE | F-02 | BE | 8 | 5.6 | 3.2 | 8.8 | Senior | 2.640.000 | Must |
| FR-02-02-INT | F-02 | INT | 8 | 8.0 | 3.2 | 11.2 | Senior | 3.360.000 | Must |
| FR-02-03-FE | F-02 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800.000 | Must |
| FR-02-04-FE | F-02 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-02-04-BE | F-02 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-02-05-BE | F-02 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-02-06-BE | F-02 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-02-07-BE | F-02 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-02-08-FE | F-02 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800.000 | Must |
| FR-02-08-BE | F-02 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-02-09-FE | F-02 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-02-09-BE | F-02 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-02-10-FE | F-02 | FE | 8 | 2.8 | 3.2 | 6.0 | Junior | 1.200.000 | Must |
| FR-02-10-BE | F-02 | BE | 8 | 5.6 | 3.2 | 8.8 | Senior | 2.640.000 | Must |
| FR-02-11-FE | F-02 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-02-11-BE | F-02 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-02-12-BE | F-02 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-02-13-FE | F-02 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-02-13-BE | F-02 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-02-14-FE | F-02 | FE | 8 | 2.8 | 3.2 | 6.0 | Junior | 1.200.000 | Must |
| FR-02-14-BE | F-02 | BE | 4 | 2.8 | 1.6 | 4.4 | Senior | 1.320.000 | Must |
| FR-02-15-BE | F-02 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-02-16-BE | F-02 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-02-17-FE | F-02 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-02-17-BE | F-02 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-02-18-FE | F-02 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-02-18-BE | F-02 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-03-01-FE | F-03 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-03-01-BE | F-03 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-03-02-FE | F-03 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-03-03-FE | F-03 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-03-04-FE | F-03 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800.000 | Must |
| FR-03-04-BE | F-03 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-03-05-FE | F-03 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-03-05-BE | F-03 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-03-06-BE | F-03 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-03-07-BE | F-03 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-03-08-BE | F-03 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-03-09-FE | F-03 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-03-09-BE | F-03 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-03-10-FE | F-03 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-03-11-FE | F-03 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-03-11-BE | F-03 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-03-12-BE | F-03 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-03-13-FE | F-03 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-03-13-BE | F-03 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-03-14-FE | F-03 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-04-01-FE | F-04 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-04-01-BE | F-04 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-04-02-FE | F-04 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-04-02-BE | F-04 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-04-03-FE | F-04 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-04-03-BE | F-04 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-04-04-BE | F-04 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-04-05-FE | F-04 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-04-05-BE | F-04 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-04-06-FE | F-04 | FE | 4 | 1.4 | 1.6 | 3.0 | Junior | 600.000 | Should |
| FR-04-06-BE | F-04 | BE | 4 | 2.8 | 1.6 | 4.4 | Senior | 1.320.000 | Should |
| FR-04-07-FE | F-04 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Should |
| FR-04-08-BE | F-04 | BE | 8 | 5.6 | 3.2 | 8.8 | Senior | 2.640.000 | Should |
| FR-04-09-FE | F-04 | FE | 4 | 1.4 | 1.6 | 3.0 | Junior | 600.000 | Should |
| FR-04-09-BE | F-04 | BE | 8 | 5.6 | 3.2 | 8.8 | Senior | 2.640.000 | Should |
| FR-04-10-INT | F-04 | INT | 8 | 8.0 | 3.2 | 11.2 | Senior | 3.360.000 | Should |
| FR-04-11-FE | F-04 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800.000 | Won't |
| FR-04-11-BE | F-04 | BE | 16 | 11.2 | 2.4 | 13.6 | Senior | 4.080.000 | Won't |
| FR-05-01-FE | F-05 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-05-01-BE | F-05 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| F-05-02-FE | F-05 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-05-02-BE | F-05 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-05-03-BE | F-05 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-05-04-BE | F-05 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-05-05-BE | F-05 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-05-06-BE | F-05 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-05-07-FE | F-05 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Could |
| FR-05-07-BE | F-05 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Could |
| FR-05-08-BE | F-05 | BE | 8 | 5.6 | 3.2 | 8.8 | Senior | 2.640.000 | Should |
| FR-05-09-FE | F-05 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Should |
| FR-06-01-FE | F-06 | FE | 8 | 2.8 | 3.2 | 6.0 | Junior | 1.200.000 | Should |
| FR-06-01-BE | F-06 | BE | 8 | 5.6 | 3.2 | 8.8 | Senior | 2.640.000 | Should |
| FR-06-02-FE | F-06 | FE | 4 | 1.4 | 1.6 | 3.0 | Junior | 600.000 | Must |
| FR-06-02-BE | F-06 | BE | 8 | 5.6 | 3.2 | 8.8 | Senior | 2.640.000 | Must |
| FR-06-03-FE | F-06 | FE | 4 | 1.4 | 1.6 | 3.0 | Junior | 600.000 | Must |
| FR-06-03-BE | F-06 | BE | 8 | 5.6 | 3.2 | 8.8 | Senior | 2.640.000 | Must |
| FR-06-04-BE | F-06 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-06-05-BE | F-06 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-06-06-BE | F-06 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-06-07-BE | F-06 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-06-08-BE | F-06 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-06-09-BE | F-06 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-06-10-INT | F-06 | INT | 8 | 8.0 | 1.2 | 9.2 | Senior | 2.760.000 | Must |
| FR-06-11-INT | F-06 | INT | 8 | 8.0 | 1.2 | 9.2 | Senior | 2.760.000 | Must |
| FR-06-12-BE | F-06 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-06-13-BE | F-06 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-06-14-BE | F-06 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |

All 125 tasks: **688 base h; 552.0 person-hours; 151.700.000 VND**. Won't: **17.6 person-hours; 4.880.000 VND**. In scope: **534.4 person-hours; 146.820.000 VND**.

## Roll-up per feature

| Feature | Name | L2 | L3 | All base h | Must p-h | Should p-h | Could p-h | Won't p-h | In-scope p-h | In-scope cost (VND) | Must share | Transactional FP (all) |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| F-01 | Supplier Onboarding & Pickup Management | 20 | 30 | 152 | 80.2 | 25.0 | 8.8 | 0.0 | 114.0 | 30.000.000 | 70.4% | 34 |
| F-02 | Collection Operations & Handover | 18 | 28 | 160 | 129.4 | 0.0 | 0.0 | 0.0 | 129.4 | 35.320.000 | 100.0% | 39 |
| F-03 | UCO Transaction | 14 | 20 | 100 | 69.6 | 0.0 | 0.0 | 0.0 | 69.6 | 18.680.000 | 100.0% | 12 |
| F-04 | Warehouse & Traceability | 11 | 18 | 100 | 25.0 | 41.2 | 0.0 | 17.6 | 66.2 | 18.260.000 | 37.8% | 17 |
| F-05 | Supplier Value & ESG / Green Profile | 9 | 12 | 60 | 31.2 | 10.8 | 5.4 | 0.0 | 47.4 | 13.420.000 | 65.8% | 17 |
| F-06 | Operations, Audit & Compliance | 14 | 17 | 116 | 93.0 | 14.8 | 0.0 | 0.0 | 107.8 | 31.140.000 | 86.3% | 13 |
| **Project** |  | 86 | 125 | 688 | 428.4 | 91.8 | 14.2 | 17.6 | 534.4 | 146.820.000 | 80.2% | 132 |

## The three headline figures

| Scenario | Scope | Person-hours | Cost (VND) | Meaning |
|---|---|---|---|---|
| Minimum scope | Must only | 428.4 | 117.820.000 | Minimum usable subset; capacity and acceptance blockers remain open |
| Expected scope | Must + Should | 520.2 | 142.960.000 | Planning case for team/client review |
| Ceiling | Must + Should + Could | 534.4 | 146.820.000 | All in-scope work; excludes the deferred trace query |

## Threshold check (after W4)

| Indicator | Value | Threshold | Verdict |
|---|---|---|---|
| Must share of in-scope effort | 80.2% | at most 60% | ABOVE CEILING |
| Could share (scope contingency) | 2.7% | target 20% | POOL TOO THIN |
| In-scope effort vs team capacity | 534.4 p-h | at most 360 p-h | OVER CAPACITY |
| Hidden technical work, share of in-scope effort | 13.0% | at least 10% | PASS |
| L3 rows outside 4–16 base h | 0 | 0 | PASS |
| Feature count | 6 | 3 to 6 | PASS |
| FP cross-check variance (same all-task boundary) | 2.2% | at most 30% | PASS |

At the retained 360 p-h capacity, Must exceeds capacity by 68.4 p-h; expected scope by 160.2 p-h; full scope by 174.4 p-h. Removing the seven withdrawal tasks removes 36 base h, 28.4 p-h and 7.820.000 VND from the previous estimate. W4 preserves surviving priority decisions by function meaning. W5 carries the remaining constraints into handover.

## Function-point cross-check

Use the retained workshop average weights: EI 4, EO 5, EQ 4, NC 0. W1 marks one representative row per boundary transaction; internal steps are NC and do not multiply transactional FP. Data groups remain counted once in [W0 Table 4](w0-scope.md#table-4-data-functions).

| Component | Calculation | Result |
|---|---|---|
| Transactional FP, all W1 L2 rows | 12 EI × 4 + 8 EO × 5 + 11 EQ × 4 | 132 FP |
| Data-function FP | W0 Table 4; source total retained | 77 FP |
| Gross functional estimate | (132 + 77) × 3 h/FP | 627.0 base h |
| Non-functional platform effort | W1 hidden-work tasks FR-06-04 through FR-06-14, counted once | 76.0 base h |
| Comparable FP estimate | gross functional estimate + platform effort | 703.0 base h |
| Bottom-up estimate | all 125 W1 tasks, including Won't | 688.0 base h |
| Absolute variance | absolute(FP estimate − bottom-up) ÷ bottom-up | 2.2% |

Only hidden non-functional platform work is added to the functional estimate. Other NC rows are internal steps of counted transactions and are not added again. This comparison uses pre-AI base hours, not adjusted person-hours. The map data group in W0 has weight 10 although the retained workshop EIF weight is 7; W3 D-07 records the discrepancy. No W0 weight is silently changed.
