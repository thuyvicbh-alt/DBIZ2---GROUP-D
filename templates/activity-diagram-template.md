# Activity Diagram: AD-<UC-ID> <Use case name>

<!--
DBIZ2 Lesson 7 template. At most one file per use case, only when the use case needs one:
docs/activity/AD-<UC-ID>.md. One file never covers two use cases (that is a usage flow, Lesson 8).
Keep the Use Case ID unchanged from your Use Case inventory (W0 Table 1).
Delete the HTML comments as you go. Notation: templates/mermaid-activity-patterns.md
-->

| Field | Value |
|---|---|
| AD ID | `AD-<UC-ID>` e.g. AD-UC-04 |
| Use case | `<UC-ID> <name>` |
| Feature | `<F-ID> <feature name>` (W0 Table 2) |
| Primary actor | <from the use case; "none: started by time" for system-initiated flows> |
| Lanes | <actor lanes> · <your system, ONE lane, named as on the Context Diagram> · <external systems from the Context Diagram> |
| Inputs (L3-L5) | Context Diagram v<n> · Use Case Diagram v<n> · Sequence: `SD-<UC-ID>` · Architecture/ADR used · W1 rows |
| Trigger | <the event that starts the activity> |
| Preconditions | <what must already be true> |
| Success end state | <what is true when the main flow ends, in business words> |
| Failure end state(s) | <what is true when an alternative flow ends> |
| FRs covered | `<FR IDs from W1 for this use case>` |
| Business rules applied | `<BR IDs>` |
| Version / status | v0.1 · Draft / Reviewed |
| Author | <member> |

## 1. Diagram

<!-- Start from the course default pattern. Replace every label. Keep IDs short (a1, s1, d1...). -->

```mermaid
swimlane-beta TB
  subgraph ACT["<Actor>"]
    start@{ shape: sm-circ }
    a1("<verb + object>")
    stop1@{ shape: fr-circ }
  end
  subgraph SYS["<Your system>"]
    d1{"<question>?"}
    s1("<verb + object> (FR-xx-xx)")
    s2("<verb + object> (FR-xx-xx)")
    m1{" "}
  end
  start --> a1 --> d1
  d1 -->|"[<guard 1>]"| s1 --> m1
  d1 -->|"[<guard 2>]"| s2 --> m1
  m1 --> stop1
```

Reading notes (optional, two or three bullets):

- <anything a reader could misunderstand>

## 2. Node trace

<!-- Every system action and every decision gets a row. An action you cannot trace is either a
     missing FR (add it to W1 and tell the integrator) or an invented step (delete it). -->

| Node | Kind | Text | Traces to |
|---|---|---|---|
| s1 | Action | | FR-xx-xx |
| d1 | Decision | | FR-xx-xx / BR-xx |

## 3. Branches and acceptance tests

<!-- One row per outgoing branch of every decision. Write Given / When / Then with concrete values. -->

| Branch | Given | When | Then |
|---|---|---|---|
| d1 [<guard 1>] | | | |
| d1 [<guard 2>] | | | |

## 4. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
|---|---|---|
| | | |

## 5. Screens touched

<!-- Leave empty in Lesson 7. Fill in during Lesson 8 from your Screen List. -->

| Actor action | Screen ID |
|---|---|
| | |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: ...] | Yes / No | |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders on mermaid.live | | | |
| Every decision has guards on all outgoing flows, no overlap, no gap | | | |
| No action has two incoming flows (merge used instead) | | | |
| Every fork has a matching join | | | |
| Every system action cites an FR | | | |
| Every FR of this use case appears in the diagram | n of n | | |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | | | |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | | | |
| AI critic prompt run, findings resolved or logged as open questions | | | |

---

Template source: DBIZ2 Lesson 7, VJCBI College - FTU. Notation: OMG UML 2.5.1, Clause 15.
Position: the black-box activity diagram of a use case (MBSE Grid cell "Use Cases", item 2), the cell
left open in Prof. Seki's Lesson 4. The inside of the system stays in the Lesson 4 sequence diagrams.
Structure aligned with the DBIZ3 Spec Document, section 4 (Flows), so the file can be linked from a
module spec without rewriting.
