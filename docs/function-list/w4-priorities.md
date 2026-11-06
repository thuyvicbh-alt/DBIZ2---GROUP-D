# W4 Priorities: ReFeed

> ReFeed Group D, K63DB, DBIZ II. 

Lesson 5 (block W4). Cut by lowering priority, never by lowering estimates. Every change names its rule.

| Rule | MoSCoW meaning | Test question |
|---|---|---|
| R1 Must | The release has no viable use without it | If it is absent on delivery day, is the release useless? |
| R2 Should | Important, but a workaround exists | Painful to leave out, yet the solution still works? |
| R3 Could | Desirable, low impact if dropped | Would anyone notice within the first month? |
| R4 Won't | Agreed out of scope for this release | Did the client accept deferring it? |

| Feature | L2 ID | L2 function name | Initial priority | Effective priority | Justification (required if changed) | Person-hours | Cost (VND) |
|---|---|---|---|---|---|---|---|
| F-01 | FR-01-01 | Register/Login as a UCO supplier | Must | Must |  | 10.8 | 2.840.000 |
| F-01 | FR-01-02 | Manage Point-of-Origin and compliance profile | Must | Must |  | 10.8 | 2.840.000 |
| F-01 | FR-01-03 | Request and schedule a UCO pickup | Must | Must |  | 10.8 | 2.840.000 |
| F-01 | FR-01-04 | Manage an existing pickup request | Must | Should | R2: Operations staff can temporarily handle reschedule or cancellation requests while the core pickup flow remains usable. | 10.8 | 2.840.000 |
| F-01 | FR-01-05 | Track pickup status | Must | Could | R3: Proactive status visibility is useful, but suppliers can confirm status through operations during Release 1. | 5.4 | 1.420.000 |
| F-02 | FR-02-01 | Plan and dispatch a UCO collection route | Must | Must |  | 26.8 | 7.640.000 |
| F-02 | FR-02-02 | View assigned route and complete a UCO collection | Must | Must |  | 10.8 | 2.840.000 |
| F-02 | FR-02-03 | Record UCO handover information | Must | Must |  | 10.8 | 2.840.000 |
| F-03 | FR-03-01 | Confirm and sign a UCO transaction | Must | Must |  | 10.8 | 2.840.000 |
| F-03 | FR-03-02 | View or download transaction receipt | Must | Must |  | 5.4 | 1.420.000 |
| F-04 | FR-04-01 | Receive and verify collected UCO at warehouse | Must | Must |  | 10.8 | 2.840.000 |
| F-04 | FR-04-02 | Manage UCO batch and mass-balance records | Must | Should | R2: Traceability is important for compliance, but Release 1 can use batch and transaction records as a manual audit path. | 17.6 | 4.880.000 |
| F-04 | FR-04-03 | Trace UCO through the supply chain | Must | Won't | R4: Deferred to Release 2; Release 1 prioritises collection, transaction, warehouse and compliance capabilities. | 17.6 | 4.880.000 |
| F-05 | FR-05-01 | View loyalty status and accumulated payout balance | Must | Must |  | 10.8 | 2.840.000 |
| F-05 | FR-05-02 | Withdraw accumulated payout balance | Must | Should | R2: Deferred to Release 2. The payment process will be handled outside of the application | 10.8 | 2.840.000 |
| F-05 | FR-05-03 | View Green Profile and UCO contribution history | Should | Could | R3: Contribution history supports supplier value but is not required to operate the collection chain. | 5.4 | 1.420.000 |
| F-05 | FR-05-04 | Access ESG / environmental contribution reports | Should | Should |  | 10.8 | 2.840.000 |
| F-06 | FR-06-01 | Monitor operations and compliance status | Must | Should | R2: Operations and compliance staff need a minimum monitoring view to run and supervise the release. | 10.8 | 2.840.000 |
| F-06 | FR-06-02 | Review audit trail and generate compliance reports | Must | Must |  | 17.6 | 4.880.000 |
| F-06 | FR-06-03 | Enforce role-based access control | Must | Must |  | 6.8 | 2.040.000 |
| F-06 | FR-06-04 | Run automated regression and acceptance tests | Must | Must |  | 6.8 | 2.040.000 |
| F-06 | FR-06-05 | Deploy and monitor the service | Must | Must |  | 9.2 | 2.760.000 |
| F-06 | FR-06-06 | Back up and recover operational records | Must | Must |  | 3.4 | 1.020.000 |
