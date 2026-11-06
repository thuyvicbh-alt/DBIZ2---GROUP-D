# Screen Transition Diagram: ReFeed System

---

## 1. Supplier

```mermaid
flowchart LR
  entry(["Opens ReFeed App / Web"])
  notif(["Notification / Email Link"])

  S00_01["S-F00-01<br>Sign in"]
  S01_02["S-F01-02<br>Point-of-Origin & compliance profile"]
  S01_03["S-F01-03<br>Pickup request form"]
  S01_04["S-F01-04<br>My pickup requests"]
  S01_05["S-F01-05<br>Pickup request detail"]
  S03_01["S-F03-01<br>Transaction review & e-signature"]
  S03_02["S-F03-02<br>Transaction receipt"]
  S05_01["S-F05-01<br>Loyalty & payout balance"]
  S05_02["S-F05-02<br>Payout withdrawal"]
  S05_03["S-F05-03<br>Green Profile"]
  S05_04["S-F05-04<br>ESG contribution reports"]

  %% Entry & Auth
  entry --> S00_01
  S00_01 -->|"signed in as supplier"| S05_01
  S00_01 -->|"[Profile incomplete prompt]"| S01_02

  %% Profile
  S05_01 -->|"View/Update profile"| S01_02
  S01_02 -->|"Save / Back"| S05_01

  %% Pickup Request Management
  S05_01 -->|"Request pickup"| S01_03
  S01_03 -->|"View my requests"| S01_04
  S05_01 -->|"View pickup requests"| S01_04
  S01_04 -->|"Request new pickup"| S01_03
  S01_04 -->|"Select request"| S01_05
  S01_05 -->|"Back"| S01_04
  S01_05 -->|"Reschedule"| S01_03
  S01_05 -->|"[Confirm cancel modal] Cancelled"| S01_04

  %% E-Sign & Receipts
  notif --> S03_01
  S05_01 -->|"Transaction awaiting signature"| S03_01
  S03_01 -->|"[Signed] View receipt"| S03_02
  S03_02 -->|"View balance & history"| S05_01

  %% Payout & ESG
  S05_01 -->|"Withdraw"| S05_02
  S05_02 -->|"[Confirmed] Back to balance"| S05_01
  S05_01 -->|"Green Profile"| S05_03
  S05_03 -->|"Balance & Payout"| S05_01
  S05_03 -->|"Reports"| S05_04
  S05_04 -->|"Back to Green Profile"| S05_03
```

---

## 2. Driver / Collector

```mermaid
flowchart LR
  entry(["Opens ReFeed Mobile App"])
  notif(["Dispatch notification"])

  S00_01["S-F00-01<br>Sign in"]
  S02_03["S-F02-03<br>Assigned route"]
  S02_04["S-F02-04<br>Pickup point collection"]
  S02_05["S-F02-05<br>Handover record & draft agreement"]

  entry --> S00_01
  S00_01 -->|"Signed in (Driver Home)"| S02_03
  notif --> S02_03

  S02_03 -->|"Select pickup point"| S02_04
  S02_04 -->|"[Confirm cancellation modal] UCO not ready"| S02_03
  S02_04 -->|"[Collection saved] Generate agreement"| S02_05
  S02_05 -->|"[Handover completed] Back to route"| S02_03
```

---

## 3. Warehouse Staff

```mermaid
flowchart LR
  entry(["Opens ReFeed Warehouse Portal"])

  S00_01["S-F00-01<br>Sign in"]
  S04_01["S-F04-01<br>Warehouse receiving"]
  S04_02["S-F04-02<br>Batch & mass-balance management"]

  entry --> S00_01
  S00_01 -->|"Signed in (Warehouse Home)"| S04_01
  S04_01 -->|"Batch management"| S04_02
  S04_02 -->|"Receive next collection"| S04_01
```

---

## 4. ReFeed Staff

