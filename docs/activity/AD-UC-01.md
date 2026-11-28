# Activity Diagram: AD-UC-01 Register / Login as a UCO supplier

| Field | Value |
|---|---|
| AD ID | AD-UC-01 |
| Use case | UC-01 Register/Login as a UCO supplier |
| Feature | F-01 Supplier Onboarding & Pickup Management |
| Primary actor | Supplier |
| Lanes | Supplier · ReFeed system |
| Inputs (L3-L5) | Context Diagram v2 (Supplier) · Use Case Diagram v2 · Sequence: SD-UC-01 · Architecture v2 · W1 rows FR-01-01 to FR-01-06 |
| Trigger | The supplier submits credentials or registration data on the portal |
| Preconditions | The supplier has access to the ReFeed Web/Mobile Portal. |
| Success end state | A new supplier has an `Active` account and sees the dashboard, or a returning supplier is authenticated. |
| Failure end state | No account or session is created; the supplier sees an error message and is prompted to retry. |
| FRs covered | FR-01-01 Validate Credentials and Compliance Data · FR-01-02 Create Supplier Account with Unique Supplier_ID · FR-01-03 Display Account Already Exists & Redirect to Login · FR-01-04 Display Dashboard & Authentication Result · FR-01-05 Display Error Message & Prompt Retry · FR-01-06 Authenticate returning supplier |
| Business rules applied | BR-01 (Unique Supplier Identifier & Tax ID validation) |
| Version / status | v2.0 · Rebuilt on the current W1 function list, each FR appears once |
| Author | Nguyen Thuy Vi - 2411560050 |

## 1. Diagram

Notation follows the DBIZ2 activity profile (`mermaid-activity-patterns.md`). Rendered with
`swimlane-beta`, which needs Mermaid 11.16 or later.

```mermaid
swimlane-beta TB
  subgraph SUP["Supplier"]
    start@{ shape: sm-circ }
    a1("Submit credentials or registration data")
    a2("Sign in with existing credentials")
    stop1@{ shape: fr-circ }
    stop2@{ shape: fr-circ }
  end
  subgraph SYS["ReFeed system"]
    s1("Validate Credentials and Compliance Data (FR-01-01)")
    d1{"Authentication<br>valid?"}
    s2("Display Error Message & Prompt Retry (FR-01-05)")
    d2{"New registration or<br>existing supplier?"}
    s3("Create Supplier Account with Unique Supplier_ID (FR-01-02)")
    o1["Supplier Account [Active]"]
    s4("Display Dashboard & Authentication Result (FR-01-04)")
    s5("Display Account Already Exists & Redirect to Login (FR-01-03)")
    s6("Authenticate returning supplier (FR-01-06)")
  end
  start --> a1 --> s1 --> d1
  d1 -->|"[no]"| s2 --> stop1
  d1 -->|"[yes]"| d2
  d2 -->|"[new registration]"| s3 --> o1 --> s4 --> stop2
  d2 -->|"[existing supplier]"| s5 --> a2 --> s6 --> stop2
```

Reading notes:

- Every action in the system lane carries one FR with its exact W1 name, and each FR appears once: `s1` FR-01-01, `s3` FR-01-02, `s5` FR-01-03, `s4` FR-01-04, `s2` FR-01-05, `s6` FR-01-06.
- Supplier actions and the object node `o1` carry no FR. Decisions follow the LabDesk example: the label has no tag, and section 2 gives the FR that decides each branch.
- A supplier who already has an account is told so and redirected to login (`s5`), signs in (`a2`) and is authenticated (`s6`).
- Two flows enter `stop2`. This is allowed: a final node is not an action.

## 2. Node trace

Every action in the system lane cites a requirement. Every decision cites the rule or requirement
that decides it.

| Node | Kind | Text | Traces to |
|---|---|---|---|
| s1 | Action | Validate Credentials and Compliance Data | FR-01-01, BR-01 |
| d1 | Decision | Authentication valid? | FR-01-01, FR-01-05, BR-01 |
| s2 | Action | Display Error Message & Prompt Retry | FR-01-05 |
| d2 | Decision | New registration or existing supplier? | FR-01-02, FR-01-03 |
| s3 | Action | Create Supplier Account with Unique Supplier_ID | FR-01-02, BR-01 |
| o1 | Object node | Supplier Account [Active] | FR-01-02 (state value) |
| s4 | Action | Display Dashboard & Authentication Result | FR-01-04 |
| s5 | Action | Display Account Already Exists & Redirect to Login | FR-01-03 |
| s6 | Action | Authenticate returning supplier | FR-01-06 |

## 3. Branches and acceptance tests

One row per outgoing branch of every decision. DBIZ3 turns each row into at least one test.

| Branch | Given | When | Then |
|---|---|---|---|
| d1 [no] | a supplier submits invalid credentials or incomplete compliance data | they submit the data | no account or session is created and an error message prompts a retry |
| d1 [yes] → d2 [new registration] | a new supplier submits valid registration data | they submit the data | an Active account with a unique Supplier_ID is created and the dashboard and authentication result are shown |
| d1 [yes] → d2 [existing supplier] | the supplier already has an account | they submit registration data | the system says the account already exists and redirects to login; a valid login then authenticates the supplier |

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
| Submit credentials or registration data, see error and retry prompt | S-F01-01 (Supplier registration) |
| See account-already-exists message, sign in with existing credentials | S-F00-01 (Sign in) |
| See dashboard and authentication result | S-F00-02 (Supplier home) |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: is OTP/2FA required at registration?] | No | Standard email and password first; 2FA can be configured after registration. |
| 2 | [NEEDS CLARIFICATION: does a returning supplier see the dashboard after `FR-01-06`?] | No | Yes, the supplier lands on S-F00-02. W1 `FR-01-04` only names the new-account trigger; add the returning case to its wording when W1 is next revised. |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders (mermaid-cli 11.16.0) | Pass | | 28/11/2026 |
| Every decision has guards on all outgoing flows | Pass | | 28/11/2026 |
| Every system action cites an FR | Pass (6 of 6) | | 28/11/2026 |
| Every FR of UC-01 appears in the diagram | Pass (FR-01-01 to FR-01-06) | | 28/11/2026 |
| Each FR appears exactly once, with the exact W1 name | Pass | | 28/11/2026 |
| Lane names match the Context Diagram | Pass | | 28/11/2026 |
