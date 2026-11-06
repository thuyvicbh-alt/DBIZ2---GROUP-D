# CVCA: ReFeed

Lesson 3 (Prof. Seki, CVCA to Context Diagram). The Customer Value Chain Analysis lists every
stakeholder of the used cooking oil (UCO) chain and what flows between them: money (`$`),
information (`i`) and goods or services. It is drawn before the system is designed, to find the
problems and opportunities.

| Field | Value |
|---|---|
| Version | v1.0 (Lesson 3) |
| Next | [Context Diagram](context-diagram.md) |

## 1. Value chain (with the new system)

```mermaid
---
config:
  layout: elk
  flowchart:
    curve: step
---
flowchart LR
  SUP["UCO Suppliers<br>(hospitality and F&B chains,<br>small and medium-sized<br>food businesses)"]
  COL["UCO collectors /<br>Drivers"]
  WH["Warehouse"]
  OFF["ReFeed Office<br>(client)"]
  LOG["Logistics operator"]
  DB["Database-based system"]
  ECO["Eco Oil<br>(processor and exporter)"]
  EUR["EU regulators<br>(RED II)"]
  CERT["Certification bodies<br>(ISCC / RSB, SGS,<br>Bureau Veritas)"]
  SAF["EU SAF producer / buyer"]
  RF(["ReFeed app<br>(new system)"])

  SUP -->|"goods: UCO"| COL
  COL -->|"goods: UCO"| WH
  COL -->|"i: shipping status, inventory information"| WH
  WH -->|"goods: UCO"| ECO
  ECO -->|"goods: UCO"| SAF
  SAF -->|"$ payment"| ECO
  OFF <-->|"service: processing & distribution partnership"| ECO
  ECO -->|"i: certification request"| CERT
  CERT -->|"i: certificate"| ECO
  ECO -->|"i: information sharing"| EUR
  EUR -->|"i: approval"| ECO
  SUP -->|"i: registration, profile, pickup request, e-signature, withdrawal request"| RF
  RF -->|"i: pickup status, agreement, e-receipt, Green Profile / ESG report"| SUP
  RF -->|"$ payment"| SUP
  COL -->|"i: delivery confirmation, quantity confirmed"| RF
  RF -->|"i: delivery request, schedule, optimized route"| COL
  RF -->|"i: UCO information"| WH
  WH -->|"i: inventory information"| RF
  RF -->|"i: data input"| DB
  DB -->|"i: data sharing"| ECO
  DB -->|"i: system alerts, notifications"| OFF
  OFF -->|"i: system config, approvals"| RF
  RF -->|"i: operation data"| OFF
  LOG -->|"service: app operation and management"| RF
```

Today (before the ReFeed app) suppliers arrange each pickup with ReFeed by phone call or Zalo
message, and the schedule is planned by hand. The quantity collected, the price and the payment
are agreed and noted manually at the supplier's door, so nothing links a warehouse batch back to
the suppliers and collections it came from.

## 2. Problems and opportunities

| # | Stakeholder | Problem or opportunity |
|---|---|---|
| P-1 | UCO Suppliers, ReFeed Office | Pickups are arranged by phone or Zalo and scheduled by hand; the supplier does not know when the driver will come. |
| P-2 | ReFeed Office, UCO collectors / Drivers | Small suppliers are scattered across the city; dispatching each request by hand makes routes long and each small pickup expensive. |
| P-3 | UCO Suppliers, ReFeed Office | The collected quantity, price and payment are recorded manually, so there is no signed record when the two sides disagree. |
| P-4 | Warehouse | A difference between the quantity collected and the quantity received at the warehouse is noticed late or not at all. |
| P-5 | Eco Oil, Certification bodies, EU regulators | A UCO batch cannot be traced back to its Point of Origin, while ISCC / RSB certification and RED II need chain of custody and mass balance. |
| P-6 | UCO Suppliers, ReFeed Office | Small suppliers switch to another collector that offers a higher price. |
| P-7 | UCO Suppliers | Business and SME suppliers have no proof that their UCO was handled responsibly (opportunity). |

## 3. Stakeholder requirements

Written as "<stakeholder> shall be able to ...", as in Prof. Seki's Lesson 4 design process. They
become system requirements in [the architecture](../architecture/architecture.md).

| ID | Stakeholder requirement | From |
|---|---|---|
| StR-01 | UCO Suppliers shall be able to request a pickup and follow its status without calling ReFeed. | P-1 |
| StR-02 | The ReFeed Office shall be able to group pickup requests and send drivers on planned routes. | P-2 |
| StR-03 | UCO Suppliers and the ReFeed Office shall be able to confirm the collected quantity and the amount of each handover in a signed record. | P-3 |
| StR-04 | Warehouse staff shall be able to compare the received quantity with the collected quantity of each collection. | P-4 |
| StR-05 | Eco Oil shall be able to trace each UCO batch back to the collections and suppliers it came from. | P-5 |
| StR-06 | UCO Suppliers shall be able to earn a better price the more UCO they sell to ReFeed. | P-6 |
| StR-07 | UCO Suppliers shall be able to show their verified UCO contribution. | P-7 |
