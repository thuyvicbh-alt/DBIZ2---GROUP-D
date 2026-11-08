# Activity Diagram Index: ReFeed

Lesson 7. The inventory follows ReFeed Cost Workshop `W0_Scope` Table 1. Priorities are effective W4 priorities. Where a use case covers more than one FR, the table shows the relevant split. File status reflects the contents of this repository.

## Inputs check (Block 1: Lessons 3 and 4)

| # | Finding | Source to fix | Action | Owner / follow-up | Status |
|---|---|---|---|---|---|
| 1 | The project sources use different labels for the UC-05 dispatch role. The Screen List calls it `ReFeed Staff`. | W0, Context Diagram, Screen List | Use `ReFeed Staff` throughout this submission. Align the upstream actor labels and confirm whether `ReFeed Staff` and compliance staff have distinct permissions. | Context and use-case owners | Submission aligned · source alignment open |
| 2 | W1 cites the shared `SEQ:S-01` for several use cases. UC-05 needed its own sequence. | W1, sequence index | Add [SD-UC-05](../use-case/SD-UC-05.md) and move the staff confirmation decision to [AD-UC-05](AD-UC-05.md). Update the legacy W1 trace when the team reconciles all sequence IDs. | Đức Minh (UC-05) · W1 owner (trace) | UC-05 split done · source trace open |
| 3 | W1 `FR-02-01-INT` and SD-UC-05 use Google Maps API. Context v2.1 lists four actors and does not show that service. | [Context Diagram](../context/context-diagram.md), physical design | Confirm the course-required treatment of this integration. Keep it explicit in SD-UC-05 and within the system responsibility in AD-UC-05. | Context and architecture owners | Open |
| 4 | The repository Use Case Diagram and architecture still describe LabDesk. | [Use Case Diagram](../use-case/use-case-diagram.md), [Architecture](../architecture/architecture.md) | Replace the templates with the ReFeed versions and review physical lifelines and actor mappings against them. The ReFeed Cost Workshop and Context Diagram supply the current inventory and boundary. | Use-case and architecture owners | Open |
| 5 | UC-07's handover row in repository W1 L2 repeats `FR-03-01` from UC-08. The live workbook W1 and W4 refer to handover as `FR-02-03`. | [W1](../function-list/w1-function-list.md), W4 | Align the handover L2 ID to `FR-02-03` and confirm the boundary between UC-06 collection capture and UC-07 handover agreement. | W1 and F-02 owners | Open |
| 6 | W0 requests diagrams for UC-02 and UC-08, but neither file exists. The Usage Flow omits diagrams for both. | W0 and [Usage Flow](../external-design/usage-flow.md) | Add the two diagrams and reconcile the activity references. Confirm the UC-01 W0 flag against its reviewed diagram. | F-01 and F-03 owners | Open |
| 7 | UC-05 uses `S-F02-01` and `S-F02-02` in the Screen List and Screen Transition. The Usage Flow still uses `SCR-ADM-01`. | Usage Flow | Align the UC-05 screen references with the current Screen List. | External-design owner | Open |
| 8 | UC-12 activity documentation cites withdrawal `FR-05-02` as well as `FR-05-01`. W0/W1 assign withdrawals to UC-13. W4 marks `FR-05-02` Should while its rationale refers to Release 2. | AD-UC-12, W4 and scope | Keep a separate row for each use case. Resolve the Release 1 scope of withdrawal tracking before sign-off. | F-05 and W4 owners | Open |
| 9 | Cost Workshop W1 row 19 repeats the UC-05 criterion on a UC-04 tracking work item. UC-05 uses the detailed criterion in rows 21–22. | W1 workbook | Align the tracking criterion with the UC-04 requirement and the repository W1 wording. | W1 owner | Open |

## Use cases

A Must use case needs a diagram if it has a decision, crosses lanes, or starts on a timer. A single read or report request can be listed as **Not needed** when the current requirements describe a single interaction without a business branch. W0 also selects several Should or mixed-priority flows for documentation. A listed placeholder is an empty file. Each pending diagram goes to one author, confirmed by its feature owner.

