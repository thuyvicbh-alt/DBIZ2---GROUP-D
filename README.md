# ReFeed System — DBIZ2 Group D

ReFeed System is a digital platform for collecting used cooking oil (UCO), coordinating pickup operations, recording signed handovers, verifying warehouse receipts, and preparing traceability and compliance records. This repository is Group D's DBIZ2 Digital Business Project II package for the ReFeed Vietnam case.

The package is the agreed baseline for the DBIZ3 implementation handover. It contains the project context, requirements, architecture decisions, business analysis, activity diagrams, usage flows, screen inventory, screen transitions, screen specifications, and shared UI conventions.

## Product context

Suppliers currently arrange UCO pickups through calls or Zalo. ReFeed Staff plan routes manually, collection quantities and payment details are recorded at handover, and warehouse discrepancies may be discovered late. ReFeed System gives suppliers, drivers / collectors, warehouse staff, and ReFeed Staff a shared record from pickup request through route dispatch, collection, signed handover, warehouse verification, payout-balance visibility, and audit review.

Release 1 covers the core operational flow:

- supplier registration, Point-of-Origin and compliance profile, and pickup requests;
- route planning and dispatch, driver collection, and handover evidence;
- supplier transaction review and electronic signature;
- warehouse receiving, quantity discrepancy handling, UCO batches, and mass balance;
- supplier loyalty, payout-balance visibility, Green Profile, and ESG reports;
- ReFeed operations monitoring, audit trail, and compliance reporting.

In-app payout transfers, real-time driver–supplier chat, and end-to-end supply-chain trace queries are outside the current release boundary or deferred to Release 2. Any payout transfer is executed outside the app.

## Documentation map

Each lesson writes to its own folder. Cross-document references use stable IDs instead of repeating the same requirements in multiple places.

```text
docs/
  project/            Lessons 1-2   project-brief.md, ai-log.md
  context/            Lesson 3      cvca.md, context-diagram.md
  use-case/           Lesson 4      use-case-diagram.md, SD-<UC-ID>.md
  architecture/       Lesson 4      architecture.md, adr/ADR-<nnn>-<title>.md
  function-list/      Lessons 5-6   w0-scope.md ... w6-cash-flow.md
  activity/           Lesson 7      activity-index.md, AD-<UC-ID>.md
  external-design/    Lesson 8      usage-flow.md, screen-list.md,
                                    screen-transition.md, ui-conventions.md
    screens/          Lesson 8      screen-spec-S-<Screen ID>.md
      img/            Lesson 8      <Screen ID>.png
```

## Recommended reading order

| Step | Area | File | Purpose |
|---|---|---|---|
| 1 | Project brief | [project-brief.md](docs/project/project-brief.md) | Product problem, outcomes, scope boundary, actors, and milestones |
| 2 | Context | [cvca.md](docs/context/cvca.md), [context-diagram.md](docs/context/context-diagram.md) | Stakeholders, information and money flows, and system boundary |
| 3 | Use cases | [use-case-diagram.md](docs/use-case/use-case-diagram.md), [SD-UC-*.md](docs/use-case/) | Use-case inventory and interaction sequences |
| 4 | Architecture | [architecture.md](docs/architecture/architecture.md), [adr/](docs/architecture/adr/) | System requirements, layers, integrations, and architecture decisions |
| 5 | Function list | [w0-scope.md](docs/function-list/w0-scope.md), [w1-function-list.md](docs/function-list/w1-function-list.md) | Features, FRs, BRs, EARS criteria, and work items |
| 6 | Priorities and appraisal | [w2-cost.md](docs/function-list/w2-cost.md), [w4-priorities.md](docs/function-list/w4-priorities.md), [w5-handover.md](docs/function-list/w5-handover.md), [w6-cash-flow.md](docs/function-list/w6-cash-flow.md) | Cost, priority, handover scope, and financial appraisal |
| 7 | Activities | [activity-index.md](docs/activity/activity-index.md), [AD-UC-*.md](docs/activity/) | Actor/system actions and FR coverage |
| 8 | External design | [usage-flow.md](docs/external-design/usage-flow.md) | One operational flow for each actor |
| 9 | External design | [screen-list.md](docs/external-design/screen-list.md), [screen-transition.md](docs/external-design/screen-transition.md), [ui-conventions.md](docs/external-design/ui-conventions.md) | Screen inventory, navigation, shared components, messages, accessibility, and tokens |
| 10 | Screen specifications | [screens/](docs/external-design/screens/) | Per-screen purpose, states, data fields, validation, interactions, and mockups |
| all | Decision record | [ai-log.md](docs/project/ai-log.md) | AI suggestions, corrections, and unresolved decisions |

## External design baseline

The external-design package contains 24 screens and five modal overlays across four actors:

- **Supplier:** onboarding and Point-of-Origin, pickup requests, transaction signature and receipt, loyalty and payout balance, Green Profile, and ESG reports;
- **Driver / Collector:** assigned route, pickup-point collection, and handover record;
- **Warehouse Staff:** warehouse home, inbound UCO receiving, discrepancy handling, batch and mass-balance management;
- **ReFeed Staff:** operations and compliance monitoring, dispatch queue, route review and dispatch, audit trail, and compliance reports.

The primary flows are:

1. Supplier requests a pickup, follows its status, signs the transaction, and sees the resulting balance and environmental contribution.
2. Driver / Collector follows the assigned route, records actual quantity and evidence, and creates the draft handover agreement.
3. Warehouse Staff receives and verifies the collection, surfaces quantity discrepancies, and maintains the UCO batch record.
4. ReFeed Staff monitors operations, plans and dispatches routes, and reviews audit history.

All screen specs follow the shared rules in [ui-conventions.md](docs/external-design/ui-conventions.md). The mobile baseline is a 370 px content screen inside a 390 × 844 px device frame, with a minimum supported width of 360 px.

## How the documents connect

- `StR-xx` stakeholder requirements in the CVCA become `SyR-xx` system requirements in the architecture.
- `UC-xx` use cases in W0 are grouped into `F-xx` features.
- `FR-xx-yy` functions carry their EARS acceptance criteria; `BR-xx` business rules are defined in W0.
- `AD-UC-xx` activity diagrams cite the FR covered by every system action.
- `S-Fxx-nn` screen specs cite the use cases and FRs they serve and appear in the usage flow and transition diagrams.

## Identifier and notation rules

Use the existing identifier formats consistently across requirements, screen specs, mockups, messages, and audit records. In particular, the warehouse identifier is **`WT-` followed by exactly five digits**, for example `WT-01234`. Keep the `WT-` prefix, hyphen, and five-digit width unchanged.

Other examples used by the package include `SUP-00125` (Supplier), `PK-00125` (pickup request), `COL-00891` (collection), `D018` (driver), `B001` (batch), and `IMG-001` (evidence photo). Screen IDs use the `S-Fxx-nn` format.

## Notes on diagrams and numbers

- Diagrams are Mermaid code blocks. Activity diagrams use `swimlane-beta`, which requires Mermaid 11.16 or later; if a viewer cannot render one, paste the block into [mermaid.live](https://mermaid.live).
- The supplied Lesson 7 references are preserved in [activity-diagram-template.md](templates/activity-diagram-template.md) and [mermaid-activity-patterns.md](templates/mermaid-activity-patterns.md).
- Mermaid has no dedicated use-case diagram type, so the use-case diagram is represented as a flowchart; use UML notation when redrawing it in a diagramming tool.
- Money is in VND. Rates, AI leverage factors, thresholds, and benefit figures are the team's teaching estimates recorded in the function-list and appraisal documents.