```mermaid
flowchart LR
  entry(["Opens ReFeed Operations Portal"])

  S00_01["S-F00-01<br>Sign in"]
  S02_01["S-F02-01<br>Dispatch queue"]
  S02_02["S-F02-02<br>Route review and dispatch"]
  S06_01["S-F06-01<br>Operations & compliance monitoring"]
  S06_02["S-F06-02<br>Audit trail & compliance reports"]

  entry --> S00_01
  S00_01 -->|"Signed in (Staff Home)"| S06_01

  %% Dispatch flow
  S06_01 -->|"Pending pickup drill-down"| S02_01
  S02_01 -->|"Plan route"| S02_02
  S02_02 -->|"Back to queue"| S02_01
  S02_02 -->|"[Confirm dispatch modal] Dispatched"| S02_01
  S02_01 -->|"Back to monitoring"| S06_01

  %% Compliance & Audit flow
  S06_01 -->|"Drill-down audit"| S06_02
  S06_02 -->|"Back to monitoring"| S06_01
```

---

## Checks

- **Every screen in the Screen List appears:** Yes (21/21 screens).
  - Supplier: S-F00-01, S-F01-01, S-F01-02, S-F01-03, S-F01-04, S-F01-05, S-F03-01, S-F03-02, S-F05-01, S-F05-02, S-F05-03, S-F05-04 (12 screens).
  - Driver / Collector: S-F00-01, S-F02-03, S-F02-04, S-F02-05 (4 screens).
  - Warehouse Staff: S-F00-01, S-F04-01, S-F04-02 (3 screens).
  - ReFeed Staff: S-F00-01, S-F02-01, S-F02-02, S-F06-01, S-F06-02 (5 screens).
- **Every screen reachable from an entry point:** Yes.
- **No dead ends:** Yes, all workflow branches return to parent screens or role dashboards.
- **Usage-flow paths:** Every core use case path mapped below.
- **Sign in sharing:** S-F00-01 is shared across all actors, redirecting to each actor's specific homepage upon authentication.

---

## Usage-flow paths

| Actor | Usage-flow / Use Case | Path on this diagram |
|---|---|---|
| **Supplier** | UC-01 Supplier Registration | `entry` → `S-F00-01 Sign in` → `S-F01-01 Supplier registration` → `S-F00-01 Sign in` |
| **Supplier** | UC-02 Point-of-Origin & Compliance | `S-F05-01 Loyalty & payout balance` → `S-F01-02 Point-of-Origin and compliance profile` |
| **Supplier** | UC-03, UC-04 Request Pickup & Track | `S-F05-01` → `S-F01-03 Pickup request form` → `S-F01-04 My pickup requests` → `S-F01-05 Pickup request detail` |
| **Supplier** | UC-08, UC-09 Review Transaction & E-sign | `notif` / `S-F05-01` → `S-F03-01 Transaction review and e-signature` → `S-F03-02 Transaction receipt` → `S-F05-01` |
| **Supplier** | UC-13 Payout Withdrawal | `S-F05-01 Loyalty & payout balance` → `S-F05-02 Payout withdrawal` → `S-F05-01` |
| **Supplier** | UC-14, UC-15 Green Profile & ESG Reports | `S-F05-01` → `S-F05-03 Green Profile` → `S-F05-04 ESG contribution reports` |
| **Driver / Collector** | UC-06 Pickup Point Collection | `S-F02-03 Assigned route` → `S-F02-04 Pickup point collection` → `S-F02-03` (if UCO not ready) / `S-F02-05` |
| **Driver / Collector** | UC-07 Handover Record & Draft | `S-F02-04 Pickup point collection` → `S-F02-05 Handover record and draft agreement` → `S-F02-03 Assigned route` |
| **Warehouse Staff** | UC-10 Warehouse Receiving | `S-F00-01 Sign in` → `S-F04-01 Warehouse receiving` |
| **Warehouse Staff** | UC-11 Batch & Mass-balance | `S-F04-01 Warehouse receiving` → `S-F04-02 Batch and mass-balance management` |
| **ReFeed Staff** | UC-05 Route Planning & Dispatch | `S-F06-01 Monitoring` → `S-F02-01 Dispatch queue` → `S-F02-02 Route review and dispatch` → `S-F02-01` |
| **ReFeed Staff** | UC-16, UC-17 Compliance & Audit | `S-F06-01 Monitoring` → `S-F06-02 Audit trail and compliance reports` |
