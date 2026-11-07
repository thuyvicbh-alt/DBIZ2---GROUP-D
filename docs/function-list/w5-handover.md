# W5 Baseline and handover: ReFeed

> Baseline for ReFeed System (Lesson 5, block W5). The baseline every later lesson and DBIZ3 refer to.

## 1. Three scenarios

| Scenario | Person-hours | Cost (VND) | Role in the cash-flow analysis (W6) |
|---|---|---|---|
| Pessimistic: Must only | 162.4 | 44.520.000 | Lowest investment, lowest benefit |
| Base: Must + Should | 223.2 | 60.760.000 | The client figure; base cash flow |
| Optimistic: full scope | 234.0 | 63.600.000 | If the Could pool is never consumed |

> *The three MoSCoW tiers ARE the three scenarios — no invented plus-or-minus 20%.*

## 2. Assumptions

| Assumption | Value | Source |
|---|---|---|
| Junior / Senior / Expert rate | 200.000 / 300.000 / 400.000 VND per hour | Class Parameters (ITviec 2025-26 salary report) |
| Blended hourly rate | 280.000 VND per hour | Class Parameters (Weighted average team cost) |
| AI leverage FE / BE / INT | 0.35 / 0.70 / 1.00 | Class Parameters |
| Verification factor precise / vague | 0.15 / 0.40 | Class Parameters |
| Hours per function point (cross-check only) | 3 | Class Parameters |
| Team capacity | 360 person-hours (6 people x 12 weeks x 5 h) | Class Parameters |
| Money contingency on the Must set | 10% = 4.452.000 VND | Class Parameters |

## 3. tasks.md seed (effective priority Must)

Risk order: 1 = INT (external services), 2 = BE, 3 = FE. DBIZ3 starts with the riskiest rows.

| L3 ID | Layer | Person-hours | Risk order |
|---|---|---|---|
| FR-02-01-INT | INT | 9.2 | 1 |
| FR-06-05-INT | INT | 9.2 | 1 |
| FR-01-01-BE | BE | 6.4 | 2 |
| FR-01-02-BE | BE | 6.4 | 2 |
| FR-01-03-BE | BE | 6.4 | 2 |
| FR-01-04-BE | BE | 6.4 | 2 |
| FR-01-05-BE | BE | 3.2 | 2 |
| FR-02-01-BE | BE | 12.9 | 2 |
| FR-02-02-BE | BE | 6.4 | 2 |
| FR-02-03-BE | BE | 6.4 | 2 |
| FR-03-01-BE | BE | 6.4 | 2 |
| FR-03-02-BE | BE | 3.2 | 2 |
| FR-04-01-BE | BE | 6.4 | 2 |
| FR-04-02-BE | BE | 6.4 | 2 |
| FR-04-03-BE | BE | 6.4 | 2 |
| FR-05-01-BE | BE | 6.4 | 2 |
| FR-05-02-BE | BE | 3.2 | 2 |
| FR-06-01-BE | BE | 6.4 | 2 |
| FR-06-02-BE | BE | 6.4 | 2 |
| FR-06-03-BE | BE | 6.4 | 2 |
| FR-06-04-BE | BE | 6.4 | 2 |
| FR-06-06-BE | BE | 6.4 | 2 |
| FR-01-01-FE | FE | 3.2 | 3 |
| FR-01-02-FE | FE | 3.2 | 3 |
| FR-01-03-FE | FE | 3.2 | 3 |
| FR-01-04-FE | FE | 3.2 | 3 |
| FR-01-05-FE | FE | 1.6 | 3 |
| FR-02-01-FE | FE | 3.2 | 3 |
| FR-02-02-FE | FE | 3.2 | 3 |
| FR-02-03-FE | FE | 3.2 | 3 |
| FR-03-01-FE | FE | 3.2 | 3 |
| FR-03-02-FE | FE | 1.6 | 3 |
| FR-04-01-FE | FE | 3.2 | 3 |
| FR-04-02-FE | FE | 3.2 | 3 |
| FR-04-03-FE | FE | 3.2 | 3 |
| FR-05-01-FE | FE | 3.2 | 3 |
| FR-05-02-FE | FE | 1.6 | 3 |
| FR-06-01-FE | FE | 3.2 | 3 |
| FR-06-02-FE | FE | 3.2 | 3 |

## 4. Open questions for the client

| # | Question | Blocking? |
|---|---|---|
| 1 | May Eco Oil data sync run as an end-of-day batch job if API connection fails? | No (FR-04-02, FR-04-03) |
| 2 | Is 2% the agreed volume tolerance threshold for warehouse receipt discrepancy alerts? | Yes (FR-04-01) |
| 3 | Is there a minimum threshold (e.g., 100,000 VND) required before suppliers can submit a payout withdrawal request? | No (FR-05-02) |