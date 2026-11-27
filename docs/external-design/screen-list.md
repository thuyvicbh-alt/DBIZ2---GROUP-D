# Screen List: ReFeed System

| Screen ID | Screen name | Actor | Feature | Shown when (purpose) | Entry points | Use cases / FRs served | Priority | Screen spec |
|---|---|---|---|---|---|---|---|---|
| S-F00-01 | Sign in | Supplier; Driver / Collector; Warehouse Staff; ReFeed Staff | F-00 | A user who is not signed in opens the system; checks credentials and opens the home of the user's role (supplier S-F00-02, driver S-F02-03, warehouse S-F00-03, ReFeed staff S-F06-01) | System launch; S-F01-01 ("account already exists" redirect); S-F01-06 and the other homes ("Sign out") | UC-01 · FR-01-01, FR-01-05, FR-01-06, FR-06-04 | Must | `screens/screen-spec-S-F00-01.md` |
| S-F00-02 | Supplier home | Supplier | F-00 | A signed-in supplier sees key figures that open the screen behind them (open requests, next pickup, UCO delivered, latest receipt), a banner when a transaction awaits signature, and the bottom navigation to the supplier areas | S-F00-01 (after supplier sign-in); bottom navigation "Home"; S-F03-01 ("Back to home") | UC-01, UC-12 · FR-01-04, FR-05-01, FR-06-04 | Must | `screens/screen-spec-S-F00-02.md` |
| S-F00-03 | Warehouse home | Warehouse Staff | F-00 | Signed-in warehouse staff see today's receiving figures and open warehouse receiving or batch management | S-F00-01 (after warehouse staff sign-in) | UC-01 · FR-01-06, FR-06-04 | Must | `screens/screen-spec-S-F00-03.md` |
| S-F01-01 | Supplier registration | Supplier | F-01 | A new supplier creates an account and enters business, representative, address, GPS and Point-of-Origin / compliance data; the system creates a unique Supplier_ID | S-F00-01 ("Register") | UC-01, UC-02 · FR-01-01, FR-01-02, FR-01-03, FR-01-05, FR-01-07 | Must | `screens/screen-spec-S-F01-01.md` |
| S-F01-02 | Point-of-Origin and compliance profile | Supplier | F-01 | A supplier views or updates the Point-of-Origin and compliance information stored against the Supplier_ID | S-F01-06 ("Point-of-Origin profile"); S-F01-03 (profile-incomplete prompt) | UC-02 · FR-01-07, FR-01-08 | Must | `screens/screen-spec-S-F01-02.md` |
| S-F01-03 | Pickup request form | Supplier | F-01 | A supplier requests a UCO pickup (location, estimated volume, preferred time window, notes) and receives the request ID; reused in edit mode to reschedule an existing request | S-F00-02 (bottom navigation "Collection"); S-F01-05 ("Reschedule") | UC-03, UC-04 · FR-01-09, FR-01-10, FR-01-11, FR-01-12, FR-01-13, FR-01-14, FR-01-17 | Must | `screens/screen-spec-S-F01-03.md` |
| S-F01-04 | My pickup requests | Supplier | F-01 | A supplier sees all own pickup requests with their current status | S-F00-02 (open-requests shortcut); S-F01-03 (after a request is created) | UC-04 · FR-01-15 | Must | `screens/screen-spec-S-F01-04.md` |
| S-F01-05 | Pickup request detail | Supplier | F-01 | A supplier sees one request: status, scheduled date/time, assigned driver and vehicle when available, last status-update time; can reschedule or cancel an eligible request | S-F01-04 (select a request); S-F00-02 (next-pickup shortcut) | UC-04 · FR-01-15, FR-01-16, FR-01-17, FR-01-20 | Must | `screens/screen-spec-S-F01-05.md` |
| S-F01-06 | Supplier account | Supplier | F-01 | A signed-in supplier sees the account summary (business name, Supplier_ID, representative, contact, loyalty tier), opens the Point-of-Origin and compliance profile, changes the interface language and signs out | S-F00-02 (bottom navigation "Profile") | UC-01, UC-02 · FR-01-08, FR-05-01 | Must | `screens/screen-spec-S-F01-06.md` |
| S-F02-01 | Dispatch queue | ReFeed Staff | F-02 | Operations staff see pending pickup requests by area, with estimated volume, waiting time, and available drivers and vehicles, and start planning a route | S-F06-01 (staff home: dispatch entry and pending-pickup drill-down) | UC-05 · FR-02-01 | Must | `screens/screen-spec-S-F02-01.md` |
| S-F02-02 | Route review and dispatch | ReFeed Staff | F-02 | Operations staff review the draft route (grouped requests within vehicle capacity, optimized visiting order), assign driver and vehicle, and dispatch it | S-F02-01 ("Plan route" for a selected area) | UC-05 · FR-02-02, FR-02-03, FR-02-04, FR-02-05 | Must | `screens/screen-spec-S-F02-02.md` |
| S-F02-03 | Assigned route | Driver / Collector | F-02 | A driver/collector opens the assigned route and sees its pickup points in visiting order | S-F00-01 (driver home); dispatch notification; S-F02-04 (after a pickup is saved or cancelled) | UC-06 · FR-02-08 | Must | `screens/screen-spec-S-F02-03.md` |
| S-F02-04 | Pickup point collection | Driver / Collector | F-02 | At one pickup point the driver/collector confirms arrival, reports whether UCO is ready, and enters and submits the actual quantity with optional photo and notes | S-F02-03 (select a pickup point) | UC-06 · FR-02-09, FR-02-10, FR-02-11, FR-02-12, FR-02-13 | Must | `screens/screen-spec-S-F02-04.md` |
| S-F02-05 | Handover record and draft agreement | Driver / Collector | F-02 | The driver/collector records handover time and evidence and sees the calculated transaction amount and the draft handover agreement | S-F02-04 (after collection details are saved) | UC-07 · FR-02-14, FR-02-15, FR-02-16, FR-02-17, FR-02-18 | Must | `screens/screen-spec-S-F02-05.md` |
| S-F03-01 | Transaction review and e-signature | Supplier | F-03 | A supplier reviews the recorded handover (quantity, base price, loyalty bonus, total amount) and signs the transaction electronically | Notification after the draft agreement is generated; S-F00-02 (awaiting-signature banner); S-F05-01 (transaction awaiting signature) | UC-08 · FR-03-01, FR-03-02, FR-03-03, FR-03-04, FR-03-05, FR-03-06, FR-03-07, FR-03-08, FR-03-09, FR-03-10 | Must | `screens/screen-spec-S-F03-01.md` |
| S-F03-02 | Transaction receipt | Supplier | F-03 | A supplier views or downloads the electronic receipt (PDF) of a signed transaction, with the warehouse verification status shown for information only | S-F03-01 (after signing); S-F05-01 (transaction history); S-F00-02 (latest-receipt shortcut) | UC-09 · FR-03-11, FR-03-12, FR-03-13, FR-03-14 | Must | `screens/screen-spec-S-F03-02.md` |
| S-F04-01 | Warehouse receiving | Warehouse Staff | F-04 | Warehouse staff scan or enter the Collection_ID, record the received quantity and see the comparison with the collected quantity, with a discrepancy alert above tolerance | S-F00-03 | UC-10 · FR-04-01, FR-04-02, FR-04-03, FR-04-04, FR-04-05 | Must | `screens/screen-spec-S-F04-01.md` |
| S-F04-02 | Batch and mass-balance management | Warehouse Staff | F-04 | Warehouse staff create or update a UCO batch from received collections and maintain its mass-balance record | S-F00-03; S-F04-01 (after receiving) | UC-11 · FR-04-06, FR-04-07, FR-04-08, FR-04-09 | Must | `screens/screen-spec-S-F04-02.md` |
| S-F05-01 | Loyalty and payout balance | Supplier | F-05 | A supplier sees current loyalty tier, verified UCO volume and progress to the next tier, accumulated payout balance, and transaction history (base payment, loyalty bonus, tier upgrade bonus) | S-F00-02 (bottom navigation "My wallet") | UC-12 · FR-05-01, FR-05-02 | Must | `screens/screen-spec-S-F05-01.md` |
| S-F05-02 | Green Profile | Supplier | F-05 | A supplier sees verified UCO contribution and collection history | S-F00-02 (bottom navigation "Green Profile"; UCO-delivered shortcut); S-F05-01 | UC-13 · FR-05-07 | Should | `screens/screen-spec-S-F05-02.md` |
| S-F05-03 | ESG contribution reports | Supplier | F-05 | A supplier requests and downloads ESG / environmental contribution reports built from verified collection data | S-F05-02 ("Reports") | UC-14 · FR-05-08, FR-05-09 | Should | `screens/screen-spec-S-F05-03.md` |
| S-F06-01 | Operations and compliance monitoring (staff home) | ReFeed Staff | F-06 | The home of ReFeed staff after sign-in: authorised staff oversee suppliers, pickups, collections, transactions and compliance statuses, and open the dispatch queue and the audit trail from it | S-F00-01 (after ReFeed staff sign-in) | UC-15 · FR-06-01 | Must | `screens/screen-spec-S-F06-01.md` |
| S-F06-02 | Audit trail and compliance reports | ReFeed Staff | F-06 | Authorised staff review historical changes and generate compliance reports | S-F06-01 (drill-down) | UC-16 · FR-06-02, FR-06-03 | Must | `screens/screen-spec-S-F06-02.md` |