| Use case | Name | Feature | Priority | Activity diagram | Status | Rationale |
|---|---|---|---|---|---|---|
| UC-01 | Register/Login as a UCO supplier | F-01 | Must | [AD-UC-01](AD-UC-01.md) | Reviewed | Keep the existing registration and returning-login branches. W0's Not needed flag is open in row 6. |
| UC-02 | Manage Point-of-Origin and compliance profile | F-01 | Must | `AD-UC-02.md` | Missing | The supplier submits or updates data and ReFeed validates it before storing it. W0 requests a diagram. |
| UC-03 | Request and schedule a UCO pickup | F-01 | Must | [AD-UC-03](AD-UC-03.md) | Draft | Profile complete or incomplete determines whether ReFeed creates the request. |
| UC-04 | Manage and track an existing pickup status | F-01 | Should / Could | [AD-UC-04](AD-UC-04.md) | Placeholder | W0 requests the diagram. Change or cancellation requires an eligible pickup status. |
| UC-05 | Plan and dispatch a UCO collection route | F-02 | Must | [AD-UC-05](AD-UC-05.md) | Draft for group review | ReFeed proposes a capacity-constrained route. Staff confirmation gates dispatch. |
| UC-06 | Check scheduled collection route and Confirm Delivery to supplier points | F-02 | Must | [AD-UC-06](AD-UC-06.md) | Draft | Readiness and validation branches plus progression to the next pickup. |
| UC-07 | Input Handover Information | F-02 | Must | [AD-UC-07](AD-UC-07.md) | Placeholder | Driver submits handover details for validation, amount calculation and draft agreement generation. |
| UC-08 | Confirm and sign a UCO transaction | F-03 | Must | `AD-UC-08.md` | Missing | W0 requests a diagram for supplier review, signing, locking the agreement and marking the pickup Completed. |
| UC-09 | View or download transaction receipt | F-03 | Must | Not needed | — | One receipt retrieval. Ownership and signed-status checks are interaction conditions. |
| UC-10 | Receive and verify collected UCO at warehouse | F-04 | Must | [AD-UC-10](AD-UC-10.md) | Reviewed | The tolerance check can trigger a discrepancy alert. |
| UC-11 | Manage UCO batch and mass-balance records | F-04 | Should / Won't | [AD-UC-11](AD-UC-11.md) | Placeholder | W0 requests the batch-maintenance diagram. End-to-end trace query is deferred under `FR-04-03`. |
| UC-12 | Manage loyalty status and accumulated payout balance | F-05 | Must | [AD-UC-12](AD-UC-12.md) | Reviewed · scope check open | Signed transactions update balance and volume, with a conditional tier upgrade. Withdrawal scope is tracked in row 8 above. |
| UC-13 | Withdraw accumulated payout balance | F-05 | Should | [AD-UC-13](AD-UC-13.md) | Draft · release scope open | W0 requests the diagram for withdrawal eligibility and tracking. Transfer execution is external. |
| UC-14 | View Green Profile and UCO contribution history | F-05 | Could | Not needed | — | Read-only contribution history. |
| UC-15 | Access ESG / environmental contribution reports | F-05 | Should | Not needed | — | One report request from verified collection data. |
| UC-16 | Monitor operations, compliance status, trace UCO through supply chain | F-06 | Should for monitoring | Not needed | — | Monitoring is a query. W1 traces the separate deferred end-to-end trace FR to UC-11. |
| UC-17 | Review audit trail and generate compliance reports | F-06 | Must | Not needed | — | W0 treats this as read/report. W1 describes access and retrieval without a separate business workflow. |

## Object states used

Use the cited spelling when reconciling the DBIZ3 data model.

| Object | Known state values and source |
|---|---|
| Pickup request | `Requested` (W1 `FR-01-03`), `Driver Assigned` (W1 `FR-02-01-BE`), `Arrived` and `Cancelled` (W1 `FR-02-02`), `Completed` (W1 `FR-03-01` for UC-08). Check the full status catalogue when UC-04 is written. |
| Collection route | Proposed, saved and dispatched describe stages of UC-05. The route-status vocabulary remains open. |
| Other object states | Use the object-state tables in the linked diagrams to reconcile terminology with W1 before adopting a shared state model. |
