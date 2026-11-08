# Activity Diagram: AD-UC-02 Manage Point-of-Origin and compliance profile

| Field                  | Value                                                                         |
| ---------------------- | ----------------------------------------------------------------------------- |
| AD ID                  | AD-UC-02                                                                      |
| Use case               | UC-02 Manage Point-of-Origin and compliance profile                           |
| Feature                | F-01                                                                          |
| Primary actor          | Supplier                                                                      |
| Lanes                  | Supplier · System                                                             |
| Inputs (L3-L5)         | Context Diagram · Use Case Diagram · W1 row FR-01-02                          |
| Trigger                | The supplier submits or updates their point-of-origin and compliance profile. |
| Preconditions          | The supplier is registered/logged in.                                         |
| Success end state      | The profile is validated and stored against the Supplier\_ID.                 |
| Failure end state      | The profile information is rejected; the supplier is prompted to correct it.  |
| FRs covered            | FR-01-02                                                                      |
| Business rules applied | (N/A or BR regarding compliance validation)                                   |
| Version / status       | v1.0 · Draft                                                                  |
| Author                 | (team member)                                                                 |

## 1. Diagram

Notation follows the DBIZ2 activity profile (`mermaid-activity-patterns.md`). Rendered with `swimlane-beta`.
``` mermaid
swimlane-beta LR
  subgraph SYS["System"]
    m1{" "}
    a2("Validate profile and compliance records (FR-01-02)")
    d1{"Is information valid?"}
    a3("Store profile against Supplier_ID (FR-01-02)")
    a5("Reject and prompt for correction (FR-01-02)")
    stop1@{ shape: fr-circ }
  end
  subgraph SUP["Supplier"]
    start1@{ shape: sm-circ }
    a1("Submit or update compliance information")
    a4("Correct invalid information")
  end

  start1 --> a1 --> m1
  m1 --> a2 --> d1
  d1 -->|"[Valid]"| a3 --> stop1
  d1 -->|"[Invalid]"| a5 --> a4 --> m1
```
## 2. Node trace

Every action in the system lane cites a requirement. Every decision cites the rule or requirement that decides it.

| Node | Kind     | Text                                    | Traces to                           |
| ---- | -------- | --------------------------------------- | ----------------------------------- |
| a2   | Action   | Validate profile and compliance records | FR-01-02                            |
| d1   | Decision | Is information valid?                   | FR-01-02 (implicit validation rule) |
| a3   | Action   | Store profile against Supplier\_ID      | FR-01-02                            |
| a5   | Action   | Reject and prompt for correction        | FR-01-02                            |

## 3. Branches and acceptance tests

One row per outgoing branch of every decision. DBIZ3 turns each row into at least one test.

| Branch        | Given                                                                    | When                            | Then                                                                                      |
| ------------- | ------------------------------------------------------------------------ | ------------------------------- | ----------------------------------------------------------------------------------------- |
| d1 \[Valid]   | the supplier inputs a valid 10-digit Tax ID and accurate GPS coordinates | they submit the compliance form | one Profile in state Validated is stored against the Supplier\_ID                         |
| d1 \[Invalid] | the supplier leaves the mandatory Tax ID field blank                     | they submit the compliance form | no profile is saved; the form reloads with an error message explaining the missing Tax ID |

## 4. Object states used

| Object  | States that appear in this diagram | Full state list (all diagrams) |
| ------- | ---------------------------------- | ------------------------------ |
| Profile | Validated, Rejected                | Draft, Validated, Rejected     |

These state names become the allowed values of `profile.status` in the DBIZ3 data model. Keep the spelling identical everywhere.

## 5. Screens touched

Filled in during Lesson 8, from the Screen List.

| Supplier action                         | Screen ID                                      |
| --------------------------------------- | ---------------------------------------------- |
| Submit or update compliance information | S-F01-02 (Point-of-Origin and compliance form) |
| Correct invalid information             | S-F01-02 (form with error state)               |

## 6. Open questions

| # | Question                                                                                                                    | Blocking? | Default applied                                               |
| - | --------------------------------------------------------------------------------------------------------------------------- | --------- | ------------------------------------------------------------- |
| 1 | \[NEEDS CLARIFICATION: What exact fields make up the "compliance information" (e.g., Tax ID, Legal Name, Operating Hours)?] | No        | Basic fields (Name, Phone, Address, GPS) are assumed for now. |

## 7. Verification log

| Check                                             | Result                            | By     | Date   |
| ------------------------------------------------- | --------------------------------- | ------ | ------ |
| Renders on mermaid.live                           | Pass                              | <br /> | <br /> |
| Every decision has guards on all outgoing flows   | Pass                              | <br /> | <br /> |
| Every fork has a matching join                    | N/A (no forks)                    | <br /> | <br /> |
| Every system action cites an FR                   | Pass                              | <br /> | <br /> |
| Every FR of UC-02 appears in the diagram          | Pass (1 of 1)                     | <br /> | <br /> |
| Lane names match the Context Diagram              | Pass                              | <br /> | <br /> |
| AI critic prompt run, findings resolved or logged | Pass, 1 finding → open question 1 | <br /> | <br /> |

