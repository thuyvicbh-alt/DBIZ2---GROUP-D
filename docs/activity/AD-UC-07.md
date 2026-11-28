# AD-UC-07: Input Handover Information

| Field                  | Value                                                                                                                                                 |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| AD ID                  | AD-UC-07                                                                                                                                              |
| Use case               | UC-07 Input Handover Information                                                                                                                      |
| Feature                | F-02 Collection Operations & Handover                                                                                                                 |
| Primary actor          | Driver / Collector                                                                                                                                    |
| Lanes                  | Driver / Collector · System                                                                                                                           |
| Inputs (L3-L5)         | Context Diagram · Use Case Diagram · CVCA (`StR-03`, `StR-06`, `P-3`) · Architecture · W1 rows `FR-02-14` to `FR-02-18` (`FR-02-14-FE`, `FR-02-14-BE`, `FR-02-15-BE`, `FR-02-16-BE`, `FR-02-17-FE`, `FR-02-17-BE`, `FR-02-18-FE`, `FR-02-18-BE`) |
| Trigger                | The driver / collector submits handover information for an assigned pickup.                                                                           |
| Preconditions          | The driver / collector is logged into the mobile application, has an assigned pickup, and has confirmed arrival at the supplier point (`UC-06` / `FR-02-09`). |
| Success end state      | The handover record is validated and stored against the assigned pickup (`FR-02-15`), the transaction amount is calculated (`FR-02-16`), and the draft handover agreement is generated and displayed to the driver / collector (`FR-02-17`). |
| Failure end state      | If handover information is missing or invalid, the submission is rejected without saving a handover record, and validation feedback prompts the driver / collector to correct and resubmit (`FR-02-18`). |
| FRs covered            | `FR-02-14`, `FR-02-15`, `FR-02-16`, `FR-02-17`, `FR-02-18` (8 L3 work items: `FR-02-14-FE`, `FR-02-14-BE`, `FR-02-15-BE`, `FR-02-16-BE`, `FR-02-17-FE`, `FR-02-17-BE`, `FR-02-18-FE`, `FR-02-18-BE`) |
| Business rules applied | Handover validation rule (`FR-02-14`, `FR-02-18`): actual quantity, handover time, and required evidence must be valid before persisting. Transaction pricing rule (`FR-02-16`): amount is calculated from actual quantity, base price, and the supplier's current loyalty bonus rate. |
| Version / status       | v1.1 · Aligned with Refined W1 Function List                                                                                                          |
| Author                 | ReFeed Group D                                                                                                                                        |

## 1. Diagram

Notation follows the DBIZ2 activity profile (`mermaid-activity-patterns.md`). Rendered with `swimlane-beta`. Every action node maps 1-to-1 to a distinct L3 work item in W1 (`-FE` for Driver / Collector actions, `-BE` for System actions); control nodes (`start1`, `m1`, `d1`, `stop1`) do not carry function IDs.

```mermaid
swimlane-beta LR
  subgraph SYS["System"]
    m1{" "}
    a2("Validate actual quantity, handover time, and evidence [FR-02-14-BE]")
    d1{"Is information valid?"}
    a3("Store handover record against assigned pickup [FR-02-15-BE]")
    a4("Calculate transaction amount [FR-02-16-BE]")
    a5("Generate draft agreement [FR-02-17-BE]")
    a7("Reject and prompt for correction [FR-02-18-BE]")
    stop1@{ shape: fr-circ }
  end
  subgraph DVR["Driver / Collector"]
    start1@{ shape: sm-circ }
    a1("Capture & submit actual quantity, handover time, and evidence [FR-02-14-FE]")
    a6("See rejection prompt & correct invalid information [FR-02-18-FE]")
    a8("Display generated draft handover agreement [FR-02-17-FE]")
  end

  start1 --> a1 --> m1
  m1 --> a2 --> d1
  d1 -->|"[Valid]"| a3 --> a4 --> a5 --> a8 --> stop1
  d1 -->|"[Invalid]"| a7 --> a6 --> m1
```

## 2. Node trace

Every action node cites a unique W1 L3 Function List ID. Control nodes (`start1`, `m1`, `d1`, `stop1`) govern control flow only and do not add functions.

