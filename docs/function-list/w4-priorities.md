# W4 Priorities: ReFeed

> ReFeed Group D, K63DB, DBIZ II. Updated 2026-11-27 against W0/W1 at `08e751f`.

Lesson 5 (block W4). Cut by lowering priority, never by lowering estimates. Every row uses a current W1 L2 ID and sums only its own L3 rows from [W2](w2-cost.md). Initial priority comes directly from W1. Surviving effective priorities are retained by function meaning; this revision makes no new release commitment.

Withdrawal is removed from W0/W1, rather than retained as Should or Won't. FR-05-07 now means Green Profile (previously FR-05-12); FR-05-08/09 mean ESG report generation/provision (previously FR-05-13/14). Their effective priorities follow those behaviours: Could, Should, Should. Actual bank transfers remain external under W0 and ADR-003.

| Rule | MoSCoW meaning | Test question |
|---|---|---|
| R1 Must | The release has no viable use without it | If absent on delivery day, is the release useless? |
| R2 Should | Important, but a workaround exists | Painful to leave out, yet the solution still works? |
| R3 Could | Desirable, low impact if dropped | Would anyone notice within the first month? |
| R4 Won't | Deferred from this release | Is the deferral recorded in the release boundary? |

## Effective priorities by function

| Feature | L2 ID | L2 function name | Initial priority | Effective priority | Justification (required if changed) | Person-hours | Cost (VND) |
|---|---|---|---|---|---|---|---|
| F-01 | FR-01-01 | Validate Credentials and Compliance Data | Must | Must |  | 14.8 | 3.840.000 |
| F-01 | FR-01-02 | Create Supplier Account with Unique Supplier_ID | Must | Must |  | 3.4 | 1.020.000 |
| F-01 | FR-01-03 | Display Account Already Exists & Redirect to Login | Must | Must |  | 2.0 | 400.000 |
| F-01 | FR-01-04 | Display Dashboard & Authentication Result | Must | Must |  | 2.0 | 400.000 |
| F-01 | FR-01-05 | Display Error Message & Prompt Retry | Must | Must |  | 2.0 | 400.000 |
| F-01 | FR-01-06 | Authenticate returning supplier | Must | Must |  | 6.8 | 2.040.000 |
| F-01 | FR-01-07 | Validate Point-of-Origin and compliance information | Must | Must |  | 10.4 | 2.520.000 |
| F-01 | FR-01-08 | Store Point-of-Origin and compliance profile | Must | Must |  | 3.4 | 1.020.000 |
| F-01 | FR-01-09 | Check that supplier profile is complete | Must | Must |  | 5.4 | 1.420.000 |
| F-01 | FR-01-10 | Reject request and prompt profile update | Must | Must |  | 5.4 | 1.420.000 |
| F-01 | FR-01-11 | Validate pickup location, estimated volume and time window | Must | Must |  | 10.4 | 2.520.000 |
| F-01 | FR-01-12 | Return validation feedback | Must | Must |  | 5.4 | 1.420.000 |
| F-01 | FR-01-13 | Create pickup request with status Requested and record the time window | Must | Must |  | 3.4 | 1.020.000 |
| F-01 | FR-01-14 | Return pickup request ID | Must | Must |  | 5.4 | 1.420.000 |
| F-01 | FR-01-15 | Fetch pickup request status & validation rules | Must | Should | R2: Operations staff can temporarily handle reschedule or cancellation requests while the core pickup flow remains usable. | 5.4 | 1.420.000 |
| F-01 | FR-01-16 | Display modification error: driver already dispatched | Must | Should | R2: Operations staff can temporarily handle reschedule or cancellation requests while the core pickup flow remains usable. | 2.0 | 400.000 |
| F-01 | FR-01-17 | Update pickup request status & reschedule timeline | Must | Should | R2: Operations staff can temporarily handle reschedule or cancellation requests while the core pickup flow remains usable. | 10.8 | 2.840.000 |
| F-01 | FR-01-18 | Update central pickup schedule & logs | Must | Should | R2: Operations staff can temporarily handle reschedule or cancellation requests while the core pickup flow remains usable. | 3.4 | 1.020.000 |
| F-01 | FR-01-19 | Receive dispatch schedule update alert | Must | Should | R2: Operations staff can temporarily handle reschedule or cancellation requests while the core pickup flow remains usable. | 3.4 | 1.020.000 |
| F-01 | FR-01-20 | Fetch real-time tracking & driver ETA | Must | Could | R3: Proactive status visibility is useful, but suppliers can confirm status through operations during Release 1. | 8.8 | 2.440.000 |
| F-02 | FR-02-01 | Group pending pickups within vehicle capacity | Must | Must |  | 11.8 | 3.240.000 |
| F-02 | FR-02-02 | Obtain optimized visiting order from map/route data | Must | Must |  | 11.2 | 3.360.000 |
| F-02 | FR-02-03 | Display proposed collection route | Must | Must |  | 4.0 | 800.000 |
| F-02 | FR-02-04 | Assign driver and vehicle to route | Must | Must |  | 5.4 | 1.420.000 |
| F-02 | FR-02-05 | Save and dispatch collection route | Must | Must |  | 6.8 | 2.040.000 |
| F-02 | FR-02-06 | Update included pickups to Driver Assigned | Must | Must |  | 3.4 | 1.020.000 |
| F-02 | FR-02-07 | Notify assigned driver | Must | Must |  | 3.4 | 1.020.000 |
| F-02 | FR-02-08 | Display assigned route and pickup points | Must | Must |  | 7.4 | 1.820.000 |
| F-02 | FR-02-09 | Update selected pickup status to Arrived | Must | Must |  | 5.4 | 1.420.000 |
| F-02 | FR-02-10 | Validate actual quantity, GPS, timestamp and supplied evidence | Must | Must |  | 14.8 | 3.840.000 |
| F-02 | FR-02-11 | Return validation feedback | Must | Must |  | 5.4 | 1.420.000 |
| F-02 | FR-02-12 | Save actual quantity, GPS, timestamp and supplied evidence against pickup | Must | Must |  | 6.8 | 2.040.000 |
| F-02 | FR-02-13 | Cancel current pickup and update status to Cancelled | Must | Must |  | 5.4 | 1.420.000 |
| F-02 | FR-02-14 | Validate actual quantity, handover time, and evidence | Must | Must |  | 10.4 | 2.520.000 |
| F-02 | FR-02-15 | Store handover record against assigned pickup | Must | Must |  | 6.8 | 2.040.000 |
| F-02 | FR-02-16 | Calculate transaction amount | Must | Must |  | 6.8 | 2.040.000 |
| F-02 | FR-02-17 | Generate draft agreement | Must | Must |  | 8.8 | 2.440.000 |
| F-02 | FR-02-18 | Reject and prompt for correction | Must | Must |  | 5.4 | 1.420.000 |
| F-03 | FR-03-01 | Retrieve supplier draft agreement | Must | Must |  | 5.4 | 1.420.000 |
| F-03 | FR-03-02 | Display handover and calculated amount | Must | Must |  | 2.0 | 400.000 |
| F-03 | FR-03-03 | Display no transaction awaiting signature | Must | Must |  | 2.0 | 400.000 |
| F-03 | FR-03-04 | Validate signature and confirmation | Must | Must |  | 7.4 | 1.820.000 |
| F-03 | FR-03-05 | Return signing validation feedback | Must | Must |  | 5.4 | 1.420.000 |
| F-03 | FR-03-06 | Lock agreement and calculated amount | Must | Must |  | 6.8 | 2.040.000 |
| F-03 | FR-03-07 | Store electronic signature | Must | Must |  | 6.8 | 2.040.000 |
| F-03 | FR-03-08 | Mark pickup as Completed | Must | Must |  | 3.4 | 1.020.000 |
| F-03 | FR-03-09 | Return signing failure and prompt retry | Must | Must |  | 8.8 | 2.440.000 |
| F-03 | FR-03-10 | Display signing success | Must | Must |  | 2.0 | 400.000 |
| F-03 | FR-03-11 | Check receipt ownership and signed status | Must | Must |  | 5.4 | 1.420.000 |
| F-03 | FR-03-12 | Retrieve receipt data | Must | Must |  | 3.4 | 1.020.000 |
| F-03 | FR-03-13 | Render and provide electronic receipt PDF | Must | Must |  | 8.8 | 2.440.000 |
| F-03 | FR-03-14 | Show warehouse verification status | Must | Must |  | 2.0 | 400.000 |
| F-04 | FR-04-01 | Record received quantity | Must | Must |  | 5.4 | 1.420.000 |
| F-04 | FR-04-02 | Compare received quantity with collection record | Must | Must |  | 5.4 | 1.420.000 |
| F-04 | FR-04-03 | Log discrepancy alert for internal reconciliation | Must | Must |  | 5.4 | 1.420.000 |
| F-04 | FR-04-04 | Keep signed transaction amount unchanged | Must | Must |  | 3.4 | 1.020.000 |
| F-04 | FR-04-05 | Update collection status to Received | Must | Must |  | 5.4 | 1.420.000 |
| F-04 | FR-04-06 | Validate collection inputs & batch parameters | Must | Should | R2: Traceability is important for compliance, but Release 1 can use batch and transaction records as a manual audit path. | 7.4 | 1.920.000 |
| F-04 | FR-04-07 | Prompt invalid parameters & request correction | Must | Should | R2: Traceability is important for compliance, but Release 1 can use batch and transaction records as a manual audit path. | 2.0 | 400.000 |
| F-04 | FR-04-08 | Link collections & compute mass-balance metrics | Must | Should | R2: Traceability is important for compliance, but Release 1 can use batch and transaction records as a manual audit path. | 8.8 | 2.640.000 |
| F-04 | FR-04-09 | Maintain linked collection inputs & update batch balance | Must | Should | R2: Traceability is important for compliance, but Release 1 can use batch and transaction records as a manual audit path. | 11.8 | 3.240.000 |
| F-04 | FR-04-10 | Receive batch traceability & mass-balance record | Must | Should | R2: Traceability is important for compliance, but Release 1 can use batch and transaction records as a manual audit path. | 11.2 | 3.360.000 |
| F-04 | FR-04-11 | Trace UCO through the supply chain | Must | Won't | R4: Deferred to Release 2; Release 1 prioritises collection, transaction, warehouse and compliance capabilities. | 17.6 | 4.880.000 |
| F-05 | FR-05-01 | Retrieve Loyalty Tier & Payout Balance | Must | Must |  | 5.4 | 1.420.000 |
| F-05 | FR-05-02 | Display Loyalty Tier & History Details | Must | Must |  | 5.4 | 1.420.000 |
| F-05 | FR-05-03 | Add signed transaction payment to payout balance | Must | Must |  | 6.8 | 2.040.000 |
| F-05 | FR-05-04 | Add signed transaction quantity to verified UCO volume | Must | Must |  | 3.4 | 1.020.000 |
| F-05 | FR-05-05 | Upgrade loyalty tier | Must | Must |  | 3.4 | 1.020.000 |
| F-05 | FR-05-06 | Award tier upgrade bonus | Must | Must |  | 6.8 | 2.040.000 |
| F-05 | FR-05-07 | View Green Profile and UCO contribution history | Should | Could | R3: Contribution history supports supplier value but is not required to operate the collection chain. | 5.4 | 1.420.000 |
| F-05 | FR-05-08 | Generate ESG / environmental contribution report | Should | Should |  | 8.8 | 2.640.000 |
| F-05 | FR-05-09 | Provide ESG / environmental contribution report | Should | Should |  | 2.0 | 400.000 |
| F-06 | FR-06-01 | Monitor operations and compliance status | Must | Should | R2: Staff can inspect operational records manually while the consolidated monitoring view is deferred; retain the existing Should priority. | 14.8 | 3.840.000 |
| F-06 | FR-06-02 | Retrieve and display audit history | Must | Must |  | 11.8 | 3.240.000 |
| F-06 | FR-06-03 | Generate compliance report | Must | Must |  | 11.8 | 3.240.000 |
| F-06 | FR-06-04 | Check role permission for protected action | Must | Must |  | 6.8 | 2.040.000 |
| F-06 | FR-06-05 | Record denied access attempt | Must | Must |  | 3.4 | 1.020.000 |
| F-06 | FR-06-06 | Run automated unit tests | Must | Must |  | 6.8 | 2.040.000 |
| F-06 | FR-06-07 | Run automated API tests | Must | Must |  | 6.8 | 2.040.000 |
| F-06 | FR-06-08 | Run automated acceptance tests | Must | Must |  | 6.8 | 2.040.000 |
| F-06 | FR-06-09 | Block deployment on test failure | Must | Must |  | 3.4 | 1.020.000 |
| F-06 | FR-06-10 | Deploy approved release | Must | Must |  | 9.2 | 2.760.000 |
| F-06 | FR-06-11 | Expose service monitoring signals | Must | Must |  | 9.2 | 2.760.000 |
| F-06 | FR-06-12 | Create scheduled operational and audit backup | Must | Must |  | 6.8 | 2.040.000 |
| F-06 | FR-06-13 | Record backup result | Must | Must |  | 3.4 | 1.020.000 |
| F-06 | FR-06-14 | Verify backup recovery | Must | Must |  | 6.8 | 2.040.000 |

