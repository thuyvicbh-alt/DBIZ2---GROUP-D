# Activity Diagram: AD-UC-03 Request and Schedule a UCO Pickup

| Field | Value |
|---|---|
| AD ID | AD-UC-03 |
| Use case | UC-03 Request and schedule a UCO pickup |
| Feature | F-01 Supplier Onboarding & Pickup Management |
| Primary actor | Supplier (W0; confirm the exact label used on the Context Diagram) |
| Lanes | Supplier · ReFeed System |
| Inputs (L3-L5) | Current Context Diagram · Use Case Diagram containing UC-03 · Sequence: none listed in the W1 source trace for `FR-01-03` (see open question 5) · W1 rows `FR-01-03` (`FR-01-03-FE`, `FR-01-03-BE`) |
| Trigger | The supplier opens the pickup request form in the app. |
| Preconditions | The supplier has a registered account with a unique Supplier_ID and is signed in (`FR-01-01`). |
| Success end state | A pickup request linked to the Supplier_ID exists with status `Requested`, holding the pickup location, estimated volume, preferred time window and any notes; the supplier has received the pickup request ID. |
| Failure end state(s) | If the supplier profile is incomplete (missing GPS or compliance data), no pickup request is created and the supplier is prompted to update the profile (UC-02). If request details are missing or invalid, the system returns validation feedback so the supplier can correct and resubmit them. |
| FRs covered | `FR-01-03` |
| Business rules applied | Profile-completeness rule inside `FR-01-03`: a pickup request is accepted only when the supplier profile holds GPS and compliance data. No separate business-rule ID is listed in W1. |
| Version / status | v0.1 · Draft |
| Author | ReFeed Group D |

## 1. Diagram

The activity uses two lanes from the project context: `Supplier` and `ReFeed System`. The supplier enters and submits the pickup request. The system first checks that the supplier profile is complete. If it is not, the request is rejected and the supplier is prompted to update the profile. If it is complete, the system validates the request details. Invalid details return to the supplier for correction and resubmission; valid details create a pickup request with status `Requested`, and the system returns its request ID.

