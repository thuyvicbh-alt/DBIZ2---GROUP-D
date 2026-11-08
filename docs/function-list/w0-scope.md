# W0 Scope: ReFeed


## Table 1. Use case inventory

| Use case ID | Use case name | Primary actor | Assigned feature | Needed/not needed | Reasons |
|---|---|---|---|---|---|
| UC-01 | Register/Login as a UCO supplier | Supplier | F-01 | Needed | |
| UC-02 | Manage Point-of-Origin and compliance profile | Supplier | F-01 | Not Needed |  |
| UC-03 | Request and schedule a UCO pickup | Supplier | F-01 | Needed | |
| UC-04 | Manage and track an existing pickup status | Supplier | F-01 | Needed |  |
| UC-05 | Plan and dispatch a UCO collection route | Refeed Staff | F-02 | Needed | |
| UC-06 | Check scheduled collection route and Confirm Delivery to supplier points | Driver / Collector | F-02 | Needed |  |
| UC-07 | Input Handover Information | Driver/Collector | F-02 | Not Needed | |
| UC-08 | Confirm and sign a UCO transaction | Supplier | F-03 | Not Needed | |
| UC-09 | View or download transaction receipt | Supplier | F-03 | Not needed | |
| UC-10 | Receive and verify collected UCO at warehouse | Warehouse Staff | F-04 | Needed |  |
| UC-11 | Manage UCO batch and mass-balance records | Warehouse Staff | F-04 | Needed | |
| UC-12 | Manage loyalty status and accumulated payout balance | Supplier | F-05 | Needed |  |
| UC-13 | Withdraw accumulated payout balance | Supplier | F-05 | Needed | |
| UC-14 | View Green Profile and UCO contribution history | Supplier | F-05 | Not needed | Read-only/Query export |
| UC-15 | Access ESG / environmental contribution reports | Supplier | F-05 | Not needed | |
| UC-16 | Monitor operations, compliance status, trace UCO through supply chain | Refeed Staff | F-06 | Not needed | |
| UC-17 | Review audit trail and generate compliance reports | Refeed Staff | F-06 | Not needed | |

Use cases listed: 17. All assigned.

## Table 2. Feature list

| Feature ID | Feature name (business noun) | Scope in one sentence | Primary actors served | Owner | Use cases covered |
|---|---|---|---|---|---|
| F-01 | Supplier Onboarding & Pickup Management | Manage supplier accounts, compliance | Supplier | An | UC-01, UC-02, UC-03, UC-04 |
| F-02 | Collection Operations & Handover | Plan collection routes and support drivers in completing and handover | Refeed Staff; Driver / Collector | Linh | UC-05, UC-06, UC-07 |
| F-03 | UCO Transaction | Manage UCO transactions | Supplier | Vi | UC-08, UC-09 |
| F-04 | Warehouse & Traceability | Manage warehouse | Warehouse Staff | Thơ | UC-10, UC-11 |
| F-05 | Supplier Value & ESG / Green Profile | Manage loyalty and payout information and provide Green Profile and ESG reporting capabilities. | Supplier | Quang Minh | UC-12, UC-13, UC-14, UC-15 |
| F-06 | Operations, Audit & Compliance | Monitor operations, provide audit and compliance capabilities, and own cross-cutting platform controls. | Refeed Staff | Đức Minh | UC-16, UC-17; ARCH/ADR controls |

Feature count: 6. PASS. 

## Table 3. Won't-have

| Excluded from scope | How it surfaced | Reason for exclusion |
|---|---|---|
| In-app direct payment/transfer | Initially suggested by business team | Out of scope for Release 1. The system only manages "accumulated payout balance" (UC-12, UC-13); actual financial transfers are executed externally via manual bank transfers. |
| Real-time chat between Driver and Supplier | Surfaced from actual collection flow needs (UC-07) | High technical complexity and unnecessary server cost for Release 1. Direct phone contact is sufficient for the current operational scale. |

## Table 4. Data functions

| Data group | ILF / EIF | Why | Function points |
| Data group | ILF / EIF | Why | Function points |
|---|---|---|---|
| Supplier Profile & Compliance | ILF | Created and maintained internally by Suppliers within the ReFeed system (UC-01, UC-02) | 10 |
| UCO Pickup Request | ILF | Created by Suppliers (UC-03) and updated by Refeed Staff inside the system (UC-04, UC-05). | 10 |
| Collection Route & Dispatch | ILF | Scheduled and managed internally by Refeed Staff (UC-05) | 10 |
| UCO Transaction & Handover Record | ILF | Recorded at the collection points (UC-07) and verified by the Supplier (UC-08) | 10 |
| Warehouse UCO Batch Data | ILF | Created and updated by Warehouse Staff upon receiving UCO (UC-10, UC-11) | 10 |
| Loyalty Tier & Payout Balance | ILF | Automatically calculated and maintained by the system based on transaction history (UC-12, UC-13). | 10 |
| Map & Optimized Route Data (Google Maps API) | EIF | Read from the external map provider's API for distance calculation and routing. Never written or modified by the system. | 10 |
| External Compliance/Audit Standards | EIF | Read from governmental/environmental agency documents for Standard Compliance (UC-17). The system does not alter this original data. | 7 |
| **Total data-function points** | | | **77** |