## Priority totals

| Effective priority | L2 functions | L3 tasks | Person-hours | Cost (VND) | Share of in-scope effort |
|---|---|---|---|---|---|
| Must | 70 | 101 | 428.4 | 117.820.000 | 80.2% |
| Should | 13 | 18 | 91.8 | 25.140.000 | 17.2% |
| Could | 2 | 4 | 14.2 | 3.860.000 | 2.7% |
| Won't | 1 | 2 | 17.6 | 4.880.000 | Excluded |

## Review decisions and unresolved constraints

- W3 D-01/D-02: W2 has exactly 125 current task rows and W4 exactly 86 current function rows. W5 seeds all 101 effective-Must tasks, including the literal W1 task ID `F-05-02-FE`. Its naming inconsistency is logged for the W1 owner.
- W3 D-03/D-04: full scope needs 534.4 p-h against 360 p-h, Must is 80.2%, and Could is 2.7%. A team/client scope or capacity decision is required before a delivery commitment. Estimates are not reduced to force the thresholds to pass.
- W3 D-05: no withdrawal request, eligibility, deduction, result or banking integration is included. The obsolete withdrawal branch in AD-UC-12 is a source-reconciliation issue; it does not recreate removed W1 work. UC-13 now denotes Green Profile, UC-14 ESG, UC-15 monitoring and UC-16 audit/compliance.
- W3 D-06/D-08/D-09/D-10: remaining reference, warehouse-lifecycle, acceptance-rule and dependency gaps require their owners' decisions. Resolving a reference gap does not create a duplicate function.
- FR-04-11 remains Won't for Release 1. Its 17.6 p-h and 4.880.000 VND are visible in all-task totals and excluded from the three release scenarios.
- The Must-only seed is a costed subset for planning. W3 D-10 records that compliance outputs may need batch/traceability work currently classified Should; dependency closure remains required before calling it an executable release backlog.
