# Activity Diagram: AD-UC-01 Supplier Onboarding & Pickup Management

| Field | Value |
|---|---|
| AD ID | AD-UC-01 |
| Use case | UC-01 Register/Login as a UCO supplier, extended through the F-01 flow to UC-02, UC-03 and UC-04 |
| Feature | F-01 Supplier Onboarding & Pickup Management |
| Primary actor | Supplier |
| Lanes | Supplier · ReFeed System |
| Inputs (L3-L5) | Context Diagram v2 (Supplier) · Use Case Diagram v2 · Sequence: SD-UC-01 · Architecture v2 · W1 rows FR-01-01 to FR-01-05 |
| Trigger | The supplier submits credentials or registration data on the portal |
| Preconditions | The supplier has access to the ReFeed Web/Mobile Portal. |
| Success end state | The supplier is authenticated, the Point-of-Origin and compliance profile is stored, a pickup request is created with status `Requested`, and the supplier can change, cancel or track it. |
| Failure end state | No account/session is created, or no pickup request is created; the supplier sees an error or a prompt to complete the profile. |
| FRs covered | FR-01-01 Register/Login as a UCO supplier · FR-01-02 Manage Point-of-Origin and compliance profile · FR-01-03 Request and schedule a UCO pickup · FR-01-04 Manage an existing pickup request · FR-01-05 Track pickup status |
| Business rules applied | BR-01 (Unique Supplier Identifier & Tax ID validation) |
| Version / status | v1.2 · Rebuilt on the revised W1 function list, each FR appears once |
| Author | Nguyen Thuy Vi - 2411560050 |

## 1. Diagram

Notation follows the DBIZ2 activity profile (`mermaid-activity-patterns.md`). Rendered with
`swimlane-beta`, which needs Mermaid 11.16 or later. Each FR is written on **one** node only, with its
exact W1 name.

```mermaid
---
config:
  flowchart:
    nodeSpacing: 60
    rankSpacing: 45
---


swimlane-beta TB
%% UC01: Supplier Onboarding & Pickup Management (F-01)
%% ReFeed group project
subgraph supplier ["Supplier"]
initial@{ shape: f-circ, label: " " }
submitAuth("Submit Credentials or Registration Data")
submitProfile("Submit Point-of-Origin and Compliance Data")
submitPickup("Submit Pickup Request with Preferred Time Window")
choiceNext{"Next action?"}
changeRequest("Change or Cancel Pickup Request")
trackRequest("Request Pickup Status")
finalFail@{ shape: fr-circ, label: " " }
finalRejected@{ shape: fr-circ, label: " " }
finalManaged@{ shape: fr-circ, label: " " }
finalTracked@{ shape: fr-circ, label: " " }
end


subgraph system ["ReFeed System"]
registerLogin("Register/Login as a UCO supplier (FR-01-01)")
choiceAuth{"Authentication valid?"}
displayError("Display Error Message & Prompt Retry")

manageProfile("Manage Point-of-Origin and compliance profile (FR-01-02)")

requestPickup("Request and schedule a UCO pickup (FR-01-03)")
choiceProfile{"Profile complete?"}
rejectRequest("Reject Request & Prompt Profile Update")
createRequest("Create Pickup Request and Return Request ID")
requested["Pickup Request [Requested]"]

manageRequest("Manage an existing pickup request (FR-01-04)")
updateStatus("Update Request Status & Retain Appointment Information")

trackStatus("Track pickup status (FR-01-05)")
displayStatus("Display Status, Schedule, Driver, Vehicle & Last Update")
end


initial --> submitAuth
submitAuth --> registerLogin
registerLogin --> choiceAuth

choiceAuth -->|"[no]"| displayError
displayError --> finalFail

choiceAuth -->|"[yes]"| submitProfile
submitProfile --> manageProfile
manageProfile --> submitPickup
submitPickup --> requestPickup
requestPickup --> choiceProfile

choiceProfile -->|"[no]"| rejectRequest
rejectRequest --> finalRejected

choiceProfile -->|"[yes]"| createRequest
createRequest --> requested
requested --> choiceNext

choiceNext -->|"[change or cancel]"| changeRequest
changeRequest --> manageRequest
manageRequest --> updateStatus
updateStatus --> finalManaged

choiceNext -->|"[track status]"| trackRequest
trackRequest --> trackStatus
trackStatus --> displayStatus
displayStatus --> finalTracked
```

Reading notes:

- The process starts when the **Supplier** submits credentials or registration data. `FR-01-01` registers a new supplier with a unique `Supplier_ID` or authenticates a returning one. Invalid data ends with an error and a prompt to retry.
- A valid supplier submits or updates the Point-of-Origin and compliance profile (`FR-01-02`), then requests a pickup (`FR-01-03`).
- The profile-completeness decision rejects the request when GPS or compliance data is missing. Otherwise the pickup request is created in state `Requested`.
- The supplier then either changes or cancels the request (`FR-01-04`) or tracks its status (`FR-01-05`).
- Each FR is tagged on exactly one node. The untagged nodes belong to the FR of the node before them.

## 2. Node trace

Each FR appears once. Untagged nodes inherit the FR of the tagged node they follow.

| Node | Kind | Text | Traces to |
|---|---|---|---|
| s1 | Action | Register/Login as a UCO supplier | **FR-01-01**, BR-01 |
| d1 | Decision | Authentication valid? | FR-01-01 (inherited), BR-01 |
| s2 | Action | Display Error Message & Prompt Retry | FR-01-01 (inherited) |
| s3 | Action | Manage Point-of-Origin and compliance profile | **FR-01-02** |
| s4 | Action | Request and schedule a UCO pickup | **FR-01-03** |
| d2 | Decision | Profile complete? | FR-01-03 (inherited) |
| s5 | Action | Reject Request & Prompt Profile Update | FR-01-03 (inherited) |
| s6 | Action | Create Pickup Request and Return Request ID | FR-01-03 (inherited) |
| o1 | Object node | Pickup Request [Requested] | FR-01-03 (state value) |
| d3 | Decision | Next action? | FR-01-04, FR-01-05 (inherited) |
| s7 | Action | Manage an existing pickup request | **FR-01-04** |
| s8 | Action | Update Request Status & Retain Appointment Information | FR-01-04 (inherited) |
| s9 | Action | Track pickup status | **FR-01-05** |
| s10 | Action | Display Status, Schedule, Driver, Vehicle & Last Update | FR-01-05 (inherited) |

## 3. Branches and acceptance tests

One row per outgoing branch of every decision. DBIZ3 turns each row into at least one test.

| Branch | Given | When | Then |
|---|---|---|---|
| d1 [no] | a supplier enters invalid credentials or incomplete compliance data | they submit authentication data | no account or session is created and an error message prompts a retry |
| d1 [yes] | a new supplier submits valid data, or a returning supplier submits valid credentials | they submit the data | a unique Supplier_ID is created for a new supplier, or the returning supplier is authenticated |
| d2 [no] | the supplier profile is missing GPS or compliance data | the supplier submits a pickup request | the request is rejected and the supplier is prompted to update the profile |
| d2 [yes] | the supplier profile is complete | the supplier submits a pickup request with a preferred time window | the request is created with status `Requested` and the request ID is returned |
| d3 [change or cancel] | an eligible pickup request exists | the supplier changes or cancels it | the request status is updated and the appointment information is retained |
| d3 [track status] | a pickup request exists | the supplier requests the pickup status | the system displays the status, scheduled collection date/time, assigned driver and vehicle when available, and the last status-update timestamp |

## 4. Object states used

| Object | States that appear in this diagram | Full state list (all diagrams) |
|---|---|---|
| Pickup Request | Requested | Requested, Driver Assigned, Arrived, Completed, Cancelled |

These state names become the allowed values of `pickup_request.status` in the DBIZ3 data model. Keep the
spelling identical everywhere.

## 5. Screens touched

| Supplier action | Screen ID |
|---|---|
| Submit credentials or registration data, see error | S-F01-01 (Supplier registration) · S-F00-01 (Sign in) |
| Submit Point-of-Origin and compliance data | S-F01-02 (Point-of-Origin and compliance profile) |
| Submit pickup request, see profile-incomplete prompt | S-F01-03 (Pickup request form) |
| Change or cancel pickup request | S-F01-05 (Pickup request detail) |
| Request pickup status | S-F01-04 (My pickup requests) · S-F01-05 (Pickup request detail) |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: Is OTP/2FA required upon initial registration?] | No | Standard email/password authentication first; 2FA can be configured after registration. |
| 2 | [NEEDS CLARIFICATION: Which statuses allow a change or cancellation?] | No | Only a pickup request that is not yet dispatched is eligible (for example `Requested`). |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders (mermaid-cli 11.16.0) | Pass | | 28/11/2026 |
| Every decision has guards on all outgoing flows | Pass | | 28/11/2026 |
| Every FR of F-01 appears in the diagram | Pass (5 of 5: FR-01-01 to FR-01-05) | | 28/11/2026 |
| Each FR appears exactly once in the diagram | Pass | | 28/11/2026 |
| FR names match W1 | Pass | | 28/11/2026 |
| Lane names match the Context Diagram | Pass | | 28/11/2026 |