## Modals and overlays

| Modal | Opened from screen | Purpose |
|---|---|---|
| Cancel this pickup request? | S-F01-05 | Confirm that the supplier cancels an eligible pickup request (FR-01-17) |
| Confirm route and dispatch? | S-F02-02 | Confirm the reviewed route before the driver and vehicle are assigned and the route is dispatched (FR-02-04, FR-02-05) |
| UCO not ready: cancel this pickup? | S-F02-04 | Confirm that the current pickup is cancelled because UCO is not ready (FR-02-13; AD-UC-06 `s6`) |
| Confirm and e-sign | S-F03-01 | Confirm the electronic signature that locks the agreement (FR-03-04, FR-03-06, FR-03-07) |

## Checks before you save

- [x] Every Screen ID is unique. No ID is used for two different screens. *23 screens, 23 IDs.*
- [x] No row is a state (error, empty, loading, success) or a confirmation modal. *The profile-incomplete prompt and the request-created message of AD-UC-03 are states of S-F01-03; "account already exists" is a state of S-F01-01; the discrepancy alert is a state of S-F04-01. Confirm-or-cancel dialogs are in the modal table.*
- [x] Every Must use case has at least one screen, or a note saying it has no user interface
      (for example a time-triggered activity). *The Must use cases in W1 (UC-01 to UC-12, UC-15 and UC-16) all have a screen. The Should use cases UC-13 and UC-14 have S-F05-02 and S-F05-03.*
