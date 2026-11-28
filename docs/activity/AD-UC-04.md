# Activity Diagram: AD-UC-04 Manage and track an existing pickup status

| Field | Value |
|---|---|
| AD ID | `AD-UC-04` |
| Use case | `UC-04 Manage and track an existing pickup status` |
| Feature | `F-01 Supplier Onboarding & Pickup Management` |
| Primary actor | Supplier |
| Lanes | Supplier · ReFeed system · UCO Collectors |
| Inputs (L3-L5) | Context Diagram v1.0 · Use Case Diagram v1.0 · Sequence: `SD-UC-04` · Architecture/ADR used · W1 rows (`FR-01-15` to `FR-01-20`) |
| Trigger | Supplier opens the application to view real-time status/ETA, reschedule, or cancel an existing pickup request |
| Preconditions | 1. Supplier is authenticated in ReFeed system.<br>2. At least one pickup request exists for the signed-in supplier. |
| Success end state | Current pickup status, scheduled time, assigned driver/vehicle (when available) and last-update timestamp are displayed, or an eligible `Requested` pickup is rescheduled/cancelled and synchronized with the central schedule and collector dispatch alerts. |
| Failure end state(s) | Reschedule/cancellation is rejected because the driver is already dispatched; system displays the modification-lock error and retains the existing request and schedule. |
| FRs covered | `FR-01-15`, `FR-01-16`, `FR-01-17`, `FR-01-18`, `FR-01-19`, `FR-01-20` |
| Business rules applied | `BR-01-PICKUP-CANCELLATION` (W4 effective priority: `Should` for `FR-01-15`–`FR-01-19`; `Could` for `FR-01-20`) |
| Version / status | v1.1 · Aligned with W1 (`08e751f`) |
| Author | Nguyen Quang Minh | Group D |

## 1. Diagram

```mermaid
swimlane-beta LR
  subgraph SUP["Supplier"]
    start@{ shape: sm-circ }
    a1("Select existing pickup request & choose action")
    a2("View pickup status & confirmation summary")
    d3{"More requests to manage?"}
    stop1@{ shape: fr-circ }
  end

  subgraph SYS["ReFeed system"]
    m1{" "}
    s1("Fetch pickup request status & validation rules (FR-01-15)")
    d1{"Select action?"}
    s2("Fetch real-time tracking & driver ETA (FR-01-20)")
    d2{"Is request modifiable?"}
    s3("Display modification error: driver already dispatched (FR-01-16)")
    s4("Update pickup request status & reschedule timeline (FR-01-17)")
    o1["Pickup Request [Updated / Cancelled]"]
    f1@{ shape: fork }
    s5("Update central pickup schedule & logs (FR-01-18)")
    j1@{ shape: join }
    m2{" "}
  end

  subgraph COL["UCO Collectors"]
    c1("Receive dispatch schedule update alert (FR-01-19)")
  end

  start --> m1 --> a1 --> s1 --> d1
  d1 -->|"[view status]"| s2 --> m2
  d1 -->|"[modify or cancel]"| d2
  d2 -->|"[no / driver dispatched]"| s3 --> m2
  d2 -->|"[yes / status requested]"| s4 --> o1 --> f1
  f1 --> s5 --> j1
  f1 --> c1 --> j1
  j1 --> m2
  m2 --> a2 --> d3
  d3 -->|"[YES]"| m1
  d3 -->|"[NO]"| stop1
```

Reading notes:

- The Merge node (`m1`) consolidates the initial trigger (`start`) and the loopback branch (`d3 [YES]`) so that actor action `a1` receives a single incoming control flow.
- Decision (`d2`) enforces `BR-01-PICKUP-CANCELLATION`: modifications or cancellations are permitted only while pickup status is `Requested` (`s4`). Once a driver is dispatched (`Driver_Assigned` or later), changes are rejected and routed to the lock error display (`s3`), retaining the existing schedule.
- Fork (`f1`) and Join (`j1`) execute the central schedule/log update (`s5`, `FR-01-18`) in parallel with sending the dispatch-schedule update alert to UCO Collectors (`c1`, `FR-01-19`).

## 2. Node trace

