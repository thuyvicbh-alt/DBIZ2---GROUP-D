# Activity Diagram: AD-UC-01 Register / Login as a UCO supplier

| Field | Value |
|---|---|
| AD ID | AD-UC-01 |
| Use case | UC-01 Register / Login as a UCO supplier |
| Feature | F-01 Supplier Portal & Account Management |
| Primary actor | Supplier |
| Lanes | Supplier · ReFeed system |
| Inputs (L3-L5) | Context Diagram v2 (Supplier) · Use Case Diagram v2 · Sequence: SD-UC-01 · Architecture v2 · W1 row FR-01-01 (Register/Login as a UCO supplier) |
| Trigger | The supplier submits credentials or registration data on the portal |
| Preconditions | The supplier has access to the ReFeed Web/Mobile Portal. |
| Success end state | Account is successfully authenticated or created; `Supplier` account is in state `Active`, session token is issued, and Dashboard is displayed. |
| Failure end state | No account/session is created; error message displayed to prompt retry. |
| FRs covered | FR-01-01 Register/Login as a UCO supplier |
| Business rules applied | BR-01 (Unique Supplier Identifier & Tax ID validation) |
| Version / status | v1.1 · Updated to the revised W1 function list |
| Author | Nguyen Thuy Vi - 2411560050 |

## 1. Diagram

Notation follows the DBIZ2 activity profile (`mermaid-activity-patterns.md`). Rendered with
`swimlane-beta`, which needs Mermaid 11.16 or later.

```mermaid
---
config:
  flowchart:
    nodeSpacing: 60
    rankSpacing: 45
---


swimlane-beta LR
%% UC01: Register/Login as a UCO supplier (F-01)
%% ReFeed group project
subgraph supplier ["Supplier"]
initial@{ shape: f-circ, label: " " }
submitAuth("Submit Credentials or Registration Data")
choiceAuth{" "}
finalFail@{ shape: fr-circ, label: " " }
end


subgraph system ["ReFeed System"]
validateAuth("Validate Credentials and Compliance Data (FR-01-01)")
createAccount("Create Supplier Account with Unique Supplier_ID")
authenticateUser("Display Account Already Exists & Redirect to Login")
finalLogin@{ shape: fr-circ, label: " " }


displayAuthSuccess("Display Dashboard & Authentication Result")
finalSuccess@{ shape: fr-circ, label: " " }


displayAuthError("Display Error Message & Prompt Retry")
end


initial --> submitAuth
submitAuth --> validateAuth
validateAuth -->|Authentication Valid?| choiceAuth


choiceAuth -->|No - Invalid Data/Credentials| displayAuthError
displayAuthError --> finalFail


choiceAuth -->|Yes - New Registration| createAccount
createAccount --> displayAuthSuccess


choiceAuth -->|Yes - Existing Supplier Login| authenticateUser
authenticateUser --> finalLogin


displayAuthSuccess --> finalSuccess


%% Invisible link for layout only, not a process transition
finalLogin ~~~ displayAuthSuccess
```
Reading notes:

- The process starts when the **Supplier** submits authentication credentials or new account registration data to the ReFeed system.
- Decision `d1` validates the input credentials and compliance data. If invalid, the system displays the error message via `s2` and terminates at `stop2`.
- Decision `d2` branches between two scenarios: New account registration (generating a unique `Supplier_ID` and transitioning the account object `o1` to state `Active`) and Returning login for an existing registered supplier (`s5`).

## 2. Node trace

UC-01 has a single requirement, **FR-01-01 Register/Login as a UCO supplier**. It is tagged once in the
diagram, on the first system action. The other nodes belong to the same FR and are not tagged again.

| Node | Kind | Text | Traces to |
|---|---|---|---|
| s1 | Action | Validate Credentials and Compliance Data | **FR-01-01** Register/Login as a UCO supplier (the only node that carries the FR tag), BR-01 |
| d1 | Decision | Authentication valid? | BR-01 |
| s2 | Action | Display Error Message & Prompt Retry | Part of the FR-01-01 flow (not tagged again) |
| d2 | Decision | New registration or existing login? | Part of the FR-01-01 flow (not tagged again) |
| s3 | Action | Create Supplier Account with Unique Supplier_ID | Part of the FR-01-01 flow, BR-01 |
| o1 | Object node | Supplier Account [Active] | State value |
| s4 | Action | Display Dashboard & Authentication Result | Part of the FR-01-01 flow (not tagged again) |
| s5 | Action | Display Account Already Exists & Redirect to Login | Part of the FR-01-01 flow (not tagged again) |

## 3. Branches and acceptance tests

One row per outgoing branch of every decision. DBIZ3 turns each row into at least one test.

| Branch | Given | When | Then |
|---|---|---|---|
| d1 [no] | a supplier inputs invalid credentials or incomplete compliance data | they submit authentication data | no session is created and an error message is shown prompting retry |
| d1 [yes] → d2 [new registration] | a new supplier provides valid compliance data and credentials | they submit registration data | a unique Supplier_ID is generated, account state becomes Active, and Dashboard access is granted |
| d1 [yes] → d2 [existing login] | an existing registered supplier enters valid credentials | they attempt to login or register | system authenticates the user, notifies account existence, and redirects to login/dashboard |

## 4. Object states used

| Object | States that appear in this diagram | Full state list (all diagrams) |
|---|---|---|
| Supplier Account | Active | Pending Verification, Active, Suspended |

These state names become the allowed values of `supplier_account.status` in the DBIZ3 data model. Keep the
spelling identical everywhere.

## 5. Screens touched

Filled in during Lesson 8, from the Screen List.

| Supplier action | Screen ID |
|---|---|
| Submit Credentials or Registration Data | S-F01-01 (Authentication & Registration Portal) |
| See Error Message & Prompt Retry | S-F01-01 (Error State Feedback) |
| See Dashboard & Authentication Result | S-F01-02 (Supplier Dashboard) |
| See Login Redirection & Sign-in | S-F01-01 (Redirection Modal) |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: Is OTP/2FA required upon initial registration?] | No | Yes: Standard email/password authentication is verified first; 2FA can be configured post-registration. |
| 2 | [NEEDS CLARIFICATION: Does account activation require immediate manual compliance check by Admin?] | No | Account is created in `Active` status for basic portal access, with extended pickup functions locked until profile completeness (FR-01-02 / FR-01-03 in the revised W1). |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders (mermaid-cli 11.16.0) | Pass | | 28/11/2026 |
| Every decision has guards on all outgoing flows | Pass | | 06/11/2026 |
| FR-01-01 appears once in the diagram, with the exact W1 name | Pass | | 28/11/2026 |
| Every FR of UC-01 appears in the diagram | Pass (1 of 1) || 28/11/2026 |
| Lane names match the Context Diagram | Pass || 06/11/2026 |
| AI critic prompt run, findings resolved or logged | Pass, 2 findings → open questions 1 and 2| | 06/11/2026 |
