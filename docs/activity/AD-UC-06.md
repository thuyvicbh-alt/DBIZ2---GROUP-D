# Activity Diagram: AD-UC-06 Check Scheduled Collection Route and Confirm Delivery to Supplier Points

| Field | Value |
|---|---|
| AD ID | AD-UC-06 |
| Use case | UC-06 Check scheduled collection route and Confirm Delivery to supplier points |
| Feature | F-02 Collection Operations & Handover |
| Primary actor | Driver / Collector (W0; Context Diagram label: `UCO Collector / Driver`) |
| Lanes | UCO Collector / Driver · ReFeed System |
| Inputs (L3-L5) | Current Context Diagram · Use Case Diagram containing UC-06 · Sequence `S-01` (the sequence reference in W1; confirm whether a UC-specific sequence ID is required) · W1 rows `FR-02-02` (`FR-02-02-FE`, `FR-02-02-BE`) |
| Trigger | The driver/collector opens an assigned collection route. |
| Preconditions | A collection route has been assigned to the driver/collector and contains one or more pickup points. |
| Success end state | For each assigned pickup point, the driver confirms arrival and checks whether the UCO is ready. If ready, valid collection details are saved. If not ready, the current pickup is cancelled. The activity ends after all assigned pickup points have been processed. |
| Failure end state(s) | If required collection data is missing or invalid, the system returns validation feedback so the driver/collector can correct and resubmit it. The `Cancelled` pickup status and the `UCO ready?` rule are proposed behavior and must be confirmed and traced in W1 before final sign-off. |
| FRs covered | `FR-02-02` |
| Business rules applied | Proposed rule: after arrival is confirmed, check whether UCO is ready at the selected supplier point. If not ready, cancel that pickup point and continue to the next assigned point, if any. |
| Version / status | v0.1 · Draft — readiness/cancellation rule pending requirements traceability |
| Author | ReFeed Group D |

## 1. Diagram

The activity uses two lanes from the project context: `UCO Collector / Driver` and `ReFeed System`. After the system updates the selected pickup to `Arrived`, the driver checks whether UCO is ready. If it is ready, the driver enters and submits the collection details; the system validates and saves them. If UCO is not ready, the system cancels the current pickup. After either a successful save or cancellation, the flow checks whether more assigned pickup points remain. Invalid collection details return to the driver for correction and resubmission.

```mermaid
swimlane-beta TB
  subgraph ACT["UCO Collector / Driver"]
    start@{ shape: sm-circ }
    a1("Open assigned collection route")
    m2{" "}
    a2("Select supplier point and confirm arrival")
    d0{" "}
    m1{" "}
    a3("Enter or correct actual UCO quantity and optional evidence")
    a4("Submit collection details")
  end

  subgraph SYS["ReFeed System"]
    s1("Display assigned route and pickup points (FR-02-02)")
    s2("Update selected pickup status to Arrived (FR-02-02)")
    o1["Pickup [Arrived]"]
    s3("Validate actual quantity, GPS, timestamp and supplied evidence (FR-02-02)")
    d1{" "}
    s4("Return validation feedback (FR-02-02)")
    s5("Save actual quantity, GPS, timestamp and supplied evidence against pickup (FR-02-02)")
    s6("Cancel current pickup and update status to Cancelled")
    d2{" "}
    stop1@{ shape: fr-circ }
  end

  start --> a1 --> s1 --> m2 --> a2 --> s2 --> o1
  o1 -->|UCO ready?| d0
  d0 -->|"[Yes: UCO ready]"| m1
  m1 --> a3 --> a4 --> s3
  s3 -->|Required collection data valid?| d1

  d0 -->|"[No: UCO not ready]"| s6
  s6 -->|More assigned pickup points remain?| d2

  d1 -->|"[Yes: valid]"| s5
  s5 --> d2
  d1 -->|"[No: missing/invalid required data]"| s4 --> m1

  d2 -->|"[Yes: more pickups]"| m2
  d2 -->|"[No: no remaining pickups]"| stop1
```