| Node | Kind | Text | Traces to |
|---|---|---|---|
| s1 | Action | Fetch pickup request status & validation rules | FR-01-15 |
| d1 | Decision | Select action? | FR-01-15 |
| s2 | Action | Fetch real-time tracking & driver ETA | FR-01-20 |
| d2 | Decision | Is request modifiable? | FR-01-15 |
| s3 | Action | Display modification error: driver already dispatched | FR-01-16 |
| s4 | Action | Update pickup request status & reschedule timeline | FR-01-17 |
| s5 | Action | Update central pickup schedule & logs | FR-01-18 |
| c1 | Action | Receive dispatch schedule update alert | FR-01-19 |

## 3. Branches and acceptance tests

| Branch | Given | When | Then |
|---|---|---|---|
| d1 [view status] | Pickup request `#REQ-101` is in `Driver_Assigned` status. | Supplier selects "View status" on `S-F01-05`. | System retrieves tracking, driver ETA, scheduled collection time, assigned driver/vehicle, and last-update timestamp (`s2`, `FR-01-20`) and displays them (`a2`). |
| d2 [no / driver dispatched] | Pickup request `#REQ-101` has an assigned driver already dispatched (`Driver_Assigned`). | Supplier attempts to reschedule or cancel the request. | System blocks modification, displays the modification-lock error (`s3`, `FR-01-16`), and retains the existing request and schedule unchanged. |
| d2 [yes / status requested] | Pickup request `#REQ-102` is in `Requested` status (not yet dispatched). | Supplier submits a rescheduled time window (`S-F01-03`) or confirms cancellation modal (`S-F01-05`). | System updates the pickup status and appointment timeline (`s4`, `FR-01-17`), updates the central pickup schedule and logs (`s5`, `FR-01-18`) in parallel with sending a dispatch update alert to UCO Collectors (`c1`, `FR-01-19`), and displays confirmation (`a2`). |
| d3 [YES] | Supplier has finished viewing or updating `#REQ-101` and wants to check `#REQ-102`. | Supplier returns to `S-F01-04` to manage another request. | Control flow loops back through merge node `m1` to select another request (`a1`). |
| d3 [NO] | Supplier has finished managing all pickup requests. | Supplier navigates back to `S-F00-02` (Supplier home). | Activity terminates at `stop1`. |

## 4. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
|---|---|---|
| Pickup Request | `Updated`, `Cancelled` (checked against `Requested`, `Driver_Assigned`) | `Requested`, `Driver_Assigned`, `Arrived`, `Completed`, `Cancelled`, `Updated` |

## 5. Screens touched

| Actor action | Screen ID |
|---|---|
| Select existing pickup request & choose action (`a1`) | `S-F01-04` (My pickup requests) · `S-F01-05` (Pickup request detail, including modal *Cancel this pickup request?*) · `S-F01-03` (Pickup request form — Reschedule mode) |
| View pickup status & confirmation summary (`a2`) | `S-F01-05` (Pickup request detail) |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | What is the minimum cutoff time window before a scheduled pickup for a supplier to cancel without penalty? | No | Cancellation is permitted while pickup status remains `Requested` (before driver dispatch). |
| 2 | Should dispatch-schedule update alerts (`FR-01-19`) be pushed directly to the assigned collector's device as well as the central dispatch queue? | No | Broadcast dispatch-schedule update alert to UCO Collectors (`c1`) in parallel with central schedule logging (`s5`). |
| 3 | Release 1 scope note (W4): `FR-01-15`–`FR-01-19` are classified `Should` and `FR-01-20` is classified `Could` (operations staff can handle reschedules/cancellations manually in Release 1). | No for Must baseline | Retain full UC-04 decomposition in `AD-UC-04` and `SD-UC-04` for Should/Could backlog readiness. |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders on mermaid.live | Pass | Group D | 2026-11-28 |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass | Group D | 2026-11-28 |
| No action has two incoming flows (merge used instead) | Pass | Group D | 2026-11-28 |
| Every fork has a matching join | Pass | Group D | 2026-11-28 |
| Every system action cites an FR | Pass | Group D | 2026-11-28 |
| Every FR of this use case appears in the diagram | 6 of 6 | Group D | 2026-11-28 |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass | Group D | 2026-11-28 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass | Group D | 2026-11-28 |
| AI critic prompt run, findings resolved or logged as open questions | Pass | Group D | 2026-11-28 |
