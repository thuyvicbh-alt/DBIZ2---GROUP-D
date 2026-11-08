# W0 Scope: ReFeed

> Worked example for teaching based on ReFeed project. 

## Table 1. Use case inventory

| Use case ID | Use case name | Primary actor | Assigned feature | Needed/not needed | Reasons |
|---|---|---|---|---|---|
| UC-01 | Register/Login as a UCO supplier | Supplier | F-01 | Not needed | |
| UC-02 | Manage Point-of-Origin and compliance profile | Supplier | F-01 | Needed | X |
| UC-03 | Request and schedule a UCO pickup | Supplier | F-01 | Needed | |
| UC-04 | Manage and track an existing pickup status | Supplier | F-01 | Needed | X |
| UC-05 | Plan and dispatch a UCO collection route | Operations Staff | F-02 | Needed | |
| UC-06 | Check scheduled collection route and Confirm Delivery to supplier points | Driver / Collector | F-02 | Needed | X |
| UC-07 | Input Handover Information | Driver/Collector | F-02 | Needed | |
| UC-08 | Confirm and sign a UCO transaction | Supplier | F-03 | Needed | |
| UC-09 | View or download transaction receipt | Supplier | F-03 | Not needed | |
| UC-10 | Receive and verify collected UCO at warehouse | Warehouse Staff | F-04 | Needed | X |
| UC-11 | Manage UCO batch and mass-balance records | Warehouse Staff | F-04 | Needed | |
| UC-12 | Manage loyalty status and accumulated payout balance | Supplier | F-05 | Needed | X |
| UC-13 | Withdraw accumulated payout balance | Supplier | F-05 | Needed | |
| UC-14 | View Green Profile and UCO contribution history | Supplier | F-05 | Not needed | Read-only/Query export |
| UC-15 | Access ESG / environmental contribution reports | Supplier | F-05 | Not needed | |
| UC-16 | Monitor operations, compliance status, trace UCO through supply chain | Admin / Operations / Compliance Staff | F-06 | Not needed | |
| UC-17 | Review audit trail and generate compliance reports | Admin / Compliance Staff | F-06 | Not needed | |

Use cases listed: 17. All assigned.

## Table 2. Feature list

| Feature ID | Feature name (business noun) | Scope in one sentence | Primary actors served | Owner | Use cases covered |
|---|---|---|---|---|---|
| F-01 | Supplier Onboarding & Pickup Management | Manage supplier accounts, compliance | Supplier | An | UC-01, UC-02, UC-03, UC-04 |
| F-02 | Collection Operations & Handover | Plan collection routes and support drivers in completing and handover | Operations Staff; Driver / Collector | Linh | UC-05, UC-06, UC-07 |
| F-03 | UCO Transaction | Manage UCO transactions | Supplier | Vi | UC-08, UC-09 |
| F-04 | Warehouse & Traceability | Manage warehouse | Warehouse Staff | Thơ | UC-10, UC-11 |
| F-05 | Supplier Value & ESG / Green Profile | Manage loyalty and payout information and provide Green Profile and ESG reporting capabilities. | Supplier | Quang Minh | UC-12, UC-13, UC-14, UC-15 |
| F-06 | Operations, Audit & Compliance | Monitor operations, provide audit and compliance capabilities, and own cross-cutting platform controls. | Admin / Operations / Compliance Staff | Đức Minh | UC-16, UC-17; ARCH/ADR controls |

Feature count: 6. PASS. 

## Table 3. Won't-have

| Excluded from scope | How it surfaced | Reason for exclusion |
|---|---|---|
| In-app direct payment/transfer | Initially suggested by business team | Out of scope for Release 1. The system only manages "accumulated payout balance" (UC-12, UC-13); actual financial transfers are executed externally via manual bank transfers. |
| Real-time chat between Driver and Supplier | Surfaced from actual collection flow needs (UC-07) | High technical complexity and unnecessary server cost for Release 1. Direct phone contact is sufficient for the current operational scale. |

## Table 4. Data functions

| Data group | ILF / EIF | Why | Function points |
