# Activity Diagram: AD-UC-03 Request and Schedule a UCO Pickup

| Field | Value |
|---|---|
| AD ID | AD-UC-03 |
| Use case | UC-03 Request and schedule a UCO pickup |
| Feature | F-01 Supplier Onboarding & Pickup Management |
| Primary actor | Supplier (confirmed in W0 Table 1 & Table 2) |
| Lanes | Supplier · ReFeed System |
| Inputs (L3-L5) | Current Context Diagram (`CTX:supplier-profile`) · Use Case Diagram (`UC-03`) · CVCA (`CVCA:compliance-flow`) · Sequence: none listed in W1 source trace for `FR-01-03` · W1 row `FR-01-03` (`FR-01-03-FE`, `FR-01-03-BE`) |
| Trigger | The supplier opens the pickup request form in the app. |
| Preconditions | The supplier has a registered account with a unique `Supplier_ID` and is signed in (`FR-01-01`). |
| Success end state | A pickup request linked to the `Supplier_ID` exists in `UCO Pickup Request` (ILF) with status `Requested`, holding the pickup location, estimated volume, preferred time window and any notes; the supplier has received the pickup request ID. |
| Failure end state(s) | If the supplier profile is incomplete (missing GPS or compliance data), no pickup request is created and the supplier is prompted to update the profile (`UC-02` / `FR-01-02`). If request details are missing or invalid, the system returns validation feedback so the supplier can correct and resubmit them. |
| FRs covered | `FR-01-03` (`FR-01-03-FE`, `FR-01-03-BE`) |
| Business rules applied | Profile-completeness rule inside `FR-01-03-BE`: a pickup request is accepted only when `Supplier Profile & Compliance` (ILF) holds complete GPS and compliance data. No separate `NC` business-rule row is listed in W1. |
| Version / status | v0.2 · Aligned with W1–W5 Baseline |
| Author | ReFeed Group D (Owner: Bùi Thị Xuân An) |

## 1. Diagram

The activity uses two lanes from the project context: `Supplier` and `ReFeed System`. Every action node explicitly carries its corresponding L3 Function List ID (`FR-01-03-FE` for UI actions and `FR-01-03-BE` for backend validation and persistence). Control nodes (`Start`, `Merge`, `Decision`, `Final`) and object nodes (`Pickup [Requested]`) do not carry FR IDs.

```mermaid
swimlane-beta TB
  subgraph ACT["Supplier"]
    start@{ shape: sm-circ }
    a1("Open pickup request form [FR-01-03-FE]")
    m1{" "}
    a2("Enter or correct pickup location, estimated volume, preferred time window and notes [FR-01-03-FE]")
    a3("Submit pickup request [FR-01-03-FE]")
    a4("See prompt to update supplier profile [FR-01-03-FE]")
    a5("See pickup request ID and Requested status [FR-01-03-FE]")
    stop1@{ shape: fr-circ }
    stop2@{ shape: fr-circ }
  end

  subgraph SYS["ReFeed System"]
    s1("Check that supplier profile is complete [FR-01-03-BE]")
    d1{" "}
    s2("Reject request and prompt profile update [FR-01-03-BE]")
    s3("Validate pickup location, estimated volume and time window [FR-01-03-BE]")
    d2{" "}
    s4("Return validation feedback [FR-01-03-BE]")
    s5("Create pickup request with status Requested and record time window [FR-01-03-BE]")
    o1["Pickup [Requested]"]
    s6("Return pickup request ID and status [FR-01-03-BE]")
  end

  start --> a1 --> m1 --> a2 --> a3 --> s1
  s1 -->|Supplier profile complete?| d1
  d1 -->|"[No: GPS or compliance data missing]"| s2 --> a4 --> stop2
  d1 -->|"[Yes: profile complete]"| s3
  s3 -->|Request details valid?| d2
  d2 -->|"[No: missing/invalid request details]"| s4 --> m1
  d2 -->|"[Yes: valid]"| s5 --> o1 --> s6 --> a5 --> stop1
```

### Reading notes

- **Granularity mapping (N-to-1):** In W1, `FR-01-03` is decomposed by technical layer into two work items: `FR-01-03-FE` (pickup request screen/form and status display) and `FR-01-03-BE` (profile validation, request creation, and time-window persistence). Consequently, all UI action nodes in the `Supplier` lane map to `FR-01-03-FE`, and all service/validation action nodes in the `ReFeed System` lane map to `FR-01-03-BE`.
- `d1` evaluates the profile-completeness check performed in `s1` (`FR-01-03-BE`). **No** rejects the request; no pickup request is created and the activity ends at `stop2` after the supplier sees the prompt (`a4`). Updating the profile belongs to `UC-02` (`FR-01-02`), so it is out of scope here.
- `d2` evaluates the request-detail validation performed in `s3` (`FR-01-03-BE`). If details are missing or invalid, `s4` returns feedback and the supplier corrects them via `m1` before resubmitting.
- `m1` merges the initial form entry and the validation-failure correction loop. It is a control-flow merge node, not a decision.
- Managing, rescheduling, cancelling, or tracking an existing pickup request belongs to `UC-04` (`FR-01-04` and `FR-01-05`) and is out of scope for this diagram.

## 2. Node trace

