# Project brief: ReFeed

Lessons 1 and 2. Group D, K63DB, DBIZ2 Digital Business Project II at VJCBI College, Foreign Trade University. The brief sets the product and release boundaries for the design package handed to DBIZ3.

| Field | Value |
|---|---|
| Product | ReFeed System, a digital platform for used cooking oil (UCO) collection and traceability |
| Client | ReFeed Vietnam |
| Business counterpart | ReFeed Office and its collection operations team. The named client contact is open. |
| Team | Group D, six members |
| Design baseline | [CVCA](../context/cvca.md), [Context Diagram](../context/context-diagram.md), ReFeed Cost Workshop W0/W1 and the [W5 handover](../function-list/w5-handover.md) |

## Problem

Suppliers arrange pickups through calls or Zalo. ReFeed Staff schedule routes by hand, making it harder to group small pickups and give suppliers a clear collection time. Quantity and payment details are agreed at the handover and recorded manually. Warehouse staff can learn about quantity discrepancies late, and auditors cannot reliably trace a batch back to its collections and Points of Origin.

## Goal of the project

Give suppliers, collectors, warehouse staff and ReFeed Staff a shared record from the initial pickup request through route dispatch, collection, signed handover and warehouse verification. The [CVCA](../context/cvca.md) defines the stakeholder outcomes. The release uses W4 priorities and the W5 scope figures.

| Outcome | What the team can measure in a pilot |
|---|---|
| Make pickup and dispatch easier to coordinate | Share of requests entered digitally, elapsed time from request to dispatch, and eligible vehicle kilometres per completed pickup |
| Make each handover verifiable | Share of completed pickups linked to a signed record with the agreed quantity and amount |
| Detect warehouse discrepancies earlier | Share of inbound quantities matched to a collection and time to surface a discrepancy |
| Support traceability and audit preparation | Share of in-scope batch inputs linked to a collection and Point of Origin, and time to retrieve supporting records |

ReFeed and the team still need to establish the baselines and agree on targets. The [W6 appraisal](../function-list/w6-cash-flow.md) values modelled incremental cash flows against current manual operations.

## Scope and release boundary

The core flow covers supplier onboarding and requests, route dispatch and collection, handover and signature, warehouse verification, payout-balance visibility and audit controls. W5's Must-only estimate defines the committed baseline. Must + Should is the expected planning scope, and Could work forms the in-scope ceiling.

W0 excludes in-app transfers and real-time driver–supplier chat. W4 defers end-to-end supply-chain trace queries. W4's withdrawal priority and Release 2 rationale need reconciliation. The team should confirm the boundary of withdrawal tracking before promising it in Release 1. Any transfer is executed outside the app.

## Team and roles

Feature ownership follows ReFeed Cost Workshop W0 Table 2. Document assignments follow the Group D allocation.

| Member | Role | Owns |
|---|---|---|
| Xuân An | Team lead and feature owner | F-01 Supplier Onboarding & Pickup Management |
| Linh Trần | Feature owner | F-02 Collection Operations & Handover |
| Thúy Vi | Feature owner | F-03 UCO Transaction |
| Quỳnh Thơ | Feature owner | F-04 Warehouse & Traceability |
| Quang Minh | Feature owner | F-05 Supplier Value & ESG / Green Profile |
| Đức Minh | Feature owner and document contributor | F-06 Operations, Audit & Compliance. AD-UC-05, activity index, W6, AI log, project brief and SD-UC-05. |

## Milestones in DBIZ2

| When | Milestone | Deliverables for review |
|---|---|---|
| After Lesson 4 | Senior Meeting · Scope confirmation | CVCA, Context Diagram, Use Case Diagram, sequence diagrams and architecture |
| After Lesson 6 | Midterm · Function list and appraisal | W0–W6, including release priorities, cost estimates and cash-flow appraisal |
| After Lesson 8 | Senior Meeting · Final Package Review | Activity diagrams and open questions, usage flow, screen list, screen transitions and screen specs |
| DBIZ3 handover | Implementation baseline | Agreed scope, requirements and design package, risk-first work list and open issues |

The [Activity Diagram Index](../activity/activity-index.md) records cross-document issues for integration. The [AI log](ai-log.md) records the AI-generated drafts and proposals corrected during preparation.
