# W5 Baseline and handover: ReFeed

> ReFeed Group D. Revised planning baseline, 2026-11-27, aligned with W0/W1 at `08e751f` and current W2–W4. Estimate and release approval remain open where recorded below.

## 1. Three scenarios

| Scenario | Person-hours | Cost (VND) | Role in the cash-flow analysis (W6) |
|---|---|---|---|
| Pessimistic: Must only | 428.4 | 117.820.000 | Minimum scope; check feasibility and dependency closure below |
| Base: Must + Should | 520.2 | 142.960.000 | Expected planning scope for team/client review |
| Optimistic: full scope | 534.4 | 146.820.000 | Includes Could; excludes Won't |

The three MoSCoW tiers define scope scenarios. These are build costs before the separate money contingency; no invented ±20% scenario is applied. FR-04-11 is excluded. W6 remains the historical appraisal until its cash flows are recalculated with these revised inputs; its existing financial results must not be presented as results of this baseline.

The handover uses W0's 16 current use cases and W1's 86 L2 / 125 L3 rows. Withdrawal functionality has been removed. FR-05-07 now covers Green Profile (UC-13), FR-05-08/09 ESG (UC-14); monitoring is UC-15 and audit/compliance UC-16. Payout work is limited to accumulating and displaying balances under UC-12; actual bank transfers remain external. No withdrawal task or bank gateway is seeded.

## 2. Assumptions

| Assumption | Value | Source |
|---|---|---|
| L3 sizing | S = 4 h; M = 8 h; L = 16 h; bottom-up estimation | W1; retained workshop Parameters!B9:B11, B63:B64 |
| Junior / Senior / Expert rate | 200.000 / 300.000 / 400.000 VND/h; W2 uses Junior FE, Senior BE/INT | Retained Parameters!B21:B23 |
| AI leverage FE / BE / INT | 0.35 / 0.70 / 1.00 | Retained Parameters!B15:B17 |
| Verification precise / imprecise | 0.15 / 0.40; 26 current tasks flagged No | W1; retained Parameters!B27:B28 |
| Hours per function point | 3 pre-AI gross hours; cross-check only | Retained Parameters!B38; W2 |
| Team capacity | 360 p-h = 6 people × 12 weeks × 5 productive h/week | Retained Parameters!B42:B45 |
| Money contingency on Must | 10% = 11.782.000 VND; separate from build costs | Retained W5 rule; recalculated from W2 |
| Scope contingency | Could = 2.7%; class target 20%; no extra 20% money buffer | W2/W4 |
| Task identifier | Preserve literal F-05-02-FE with parent FR-05-02 | W1; W3 D-02 |
| Estimation status | Task-level planning estimates; team review pending | W1; W3 |

### Feasibility against the fixed capacity

| Scope | Required p-h | Capacity p-h | Shortfall p-h | Status |
|---|---|---|---|---|
| Must | 428.4 | 360.0 | 68.4 | OVER CAPACITY |
| Must + Should | 520.2 | 360.0 | 160.2 | OVER CAPACITY |
| Must + Should + Could | 534.4 | 360.0 | 174.4 | OVER CAPACITY |

Must is 80.2% of in-scope effort against a 60% maximum. Review priorities, dependencies or capacity before committing the release. The Must-only scenario is a costed planning subset; the compliance-to-batch dependency in W3 D-10 is not yet closed.

## 3. tasks.md seed (effective priority Must)

Risk order: 1 = INT, 2 = BE, 3 = FE, retaining the previous handover rule. This is risk triage, not an execution sequence. The list contains every effective-Must L3 row from W2 exactly once, using its literal W1 ID and parent. Source-rule and dependency blockers in section 4 must be resolved before the affected work is accepted.

