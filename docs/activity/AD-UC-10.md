# Activity Diagram: AD-UC-10 Receive and verify collected UCO at warehouse

| Field | Value |
|---|---|
| AD ID | `AD-UC-10` |
| Use case | `UC-10 Receive and verify collected UCO at warehouse` |
| Feature | `F-04 Warehouse & Traceability` |
| Primary actor | Warehouse Staff |
| Lanes | Warehouse Staff · ReFeed system |
| Inputs (L3-L5) | Context Diagram v1.0 · Use Case Diagram v1.0 · Sequence: `SD-UC-10` · Architecture/ADR used · W1 rows (FR-04-01) |
| Trigger | Collected Used Cooking Oil (UCO) batch arrives at the warehouse for receiving and verification |
| Preconditions | The UCO collection transaction has been recorded and signed in the system |
| Success end state | The received quantity is recorded, verified against collection records, and status is updated to Received while keeping signed transaction amount unchanged |
| Failure end state(s) | N/A (Discrepancies log an internal alert for reconciliation without halting receiving) |
| FRs covered | `FR-04-01` |
| Business rules applied | `BR-04-01` |
| Version / status | v0.1 · Reviewed |
| Author | Group D |

## 1. Diagram

```mermaid
swimlane-beta TB
  subgraph WHS["Warehouse Staff"]
    start@{ shape: sm-circ }
    a1("Initiate warehouse receiving for collected UCO")
    a2("Review verification and discrepancy result")
    stop1@{ shape: fr-circ }
  end
  subgraph SYS["ReFeed system"]
    s1("Record received quantity (FR-04-01)")
    s2("Compare received quantity with collection record (FR-04-01)")
    d1{" "}
    s3("Log discrepancy alert for internal reconciliation (FR-04-01)")
    m1{" "}
    s4("Keep signed transaction amount unchanged (FR-04-01)")
    s5("Update collection status to Received (FR-04-01)")
  end
  start --> a1 --> s1 --> s2 --> d1
  d1 -->|"[difference exceeds tolerance limit]"| s3 --> m1
  d1 -->|"[within tolerance limit]"| m1
  m1 --> s4 --> s5 --> a2 --> stop1
```

Reading notes:

- Discrepancy alerts trigger only when the difference between recorded collection volume and actual received volume exceeds the predefined tolerance threshold.
- The signed transaction amount is explicitly preserved unchanged regardless of receiving quantity differences to ensure financial integrity.

## 2. Node trace

| Node | Kind | Text | Traces to |
|---|---|---|---|
| s1 | Action | Record received quantity | FR-04-01 |
| s2 | Action | Compare received quantity with collection record | FR-04-01 |
| d1 | Decision | Difference exceeds tolerance limit? | FR-04-01 |
| s3 | Action | Log discrepancy alert for internal reconciliation | FR-04-01 |
| s4 | Action | Keep signed transaction amount unchanged | FR-04-01 |
| s5 | Action | Update collection status to Received | FR-04-01 |

## 3. Branches and acceptance tests

| Branch | Given | When | Then |
|---|---|---|---|
| d1 [[yes]] | Collection volume is 100L, received volume is 85L (tolerance 5%) | Received quantity is entered and verified | System logs a discrepancy alert for reconciliation, keeps original signed amount unchanged, and updates status to Received |
| d1 [[no]] | Collection volume is 100L, received volume is 99L (tolerance 5%) | Received quantity is entered and verified | System updates status to Received without logging a discrepancy alert, keeping original signed amount unchanged |

## 4. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
|---|---|---|
| Collection | `Received` | `Scheduled`, `Collected`, `Received`, `Reconciled` |

## 5. Screens touched

| Actor action | Screen ID |
|---|---|
| Initiate warehouse receiving for collected UCO | SCR-WHS-01 |
| Review verification and discrepancy result | SCR-WHS-02 |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| - | None | No | All conditions and rules fully covered by FR-04-01. |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders on mermaid.live | Passed | Group D | 2026-10-27 |
| Every decision has guards on all outgoing flows, no overlap, no gap | Passed | Group D | 2026-10-27 |
| No action has two incoming flows (merge used instead) | Passed | Group D | 2026-10-27 |
| Every fork has a matching join | Passed | Group D | 2026-10-27 |
| Every system action cites an FR | Passed | Group D | 2026-10-27 |
| Every FR of this use case appears in the diagram | 1 of 1 | Group D | 2026-10-27 |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Passed | Group D | 2026-10-27 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Passed | Group D | 2026-10-27 |
| AI critic prompt run, findings resolved or logged as open questions | Passed | Group D | 2026-10-27 |
