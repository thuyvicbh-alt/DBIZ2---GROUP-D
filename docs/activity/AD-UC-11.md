# Activity Diagram: AD-UC-11 Manage UCO batch and mass-balance records

| Field | Value |
| :---- | :---- |
| AD ID | `AD-UC-11` |
| Use case | `UC-11 Manage UCO batch and mass-balance records` |
| Feature | `F-04 Warehouse & Traceability` |
| Primary actor | Warehouse Staff |
| Lanes | Warehouse Staff · ReFeed System · Eco Oil System |
| Inputs (L3-L5) | Context Diagram v1.0 · Use Case Diagram v1.0 · Sequence: `SD-UC-11` · Architecture/ADR used · W1 rows: `FR-04-02` |
| Trigger | Warehouse staff begins aggregating verified individual UCO collections into a main UCO batch or updating mass-balance records |
| Preconditions | 1\. Warehouse staff is logged into ReFeed System.<br>2\. Component collection records are in `Verified_At_Warehouse` status. |
| Success end state | A new UCO batch (`UCO Batch`) is created, linked to component collection records, mass-balance metrics are updated, and batch data is ready for sharing with Eco Oil System. |
| Failure end state(s) | Batch parameters are invalid (e.g., negative volume or duplicate inputs); the system displays an error message and prompts for parameter correction. |
| FRs covered | `FR-04-02` |
| Business rules applied | `BR-04-MASS-BALANCE` |
| Version / status | v0.1 · Draft |
| Author | Nguyen Quang Minh |

## 1\. Diagram

```mermaid
swimlane-beta LR
  subgraph WHS["Warehouse Staff"]
    start@{ shape: sm-circ }
    a1("Select verified UCO collections & enter batch parameters")
    a2("View batch creation & mass-balance summary")
    d1{"More batches to process?"}
    stop1@{ shape: fr-circ }
  end

  subgraph SYS["ReFeed System"]
    m1{" "}
    s1("Validate collection inputs & batch parameters (FR-04-02)")
    d2{"Is batch data valid?"}
    s2("Prompt invalid parameters & request correction (FR-04-02)")
    s3("Link collections & compute mass-balance metrics (FR-04-02)")
    o1["UCO Batch [Created]"]
    f1@{ shape: fork }
    s4("Maintain linked collection inputs & update batch balance (FR-04-02)")
    j1@{ shape: join }
  end

  subgraph ECO["Eco Oil System"]
    e1("Receive batch traceability & mass-balance record (FR-04-02)")
  end

  start --> a1 --> m1 --> s1 --> d2
  d2 -->|"[no]"| s2 --> a1
  d2 -->|"[yes]"| s3 --> o1 --> f1
  f1 --> s4 --> j1
  f1 --> e1 --> j1
  j1 --> a2 --> d1
  d1 -->|"[yes]"| m1
  d1 -->|"[no]"| stop1
```

&nbsp;

Reading notes:

- The Fork (`f1`) and Join (`j1`) nodes split and synchronize two parallel execution paths: updating internal inventory balances (`SYS`) and syncing batch traceability records with the external refinery partner (`ECO`).  
- The Merge node (`m1`) ensures that loopbacks from either invalid inputs (`d2 [no]`) or processing subsequent batches (`d1 [yes]`) safely merge before re-entering data validation `s1`.

## 2\. Node trace

| Node | Kind | Text | Traces to |
| :---- | :---- | :---- | :---- |
| s1 | Action | Validate collection inputs & batch parameters | FR-04-02 |
| d2 | Decision | Is batch data valid? | FR-04-02 / BR-04-MASS-BALANCE |
| s2 | Action | Prompt invalid parameters & request correction | FR-04-02 |
| s3 | Action | Link collections & compute mass-balance metrics | FR-04-02 |
| s4 | Action | Maintain linked collection inputs & update batch balance | FR-04-02 |
| e1 | Action | Receive batch traceability & mass-balance record | FR-04-02 / Context Diagram: Eco Oil System |

## 3\. Branches and acceptance tests

| Branch | Given | When | Then |
| :---- | :---- | :---- | :---- |
| d2 \[no\] | Warehouse staff selects 2 verified collections (500L total), but inputs sediment/water loss volume exceeding 100% total volume. | Warehouse staff clicks "Create Batch". | System displays an invalid parameter error s2 and prompts staff to correct input parameters a1. |
| d2 \[yes\] | Warehouse staff selects 2 verified collections (Verified\_At\_Warehouse, 500L total) and enters valid batch parameters (490L net volume, 2% loss). | Warehouse staff clicks "Create Batch". | System links records, sets batch status to Created, automatically updates inventory, and sends batch records to Eco Oil System. |
| d1 \[yes\] | Batch \#BATCH-01 is created, and verified unbatched UCO collections remain in the warehouse. | Warehouse staff selects "Process another batch". | Control flow loops back to Merge node m1 to select collections and enter parameters for the next batch. |
| d1 \[no\] | All verified collections for the shift have been consolidated into complete batches. | Warehouse staff selects "Finish shift tasks". | Activity ends at stop1. |

## 4\. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
| :---- | :---- | :---- |
| UCO Batch | Created | Created, Processing, Exported |
| Collection Record | Verified\_At\_Warehouse | Requested, Driver\_Assigned, Arrived, Completed, Verified\_At\_Warehouse |

## 5\. Screens touched

| Actor action | Screen ID |
| :---- | :---- |
| Select verified UCO collections & enter batch parameters |  |
| View batch creation & mass-balance summary |  |

## 6\. Open questions

| \# | Question | Blocking? | Default applied |
| :---- | :---- | :---- | :---- |
| 1 | \[NEEDS CLARIFICATION: What is the maximum batch volume limit per processing tank at the warehouse?\] | No | Default: System allows selecting an unlimited number of collection records per batch |
| 2 | \[NEEDS CLARIFICATION: If network connectivity to Eco Oil System is interrupted, is the batch sync queued for retry?\] | No | Default: Save batch record locally in ReFeed System first and mark as pending synchronization |

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
