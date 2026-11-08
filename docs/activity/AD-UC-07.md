# AD-UC-07: Input Handover Information

| Field                  | Value                                                                                               |
| ---------------------- | --------------------------------------------------------------------------------------------------- |
| AD ID                  | AD-UC-07                                                                                            |
| Use case               | UC-07 Input Handover Information                                                                    |
| Feature                | F-02                                                                                                |
| Primary actor          | Driver / Collector                                                                                  |
| Lanes                  | Driver / Collector · System                                                                         |
| Inputs (L3-L5)         | Context Diagram · Use Case Diagram · W1 row FR-02-03                                                |
| Trigger                | The driver / collector submits or updates UCO handover information.                                 |
| Preconditions          | The driver / collector is logged into the mobile application and has an assigned pickup.            |
| Success end state      | The handover information is validated and stored, and a draft agreement is generated.               |
| Failure end state      | The submission is rejected; the driver is prompted to correct invalid input.                        |
| FRs covered            | FR-02-03                                                                                            |
| Business rules applied | (N/A or BR regarding handover validation)                                                           |
| Version / status       | v1.0 · Draft                                                                                        |
| Author                 | (team member)                                                                                       |

## 1. Diagram

Notation follows the DBIZ2 activity profile (`mermaid-activity-patterns.md`). Rendered with `swimlane-beta`.

```mermaid
swimlane-beta LR
  subgraph SYS["System"]
    m1{" "}
    a2("Validate actual quantity, handover time, and evidence (FR-02-03)")
    d1{"Is information valid?"}
    a3("Store handover record against assigned pickup (FR-02-03)")
    a4("Calculate transaction amount (FR-02-03)")
    a5("Generate draft agreement (FR-02-03-BE)")
    a7("Reject and prompt for correction (FR-02-03)")
    stop1@{ shape: fr-circ }
  end
  subgraph DVR["Driver / Collector"]
    start1@{ shape: sm-circ }
    a1("Capture & submit actual quantity, handover time, and evidence")
    a6("Correct invalid information")
  end

  start1 --> a1 --> m1
  m1 --> a2 --> d1
  d1 -->|"[Valid]"| a3 --> a4 --> a5 --> stop1
  d1 -->|"[Invalid]"| a7 --> a6 --> m1
```

## 2. Node trace

Every action in the system lane cites a requirement. Every decision cites the rule or requirement that decides it.

| Node | Kind     | Text                                                    | Traces to                           |
| ---- | -------- | ------------------------------------------------------- | ----------------------------------- |
| a2   | Action   | Validate actual quantity, handover time, and evidence   | FR-02-03                            |
| d1   | Decision | Is information valid?                                   | FR-02-03 (implicit validation rule) |
| a3   | Action   | Store handover record against assigned pickup           | FR-02-03                            |
| a4   | Action   | Calculate transaction amount                            | FR-02-03                            |
| a5   | Action   | Generate draft agreement                                | FR-02-03-BE                         |
| a7   | Action   | Reject and prompt for correction                        | FR-02-03                            |

## 3. Branches and acceptance tests

One row per outgoing branch of every decision. DBIZ3 turns each row into at least one test.

| Branch        | Given                                                                                 | When                              | Then                                                                                                 |
| ------------- | ------------------------------------------------------------------------------------- | --------------------------------- | ---------------------------------------------------------------------------------------------------- |
| d1 \[Valid]   | the driver inputs valid actual quantity (e.g. 50L), handover time, and upload evidence photo | they submit the handover form     | the handover record is stored, transaction amount is calculated, and a draft agreement is generated  |
| d1 \[Invalid] | the driver leaves the mandatory actual quantity field blank or has no evidence photo  | they submit the handover form     | no record is saved; the app reloads with an error message prompting to fill all mandatory fields     |

## 4. Object states used

| Object          | States that appear in this diagram | Full state list (all diagrams) |
| --------------- | ---------------------------------- | ------------------------------ |
| Handover Record | Validated, Rejected                | Draft, Validated, Rejected     |

These state names become the allowed values of `handover.status` in the DBIZ3 data model. Keep the spelling identical everywhere.

## 5. Screens touched

Filled in during Lesson 8, from the Screen List.

| Driver / Collector action                                 | Screen ID                                      |
| --------------------------------------------------------- | ---------------------------------------------- |
| Capture & submit actual quantity, handover time, and evidence | S-F02-03 (Mobile Handover Form)                |
| Correct invalid information                               | S-F02-03 (Mobile form with error state)        |

## 6. Open questions

| # | Question                                                                                                                                  | Blocking? | Default applied                                               |
| - | ----------------------------------------------------------------------------------------------------------------------------------------- | --------- | ------------------------------------------------------------- |
| 1 | \[NEEDS CLARIFICATION: What are the specific rules for 'evidence' (e.g., photo file type, size limits, or GPS location match)?]         | No        | Photo upload via mobile app is assumed as evidence for now.   |

## 7. Verification log

| Check                                             | Result                            | By     | Date   |
| ------------------------------------------------- | --------------------------------- | ------ | ------ |
| Renders on mermaid.live                           | Pass                              | <br /> | <br /> |
| Every decision has guards on all outgoing flows   | Pass                              | <br /> | <br /> |
| Every fork has a matching join                    | N/A (no forks)                    | <br /> | <br /> |
| Every system action cites an FR                   | Pass                              | <br /> | <br /> |
| Every FR of UC-07 appears in the diagram          | Pass (1 of 1)                     | <br /> | <br /> |
| Lane names match the Context Diagram              | Pass                              | <br /> | <br /> |
| AI critic prompt run, findings resolved or logged | Pass, 1 finding → open question 1 | <br /> | <br /> |