### Reading notes

- `d0` is the readiness decision. **Yes** routes to collection-detail capture; **No** cancels the current pickup and then proceeds to the check for remaining pickup points.
- `d1` checks the submitted collection details. If required data is missing or invalid, the system returns feedback and the driver/collector corrects the details through `m1` before resubmitting.
- `d2` is reached after either a valid collection record is saved or the current pickup is cancelled. If another assigned pickup point remains, the flow returns to `m2`; otherwise, the activity ends.
- `m1` merges the initial detail-capture path and the correction path. `m2` merges the first pickup point and each subsequent pickup point. These are merge nodes, not decisions.
- `FR-02-02` specifies collection-detail validation and persistence.
- `UC-07` / `FR-02-03` also mentions actual quantity and evidence, and additionally calculates the transaction amount and generates a draft handover agreement. Those latter functions remain out of scope for this UC-06 diagram.

## 2. Node trace

| Node | Kind | Text | Traces to |
|---|---|---|---|
| `s1` | Action | Display assigned route and pickup points | `FR-02-02` |
| `a2` | Actor action | Select supplier point and confirm arrival | UC-06 / `FR-02-02` flow |
| `s2` | Action | Update selected pickup status to `Arrived` | `FR-02-02` |
| `o1` | Object node | Pickup `[Arrived]` | `FR-02-02` (state value) |
| `d0` | Decision | UCO ready? | `FR-02-02` |
| `s6` | Action | Cancel current pickup and update status to `Cancelled` | `FR-02-02` |
| `a3` | Actor action | Enter or correct actual UCO quantity and optional evidence | `FR-02-02` |
| `a4` | Actor action | Submit collection details | `FR-02-02` |
| `s3` | Action | Validate actual quantity, GPS, timestamp and supplied evidence | `FR-02-02` |
| `d1` | Decision | Required collection data valid? | `FR-02-02` (validation) |
| `s4` | Action | Return validation feedback | `FR-02-02` (validation; feedback behavior is a draft default) |
| `s5` | Action | Save actual quantity, GPS, timestamp and supplied evidence against pickup | `FR-02-02` |
| `d2` | Decision | More assigned pickup points remain? | Loop behavior inferred from the assigned route; confirm that one UC invocation processes all assigned pickup points |
| `m1` | Merge | Continue to detail capture or retry after validation feedback | Control-flow merge; no separate FR |
| `m2` | Merge | Process the first or a subsequent pickup point | Control-flow merge; no separate FR |
| `stop1` | Activity final | All assigned pickup points have been processed | UC-06 activity end; no route-completed status is asserted |

## 3. Branches and acceptance tests

| Branch | Given | When | Then |
|---|---|---|---|
| `d0` `[Yes: UCO ready]` | Driver D018 has confirmed arrival at pickup PK-00125 (supplier SUP-00125, 12 Hàng Bài); PK-00125 is `Arrived`; the supplier has UCO ready for handover. | The driver/collector confirms that UCO is ready. | The flow continues to collection-detail entry for PK-00125; no status changes yet. |
| `d0` `[No: UCO not ready]` | Driver D018 has confirmed arrival at pickup PK-00126 (supplier SUP-00126) at 10:40 on 25/09/2026; PK-00126 is `Arrived`; the shop is closed. | The driver/collector indicates that UCO is not ready. | PK-00126 becomes `Cancelled` (proposed status); no quantity, GPS or evidence is saved for PK-00126; the flow continues to `d2`. |
| `d1` `[Yes: valid]` | PK-00125 is `Arrived`; the device reports a GPS position; the timestamp is 10:24 on 25/09/2026. | The driver/collector submits an actual quantity of 32.5 kg with photo IMG-001 and no notes. | The system records 32.5 kg, the GPS position, timestamp 10:24 25/09/2026 and photo IMG-001 against PK-00125. |
| `d1` `[No: missing/invalid required data]` | PK-00125 is `Arrived`. | The driver/collector submits an actual quantity of 0 kg (or leaves it blank), or the device returns no GPS position. | Nothing is saved for PK-00125; the system returns validation feedback naming the invalid field; PK-00125 stays `Arrived` and the driver/collector can correct and resubmit. |
| `d2` `[Yes: more pickups]` | Route R-001 assigned to D018 has 5 pickup points; PK-00125 is the 2nd and has just been saved (or cancelled). | The flow checks for remaining assigned pickup points. | The activity returns to `m2` and the driver/collector can select the 3rd pickup point. |
| `d2` `[No: no remaining pickups]` | PK-00129 is the 5th and last pickup point on route R-001 and has just been saved (or cancelled). | The flow checks for remaining assigned pickup points. | The activity ends; no pickup point on route R-001 is still `Driver Assigned`. No separate route-completed status is asserted. |

