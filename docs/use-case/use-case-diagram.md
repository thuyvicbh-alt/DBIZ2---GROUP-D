# Use Case Diagram: ReFeed App

> Worked example based on the ReFeed App system scope ([ReFeed] Group D_K63DB_DBIZ II.xlsx). 

Use case diagram for the ReFeed App, mapping primary actors to their respective system functionalities. Mermaid has no use case diagram type, so the diagram below is drawn as a flowchart: people on the outside, use cases (rounded) inside the system boundary.

| Field | Value |
|---|---|
| Version | v2 |
| Actors | Supplier, Operations Staff, Driver / Collector, Warehouse Staff, Admin / Compliance Staff (primary) |

## Diagram

```mermaid
flowchart LR
  SUP["Supplier"]
  OPS["Operations Staff"]
  DRV["Driver / Collector"]
  WAR["Warehouse Staff"]
  ADM["Admin / Compliance Staff"]

  subgraph APP["ReFeed App"]
    UC01(["UC-01 Register/Login as a UCO supplier"])
    UC02(["UC-02 Manage Point-of-Origin and compliance profile"])
    UC03(["UC-03 Request and schedule a UCO pickup"])
    UC04(["UC-04 Manage and track an existing pickup status"])
    UC05(["UC-05 Plan and dispatch a UCO collection route"])
    UC06(["UC-06 Check scheduled route and Confirm Delivery"])
    UC07(["UC-07 Input Handover Information"])
    UC08(["UC-08 Confirm and sign a UCO transaction"])
    UC09(["UC-09 View or download transaction receipt"])
    UC10(["UC-10 Receive and verify collected UCO at warehouse"])
    UC11(["UC-11 Manage UCO batch and mass-balance records"])
    UC12(["UC-12 Manage loyalty status & payout balance"])
    UC13(["UC-13 Withdraw accumulated payout balance"])
    UC14(["UC-14 View Green Profile & contribution history"])
    UC15(["UC-15 Access ESG / environmental reports"])
    UC16(["UC-16 Monitor ops, trace UCO through supply chain"])
    UC17(["UC-17 Review audit trail & generate compliance reports"])
  end

  SUP --- UC01
  SUP --- UC02
  SUP --- UC03
  SUP --- UC04
  SUP --- UC08
  SUP --- UC09
  SUP --- UC12
  SUP --- UC13
  SUP --- UC14
  SUP --- UC15

  OPS --- UC05

  DRV --- UC06
  DRV --- UC07

  WAR --- UC10
  WAR --- UC11

  ADM --- UC16
  ADM --- UC17
```

The full use case inventory, with features and login rules, is documented in W0 Table 1 of the system scope file (`W0_Scope`).

## Sequence diagrams

One sequence diagram per use case, at analysis level: the lifelines are the elements of the physical design. Based on `W1_FunctionList` source traces:

| Use case | Sequence diagram | Status |
|---|---|---|
| UC-04, UC-05, UC-06, UC-07, UC-10, UC-11 | [SD-S-01](SD-S-01.md) | Done (Pickup, Route & Handover flows) |
| UC-08, UC-09, UC-11 | [SD-S-02](SD-S-02.md) | Done (Transaction & Traceability flows) |
| UC-15, UC-17 | [SD-S-03](SD-S-03.md) | Done (ESG & Audit Report flows) |
| Other UC (01, 02, 03, 12, 13, 14, 16) | (not drawn in this example) | Students practise on these |

## Change log

| Version | When | Change | Why |
|---|---|---|---|
| v1 | Mid-term | Nineteen use cases, actors UCO Supplier, UCO Collector, Admin, Warehouse | Extracted from Group D - Mid-term Presentation |
| v2 | Current | Refined to 17 use cases across 5 explicit actor roles | Aligned with W0_Scope and W1_FunctionList in `[ReFeed] Group D_K63DB_DBIZ II.xlsx` |
