# W5 Baseline and handover: ReFeed

> ReFeed Group D. Revised planning baseline, 2026-11-26, aligned with W1–W4. Estimate and release approval remain open where recorded below.

## 1. Three scenarios

| Scenario | Person-hours | Cost (VND) | Role in the cash-flow analysis (W6) |
|---|---|---|---|
| Pessimistic: Must only | 428.4 | 117.820.000 | Minimum usable subset; check feasibility below |
| Base: Must + Should | 548.6 | 150.780.000 | Expected planning scope for client review |
| Optimistic: full scope | 562.8 | 154.640.000 | Includes Could; excludes Won't |

The three MoSCoW tiers define the three scope scenarios. These are build costs before the separate money contingency; no invented ±20% scenario is applied. FR-04-11 is excluded. W6 still contains the previous appraisal and is marked as a historical baseline until its cash flows are recalculated with these inputs.

## 2. Assumptions

| Assumption | Value | Source |
|---|---|---|
| L3 sizing | S = 4 h; M = 8 h; L = 16 h; bottom-up estimation | W1 sizing basis; workshop Parameters!B9:B11, B63:B64 |
| Junior / Senior / Expert rate | 200.000 / 300.000 / 400.000 VND/h | Workshop Parameters!B21:B23; W2 uses Junior for FE, Senior for BE/INT |
| AI leverage FE / BE / INT | 0.35 / 0.70 / 1.00 | Workshop Parameters!B15:B17 |
| Verification precise / imprecise | 0.15 / 0.40 | Workshop Parameters!B27:B28; per-task flag in W1 |
| Hours per function point | 3 pre-AI gross hours; cross-check only | Workshop Parameters!B38; W2 FP cross-check |
| Team capacity | 360 p-h = 6 people × 12 weeks × 5 productive h/week | Workshop Parameters!B42:B45 |
| Money contingency on Must | 10% = 11.782.000 VND | Retained W5 contingency rule; recalculated from current Must cost |
| Scope contingency | Could = 2.5%; class target 20% | W2/W4; no extra 20% money buffer |
| Estimation status | Task-level planning estimates; team review pending | W1; W3 defect log |

### Feasibility against the fixed capacity

| Scope | Required p-h | Capacity p-h | Shortfall p-h | Status |
|---|---|---|---|---|
| Must | 428.4 | 360.0 | 68.4 | OVER CAPACITY |
| Must + Should | 548.6 | 360.0 | 188.6 | OVER CAPACITY |
| Must + Should + Could | 562.8 | 360.0 | 202.8 | OVER CAPACITY |

Must is 76.1% of in-scope effort against a 60% maximum. The team must review priorities/dependencies or capacity before committing the release. This revision preserves the existing decisions and reports the shortfall rather than changing estimates to fit.

## 3. tasks.md seed (effective priority Must)

Risk order: 1 = INT, 2 = BE, 3 = FE, matching the previous handover rule. This is risk triage, not a dependency-free execution sequence. Source-rule blockers in section 4 must be resolved before their affected tasks can be accepted. The list contains every Must L3 row in W2 exactly once and no Should, Could or Won't row.

