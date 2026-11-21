# Screen Transition Diagram: ReFeed System

## 1. Supplier

```mermaid
flowchart LR
  entry(["Opens ReFeed"])
  notif(["Notification: draft agreement ready"])
  S00_01["S-F00-01<br>Sign in"]
  S00_02["S-F00-02<br>Supplier home"]
  S01_01["S-F01-01<br>Supplier registration"]
  S01_02["S-F01-02<br>Point-of-Origin"]
  S01_03["S-F01-03<br>Pickup request form"]
  S01_04["S-F01-04<br>My pickup requests"]
  S01_05["S-F01-05<br>Pickup request detail"]
  S01_06["S-F01-06<br>Supplier account"]
  S03_01["S-F03-01<br>Transaction review and e-signature"]
  S03_02["S-F03-02<br>Transaction receipt"]
  S05_01["S-F05-01<br>Loyalty and payout balance"]
  S05_02["S-F05-02<br>Payout withdrawal"]
  S05_03["S-F05-03<br>Green Profile"]
  S05_04["S-F05-04<br>ESG contribution reports"]
  entry --> S00_01
  notif --> S03_01
  S00_01 -->|"Register"| S01_01
  S00_01 -->|"signed in as supplier"| S00_02
  S01_01 -->|"[account created] Continue"| S00_02
  S00_02 -->|"Profile"| S01_06
  S00_02 -->|"Collection"| S01_03
  S00_02 -->|"open-requests shortcut"| S01_04
  S00_02 -->|"next-pickup shortcut"| S01_05
  S00_02 -->|"awaiting-signature banner"| S03_01
  S00_02 -->|"latest-receipt shortcut"| S03_02
  S00_02 -->|"My wallet"| S05_01
  S00_02 -->|"Green Profile / UCO-delivered shortcut"| S05_03
  S01_06 -->|"Point-of-Origin profile"| S01_02
  S01_06 -->|"Sign out"| S00_01
  S01_06 -->|"Home"| S00_02
  S01_02 -->|"saved / back"| S01_06
  S01_03 -->|"[profile incomplete] Complete profile"| S01_02
  S01_03 -->|"[request created] View my requests"| S01_04
  S01_03 -->|"[rescheduled] View request"| S01_05
  S01_03 -->|"Home / back"| S00_02
  S01_04 -->|"selects a request"| S01_05
  S01_04 -->|"back"| S00_02
  S01_05 -->|"Reschedule"| S01_03
  S01_05 -->|"back"| S01_04
  S05_01 -->|"Transaction awaiting signature"| S03_01
  S05_01 -->|"History: View receipt"| S03_02
  S05_01 -->|"Withdraw"| S05_02
  S05_01 -->|"Green Profile"| S05_03
  S05_01 -->|"Home / back"| S00_02
  S03_01 -->|"[signed] View receipt"| S03_02
  S03_01 -->|"Back to home / back"| S00_02
  S03_02 -->|"back"| S05_01
  S05_02 -->|"back"| S05_01
  S05_03 -->|"Reports"| S05_04
  S05_03 -->|"Home / back"| S00_02
  S05_04 -->|"back"| S05_03
```

Main task "request a pickup": S-F00-02 (Supplier home) → S-F01-03 → success state (request ID shown). One screen from the supplier home.

## 2. Driver / Collector

```mermaid
flowchart LR
  entry(["Opens ReFeed"])
  notif(["Dispatch notification"])
  S00_01["S-F00-01<br>Sign in"]
  S02_03["S-F02-03<br>Assigned route"]
  S02_04["S-F02-04<br>Pickup point collection"]
  S02_05["S-F02-05<br>Handover record and draft agreement"]
  entry --> S00_01
  notif --> S02_03
  S00_01 -->|"signed in as driver"| S02_03
  S02_03 -->|"selects a pickup point"| S02_04
  S02_03 -->|"Sign out"| S00_01
  S02_04 -->|"[collection saved] Record handover"| S02_05
  S02_04 -->|"[pickup saved or cancelled] Back to route"| S02_03
  S02_04 -->|"back"| S02_03
  S02_05 -->|"[handover recorded] Back to route"| S02_03
```

