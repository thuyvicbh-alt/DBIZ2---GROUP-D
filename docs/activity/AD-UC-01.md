# Activity Diagram: AD-UC-01 Supplier Onboarding & Pickup Management

| Field | Value |
|---|---|
| AD ID | AD-UC-01 |
| Use case | UC-01 Register/Login as a UCO supplier, followed through the F-01 flow (profile, pickup request, change or cancel, tracking) |
| Feature | F-01 Supplier Onboarding & Pickup Management |
| Primary actor | Supplier |
| Lanes | Supplier · ReFeed system |
| Inputs (L3-L5) | Context Diagram v2 (Supplier) · Use Case Diagram v2 · Sequence: SD-UC-01 · Architecture v2 · W1 rows FR-01-01 to FR-01-05 |
| Trigger | The supplier submits credentials or registration data on the portal |
| Preconditions | The supplier has access to the ReFeed Web/Mobile Portal. |
| Success end state | The supplier is authenticated, the Point-of-Origin and compliance profile is stored, and a pickup request in state `Requested` exists; it can be changed, cancelled or tracked. |
| Failure end state | No account or session is created, or no new pickup request is created or changed; the supplier sees why (invalid credentials, incomplete profile, or request not eligible). |
| FRs covered | FR-01-01 Register/Login as a UCO supplier · FR-01-02 Manage Point-of-Origin and compliance profile · FR-01-03 Request and schedule a UCO pickup · FR-01-04 Manage an existing pickup request · FR-01-05 Track pickup status |
| Business rules applied | BR-01 (Unique Supplier Identifier & Tax ID validation) |
| Version / status | v1.4 · LabDesk pattern: every system action carries one FR, each FR appears once |
| Author | Nguyen Thuy Vi - 2411560050 |

## 1. Diagram

Notation follows the DBIZ2 activity profile (`mermaid-activity-patterns.md`). Rendered with
`swimlane-beta`, which needs Mermaid 11.16 or later.

```mermaid
swimlane-beta TB
  subgraph SUP["Supplier"]
    start@{ shape: sm-circ }
    a1("Submit credentials or registration data")
    a2("Submit or update Point-of-Origin and compliance data")
    a3("Submit pickup request with preferred time window")
    a4("Choose to change, cancel or track a request")
    stop1@{ shape: fr-circ }
    stop2@{ shape: fr-circ }
  end
  subgraph SYS["ReFeed system"]
    s1("Register/Login as a UCO supplier (FR-01-01)")
    d1{"Authentication<br>valid?"}
    d2{"New registration or<br>returning login?"}
    m1{" "}
    s2("Manage Point-of-Origin and compliance profile (FR-01-02)")
    m2{" "}
    s3("Request and schedule a UCO pickup (FR-01-03)")
    d3{"Supplier profile<br>complete?"}
    o1["Pickup Request [Requested]"]
    d4{"Which<br>action?"}
    s4("Manage an existing pickup request (FR-01-04)")
    d5{"Request<br>eligible?"}
    s5("Track pickup status (FR-01-05)")
  end
  start --> a1 --> s1 --> d1
  d1 -->|"[no, show error and retry]"| stop1
  d1 -->|"[yes]"| d2
  d2 -->|"[new registration]"| m1 --> a2 --> s2 --> m2
  d2 -->|"[returning login]"| m2
  m2 --> a3 --> s3 --> d3
  d3 -->|"[no, prompt profile update]"| m1
  d3 -->|"[yes]"| o1 --> a4 --> d4
  d4 -->|"[change or cancel]"| s4 --> d5
  d5 -->|"[no, show reason]"| stop1
  d5 -->|"[yes, status updated]"| stop2
  d4 -->|"[track status]"| s5 --> stop2
```

Reading notes:

- Every action in the system lane carries one FR with its exact W1 name, and each FR appears once: `s1` FR-01-01, `s2` FR-01-02, `s3` FR-01-03, `s4` FR-01-04, `s5` FR-01-05.
- Supplier actions (submit, choose) and the object node `o1` carry no FR. Decisions follow the LabDesk example: the label has no tag, and section 2 gives the FR that decides each branch.
- `m1` is a **merge** node. It joins the new-registration flow and the flow back from an incomplete profile, so the supplier returns to the profile step instead of the diagram ending.
- `m2` is a merge node. A new supplier reaches it after the profile step. A returning supplier reaches it straight from `d2`, so a returning login does not repeat the profile step.
- The error message, the profile-update prompt, the creation of the request and the status update are parts of `s1`, `s3` and `s4`. They are written on the flow guards and do not need a separate FR.
- Two flows enter `stop1` and two enter `stop2`. This is allowed: a final node is not an action. A failed login ends the activity; the supplier starts again from the sign-in screen.

