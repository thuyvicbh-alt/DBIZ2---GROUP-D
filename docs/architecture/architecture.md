# Architecture: ReFeed

> Worked example for teaching (ReFeed). Not a student submission and not a real product.

Lesson 4 (Prof. Seki, architectural design). The steps follow the design process of the lecture:
stakeholder requirements, system requirements, functional design, physical design, three layers.
Decisions are recorded as ADRs in [`adr/`](adr/).

| Field | Value |
|---|---|
| Version | v1 |
| Inputs | [CVCA](../context/cvca.md) (stakeholder requirements), [Context Diagram](../context/context-diagram.md), [Use Case Diagram](../use-case/use-case-diagram.md) |

## 1. System requirements

| ID | System requirement | From |
|---|---|---|
| SyR-01 | The system shall let a supplier manage their profile and schedule a UCO pickup. | StR-01 |
| SyR-02 | The system shall let operations staff plan and dispatch collection routes. | StR-02 |
| SyR-03 | The system shall guide drivers through the route and let them record the UCO handover. | StR-03 |
| SyR-04 | The system shall generate an electronic agreement to be signed by the supplier during handover. | StR-03 |
| SyR-05 | The system shall let warehouse staff receive UCO batches and manage mass-balance. | StR-04 |
| SyR-06 | The system shall track supplier transaction history to calculate loyalty tier and payout balances. | StR-05 |

The detailed, testable version of each requirement is an EARS row in
[W1](../function-list/w1-function-list.md).

## 2. Functional design

The system requirements divided into subordinate functions.

| Function | Serves |
|---|---|
| Identify the user and the role | all |
| Manage profiles and schedule pickups | SyR-01 |
| Plan routes and dispatch drivers | SyR-02 |
| Track arrival and capture handover data | SyR-03 |
| Generate agreements and capture signatures | SyR-04 |
| Receive batches and update mass-balance | SyR-05 |
| Update loyalty points and calculate payouts | SyR-06 |
| Calculate route distances and map coordinates | SyR-02, SyR-03 |

## 3. Physical design

Each function allocated to an element of the system.

```mermaid
flowchart TB
  subgraph P["1 Presentation layer"]
    SUP["Supplier Web App"]
    OPS["Operations App (Driver & Warehouse)"]
    ADM["Admin Portal"]
  end
  subgraph B["2 Business layer"]
    direction LR
    PRM["Supplier & Profile Service"]
    COL["Collection & Routing Service"]
    TXN["Transaction & Agreement Service"]
    WHS["Warehouse Service"]
    LOY["Loyalty & Payout Service"]
  end
  subgraph D["3 Data access layer"]
    DB[("ReFeed database")]
  end
  MAPS["Google Maps API<br>(external)"]
  P --> B
  B --> D
  COL -.-> MAPS