- [x] Every actor action in the activity diagrams maps to a screen in this list. *Checked for AD-UC-03 (all actions on S-F01-03) and AD-UC-06 (`a1` on S-F02-03; `a2` to `a4` on S-F02-04), both updated to the current W1. Re-check the other activity diagrams when they are updated.*
- [x] Every screen serves at least one FR. A screen that serves none is out of scope or a missing FR. *Every row cites at least one FR from the current W1 (86 L2 functions).*

## What is deliberately not a screen

| Candidate | Why it is not a screen |
|---|---|
| "Account already exists" | State of S-F01-01 that redirects to S-F00-01 (FR-01-03) |
| "Pickup request created" | Success state of S-F01-03 (FR-01-14) |
| "Supplier profile incomplete" | Error state of S-F01-03, with a link to S-F01-02 (FR-01-10) |
| "Invalid pickup request details" | Error state of S-F01-03 (FR-01-12) |
| "Driver already dispatched" | Error state of S-F01-05 (FR-01-16) |
| "Invalid collection data" | Error state of S-F02-04 (FR-02-11) |
| "Invalid handover information" | Error state of S-F02-05 (FR-02-18) |
| "Quantity discrepancy alert" | State of S-F04-01 (FR-04-03) |
| Update payout balance, verified volume and loyalty tier after signing (FR-05-03 to FR-05-06) | System-triggered, no user interface; the supplier sees the result on S-F05-01 |
| Update the central pickup schedule and alert collectors (FR-01-18, FR-01-19); notify the assigned driver (FR-02-06, FR-02-07) | System-triggered updates and notifications; drivers see the result on S-F02-03 |
| Provide batch traceability to Eco Oil System (FR-04-10) | System integration, no user interface |
| Check role permission and record denied attempts (FR-06-04, FR-06-05) | Applied on every screen; the user meets it at S-F00-01 |
| A separate "main menu" list for supplier, warehouse or ReFeed staff | Each role opens on its home screen (S-F00-02, S-F00-03, S-F02-03, S-F06-01); the supplier's areas are reached through the bottom navigation, a component on the supplier screens (ui-conventions §3) |
| Run automated tests, deploy and monitor (FR-06-06 to FR-06-11) | Delivery pipeline, no user interface |
| Back up and verify recovery (FR-06-12 to FR-06-14) | Time-triggered, no user interface |