| L3 ID | Layer | Person-hours | Risk order |
|---|---|---|---|
| FR-02-02-INT | INT | 11.2 | 1 |
| FR-06-10-INT | INT | 9.2 | 1 |
| FR-06-11-INT | INT | 9.2 | 1 |
| FR-01-01-BE | BE | 8.8 | 2 |
| FR-01-02-BE | BE | 3.4 | 2 |
| FR-01-06-BE | BE | 6.8 | 2 |
| FR-01-07-BE | BE | 4.4 | 2 |
| FR-01-08-BE | BE | 3.4 | 2 |
| FR-01-09-BE | BE | 3.4 | 2 |
| FR-01-10-BE | BE | 3.4 | 2 |
| FR-01-11-BE | BE | 4.4 | 2 |
| FR-01-12-BE | BE | 3.4 | 2 |
| FR-01-13-BE | BE | 3.4 | 2 |
| FR-01-14-BE | BE | 3.4 | 2 |
| FR-02-01-BE | BE | 8.8 | 2 |
| FR-02-04-BE | BE | 3.4 | 2 |
| FR-02-05-BE | BE | 6.8 | 2 |
| FR-02-06-BE | BE | 3.4 | 2 |
| FR-02-07-BE | BE | 3.4 | 2 |
| FR-02-08-BE | BE | 3.4 | 2 |
| FR-02-09-BE | BE | 3.4 | 2 |
| FR-02-10-BE | BE | 8.8 | 2 |
| FR-02-11-BE | BE | 3.4 | 2 |
| FR-02-12-BE | BE | 6.8 | 2 |
| FR-02-13-BE | BE | 3.4 | 2 |
| FR-02-14-BE | BE | 4.4 | 2 |
| FR-02-15-BE | BE | 6.8 | 2 |
| FR-02-16-BE | BE | 6.8 | 2 |
| FR-02-17-BE | BE | 6.8 | 2 |
| FR-02-18-BE | BE | 3.4 | 2 |
| FR-03-01-BE | BE | 3.4 | 2 |
| FR-03-04-BE | BE | 3.4 | 2 |
| FR-03-05-BE | BE | 3.4 | 2 |
| FR-03-06-BE | BE | 6.8 | 2 |
| FR-03-07-BE | BE | 6.8 | 2 |
| FR-03-08-BE | BE | 3.4 | 2 |
| FR-03-09-BE | BE | 6.8 | 2 |
| FR-03-11-BE | BE | 3.4 | 2 |
| FR-03-12-BE | BE | 3.4 | 2 |
| FR-03-13-BE | BE | 6.8 | 2 |
| FR-04-01-BE | BE | 3.4 | 2 |
| FR-04-02-BE | BE | 3.4 | 2 |
| FR-04-03-BE | BE | 3.4 | 2 |
| FR-04-04-BE | BE | 3.4 | 2 |
| FR-04-05-BE | BE | 3.4 | 2 |
| FR-05-01-BE | BE | 3.4 | 2 |
| FR-05-02-BE | BE | 3.4 | 2 |
| FR-05-03-BE | BE | 6.8 | 2 |
| FR-05-04-BE | BE | 3.4 | 2 |
| FR-05-05-BE | BE | 3.4 | 2 |
| FR-05-06-BE | BE | 6.8 | 2 |
| FR-06-02-BE | BE | 8.8 | 2 |
| FR-06-03-BE | BE | 8.8 | 2 |
| FR-06-04-BE | BE | 6.8 | 2 |
| FR-06-05-BE | BE | 3.4 | 2 |
| FR-06-06-BE | BE | 6.8 | 2 |
| FR-06-07-BE | BE | 6.8 | 2 |
| FR-06-08-BE | BE | 6.8 | 2 |
| FR-06-09-BE | BE | 3.4 | 2 |
| FR-06-12-BE | BE | 6.8 | 2 |
| FR-06-13-BE | BE | 3.4 | 2 |
| FR-06-14-BE | BE | 6.8 | 2 |
| FR-01-01-FE | FE | 6.0 | 3 |
| FR-01-03-FE | FE | 2.0 | 3 |
| FR-01-04-FE | FE | 2.0 | 3 |
| FR-01-05-FE | FE | 2.0 | 3 |
| FR-01-07-FE | FE | 6.0 | 3 |
| FR-01-09-FE | FE | 2.0 | 3 |
| FR-01-10-FE | FE | 2.0 | 3 |
| FR-01-11-FE | FE | 6.0 | 3 |
| FR-01-12-FE | FE | 2.0 | 3 |
| FR-01-14-FE | FE | 2.0 | 3 |
| FR-02-01-FE | FE | 3.0 | 3 |
| FR-02-03-FE | FE | 4.0 | 3 |
| FR-02-04-FE | FE | 2.0 | 3 |
| FR-02-08-FE | FE | 4.0 | 3 |
| FR-02-09-FE | FE | 2.0 | 3 |
| FR-02-10-FE | FE | 6.0 | 3 |
| FR-02-11-FE | FE | 2.0 | 3 |
| FR-02-13-FE | FE | 2.0 | 3 |
| FR-02-14-FE | FE | 6.0 | 3 |
| FR-02-17-FE | FE | 2.0 | 3 |
| FR-02-18-FE | FE | 2.0 | 3 |
| FR-03-01-FE | FE | 2.0 | 3 |
| FR-03-02-FE | FE | 2.0 | 3 |
| FR-03-03-FE | FE | 2.0 | 3 |
| FR-03-04-FE | FE | 4.0 | 3 |
| FR-03-05-FE | FE | 2.0 | 3 |
| FR-03-09-FE | FE | 2.0 | 3 |
| FR-03-10-FE | FE | 2.0 | 3 |
| FR-03-11-FE | FE | 2.0 | 3 |
| FR-03-13-FE | FE | 2.0 | 3 |
| FR-03-14-FE | FE | 2.0 | 3 |
| FR-04-01-FE | FE | 2.0 | 3 |
| FR-04-02-FE | FE | 2.0 | 3 |
| FR-04-03-FE | FE | 2.0 | 3 |
| FR-04-05-FE | FE | 2.0 | 3 |
| FR-05-01-FE | FE | 2.0 | 3 |
| FR-05-02-FE | FE | 2.0 | 3 |
| FR-06-02-FE | FE | 3.0 | 3 |
| FR-06-03-FE | FE | 3.0 | 3 |

