# Screen List: ReFeed System

| Screen ID | Screen name | Actor | Feature | Shown when (purpose) | Entry points | Use cases / FRs served | Priority | Screen spec |
|---|---|---|---|---|---|---|---|---|
| S-F00-01 | Sign in | Supplier; Driver / Collector; Warehouse Staff; ReFeed Staff | F-00 | A user who is not signed in opens the system; checks credentials and opens the home of the user's role | System launch; sign out | UC-01 · FR-01-01 (supplier login), FR-06-03 (role-based access) | Must | `screens/screen-spec-S-F00-01.md` |
| S-F01-01 | Supplier registration | Supplier | F-01 | A new supplier creates an account and enters business, representative, address, GPS and Point-of-Origin / compliance data; the system creates a unique Supplier_ID | S-F00-01 ("Register") | UC-01, UC-02 · FR-01-01, FR-01-02 | Must | `screens/screen-spec-S-F01-01.md` |
| S-F01-02 | Point-of-Origin and compliance profile | Supplier | F-01 | A supplier views or updates the Point-of-Origin and compliance information stored against the Supplier_ID | Supplier main menu; S-F01-03 (profile-incomplete prompt) | UC-02 · FR-01-02 | Must | `screens/screen-spec-S-F01-02.md` |
| S-F01-03 | Pickup request form | Supplier | F-01 | A supplier requests a UCO pickup (location, estimated volume, preferred time window, notes) and receives the request ID; reused in edit mode to reschedule an existing request | Supplier main menu; S-F01-05 ("Reschedule") | UC-03, UC-04 · FR-01-03, FR-01-04 | Must | `screens/screen-spec-S-F01-03.md` |
| S-F01-04 | My pickup requests | Supplier | F-01 | A supplier sees all own pickup requests with their current status | Supplier main menu; S-F01-03 (after a request is created) | UC-04 · FR-01-05 | Could | `screens/screen-spec-S-F01-04.md` |
| S-F01-05 | Pickup request detail | Supplier | F-01 | A supplier sees one request: status, scheduled date/time, assigned driver and vehicle when available, last status-update time; can reschedule or cancel an eligible request | S-F01-04 (select a request) | UC-04 · FR-01-04, FR-01-05 | Should | `screens/screen-spec-S-F01-05.md` |
| S-F02-01 | Dispatch queue | ReFeed Staff | F-02 | Operations staff see pending pickup requests by area, with estimated volume, waiting time, and available drivers and vehicles, and start planning a route | Staff main menu; S-F06-01 (pending-pickup drill-down) | UC-05 · FR-02-01 | Must | `screens/screen-spec-S-F02-01.md` |
| S-F02-02 | Route review and dispatch | ReFeed Staff | F-02 | Operations staff review the draft route (grouped requests within vehicle capacity, optimized visiting order), assign driver and vehicle, and dispatch it | S-F02-01 ("Plan route" for a selected area) | UC-05 · FR-02-01 | Must | `screens/screen-spec-S-F02-02.md` |
| S-F02-03 | Assigned route | Driver / Collector | F-02 | A driver/collector opens the assigned route and sees its pickup points in visiting order | S-F00-01 (driver home); dispatch notification; S-F02-04 (after a pickup is saved or cancelled) | UC-06 · FR-02-02 | Must | `screens/screen-spec-S-F02-03.md` |
| S-F02-04 | Pickup point collection | Driver / Collector | F-02 | At one pickup point the driver/collector confirms arrival, reports whether UCO is ready, and enters and submits the actual quantity with optional photo and notes | S-F02-03 (select a pickup point) | UC-06 · FR-02-02 (the "UCO not ready" cancellation: see note 3) | Must | `screens/screen-spec-S-F02-04.md` |
| S-F02-05 | Handover record and draft agreement | Driver / Collector | F-02 | The driver/collector records handover time and evidence and sees the calculated transaction amount and the draft handover agreement | S-F02-04 (after collection details are saved) | UC-07 · FR-02-03 | Must | `screens/screen-spec-S-F02-05.md` |
| S-F03-01 | Transaction review and e-signature | Supplier | F-03 | A supplier reviews the recorded handover (quantity, base price, loyalty bonus, total amount) and signs the transaction electronically | Notification after the draft agreement is generated; S-F05-01 (transaction awaiting signature) | UC-08 · FR-03-01 | Must | `screens/screen-spec-S-F03-01.md` |
| S-F03-02 | Transaction receipt | Supplier | F-03 | A supplier views or downloads the electronic receipt (PDF) of a signed transaction, with the warehouse verification status shown for information only | S-F03-01 (after signing); S-F05-01 (transaction history) | UC-09 · FR-03-02 | Must | `screens/screen-spec-S-F03-02.md` |
| S-F04-01 | Warehouse receiving | Warehouse Staff | F-04 | Warehouse staff scan or enter the Collection_ID, record the received quantity and see the comparison with the collected quantity, with a discrepancy alert above tolerance | Warehouse main menu | UC-10 · FR-04-01 | Must | `screens/screen-spec-S-F04-01.md` |
| S-F04-02 | Batch and mass-balance management | Warehouse Staff | F-04 | Warehouse staff create or update a UCO batch from received collections and maintain its mass-balance record | Warehouse main menu; S-F04-01 (after receiving) | UC-11 · FR-04-02 | Should | `screens/screen-spec-S-F04-02.md` |
| S-F05-01 | Loyalty and payout balance | Supplier | F-05 | A supplier sees current loyalty tier, verified UCO volume and progress to the next tier, accumulated payout balance, and transaction history (base payment, loyalty bonus, tier upgrade bonus) | Supplier main menu | UC-12 · FR-05-01 | Must | `screens/screen-spec-S-F05-01.md` |
| S-F05-02 | Payout withdrawal | Supplier | F-05 | A supplier requests a withdrawal of the accumulated payout balance and follows its payout status | S-F05-01 ("Withdraw") | UC-13 · FR-05-02 | Should | `screens/screen-spec-S-F05-02.md` |
| S-F05-03 | Green Profile | Supplier | F-05 | A supplier sees verified UCO contribution and collection history | Supplier main menu; S-F05-01 | UC-14 · FR-05-03 | Could | `screens/screen-spec-S-F05-03.md` |
| S-F05-04 | ESG contribution reports | Supplier | F-05 | A supplier requests and downloads ESG / environmental contribution reports built from verified collection data | S-F05-03 ("Reports") | UC-15 · FR-05-04 | Should | `screens/screen-spec-S-F05-04.md` |
| S-F06-01 | Operations and compliance monitoring | ReFeed Staff | F-06 | Authorised staff oversee suppliers, pickups, collections, transactions and compliance statuses | S-F00-01 (staff home) | UC-16 · FR-06-01 | Should | `screens/screen-spec-S-F06-01.md` |
| S-F06-02 | Audit trail and compliance reports | ReFeed Staff | F-06 | Authorised staff review historical changes and generate compliance reports | Staff main menu; S-F06-01 (drill-down) | UC-17 · FR-06-02 | Must | `screens/screen-spec-S-F06-02.md` |

