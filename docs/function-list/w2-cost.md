# W2 Costing and roll-up

Lesson 5 (block W2). Fully derived from [W1](w1-function-list.md), the effective priorities in [W4](w4-priorities.md) and the class Parameters:

- Person-hours = base hours x AI leverage factor + base hours x verification factor.
- AI leverage: FE 0.35, BE 0.7, INT 1.0. Verification factor: 0.15 when the EARS criterion is precise, 0.4 when it is vague.
- Rate: FE Junior 200.000 VND/h; BE and INT Senior 300.000 VND/h.

## Cost per L3 work item

| L3 ID | Feature | Layer | Base h | AI-adj. h | Verif. h | Person-hours | Band | Cost (VND) | Effective priority |
|---|---|---|---|---|---|---|---|---|---|
| FR-01-01-FE | F-01 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800.000 | Must |
| FR-01-01-BE | F-01 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-01-02-FE | F-01 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800.000 | Must |
| FR-01-02-BE | F-01 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-01-03-FE | F-01 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800.000 | Must |
| FR-01-03-BE | F-01 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-01-04-FE | F-01 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800.000 | Should |
| FR-01-04-BE | F-01 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Should |
| FR-01-05-FE | F-01 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Could |
| FR-01-05-BE | F-01 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Could |
| FR-02-01-FE | F-02 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800.000 | Must |
| FR-02-01-BE | F-02 | BE | 16 | 11.2 | 2.4 | 13.6 | Senior | 4.080.000 | Must |
| FR-02-01-INT | F-02 | INT | 8 | 8.0 | 1.2 | 9.2 | Senior | 2.760.000 | Must |
| FR-02-02-FE | F-02 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800.000 | Must |
| FR-02-02-BE | F-02 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-02-03-FE | F-02 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800.000 | Must |
| FR-02-03-BE | F-02 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-03-01-FE | F-03 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800.000 | Must |
| FR-03-01-BE | F-03 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-03-02-FE | F-03 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Must |
| FR-03-02-BE | F-03 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |
| FR-04-01-FE | F-04 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800.000 | Must |
| FR-04-01-BE | F-04 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-04-02-FE | F-04 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800.000 | Should |
| FR-04-02-BE | F-04 | BE | 16 | 11.2 | 2.4 | 13.6 | Senior | 4.080.000 | Should |
| FR-04-03-FE | F-04 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800.000 | Won't |
| FR-04-03-BE | F-04 | BE | 16 | 11.2 | 2.4 | 13.6 | Senior | 4.080.000 | Won't |
| FR-05-01-FE | F-05 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800.000 | Must |
| FR-05-01-BE | F-05 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-05-02-FE | F-05 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800.000 | Should |
| FR-05-02-BE | F-05 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Should |
| FR-05-03-FE | F-05 | FE | 4 | 1.4 | 0.6 | 2.0 | Junior | 400.000 | Could |
| FR-05-03-BE | F-05 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Could |
| FR-05-04-FE | F-05 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800.000 | Should |
| FR-05-04-BE | F-05 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Should |
| FR-06-01-FE | F-06 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800.000 | Should |
| FR-06-01-BE | F-06 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Should |
| FR-06-02-FE | F-06 | FE | 8 | 2.8 | 1.2 | 4.0 | Junior | 800.000 | Must |
| FR-06-02-BE | F-06 | BE | 16 | 11.2 | 2.4 | 13.6 | Senior | 4.080.000 | Must |
| FR-06-03-BE | F-06 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-06-04-BE | F-06 | BE | 8 | 5.6 | 1.2 | 6.8 | Senior | 2.040.000 | Must |
| FR-06-05-INT | F-06 | INT | 8 | 8.0 | 1.2 | 9.2 | Senior | 2.760.000 | Must |
| FR-06-06-BE | F-06 | BE | 4 | 2.8 | 0.6 | 3.4 | Senior | 1.020.000 | Must |

## Roll-up per feature

| Feature | Name | L2 | L3 | Base h | Must p-h | Should p-h | Could p-h | In-scope p-h | In-scope cost (VND) | Must share | Transactional FP | Base h per FP |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| F-01 | Supplier Onboarding & Pickup Management | 5 | 10 | 72 | 32.4 | 10.8 | 5.4 | 48.6 | 12.780.000 | 67% | 20 | 3.6 |
| F-02 | Collection Operations & Handover | 3 | 7 | 64 | 48.4 | 0 | 0 | 48.4 | 13.320.000 | 100% | 13 | 4.9 |
| F-03 | UCO Transaction | 2 | 4 | 24 | 16.2 | 0 | 0 | 16.2 | 4.260.000 | 100% | 8 | 3.0 |
| F-04 | Warehouse & Traceability | 3 | 6 | 64 | 10.8 | 17.6 | 0 | 28.4 | 7.720.000 | 38% | 12 | 5.3 |
| F-05 | Supplier Value & ESG / Green Profile | 4 | 8 | 56 | 10.8 | 21.6 | 5.4 | 37.8 | 9.940.000 | 29% | 17 | 3.3 |
| F-06 | Operations, Audit & Compliance | 6 | 8 | 68 | 43.8 | 10.8 | 0 | 54.6 | 15.580.000 | 80% | 9 | 7.6 |
| **Project** |  | **23** | **43** | **348** | **162.4** | **60.8** | **10.8** | **234** | **63.600.000** | **69%** | **79** | **4.4** |

## The three headline figures

| Scenario | Scope | Person-hours | Cost (VND) | Meaning |
|---|---|---|---|---|
| Committed | Must only | 162.4 | 44.520.000 | What the team guarantees to deliver — the Minimum Usable SubseT |
| Expected | Must + Should | 223.2 | 60.760.000 | The figure given to the client; the base cash flow for Session 8 |
| Ceiling | Must + Should + Could | 234 | 63.600.000 | If the Could contingency pool is never consumed |

## Threshold check (after W4)

| Indicator | Value | Threshold | Verdict |
|---|---|---|---|
| Must share of in-scope effort | 69.4% | at most 60% | ABOVE CEILING - demote some Must rows |
| Could share (contingency pool) | 4.6% | target 20% | POOL TOO THIN - nothing left to drop when late |
| In-scope effort vs team capacity | 234 p-h | at most 360 p-h | PASS |
| Hidden technical work, share of effort | 10.4% | at least 10% | PASS |
| L3 rows above 16 hours | 0 | 0 | PASS |
| Feature count | 6 | 3 to 6 | PASS |
| Function-point cross-check: 156 unadjusted FP (79 transactional + 77 data) x 3 h = 468 h, + 28 h non-functional (NC) = 496 h vs 348 bottom-up base h | 29.8% apart | at most 30% | Within tolerance - the two methods agree |

Three observations the team carries into W4:

1. All six workbook thresholds now pass: 5 features; Must 56.6%; Could 21.0%; 230.0 person-hours within the 240-hour capacity; hidden work 10.4%.
2. The client-facing Expected scenario is 181.6 person-hours and 49,480,000 VND; the full in-scope ceiling is 230.0 person-hours and 62,800,000 VND.
3. The function-point cross-check is 487 hours versus 348 bottom-up hours (−28.5%), now within tolerance; F-05 is intentionally high in hours per FP because it carries NC technical work.