| Node     | Kind           | Lane               | Text                                                                 | Traces to (W1 L3 ID)                                      |
| -------- | -------------- | ------------------ | -------------------------------------------------------------------- | --------------------------------------------------------- |
| `start1` | Initial node   | Driver / Collector | Start handover input                                                 | Control flow (`UC-07` trigger)                            |
| `a1`     | Actor action   | Driver / Collector | Capture & submit actual quantity, handover time, and evidence        | `FR-02-14-FE`                                             |
| `m1`     | Merge node     | System             | Merge initial submission and post-rejection correction loop          | Control flow (no separate FR)                             |
| `a2`     | System action  | System             | Validate actual quantity, handover time, and evidence                | `FR-02-14-BE`                                             |
| `d1`     | Decision       | System             | Is information valid?                                                | Control flow (`FR-02-14` / `FR-02-18` validation outcome) |
| `a3`     | System action  | System             | Store handover record against assigned pickup                        | `FR-02-15-BE`                                             |
| `a4`     | System action  | System             | Calculate transaction amount                                         | `FR-02-16-BE`                                             |
| `a5`     | System action  | System             | Generate draft agreement                                             | `FR-02-17-BE`                                             |
| `a8`     | Actor action   | Driver / Collector | Display generated draft handover agreement                           | `FR-02-17-FE`                                             |
| `a7`     | System action  | System             | Reject and prompt for correction                                     | `FR-02-18-BE`                                             |
| `a6`     | Actor action   | Driver / Collector | See rejection prompt & correct invalid information                   | `FR-02-18-FE` (resubmits into `m1`)                       |
| `stop1`  | Activity final | System             | Handover recorded and draft agreement ready for supplier signature   | Control flow (`UC-07` success end; hands off to `UC-08`)  |

## 3. Branches and acceptance tests

One row per outgoing branch of every decision. DBIZ3 turns each row into at least one test.

| Branch           | Given                                                                                              | When                          | Then                                                                                                                                                 |
| ---------------- | -------------------------------------------------------------------------------------------------- | ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| `d1` `[Valid]`   | The driver inputs valid actual quantity (e.g., 50 kg), handover time, and required evidence photo | They submit the handover form | `FR-02-15-BE` stores the handover record, `FR-02-16-BE` calculates the transaction amount, `FR-02-17-BE` generates the draft agreement, and `FR-02-17-FE` displays it. |
| `d1` `[Invalid]` | The driver leaves the mandatory actual quantity field blank or omits required evidence             | They submit the handover form | No handover record is saved; `FR-02-18-BE` rejects the submission and `FR-02-18-FE` displays validation feedback prompting correction and resubmission. |

## 4. Object states used

| Object                      | States that appear in this diagram | Full state list (all diagrams)                                                |
| --------------------------- | ---------------------------------- | ----------------------------------------------------------------------------- |
| Handover Agreement / Record | `Draft` (unsigned draft generated) | `Draft` (`FR-02-17`, `FR-03-01`), `Locked` / `Signed` (`FR-03-06`, `FR-03-07`) |

These state names become the allowed status values in the DBIZ3 data model. Keep the spelling identical everywhere.

## 5. Screens touched

Filled in during Lesson 8, from the Screen List.

| Driver / Collector action                                              | W1 Function ID | Screen ID                                      |
| ---------------------------------------------------------------------- | -------------- | ---------------------------------------------- |
| Capture & submit actual quantity, handover time, and evidence (`a1`)   | `FR-02-14-FE`  | S-F02-03 (Mobile Handover Form)                |
| See rejection prompt & correct invalid information (`a6`)              | `FR-02-18-FE`  | S-F02-03 (Mobile Handover Form — Error State)  |
| Display generated draft handover agreement (`a8`)                      | `FR-02-17-FE`  | S-F02-03 (Mobile Draft Agreement Preview)      |

## 6. Open questions

| # | Question                                                                                                                                                                  | Blocking? | Default applied                                               |
| - | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- | ------------------------------------------------------------- |
| 1 | `[NEEDS CLARIFICATION: What are the specific validation rules for 'evidence' in FR-02-14 (e.g., required photo file type, size limits, or GPS proximity check)?]` (W1 marks `FR-02-14` `EARS precise? = No` for this reason.) | No        | Photo upload via mobile app is assumed as required evidence for Release 1. |

## 7. Verification log

| Check                                                | Result                                                                                                                              | By     | Date   |
| ---------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- | ------ | ------ |
| Renders on mermaid.live                              | Pass                                                                                                                                |        |        |
| Every decision has guards on all outgoing flows      | Pass (`d1`: `[Valid]`, `[Invalid]`)                                                                                                 |        |        |
| No action has two incoming flows; merge node used    | Pass (`m1` merges `a1` and `a6` before `a2`)                                                                                        |        |        |
| Every fork has a matching join                       | N/A (no forks)                                                                                                                      |        |        |
| Every action box cites a unique W1 Function ID       | Pass — 8 action boxes map 1-to-1 to `FR-02-14-FE`, `FR-02-14-BE`, `FR-02-15-BE`, `FR-02-16-BE`, `FR-02-17-FE`, `FR-02-17-BE`, `FR-02-18-FE`, `FR-02-18-BE` |        |        |
| Every FR of `UC-07` appears in the diagram           | Pass (5 of 5 L2 functions: `FR-02-14`–`FR-02-18`; 8 of 8 L3 work items)                                                             |        |        |
| Lane names match the Context Diagram & W1            | Pass (`Driver / Collector` · `System`)                                                                                              |        |        |
| AI critic prompt run, findings resolved or logged    | Pass, 1 finding → open question 1 (`FR-02-14` evidence rules)                                                                       |        |        |