## Modals and overlays

| Modal | Opened from screen | Purpose |
|---|---|---|
| Cancel this pickup request? | S-F01-05 | Confirm that the supplier cancels an eligible pickup request (FR-01-04) |
| Confirm route and dispatch? | S-F02-02 | Confirm the reviewed route before the driver and vehicle are assigned and the driver is notified (FR-02-01) |
| UCO not ready: cancel this pickup? | S-F02-04 | Confirm that the current pickup is cancelled because UCO is not ready (AD-UC-06, branch `d0 [No: UCO not ready]`) |
| Confirm and e-sign | S-F03-01 | Confirm the electronic signature that locks the agreement (FR-03-01) |
| Confirm withdrawal | S-F05-02 | Confirm the withdrawal amount before the withdrawal request is created (FR-05-02) |

## Checks before you save

- [x] Every Screen ID is unique. No ID is used for two different screens. *21 screens, 21 IDs.*
- [x] No row is a state (error, empty, loading, success) or a confirmation modal. *The profile-incomplete prompt and the request-created message of AD-UC-03 are states of S-F01-03; the discrepancy alert is a state of S-F04-01. Confirm-or-cancel dialogs are in the modal table.*
- [x] Every Must use case has at least one screen, or a note saying it has no user interface
      (for example a time-triggered activity). *Must use cases UC-01, 02, 03, 05, 06, 07, 08, 09, 10, 12, 17 all have a screen. FRs with no user interface are listed in note 2.*
- [x] Every actor action in the activity diagrams maps to a screen in this list. *Checked for AD-UC-03 and AD-UC-06, the two activity diagrams drawn so far (note 4). Re-check when more AD files are added.*
- [x] Every screen serves at least one FR. A screen that serves none is out of scope or a missing FR. *Every row cites at least one FR from W1. Screens with no FR were not added (note 5).*

## What is deliberately not a screen

| Candidate | Why it is not a screen |
|---|---|
| "Pickup request created" | Success state of S-F01-03 (FR-01-03) |
| "Supplier profile incomplete" | Error state of S-F01-03, with a link to S-F01-02 (FR-01-03) |
| "Invalid pickup request details" | Error state of S-F01-03 (FR-01-03) |
| "Invalid collection data" | Error state of S-F02-04 (FR-02-02) |
| "Quantity discrepancy alert" | State of S-F04-01 (FR-04-01) |
| Update payout balance and loyalty tier after signing (FR-05-01) | System-triggered, no user interface; the supplier sees the result on S-F05-01 |
| Enforce role-based access control (FR-06-03) | Applied on every screen; the user meets it at S-F00-01 |
| Run automated tests (FR-06-04), Deploy and monitor (FR-06-05) | Delivery pipeline, no user interface |
| Back up and recover records (FR-06-06) | Time-triggered, no user interface |