| L3 ID | L2 ID | Layer | Person-hours | Risk order |
|---|---|---|---|---|
| FR-02-02-INT | FR-02-02 | INT | 11.2 | 1 |
| FR-06-10-INT | FR-06-10 | INT | 9.2 | 1 |
| FR-06-11-INT | FR-06-11 | INT | 9.2 | 1 |
| FR-01-01-BE | FR-01-01 | BE | 8.8 | 2 |
| FR-01-02-BE | FR-01-02 | BE | 3.4 | 2 |
| FR-01-06-BE | FR-01-06 | BE | 6.8 | 2 |
| FR-01-07-BE | FR-01-07 | BE | 4.4 | 2 |
| FR-01-08-BE | FR-01-08 | BE | 3.4 | 2 |
| FR-01-09-BE | FR-01-09 | BE | 3.4 | 2 |
| FR-01-10-BE | FR-01-10 | BE | 3.4 | 2 |
| FR-01-11-BE | FR-01-11 | BE | 4.4 | 2 |
| FR-01-12-BE | FR-01-12 | BE | 3.4 | 2 |
| FR-01-13-BE | FR-01-13 | BE | 3.4 | 2 |
| FR-01-14-BE | FR-01-14 | BE | 3.4 | 2 |
| FR-02-01-BE | FR-02-01 | BE | 8.8 | 2 |
| FR-02-04-BE | FR-02-04 | BE | 3.4 | 2 |
| FR-02-05-BE | FR-02-05 | BE | 6.8 | 2 |
| FR-02-06-BE | FR-02-06 | BE | 3.4 | 2 |
| FR-02-07-BE | FR-02-07 | BE | 3.4 | 2 |
| FR-02-08-BE | FR-02-08 | BE | 3.4 | 2 |
| FR-02-09-BE | FR-02-09 | BE | 3.4 | 2 |
| FR-02-10-BE | FR-02-10 | BE | 8.8 | 2 |
| FR-02-11-BE | FR-02-11 | BE | 3.4 | 2 |
| FR-02-12-BE | FR-02-12 | BE | 6.8 | 2 |
| FR-02-13-BE | FR-02-13 | BE | 3.4 | 2 |
| FR-02-14-BE | FR-02-14 | BE | 4.4 | 2 |
| FR-02-15-BE | FR-02-15 | BE | 6.8 | 2 |
| FR-02-16-BE | FR-02-16 | BE | 6.8 | 2 |
| FR-02-17-BE | FR-02-17 | BE | 6.8 | 2 |
| FR-02-18-BE | FR-02-18 | BE | 3.4 | 2 |
| FR-03-01-BE | FR-03-01 | BE | 3.4 | 2 |
| FR-03-04-BE | FR-03-04 | BE | 3.4 | 2 |
| FR-03-05-BE | FR-03-05 | BE | 3.4 | 2 |
| FR-03-06-BE | FR-03-06 | BE | 6.8 | 2 |
| FR-03-07-BE | FR-03-07 | BE | 6.8 | 2 |
| FR-03-08-BE | FR-03-08 | BE | 3.4 | 2 |
| FR-03-09-BE | FR-03-09 | BE | 6.8 | 2 |
| FR-03-11-BE | FR-03-11 | BE | 3.4 | 2 |
| FR-03-12-BE | FR-03-12 | BE | 3.4 | 2 |
| FR-03-13-BE | FR-03-13 | BE | 6.8 | 2 |
| FR-04-01-BE | FR-04-01 | BE | 3.4 | 2 |
| FR-04-02-BE | FR-04-02 | BE | 3.4 | 2 |
| FR-04-03-BE | FR-04-03 | BE | 3.4 | 2 |
| FR-04-04-BE | FR-04-04 | BE | 3.4 | 2 |
| FR-04-05-BE | FR-04-05 | BE | 3.4 | 2 |
| FR-05-01-BE | FR-05-01 | BE | 3.4 | 2 |
| FR-05-02-BE | FR-05-02 | BE | 3.4 | 2 |
| FR-05-03-BE | FR-05-03 | BE | 6.8 | 2 |
| FR-05-04-BE | FR-05-04 | BE | 3.4 | 2 |
| FR-05-05-BE | FR-05-05 | BE | 3.4 | 2 |
| FR-05-06-BE | FR-05-06 | BE | 6.8 | 2 |
| FR-06-02-BE | FR-06-02 | BE | 8.8 | 2 |
| FR-06-03-BE | FR-06-03 | BE | 8.8 | 2 |
| FR-06-04-BE | FR-06-04 | BE | 6.8 | 2 |
| FR-06-05-BE | FR-06-05 | BE | 3.4 | 2 |
| FR-06-06-BE | FR-06-06 | BE | 6.8 | 2 |
| FR-06-07-BE | FR-06-07 | BE | 6.8 | 2 |
| FR-06-08-BE | FR-06-08 | BE | 6.8 | 2 |
| FR-06-09-BE | FR-06-09 | BE | 3.4 | 2 |
| FR-06-12-BE | FR-06-12 | BE | 6.8 | 2 |
| FR-06-13-BE | FR-06-13 | BE | 3.4 | 2 |
| FR-06-14-BE | FR-06-14 | BE | 6.8 | 2 |
| FR-01-01-FE | FR-01-01 | FE | 6.0 | 3 |
| FR-01-03-FE | FR-01-03 | FE | 2.0 | 3 |
| FR-01-04-FE | FR-01-04 | FE | 2.0 | 3 |
| FR-01-05-FE | FR-01-05 | FE | 2.0 | 3 |
| FR-01-07-FE | FR-01-07 | FE | 6.0 | 3 |
| FR-01-09-FE | FR-01-09 | FE | 2.0 | 3 |
| FR-01-10-FE | FR-01-10 | FE | 2.0 | 3 |
| FR-01-11-FE | FR-01-11 | FE | 6.0 | 3 |
| FR-01-12-FE | FR-01-12 | FE | 2.0 | 3 |
| FR-01-14-FE | FR-01-14 | FE | 2.0 | 3 |
| FR-02-01-FE | FR-02-01 | FE | 3.0 | 3 |
| FR-02-03-FE | FR-02-03 | FE | 4.0 | 3 |
| FR-02-04-FE | FR-02-04 | FE | 2.0 | 3 |
| FR-02-08-FE | FR-02-08 | FE | 4.0 | 3 |
| FR-02-09-FE | FR-02-09 | FE | 2.0 | 3 |
| FR-02-10-FE | FR-02-10 | FE | 6.0 | 3 |
| FR-02-11-FE | FR-02-11 | FE | 2.0 | 3 |
| FR-02-13-FE | FR-02-13 | FE | 2.0 | 3 |
| FR-02-14-FE | FR-02-14 | FE | 6.0 | 3 |
| FR-02-17-FE | FR-02-17 | FE | 2.0 | 3 |
| FR-02-18-FE | FR-02-18 | FE | 2.0 | 3 |
| FR-03-01-FE | FR-03-01 | FE | 2.0 | 3 |
| FR-03-02-FE | FR-03-02 | FE | 2.0 | 3 |
| FR-03-03-FE | FR-03-03 | FE | 2.0 | 3 |
| FR-03-04-FE | FR-03-04 | FE | 4.0 | 3 |
| FR-03-05-FE | FR-03-05 | FE | 2.0 | 3 |
| FR-03-09-FE | FR-03-09 | FE | 2.0 | 3 |
| FR-03-10-FE | FR-03-10 | FE | 2.0 | 3 |
| FR-03-11-FE | FR-03-11 | FE | 2.0 | 3 |
| FR-03-13-FE | FR-03-13 | FE | 2.0 | 3 |
| FR-03-14-FE | FR-03-14 | FE | 2.0 | 3 |
| FR-04-01-FE | FR-04-01 | FE | 2.0 | 3 |
| FR-04-02-FE | FR-04-02 | FE | 2.0 | 3 |
| FR-04-03-FE | FR-04-03 | FE | 2.0 | 3 |
| FR-04-05-FE | FR-04-05 | FE | 2.0 | 3 |
| FR-05-01-FE | FR-05-01 | FE | 2.0 | 3 |
| F-05-02-FE | FR-05-02 | FE | 2.0 | 3 |
| FR-06-02-FE | FR-06-02 | FE | 3.0 | 3 |
| FR-06-03-FE | FR-06-03 | FE | 3.0 | 3 |

