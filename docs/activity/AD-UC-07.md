# AD-UC-07: Input Handover Information

| Field | Value |
|---|---|
| AD ID | AD-UC-07 |
| Use case | UC-07 Input Handover Information |
| Feature | F-02 |
| Primary actor | Driver / Collector |
| Lanes | Driver / Collector · System |
| Inputs (L3-L5) | Context Diagram · Use Case Diagram · W1 rows `FR-02-14` to `FR-02-18` (`FR-02-14-FE`, `FR-02-14-BE`, `FR-02-15-BE`, `FR-02-16-BE`, `FR-02-17-FE`, `FR-02-17-BE`, `FR-02-18-FE`, `FR-02-18-BE`) |
| Trigger | The driver / collector submits or updates UCO handover information. |
| Preconditions | The driver / collector is logged into the mobile application, has an assigned pickup, and has confirmed arrival at the supplier point (`UC-06` / `FR-02-09`). |
| Success end state | The handover information is validated and stored (`FR-02-15`), transaction amount is calculated (`FR-02-16`), and a draft agreement is generated and displayed (`FR-02-17`). |
| Failure end state | The submission is rejected without saving a handover record; the driver is prompted to correct invalid input (`FR-02-18`). |
| FRs covered | `FR-02-14`, `FR-02-15`, `FR-02-16`, `FR-02-17`, `FR-02-18` (`FR-02-14-FE`, `FR-02-14-BE`, `FR-02-15-BE`, `FR-02-16-BE`, `FR-02-17-FE`, `FR-02-17-BE`, `FR-02-18-FE`, `FR-02-18-BE`) |
| Business rules applied | Handover validation rule (`FR-02-14`, `FR-02-18`): actual quantity, handover time, and required evidence must be valid before persistence. Transaction pricing rule (`FR-02-16`): amount is calculated from actual quantity, base price, and the supplier's current loyalty bonus rate. |
| Version / status | v1.1 · Aligned with W1 Function List |
| Author | ReFeed Group D |

## 1. Diagram

Notation follows the DBIZ2 activity profile (`mermaid-activity-patterns.md`). Rendered with `swimlane-beta`. Every action node maps 1-to-1 to a distinct L3 work item in W1 (`-FE` for Driver / Collector actions, `-BE` for System actions); control nodes (`start1`, `m1`, `d1`, `stop1`) do not add functions.

```mermaid
swimlane-beta LR
  subgraph DVR["Driver / Collector"]
    start1@{ shape: sm-circ }
    a1("Capture & submit actual quantity, handover time, and evidence (FR-02-14-FE)")
    a6("See rejection prompt & correct invalid information (FR-02-18-FE)")
    a8("Display generated draft handover agreement (FR-02-17-FE)")
  end
  subgraph SYS["System"]
    m1{" "}
    a2("Validate actual quantity, handover time, and evidence (FR-02-14-BE)")
    d1{"Is information valid?"}
    a3("Store handover record against assigned pickup (FR-02-15-BE)")
    a4("Calculate transaction amount (FR-02-16-BE)")
    a5("Generate draft agreement (FR-02-17-BE)")
    a7("Reject and prompt for correction (FR-02-18-BE)")
    stop1@{ shape: fr-circ }
  end

  start1 --> a1 --> m1
  m1 --> a2 --> d1
  d1 -->|"[Valid]"| a3 --> a4 --> a5 --> a8 --> stop1
  d1 -->|"[Invalid]"| a7 --> a6 --> m1
```

## 2. Node trace

Every action node cites a unique W1 L3 Function List ID. Control nodes (`start1`, `m1`, `d1`, `stop1`) govern control flow only and do not add functions.

| Node | Kind | Text | Traces to |
|---|---|---|---|
| `a1` | Actor action | Capture & submit actual quantity, handover time, and evidence | `FR-02-14-FE` |
| `m1` | Merge | Merge initial submission and correction loop | Control flow (no separate FR) |
| `a2` | Action | Validate actual quantity, handover time, and evidence | `FR-02-14-BE` |
| `d1` | Decision | Is information valid? | `FR-02-14` / `FR-02-18` (validation rule) |
| `a3` | Action | Store handover record against assigned pickup | `FR-02-15-BE` |
| `a4` | Action | Calculate transaction amount | `FR-02-16-BE` |
| `a5` | Action | Generate draft agreement | `FR-02-17-BE` |
| `a8` | Actor action | Display generated draft handover agreement | `FR-02-17-FE` |
| `a7` | Action | Reject and prompt for correction | `FR-02-18-BE` |
| `a6` | Actor action | See rejection prompt & correct invalid information | `FR-02-18-FE` |

## 3. Branches and acceptance tests

One row per outgoing branch of every decision. DBIZ3 turns each row into at least one test.

| Branch | Given | When | Then |
|---|---|---|---|
| `d1` `[Valid]` | The driver inputs valid actual quantity (e.g. 50 kg), handover time, and uploads required evidence photo | They submit the handover form | The handover record is stored (`FR-02-15-BE`), transaction amount is calculated (`FR-02-16-BE`), and a draft agreement is generated (`FR-02-17-BE`) and displayed (`FR-02-17-FE`). |
| `d1` `[Invalid]` | The driver leaves the mandatory actual quantity field blank or has no required evidence photo | They submit the handover form | No record is saved; the submission is rejected (`FR-02-18-BE`) and the app displays validation feedback prompting to fill all mandatory fields (`FR-02-18-FE`). |

## 4. Object states used

| Object | States that appear in this diagram | Full state list (all diagrams) |
|---|---|---|
| Handover Record | Validated, Draft | Draft, Validated, Rejected |

These state names become the allowed values of `handover.status` in the DBIZ3 data model. Keep the spelling identical everywhere.

## 5. Screens touched

Filled in during Lesson 8, from the Screen List.

| Driver / Collector action | Screen ID |
|---|---|
| Capture & submit actual quantity, handover time, and evidence (`FR-02-14-FE`) | S-F02-03 (Mobile Handover Form) |
| See rejection prompt & correct invalid information (`FR-02-18-FE`) | S-F02-03 (Mobile form with error state) |
| Display generated draft handover agreement (`FR-02-17-FE`) | S-F02-03 (Mobile Draft Agreement View) |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | `[NEEDS CLARIFICATION: What are the specific rules for 'evidence' in FR-02-14 (e.g., photo file type, size limits, or GPS location match)?]` (W1 marks `FR-02-14` `EARS precise? = No` for this reason.) | No | Photo upload via mobile app is assumed as evidence for now. |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders on mermaid.live | Pass | | |
| Every decision has guards on all outgoing flows | Pass | | |
| Every fork has a matching join | N/A (no forks) | | |
| Every action node cites a unique W1 Function ID | Pass (8 of 8 L3 items: `FR-02-14-FE` to `FR-02-18-BE`) | | |
| Every FR of UC-07 appears in the diagram | Pass (5 of 5 L2; 8 of 8 L3) | | |
| Lane names match the Context Diagram | Pass (`Driver / Collector · System`) | | |
| AI critic prompt run, findings resolved or logged | Pass, 1 finding → open question 1 | | |