Main task "collect UCO at a pickup point": S-F02-03 → S-F02-04 → S-F02-05. Two screens from the start screen (the assigned route is the driver's home).

## 3. Warehouse Staff

```mermaid
flowchart LR
  entry(["Opens ReFeed"])
  S00_01["S-F00-01<br>Sign in"]
  S00_03["S-F00-03<br>Warehouse home"]
  S04_01["S-F04-01<br>Warehouse receiving"]
  S04_02["S-F04-02<br>Batch and mass-balance management"]
  entry --> S00_01
  S00_01 -->|"signed in as warehouse staff"| S00_03
  S00_03 -->|"Receive UCO"| S04_01
  S00_03 -->|"Batches"| S04_02
  S00_03 -->|"Sign out"| S00_01
  S04_01 -->|"[received quantity saved] Manage batch"| S04_02
  S04_01 -->|"back"| S00_03
  S04_02 -->|"back"| S00_03
```

Main task "receive a collection": S-F00-03 (Warehouse home) → S-F04-01 → result state (match or discrepancy alert). One screen from the warehouse home.

## 4. ReFeed Staff

```mermaid
flowchart LR
  entry(["Opens ReFeed"])
  S00_01["S-F00-01<br>Sign in"]
  S06_01["S-F06-01<br>Operations and compliance monitoring (staff home)"]
  S06_02["S-F06-02<br>Audit trail and compliance reports"]
  S02_01["S-F02-01<br>Dispatch queue"]
  S02_02["S-F02-02<br>Route review and dispatch"]
  entry --> S00_01
  S00_01 -->|"signed in as ReFeed staff"| S06_01
  S06_01 -->|"dispatch entry / pending-pickup drill-down"| S02_01
  S06_01 -->|"drill-down"| S06_02
  S06_01 -->|"Sign out"| S00_01
  S02_01 -->|"Plan route (selected area)"| S02_02
  S02_01 -->|"back"| S06_01
  S02_02 -->|"[route dispatched] Dispatch queue"| S02_01
  S02_02 -->|"back"| S02_01
  S06_02 -->|"back"| S06_01
```

Main task "dispatch a route": S-F06-01 (staff home) → S-F02-01 → S-F02-02 → dispatched. Two screens from the staff home.

## Checks

* Every screen in the Screen List appears: yes (24 of 24). S-F00-01 appears in all four diagrams and is counted once (14 + 3 + 3 + 4 = 24 distinct screens).

* Every screen reachable from an entry point: yes.

* No dead ends: yes. Every screen has at least one outgoing arrow.

* Entry points of the Screen List are all drawn: system launch, sign out, session expired, the four role homes (S-F00-02, S-F02-03, S-F00-03, S-F06-01), the dispatch notification, the draft-agreement notification, and each screen-to-screen entry.

* Modals are not nodes (5 of 5 listed in the Screen List stay on their screen): "Cancel this pickup request?" on S-F01-05, "Confirm route and dispatch?" on S-F02-02, "UCO not ready: cancel this pickup?" on S-F02-04, "Confirm and e-sign" on S-F03-01, "Confirm withdrawal" on S-F05-02.

* Usage-flow paths: every arrow of [usage-flow.md](usage-flow.md) has a path (table below).

* Sign in is shared; a signed-out user who opens a notification link sees S-F00-01 first and then returns to the screen they asked for (screen-level rule in S-F00-01).

## Usage-flow paths

| Actor | Usage-flow arrow | Path on this diagram | 
| ----- | ----- | ----- | 
| Supplier | UC-01 to UC-02 | Same screen: S-F01-01 Supplier registration (Point-of-Origin data entered at registration); later edits on S-F01-02 (via S-F01-06 Supplier account) | 
| Supplier | UC-01 to UC-03 | S-F01-01 → S-F00-02 Supplier home → S-F01-03 Pickup request form ("Collection") | 
| Supplier | UC-02 to UC-03 | S-F01-02 Point-of-Origin → S-F01-06 Supplier account → S-F00-02 Supplier home → S-F01-03 ("Collection") | 
| Supplier | UC-03 to UC-02 | S-F01-03 → S-F01-02 ("[profile incomplete] Complete profile") | 
| Supplier | UC-03 to UC-04 | S-F01-03 → S-F01-04 My pickup requests ("[request created] View my requests") | 
| Supplier | UC-04 to UC-03 | S-F01-05 Pickup request detail → S-F01-03 ("Reschedule") | 
| Supplier | UC-04 to cancel | Same screen: S-F01-05, modal "Cancel this pickup request?" | 
| Supplier | UC-08 to UC-09 | S-F03-01 → S-F03-02 Transaction receipt ("[signed] View receipt") | 
| Supplier | UC-09 to UC-12 | S-F03-02 → S-F05-01 Loyalty and payout balance ("back") | 
| Supplier | UC-12 to UC-08 | S-F05-01 → S-F03-01 ("Transaction awaiting signature") | 
| Supplier | UC-12 to UC-13 | S-F05-01 → S-F05-02 Payout withdrawal ("Withdraw") | 
| Supplier | UC-12 to UC-14 | S-F05-01 → S-F05-03 Green Profile ("Green Profile") | 
| Supplier | UC-14 to UC-15 | S-F05-03 → S-F05-04 ESG contribution reports ("Reports") | 
| Driver / Collector | UC-06 to UC-07 | S-F02-04 → S-F02-05 ("[collection saved] Record handover") | 
| Driver / Collector | UC-07 to UC-06 | S-F02-05 → S-F02-03 ("[handover recorded] Back to route") | 
| Driver / Collector | UC-06 to UC-06 (next point) | S-F02-04 → S-F02-03 → S-F02-04 | 
| Warehouse Staff | UC-10 to UC-11 | S-F04-01 → S-F04-02 ("[received quantity saved] Manage batch") | 
| ReFeed Staff | UC-16 to UC-05 | S-F06-01 → S-F02-01 Dispatch queue ("pending-pickup drill-down") | 
| ReFeed Staff | UC-16 to UC-17 | S-F06-01 → S-F06-02 Audit trail and compliance reports ("drill-down") | 
| ReFeed Staff | UC-05 (plan to dispatch) | S-F02-01 → S-F02-02 ("Plan route"), modal "Confirm route and dispatch?" | 
| Supplier to Staff | UC-03 to UC-05 | Actor changes; the request created on S-F01-03 appears in the staff dispatch queue S-F02-01 | 
| Staff to Driver | UC-05 to UC-06 | Actor changes; entry point "Dispatch notification" → S-F02-03 | 
| Driver to Supplier | UC-07 to UC-08 | Actor changes; entry point "Notification: draft agreement ready" → S-F03-01 | 
| Driver to Warehouse | UC-06 to UC-10 | The driver delivers the UCO to the warehouse; the warehouse staff opens S-F04-01 from S-F00-03 Warehouse home | 

ReFeed has 24 screens, so a single diagram would be crowded; it keeps one per actor because the four actors share only the sign-in screen.