Must seed total: **101 tasks, 428.4 person-hours, 117.820.000 VND**.

## 4. Open questions for the team and client

| # | Question / action | Blocking? | Current IDs / evidence |
|---|---|---|---|
| 1 | Confirm the Eco Oil payload, acknowledgement and queued-retry behaviour when its API is unavailable. End-of-day batching is not assumed approved. | Yes for integration acceptance; Should scope | FR-04-10-INT; AD-UC-11 section 6; W3 D-09 |
| 2 | Approve the warehouse discrepancy tolerance. AD-UC-10 uses 5% as a test fixture; the previous W5 proposed 2%. Keep the signed amount unchanged. | Yes for final receiving acceptance | FR-04-02, FR-04-03; W3 D-08 |
| 3 | Which scope/dependency changes or additional capacity close the shortfalls above? | Yes for delivery commitment | W2 thresholds; W4 totals; W3 D-03/D-04/D-10 |
| 4 | Confirm unresolved validation, route eligibility/recovery, batch formulas, ESG report content and audit fields. | Yes for acceptance of affected tasks | W3 EARS precision review; 15 L2 / 26 L3 No flags in W1 |
| 5 | How does Received become Verified_At_Warehouse before UC-11 selects its collection inputs? | Yes for end-to-end warehouse acceptance | FR-04-05, FR-04-06; AD-UC-10 / AD-UC-11; W3 D-08 |
| 6 | Reconcile AD-UC-12 and related legacy references with the removed withdrawal scope. Keep Green Profile/ESG on current FR-05-07/08/09 and UC-13/14. | Yes before accepting conflicting diagrams as implementation instructions; no withdrawal cost added | W0/W1; AD-UC-12 withdrawal nodes; W3 D-05/D-06 |
| 7 | Confirm whether literal F-05-02-FE should be renamed FR-05-02-FE, and repair the two missing source links in W1. | No for costing; resolve before importing task IDs into implementation tooling | W1 L3 row and Sources swept; W3 D-02/D-06 |
| 8 | Confirm the EIF weight of map data in W0 before approving the FP comparison. | No for bottom-up costing | W0 Table 4; W3 D-07 |
| 9 | Can Must audit/compliance reports operate without the Should batch/mass-balance/export functions? Confirm a sufficient manual workaround or revise priorities together. | Yes for a viable Must-only backlog | FR-06-02/03; FR-04-06 through FR-04-10; W3 D-10 |