## 2. Node trace

Every action in the system lane cites a requirement. Every decision cites the rule or requirement
that decides it.

| Node | Kind | Text | Traces to |
|---|---|---|---|
| s1 | Action | Register/Login as a UCO supplier | FR-01-01, BR-01 |
| d1 | Decision | Authentication valid? | FR-01-01, BR-01 |
| d2 | Decision | New registration or returning login? | FR-01-01 |
| s2 | Action | Manage Point-of-Origin and compliance profile | FR-01-02 |
| s3 | Action | Request and schedule a UCO pickup | FR-01-03 |
| d3 | Decision | Supplier profile complete? | FR-01-03 (GPS and compliance data present) |
| o1 | Object node | Pickup Request [Requested] | FR-01-03 (state value) |
| d4 | Decision | Which action? | FR-01-04, FR-01-05 |
| s4 | Action | Manage an existing pickup request | FR-01-04 |
| d5 | Decision | Request eligible? | FR-01-04 (eligible pickup request) |
| s5 | Action | Track pickup status | FR-01-05 |

## 3. Branches and acceptance tests

One row per outgoing branch of every decision. DBIZ3 turns each row into at least one test.

| Branch | Given | When | Then |
|---|---|---|---|
| d1 [no] | a supplier enters invalid credentials or incomplete compliance data | they submit authentication data | no account or session is created and an error message prompts a retry |
| d1 [yes] → d2 [new registration] | a new supplier submits valid registration data | they submit the data | a unique Supplier_ID is created and the supplier is asked for the Point-of-Origin and compliance profile |
| d1 [yes] → d2 [returning login] | a registered supplier submits valid credentials | they log in | the supplier is authenticated and goes straight to the pickup request |
| d3 [no] | the supplier profile is missing GPS or compliance data | the supplier submits a pickup request | no request is created and the supplier is prompted to update the profile |
| d3 [yes] | the supplier profile is complete | the supplier submits a pickup request with a preferred time window | one Pickup Request in state Requested exists and the request ID is returned |
| d4 [change or cancel] → d5 [no] | the pickup is no longer eligible (for example already dispatched) | the supplier changes or cancels it | the request is unchanged and the reason is shown |
| d4 [change or cancel] → d5 [yes] | the pickup request is eligible | the supplier changes or cancels it | the request status is updated and the appointment information is retained |
| d4 [track status] | a pickup request exists | the supplier requests its status | the system shows the status, scheduled date/time, assigned driver and vehicle when available, and the last status-update timestamp |

## 4. Object states used

| Object | States that appear in this diagram | Full state list (all diagrams) |
|---|---|---|
| Pickup Request | Requested | Requested, Driver Assigned, Arrived, Completed, Cancelled |

These state names become the allowed values of `pickup_request.status` in the DBIZ3 data model. Keep the
spelling identical everywhere.

## 5. Screens touched

Filled in during Lesson 8, from the Screen List.

| Supplier action | Screen ID |
|---|---|
| Submit credentials or registration data, see error | S-F01-01 (Supplier registration) · S-F00-01 (Sign in) |
| Submit or update Point-of-Origin and compliance data | S-F01-02 (Point-of-Origin and compliance profile) |
| Submit pickup request, see profile-incomplete prompt | S-F01-03 (Pickup request form) |
| Choose to change, cancel or track a request | S-F01-04 (My pickup requests) · S-F01-05 (Pickup request detail) |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: is OTP/2FA required at registration?] | No | Standard email and password first; 2FA can be configured after registration. |
| 2 | [NEEDS CLARIFICATION: which pickup statuses are eligible for change or cancellation?] | Yes | Only a request that is not yet dispatched (for example `Requested`) is eligible. |
| 3 | [NEEDS CLARIFICATION: should the W1 criteria of FR-01-01 and FR-01-04 state the error message on invalid login and the reason shown when a request is not eligible?] | No | Both are treated as part of those FRs. Add them to the EARS wording of FR-01-01 and FR-01-04 when W1 is next revised. |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders (mermaid-cli 11.16.0) | Pass | | 28/11/2026 |
| Every decision has guards on all outgoing flows | Pass | | 28/11/2026 |
| No two flows enter an action directly (merges used) | Pass | | 28/11/2026 |
| Every system action cites an FR | Pass (5 of 5) | | 28/11/2026 |
| Every FR of F-01 appears in the diagram | Pass (5 of 5) | | 28/11/2026 |
| Each FR appears exactly once, with the exact W1 name | Pass | | 28/11/2026 |
| Lane names match the Context Diagram | Pass | | 28/11/2026 |