```mermaid
swimlane-beta TB
  subgraph ACT["Supplier"]
    start@{ shape: sm-circ }
    a1("Open pickup request form")
    m1{" "}
    a2("Enter or correct pickup location, estimated volume, preferred time window and notes")
    a3("Submit pickup request")
    a4("See prompt to update supplier profile")
    a5("See pickup request ID and status")
    stop1@{ shape: fr-circ }
    stop2@{ shape: fr-circ }
  end

  subgraph SYS["ReFeed System"]
    s1("Check that supplier profile is complete (FR-01-03)")
    d1{" "}
    s2("Reject request and prompt profile update (FR-01-03)")
    s3("Validate pickup location, estimated volume and time window (FR-01-03)")
    d2{" "}
    s4("Return validation feedback (FR-01-03)")
    s5("Create pickup request with status Requested and record the time window (FR-01-03)")
    o1["Pickup [Requested]"]
    s6("Return pickup request ID (FR-01-03)")
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

- `d1` is the profile-completeness decision from `FR-01-03`. **No** rejects the request; no pickup request is created and the activity ends at `stop2` after the supplier sees the prompt. Updating the profile is a separate use case (UC-02), so it is not drawn inside this diagram.
- `d2` checks the submitted request details. If they are missing or invalid, the system returns feedback and the supplier corrects them through `m1` before resubmitting.
- `m1` merges the first entry into the request form and the correction path after validation fails. It is a merge node, not a decision.
- Two activity finals are used: `stop1` for a created request and `stop2` for a rejected request.
- `FR-01-03` names the profile check, the `Requested` status and the request ID explicitly. The detailed validation rules for the request fields are not defined in W1; the correction path through `d2` is a draft default, logged below.
- Changing, cancelling or tracking an existing request belongs to UC-04 (`FR-01-04`, `FR-01-05`) and is out of scope for this diagram.

## 2. Node trace

| Node | Kind | Text | Traces to |
|---|---|---|---|
| `a1` | Actor action | Open pickup request form | `FR-01-03-FE` |
| `a2` | Actor action | Enter or correct pickup location, estimated volume, preferred time window and notes | `FR-01-03-FE` (form fields: location, estimated volume, time window, notes) |
| `a3` | Actor action | Submit pickup request | `FR-01-03` (WHEN a supplier submits a pickup request) |
| `s1` | Action | Check that supplier profile is complete | `FR-01-03` |
| `d1` | Decision | Supplier profile complete? | `FR-01-03` (IF the profile is incomplete) |
| `s2` | Action | Reject request and prompt profile update | `FR-01-03` |
| `a4` | Actor action | See prompt to update supplier profile | `FR-01-03`; profile update itself is UC-02 / `FR-01-02` |
| `s3` | Action | Validate pickup location, estimated volume and time window | `FR-01-03` (validation rules are a draft default) |
| `d2` | Decision | Request details valid? | `FR-01-03` (validation; rules are a draft default) |
| `s4` | Action | Return validation feedback | `FR-01-03` (feedback behavior is a draft default) |
| `s5` | Action | Create pickup request with status `Requested` and record the time window | `FR-01-03` |
| `o1` | Object node | Pickup `[Requested]` | `FR-01-03` (state value) |
| `s6` | Action | Return pickup request ID | `FR-01-03` |
| `a5` | Actor action | See pickup request ID and status | `FR-01-03` |
| `m1` | Merge | Enter request details or correct them after validation feedback | Control-flow merge; no separate FR |
| `stop1` | Activity final | Pickup request created | UC-03 success end |
| `stop2` | Activity final | Pickup request rejected because the profile is incomplete | UC-03 failure end |

## 3. Branches and acceptance tests

| Branch | Given | When | Then |
|---|---|---|---|
| `d1` `[No: GPS or compliance data missing]` | Supplier SUP-00130 is signed in, but its profile has no GPS location. | The supplier submits a pickup request for 30–50 kg on 25/09/2026, 08:00–10:00. | No pickup request is created; the system rejects the request and prompts the supplier to update the profile. |
| `d1` `[Yes: profile complete]` | Supplier SUP-00125 is signed in and its profile holds GPS and compliance data. | The supplier submits a pickup request. | The system continues to validate the request details. |
| `d2` `[No: missing/invalid request details]` | Supplier SUP-00125 has a complete profile. | The supplier submits a request with no preferred time window, or with a time window that has already passed. | No pickup request is created; the system returns validation feedback and the supplier can correct and resubmit. |
| `d2` `[Yes: valid]` | Supplier SUP-00125 has a complete profile. | The supplier submits a request for 12 Hàng Bài, estimated 30–50 kg, on 25/09/2026, 08:00–10:00. | One pickup request (for example PK-00125) linked to SUP-00125 exists with status `Requested`, and the supplier sees its request ID. |

## 4. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
|---|---|---|
| Pickup | `Requested` | Known from the current W1 requirements: `Requested`, `Driver Assigned`, `Arrived`, `Completed` (`FR-03-01`). `Cancelled` is proposed in AD-UC-06 and must be confirmed. Treat this as a partial list until the team confirms the complete pickup-status lifecycle. |

The state spelling `Requested` is taken from the W1 `FR-01-03` requirement. 

## 5. Screens touched


| Actor action | Screen ID |
|---|---|
| Open pickup request form; enter/correct request details; submit request; see profile-update prompt; see request ID and status | To be assigned from the Screen List |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | W0 names the actor `Supplier`. Which exact label does the Context Diagram use, and is it the same across W0, the Use Case Diagram and all activity diagrams? | Yes for final cross-diagram consistency | The lane uses the W0 label `Supplier`. |
| 2 | The app description offers `Pickup Now` or `Schedule Pickup`, but `FR-01-03` only mentions a preferred time window. Is `Pickup Now` part of Release 1? | No for this draft | Only a preferred time window is captured; `Pickup Now` is not drawn. |
| 3 | What are the exact validation rules for the request details (required fields, minimum estimated volume, earliest allowed time window, whether the location must match the registered Point of Origin)? | Yes for detailed acceptance tests | Location, estimated volume and preferred time window are required; the time window cannot be in the past; notes are optional. |
| 4 | After a request is rejected for an incomplete profile, are the entered request details kept so the supplier can resubmit after updating the profile? | No for this draft | Details are not kept; the supplier updates the profile (UC-02) and submits a new request. |
| 5 | The W1 source trace for `FR-01-03` lists no sequence diagram (only UC, CTX and CVCA). Does a sequence such as `SD-UC-03` exist or need to be created? | No for this draft; resolve before integrated review | Verified against the use case, the Context Diagram and W1 only. |
| 6 | May a supplier submit a new pickup request while another of their requests is still `Requested`? | No for this draft | Allowed; no duplicate check is drawn. |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders on mermaid.live (Mermaid 11.16+ for `swimlane-beta`) | Rendered with mermaid-cli 11.16.0; mermaid.live still to check | | |
| Every decision has guards on all outgoing flows, with no intended overlap or gap | Pass by inspection | | |
| No action has two incoming flows; merge nodes are used for loops | Pass by inspection | | |
| Every fork has a matching join | N/A — no fork used | | |
| Every system action cites an FR | Pass — `FR-01-03` | | |
| Every FR of this use case appears in the diagram | Pass (1 of 1: `FR-01-03`) | | |
| Lane names match the Context Diagram and use-case actors | Partial — system matches; supplier label to confirm (open question 1) | | |
| Related sequence diagram uses the same names and FR IDs; its alternative fragments appear here as decisions | Not yet verified — no sequence listed for `FR-01-03` (open question 5) | | |
| AI critic prompt run; findings resolved or logged | Not yet run | | |
