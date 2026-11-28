# Activity Diagram: AD-UC-11 Manage UCO batch and mass-balance records

| Field | Value |
|---|---|
| AD ID | `AD-UC-11` |
| Use case | `UC-11 Manage UCO batch and mass-balance records` |
| Feature | `F-04 Warehouse & Traceability` |
| Primary actor | Warehouse Staff |
| Lanes | Warehouse Staff · ReFeed system · Eco Oil System |
| Inputs (L3-L5) | Context Diagram v1.0 · Use Case Diagram v1.0 · Sequence: `SD-UC-11` · Architecture/ADR used · W1 rows (`FR-04-06` to `FR-04-10`) |
| Trigger | Warehouse staff begins aggregating verified individual UCO collections into a UCO batch or updating mass-balance records |
| Preconditions | 1. Warehouse staff is authenticated in ReFeed system.<br>2. Component collection records are verified at warehouse (`Verified_At_Warehouse` status). |
| Success end state | A new UCO batch (`UCO Batch [Created]`) is created, linked to component collection records, mass-balance metrics and batch balances are updated (`FR-04-08`, `FR-04-09`), and traceability records are provided to Eco Oil System (`FR-04-10`). |
| Failure end state(s) | Collection inputs or batch parameters are invalid (e.g., invalid volume or duplicate inputs); the system displays invalid-parameter feedback (`FR-04-07`) and prompts for correction. |
| FRs covered | `FR-04-06`, `FR-04-07`, `FR-04-08`, `FR-04-09`, `FR-04-10` (excludes deferred `FR-04-11`) |
| Business rules applied | `BR-04-MASS-BALANCE` (W4 effective priority: `Should` for `FR-04-06`–`FR-04-10`; `Won't` for `FR-04-11`) |
| Version / status | v1.1 · Aligned with W1 (`08e751f`) |
| Author | Nguyen Quang Minh | Group D |

## 1. Diagram

```mermaid
swimlane-beta LR
  subgraph WHS["Warehouse Staff"]
    start@{ shape: sm-circ }
    a1("Select verified UCO collections & enter batch parameters")
    a2("View batch creation & mass-balance summary")
    d1{"More batches to process?"}
    stop1@{ shape: fr-circ }
  end

  subgraph SYS["ReFeed system"]
    m1{" "}
    s1("Validate collection inputs & batch parameters (FR-04-06)")
    d2{"Is batch data valid?"}
    s2("Prompt invalid parameters & request correction (FR-04-07)")
    s3("Link collections & compute mass-balance metrics (FR-04-08)")
    o1["UCO Batch [Created]"]
    f1@{ shape: fork }
    s4("Maintain linked collection inputs & update batch balance (FR-04-09)")
    j1@{ shape: join }
  end

  subgraph ECO["Eco Oil System"]
    e1("Receive batch traceability & mass-balance record (FR-04-10)")
  end

  start --> m1 --> a1 --> s1 --> d2
  d2 -->|"[no]"| s2 --> m1
  d2 -->|"[yes]"| s3 --> o1 --> f1
  f1 --> s4 --> j1
  f1 --> e1 --> j1
  j1 --> a2 --> d1
  d1 -->|"[yes]"| m1
  d1 -->|"[no]"| stop1
```

Reading notes:

- The Merge node (`m1`) is placed ahead of actor action `a1` so that the initial trigger (`start`), validation failure loopback (`s2`), and next-batch loopback (`d1 [yes]`) consolidate cleanly without giving any action two incoming flows.
- Decision (`d2`) enforces `BR-04-MASS-BALANCE`: invalid volumes or duplicate collection inputs are rejected at `s2` (`FR-04-07`), while valid inputs proceed to create `UCO Batch [Created]` and compute mass-balance metrics at `s3` (`FR-04-08`).
- Fork (`f1`) and Join (`j1`) execute internal batch balance maintenance (`s4`, `FR-04-09`) in parallel with providing the batch traceability and mass-balance record to Eco Oil System (`e1`, `FR-04-10`, representing ReFeed's export integration boundary `FR-04-10-INT`).

## 2. Node trace

| Node | Kind | Text | Traces to |
|---|---|---|---|
| s1 | Action | Validate collection inputs & batch parameters | FR-04-06 |
| d2 | Decision | Is batch data valid? | FR-04-06 / BR-04-MASS-BALANCE |
| s2 | Action | Prompt invalid parameters & request correction | FR-04-07 |
| s3 | Action | Link collections & compute mass-balance metrics | FR-04-08 |
| s4 | Action | Maintain linked collection inputs & update batch balance | FR-04-09 |
| e1 | Action | Receive batch traceability & mass-balance record | FR-04-10 (ReFeed export boundary) |

## 3. Branches and acceptance tests

| Branch | Given | When | Then |
|---|---|---|---|
| d2 [no] | Warehouse staff selects 2 collections (`500 L` total) on `S-F04-02`, but enters invalid sediment/water loss volume exceeding `100%` of total volume or includes a duplicate collection. | Warehouse staff submits batch creation (`a1`). | System rejects the invalid parameters, displays invalid-parameter feedback (`s2`, `FR-04-07`), and routes back via `m1` for correction (`a1`). |
| d2 [yes] | Warehouse staff selects 2 verified collections (`Verified_At_Warehouse`, `500 L` total) and enters valid batch parameters (`490 L` net volume, `2%` loss). | Warehouse staff submits batch creation (`a1`). | System links collections, creates `UCO Batch [Created]`, computes mass-balance metrics (`s3`, `FR-04-08`), updates internal batch balance (`s4`, `FR-04-09`) in parallel with providing the traceability record to Eco Oil System (`e1`, `FR-04-10`), and displays the summary (`a2`). |
| d1 [yes] | Batch `#BATCH-01` is created, and verified unbatched UCO collections remain in the warehouse. | Warehouse staff chooses to process another batch on `S-F04-02`. | Control flow loops back through merge node `m1` to select collections and enter parameters for the next batch (`a1`). |
| d1 [no] | All verified collections for the shift have been consolidated into batches. | Warehouse staff exits `S-F04-02` back to `S-F00-03` (Warehouse home). | Activity terminates at `stop1`. |

## 4. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
|---|---|---|
| UCO Batch | `Created` | `Created`, `Processing`, `Exported` |
| Collection Record | `Verified_At_Warehouse` | `Requested`, `Driver_Assigned`, `Arrived`, `Completed`, `Cancelled`, `Received`, `Verified_At_Warehouse` |

## 5. Screens touched

| Actor action | Screen ID |
|---|---|
| Select verified UCO collections & enter batch parameters (`a1`) | `S-F04-02` (Batch and mass-balance management; entered from `S-F00-03` Warehouse home or `S-F04-01` Warehouse receiving) |
| View batch creation & mass-balance summary (`a2`) | `S-F04-02` (Batch and mass-balance management) |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | Confirm batch parameter validation rules and maximum tank-volume constraints (`FR-04-06` is flagged `EARS precise? = No` in W1/W3). | Yes for `FR-04-06` acceptance | Allow selecting eligible verified collections without a hard upper tank cap until warehouse parameters are configured. |
| 2 | Specify the exact mass-balance metric calculation formula (`FR-04-08`) and batch-balance update formula (`FR-04-09`), both flagged `EARS precise? = No` in W1/W3. | Yes for `FR-04-08`/`09` acceptance | Compute net batch volume and loss ratio using configured mass-balance fixtures in `BR-04-MASS-BALANCE`. |
| 3 | Confirm the Eco Oil System payload schema, acknowledgement contract, and queued-retry behaviour when its API is unavailable (`FR-04-10` is flagged `EARS precise? = No`; W5 Open Question #1). | Yes for integration acceptance | Persist `UCO Batch [Created]` locally in ReFeed system first and mark external export pending retry on failure. |
| 4 | Define the state transition from `Received` (`FR-04-05` / `AD-UC-10`) to `Verified_At_Warehouse` (`FR-04-06` / `AD-UC-11`) before collections become eligible for batching (W3 Defect D-08; W5 Open Question #5). | Yes for end-to-end warehouse acceptance | Treat `Received` collections without unresolved discrepancies as eligible `Verified_At_Warehouse` inputs pending lifecycle sign-off. |
| 5 | Release 1 scope note (W4): `FR-04-06`–`FR-04-10` are classified `Should` (and `FR-04-11` is `Won't`). Confirm manual workaround or dependency closure with Must compliance reports (`FR-06-02`/`03`) per W3 D-10 & W5 Open Question #9. | Yes for viable Must-only backlog | Retain full `UC-11` decomposition in `AD-UC-11` and `SD-UC-11` for Should backlog readiness. |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders on mermaid.live | Pass | Group D | 2026-11-28 |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass | Group D | 2026-11-28 |
| No action has two incoming flows (merge used instead) | Pass | Group D | 2026-11-28 |
| Every fork has a matching join | Pass | Group D | 2026-11-28 |
| Every system action cites an FR | Pass | Group D | 2026-11-28 |
| Every FR of this use case appears in the diagram | 5 of 5 warehouse-staff FRs (`FR-04-06`–`FR-04-10`; `FR-04-11` deferred `Won't`) | Group D | 2026-11-28 |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass | Group D | 2026-11-28 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass | Group D | 2026-11-28 |
| AI critic prompt run, findings resolved or logged as open questions | Pass | Group D | 2026-11-28 |