| Node | Kind | Text | Traces to (W1 L3 ID) |
|---|---|---|---|
| `a1` | Actor action | Open pickup request form | `FR-01-03-FE` |
| `a2` | Actor action | Enter or correct pickup location, estimated volume, preferred time window and notes | `FR-01-03-FE` |
| `a3` | Actor action | Submit pickup request | `FR-01-03-FE` |
| `s1` | System action | Check that supplier profile is complete | `FR-01-03-BE` |
| `d1` | Decision | Supplier profile complete? | Control flow (evaluates `s1` / `FR-01-03-BE`) |
| `s2` | System action | Reject request and prompt profile update | `FR-01-03-BE` |
| `a4` | Actor action | See prompt to update supplier profile | `FR-01-03-FE` (redirects to `UC-02` / `FR-01-02`) |
| `s3` | System action | Validate pickup location, estimated volume and time window | `FR-01-03-BE` |
| `d2` | Decision | Request details valid? | Control flow (evaluates `s3` / `FR-01-03-BE`) |
| `s4` | System action | Return validation feedback | `FR-01-03-BE` |
| `s5` | System action | Create pickup request with status `Requested` and record time window | `FR-01-03-BE` |
| `o1` | Object node | Pickup `[Requested]` | `UCO Pickup Request` (W0 ILF) · `FR-01-03-BE` |
| `s6` | System action | Return pickup request ID and status | `FR-01-03-BE` |
| `a5` | Actor action | See pickup request ID and `Requested` status | `FR-01-03-FE` |
| `m1` | Merge | Merge initial entry and validation-error correction loop | Control-flow merge; no FR |
| `stop1` | Activity final | Pickup request created | `UC-03` success end |
| `stop2` | Activity final | Pickup request rejected due to incomplete profile | `UC-03` failure end |

## 3. Branches and acceptance tests

| Branch | Given | When | Then |
|---|---|---|---|
| `d1` `[No: GPS or compliance data missing]` | Supplier `SUP-00130` is signed in (`FR-01-01`), but its profile lacks GPS location. | The supplier submits a pickup request for 30–50 kg on 25/09/2026, 08:00–10:00. | No pickup request is created; `FR-01-03-BE` rejects the request and `FR-01-03-FE` prompts the supplier to update the profile (`UC-02` / `FR-01-02`). |
| `d1` `[Yes: profile complete]` | Supplier `SUP-00125` is signed in and its profile holds GPS and compliance data. | The supplier submits a pickup request. | The system proceeds to validate the request details (`s3` / `FR-01-03-BE`). |
| `d2` `[No: missing/invalid request details]` | Supplier `SUP-00125` has a complete profile. | The supplier submits a request with no preferred time window, or with a time window in the past. | No pickup request is created; `FR-01-03-BE` returns validation feedback (`s4`) and the supplier can correct and resubmit via `FR-01-03-FE`. |
| `d2` `[Yes: valid]` | Supplier `SUP-00125` has a complete profile. | The supplier submits a request for 12 Hàng Bài, estimated 30–50 kg, on 25/09/2026, 08:00–10:00. | One pickup request (e.g., `PK-00125`) linked to `SUP-00125` is persisted with status `Requested`, and the supplier sees its request ID (`FR-01-03-FE`). |

## 4. Object states used

| Object | States in this diagram | Full state list (aligned with W1) |
|---|---|---|
| `UCO Pickup Request` (ILF) | `Requested` | `Requested` (`FR-01-03`), `Cancelled` (`FR-01-04`), `Driver Assigned` (`FR-02-01`), `Arrived` (`FR-02-02`), `Completed` (`FR-03-01`). |

## 5. Screens touched

| Actor action | Function ID | Screen ID |
|---|---|---|
| Open pickup request form (`a1`); enter/correct request details (`a2`); submit request (`a3`); see profile-update prompt (`a4`); see request ID and status (`a5`) | `FR-01-03-FE` | To be assigned from the Screen List |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | W0 Table 1 & Table 2 confirm the actor name `Supplier`. Confirm that all remaining diagrams (Context, Use Case, Sequence, Activity) use `Supplier` identically. | Yes for final cross-diagram consistency | The lane uses `Supplier`. |
| 2 | Does Release 1 support `Pickup Now` in addition to `Schedule Pickup`, or only a preferred time window as specified in `FR-01-03`? | No for this draft | Only a preferred time window is captured (`FR-01-03`). |
| 3 | What are the exact field validation rules (minimum estimated volume, earliest allowed time window, whether pickup location must match the registered Point-of-Origin)? Note: W1 marks `FR-01-03` as `EARS precise? = Yes` at the transaction level, so field-level constraints are documented here for test cases. | No for Release 1 baseline | Location, estimated volume, and preferred time window are required; time window cannot be in the past; notes are optional. |
| 4 | After rejection due to an incomplete profile (`s2`), are the entered form values cached while the supplier completes `UC-02` (`FR-01-02`)? | No for this draft | Values are not cached; the supplier updates the profile in `FR-01-02` and submits a new request in `FR-01-03`. |
| 5 | May a supplier submit a new pickup request while another request is still `Requested`? | No for this draft | Allowed; no duplicate-request block is drawn. |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders on mermaid.live (Mermaid 11.16+ for `swimlane-beta`) | Rendered with mermaid-cli 11.16.0 | | |
| Every decision has guards on all outgoing flows, with no overlap or gap | Pass (`d1`, `d2`) | | |
| No action has two incoming flows; merge nodes are used for loops | Pass (`m1` used for validation loop) | | |
| Every action box (UI & System) carries a valid W1 L3 Function ID | Pass — `a1`–`a5` trace to `FR-01-03-FE`; `s1`–`s6` trace to `FR-01-03-BE` | | |
| Every FR of `UC-03` in W1 appears in the diagram | Pass (`FR-01-03-FE`, `FR-01-03-BE`) | | |
| Lane names match W0 actors (`Supplier`) and system boundary (`ReFeed System`) | Pass | | |