## 4. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
|---|---|---|
| Pickup | `Arrived`; `Cancelled` | Known from the current W1 requirements: `Requested`, `Driver Assigned`, `Arrived`. |

The state spelling `Arrived`; `Cancelled` is taken from the W1 `FR-02-02` requirement. 

## 5. Screens touched

| Actor action | Screen ID |
|---|---|
| Open assigned collection route; select supplier point and confirm arrival; confirm UCO readiness; enter/correct collection details; submit details | To be assigned from the Screen List |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | W0 names the actor `Driver / Collector`, while the Context Diagram uses `UCO Collector / Driver`. Which exact label should be used across W0, the Use Case Diagram, Context Diagram and all activity diagrams? | Yes for final cross-diagram consistency | The lane uses the Context Diagram label; the W0 name is recorded in the metadata. |
| 2 | `UC-07` / `FR-02-03` overlaps `UC-06` / `FR-02-02` on actual quantity and collection evidence. Are these separate use cases, or should UC-07 be merged into UC-06 after W0/W1 are revised? | Yes for final scope and traceability review | This draft covers `FR-02-02`. Transaction-amount calculation and draft handover-agreement generation remain out of scope here. |
| 3 | What are the exact validation rules for actual quantity, GPS and timestamp, and what should happen if GPS is unavailable or the device is offline? | Yes for detailed acceptance tests | Required data are treated as actual quantity, GPS and timestamp; optional photo/notes may be absent. Invalid data return to correction; offline behavior is not defined. |
| 4 | Does UC-06 process every pickup point in one route invocation, or is each pickup point a separate invocation? | No for this draft | The diagram loops through all pickup points displayed for the assigned route. It does not invent a route-completed status. |
| 5 | W1 references sequence `S-01` for UC-06. Should the team create or identify a UC-specific sequence diagram such as `SD-UC-06`? | No for this draft; resolve before integrated review | Keep `S-01` as the workbook's current source trace until the integrator confirms the sequence naming. |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders on mermaid.live (Mermaid 11.16+ for `swimlane-beta`) | Not yet verified | | |
| Every decision has guards on all outgoing flows, with no intended overlap or gap | Pass by inspection | | |
| No action has two incoming flows; merge nodes are used for loops | Pass by inspection | | |
| Every fork has a matching join | N/A — no fork used | | |
| Every system action cites an FR | Partial — `FR-02-02` actions are traced; readiness/cancellation behavior needs a new FR or business-rule reference in W1 | | |
| Every FR of this use case appears in the diagram | Partial — current `FR-02-02` is covered; the proposed readiness/cancellation behavior is not yet represented in W1 | | |
| Lane names match the Context Diagram and use-case actors | Partial — system matches; actor-label mismatch is open question 1 | | |
| Related sequence diagram uses the same names and FR IDs; its alternative fragments appear here as decisions | Not yet verified — W1 points to `S-01` | | |
| AI critic prompt run; findings resolved or logged | Not yet run | | |