Must seed total: **101 tasks, 428.4 person-hours, 117.820.000 VND**.

## 4. Open questions for the team and client

| # | Question | Blocking? | Current IDs / evidence |
|---|---|---|---|
| 1 | May Eco Oil sync use end-of-day batching or queued retry when its API is unavailable? Confirm payload and acknowledgement. | Yes for final integration acceptance; the fallback is not approved | FR-04-10-INT; AD-UC-11 section 6 question 2; previous W5 question 1 |
| 2 | Which warehouse discrepancy tolerance is agreed? Previous W5 proposes 2%; AD-UC-10 tests use 5%. | Yes for final receiving acceptance | FR-04-02, FR-04-03; W3 D-08 |
| 3 | What is the minimum withdrawal amount and the exact supplier/bank-profile eligibility rule? 100.000 VND remains only an example. | Yes for withdrawal acceptance; Should scope | FR-05-07; previous W5 question 3 |
| 4 | Are withdrawal request/status functions delivered as Should in Release 1 or deferred to Release 2? | Yes for final release-scope approval | FR-05-07 through FR-05-11; W3 D-05; ADR-003 excludes actual transfers |
| 5 | Which scope/dependency changes or additional capacity will close the shortfall shown above? | Yes for delivery commitment | W2 threshold check; W4 priority totals; W3 D-03/D-04 |
| 6 | Confirm the unresolved field validation, optimisation recovery, batch formulas, report content and audit fields listed in W3. | Yes for acceptance of affected tasks | W3 EARS precision review; W1 No flags |
| 7 | How does Received become Verified_At_Warehouse before UC-11 selects its collection inputs? | Yes for end-to-end warehouse acceptance | FR-04-05, FR-04-06; AD-UC-10 / AD-UC-11; W3 D-08 |
| 8 | Should withdrawal creation and balance deduction run sequentially as in UC-13 or concurrently as in UC-12? Confirm atomic rejection/failure behaviour. | Yes for withdrawal consistency tests | FR-05-09, FR-05-10; W3 D-09 |
| 9 | Confirm the EIF weight of map data in W0 before approving the FP comparison. | No for bottom-up costing | W0 Table 4; W3 D-07 |