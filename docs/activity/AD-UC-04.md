# Activity Diagram: AD-UC-04 Manage and track an existing pickup status

| Field | Value |
| :---- | :---- |
| AD ID | `AD-UC-04` |
| Use case | `UC-04 Manage and track an existing pickup status` |
| Feature | `F-01 Pickup Request & Scheduling` |
| Primary actor | UCO Suppliers |
| Lanes | UCO Suppliers · ReFeed System · UCO Collectors |
| Inputs (L3-L5) | Context Diagram v1.0 · Use Case Diagram v1.0 · Sequence: `SD-UC-04` · Architecture/ADR used · W1 rows: `FR-01-04` |
| Trigger | UCO supplier opens the application to view real-time status, modify, or cancel an existing pickup request |
| Preconditions | 1\. Supplier is authenticated in ReFeed System.<br>2\. At least one pickup request exists for the supplier. |
| Success end state | Pickup status/location is displayed, or the request is successfully rescheduled/cancelled and synchronized with collector dispatch schedules. |
| Failure end state(s) | Reschedule/cancellation is rejected because the pickup is already dispatched or in transit; system displays lock error. |
| FRs covered | `FR-01-04` |
| Business rules applied | `BR-01-PICKUP-CANCELLATION` |
| Version / status | v1.0 · Verified |
| Author | Group 4 |

## 1\. Diagram

```mermaid
swimlane-beta LR
  subgraph SUP["UCO Suppliers"]
    start@{ shape: sm-circ }
    a1("Select existing pickup request & choose action")
    a2("View pickup status & confirmation summary")
    d3{"More requests to manage?"}
    stop1@{ shape: fr-circ }
  end

  subgraph SYS["ReFeed System"]
    m1{" "}
    s1("Fetch pickup request status & validation rules (FR-01-04)")
    d1{"Select action?"}
    s2("Fetch real-time tracking & driver ETA (FR-01-04)")
    d2{"Is request modifiable?"}
    s3("Display modification error: driver already dispatched (FR-01-04)")
    s4("Update pickup request status & reschedule timeline (FR-01-04)")
    o1["Pickup Request [Updated]"]
    f1@{ shape: fork }
    s5("Update central pickup schedule & logs (FR-01-04)")
    j1@{ shape: join }
    m2{" "}
  end

  subgraph COL["UCO Collectors"]
    c1("Receive dispatch schedule update alert (FR-01-04)")
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

&nbsp;

Reading notes:

- The Merge node (`m1`) consolidates the initial trigger (`start`) and the loopback branch (`d3 [YES]`) to ensure node `a1` receives only one incoming control flow.  
- Decision (`d2`) enforces business rule `BR-01-PICKUP-CANCELLATION`: requests can only be modified or cancelled if status is `Requested`. Once status reaches `Driver_Assigned` or higher, modification is locked and routed to error display (`s3`).  
- Fork (`f1`) and Join (`j1`) synchronize internal schedule updates (`SYS`) with collector fleet alerts (`COL`).

## 2\. Node trace

| Node | Kind | Text | Traces to |
| :---- | :---- | :---- | :---- |
| s1 | Action | Fetch pickup request status & validation rules | FR-01-04 |
| d1 | Decision | Select action? | FR-01-04 |
| s2 | Action | Fetch real-time tracking & driver ETA | FR-01-04 |
| d2 | Decision | Is request modifiable? | FR-01-04 |
| s3 | Action | Display modification error: driver already dispatched | FR-01-04 |
| s4 | Action | Update pickup request status & reschedule timeline | FR-01-04 |
| s5 | Action | Update central pickup schedule & logs | FR-01-04 |
| c1 | Action | Receive dispatch schedule update alert | FR-01-04 |

## 3\. Branches and acceptance tests

| Branch | Given | When | Then |
| :---- | :---- | :---- | :---- |
| d1 \[view status\] | Pickup request \#REQ-101 is in `Driver_Assigned` status. | Supplier selects "Track Status". | System retrieves driver location and estimated arrival time s2, and displays tracking summary a2. |
| d2 \[no / driver dispatched\] | Pickup request \#REQ-101 is in `Driver_Assigned` status (driver en route). | Supplier selects "Cancel Request". | System rejects cancellation, displays error message s3 explaining driver is already dispatched, and keeps original schedule. |
| d2 \[yes / status requested\] | Pickup request \#REQ-102 is in `Requested` status (no driver assigned yet). | Supplier selects "Reschedule Time Window". | System updates request schedule s4, sets status to `Updated`, updates central calendar s5, alerts collector fleet c1, and shows success confirmation a2. |
| d3 \[YES\] | Supplier has managed request \#REQ-101 and has another pending request \#REQ-102. | Supplier clicks "Manage another request". | Control flow loops back to Merge node m1 to select request a1. |
| d3 \[NO\] | Supplier finishes checking/modifying all pickup requests. | Supplier exits tracking dashboard. | Activity completes at stop1. |

## 4\. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
| :---- | :---- | :---- |
| Pickup Request | Updated | Requested, Driver\_Assigned, Arrived, Completed, Cancelled, Updated |

## 5\. Screens touched

| Actor action | Screen ID |
| :---- | :---- |
| Select existing pickup request & choose action |  |
| View pickup status & confirmation summary |  |

## 6\. Open questions

| \# | Question | Blocking? | Default applied |
| :---- | :---- | :---- | :---- |
| 1 | \[NEEDS CLARIFICATION: What is the cutoff time window before pickup for a supplier to cancel without penalty?\] | No | Default: Cancellation allowed up until driver dispatch (`Status == Requested`) |
| 2 | \[NEEDS CLARIFICATION: Are push notifications sent to both driver and regional dispatcher upon cancellation?\] | No | Default: Broadcast schedule update event to collector dispatch system |

## 7\. Verification log

| Check | Result | By | Date |
| :---- | :---- | :---- | :---- |
| Renders on mermaid.live | Pass | Group 4 | 2026-10-30 |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass | Group 4 | 2026-10-30 |
| No action has two incoming flows (merge used instead) | Pass | Group 4 | 2026-10-30 |
| Every fork has a matching join | Pass | Group 4 | 2026-10-30 |
| Every system action cites an FR | Pass | Group 4 | 2026-10-30 |
| Every FR of this use case appears in the diagram | 1 of 1 | Group 4 | 2026-10-30 |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass | Group 4 | 2026-10-30 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass | Group 4 | 2026-10-30 |
| AI critic prompt run, findings resolved or logged as open questions | Pass | Group 4 | 2026-10-30 |

&nbsp;
