# W1 Function List: ReFeed

Lesson 5 (block W1), refined against the activity diagrams present in the repository on 2026-11-25. The existing six features, use cases, business rules and release boundary remain the source of scope.

## Decomposition and trace rules

Each **system action node is one L2 function**. Action names and node IDs below come from the actual Mermaid code blocks, including nodes in the `System` lane of AD-UC-07. A node whose label contains several closely related operations remains one function because the team has already drawn it as one action. Actor actions supply the inputs or consume the outputs of the corresponding functions; their UI work is included in L3. Decisions provide acceptance conditions. Start/end, object, merge, fork and join nodes describe control or data flow and are not additional functions.

L2 and L3 numbering follows the [LabDesk W1 example](https://github.com/chienact-code/dbiz2-labdesk-example/blob/main/docs/function-list/w1-function-list.md):

- **L2:** `FR-xx-yy`, where `xx` is the existing feature number and `yy` is the function's consecutive number within that feature, starting at `01`. The sequence continues across use cases in the same feature.
- **L3:** the L2 ID plus `-FE`, `-BE` or `-INT`, for example `FR-01-01-FE`. If one L2 ever needs several tasks in the same layer, use numbered layer suffixes such as `-BE1`, `-BE2`, as in the example; the current tasks need no such suffixes.

Every source node maps to exactly one function; equivalent actions repeated across UC-12 and UC-13 reuse the same function and ID. The numbering follows the activity-based split. UC-08 now has its own AD/SD: its 10 F-03 action nodes include the previously recorded agreement lock, signature storage and pickup completion. The AD covers UC-08 only; the existing F-05 post-signing functions remain in UC-12.

| Feature | Current L2 ID range | L2 functions |
| --- | --- | --- |
| F-01 | FR-01-01 to FR-01-20 | 20 |
| F-02 | FR-02-01 to FR-02-18 | 18 |
| F-03 | FR-03-01 to FR-03-14 | 14 |
| F-04 | FR-04-01 to FR-04-11 | 11 |
| F-05 | FR-05-01 to FR-05-14 | 14 |
| F-06 | FR-06-01 to FR-06-14 | 14 |

The **Legacy family** and **Legacy estimate package** columns retain IDs from the original source snapshot for comparison with the existing ADs, screens, W2 and W4. These columns identify historical groups and estimates; current L2/L3 IDs are the IDs in the L2/L3 columns. Use the crosswalk below to translate an original family into its current functions, since a historical ID can now coincide with a differently numbered function. Existing activity diagrams other than the new AD-UC-08 and unreconciled source files retain their original IDs. Updated UC-08/UC-09 screen references use the current function IDs.

The original handover family is corrected to `FR-02-03`, matching AD-UC-07, the original L3 work items and W4; its current functions are `FR-02-14` to `FR-02-18`. Supplier signing in UC-08 now uses `FR-03-01` to `FR-03-10`; receipt functions in UC-09 use `FR-03-11` to `FR-03-14`. Tracking node AD-UC-04 `s2` maps to current function `FR-01-20`, with original tracking family `FR-01-05` retained in the Legacy family column.

Where no activity diagram exists, the functions are separated only along behaviours already stated in the original W1 L2/L3 rows. They are explicitly labelled **W1 baseline; no activity node**. No new diagram or business workflow is assumed.

Initial priority is inherited from the original W1 family. Effective priority is inherited via the Legacy family column from [W4](w4-priorities.md), including deferred traceability function `FR-04-11` (original family `FR-04-03`). The **FP family type** column preserves the original family classification for traceability; internal validation, persistence and feedback steps are not independent function-point transactions. Do not sum one FP weight per new L2 row.

## L2 functions

### F-01 — Supplier Onboarding & Pickup Management

| L2 ID | Legacy family | L2 function name | Primary actor | EARS acceptance criterion | Action-node / baseline trace | Initial priority | FP family type |
| --- | --- | --- | --- | --- | --- | --- | --- |
| FR-01-01 | FR-01-01 | Validate Credentials and Compliance Data | Supplier | WHEN a supplier submits credentials or registration data, THEN the system SHALL validate the submitted credentials and applicable compliance data and return the authentication-validity result. | [AD-UC-01](../activity/AD-UC-01.md) `validateAuth` | Must | EI |
| FR-01-02 | FR-01-01 | Create Supplier Account with Unique Supplier_ID | Supplier | WHEN submitted registration data are valid and the supplier is new, THEN the system SHALL create an Active supplier account with a unique Supplier_ID. | [AD-UC-01](../activity/AD-UC-01.md) `createAccount` | Must | EI |
| FR-01-03 | FR-01-01 | Display Account Already Exists & Redirect to Login | Supplier | WHEN the existing-supplier branch is selected, THEN the system SHALL display that the account already exists and redirect the supplier to login. | [AD-UC-01](../activity/AD-UC-01.md) `authenticateUser` | Must | EI |
| FR-01-04 | FR-01-01 | Display Dashboard & Authentication Result | Supplier | WHEN a new supplier account has been created successfully, THEN the system SHALL display the dashboard and authentication result. | [AD-UC-01](../activity/AD-UC-01.md) `displayAuthSuccess` | Must | EI |
| FR-01-05 | FR-01-01 | Display Error Message & Prompt Retry | Supplier | WHEN credentials or registration/compliance data are invalid, THEN the system SHALL display an error and prompt retry without creating an account or session. | [AD-UC-01](../activity/AD-UC-01.md) `displayAuthError` | Must | EI |
| FR-01-06 | FR-01-01 | Authenticate returning supplier | Supplier | WHEN a returning supplier submits valid login credentials, THEN the system SHALL authenticate the supplier. | Original W1 L2 FR-01-01 returning-login requirement; no explicit authentication action in AD-UC-01 | Must | EI |
| FR-01-07 | FR-01-02 | Validate Point-of-Origin and compliance information | Supplier | WHEN a supplier submits or updates Point-of-Origin or compliance information, THEN the system SHALL validate the submitted profile fields. | Original W1 L2 and L3; no activity node | Must | EI |
| FR-01-08 | FR-01-02 | Store Point-of-Origin and compliance profile | Supplier | WHEN submitted Point-of-Origin and compliance information is valid, THEN the system SHALL store the profile against the Supplier_ID. | Original W1 L2 and L3; no activity node | Must | EI |
| FR-01-09 | FR-01-03 | Check that supplier profile is complete | Supplier | WHEN a signed-in supplier submits a pickup request, THEN the system SHALL check that the supplier profile contains GPS and compliance data. | [AD-UC-03](../activity/AD-UC-03.md) `s1` | Must | EI |
| FR-01-10 | FR-01-03 | Reject request and prompt profile update | Supplier | WHEN the supplier profile lacks GPS or compliance data, THEN the system SHALL reject the pickup request without creating a pickup and prompt the supplier to update the profile. | [AD-UC-03](../activity/AD-UC-03.md) `s2` | Must | EI |
| FR-01-11 | FR-01-03 | Validate pickup location, estimated volume and time window | Supplier | WHEN the supplier profile is complete, THEN the system SHALL validate the submitted pickup location, estimated volume and preferred time window using the draft rules in AD-UC-03. | [AD-UC-03](../activity/AD-UC-03.md) `s3` | Must | EI |
| FR-01-12 | FR-01-03 | Return validation feedback | Supplier | WHEN pickup-request details are missing or invalid, THEN the system SHALL return field-validation feedback for correction without creating a pickup. | [AD-UC-03](../activity/AD-UC-03.md) `s4` | Must | EI |
| FR-01-13 | FR-01-03 | Create pickup request with status Requested and record the time window | Supplier | WHEN the supplier profile is complete and pickup details are valid, THEN the system SHALL create a pickup linked to Supplier_ID with status Requested and the submitted time window. | [AD-UC-03](../activity/AD-UC-03.md) `s5` | Must | EI |
| FR-01-14 | FR-01-03 | Return pickup request ID | Supplier | WHEN the pickup request has been created, THEN the system SHALL return its request ID to the supplier. | [AD-UC-03](../activity/AD-UC-03.md) `s6` | Must | EI |
| FR-01-15 | FR-01-04 | Fetch pickup request status & validation rules | Supplier | WHEN a supplier selects an existing pickup request, THEN the system SHALL fetch its current status and applicable modification rules. | [AD-UC-04](../activity/AD-UC-04.md) `s1` | Must | EI |
| FR-01-16 | FR-01-04 | Display modification error: driver already dispatched | Supplier | WHEN a supplier attempts to modify or cancel a pickup whose driver is already dispatched, THEN the system SHALL display the modification-lock error and retain the existing request and schedule. | [AD-UC-04](../activity/AD-UC-04.md) `s3` | Must | EI |
| FR-01-17 | FR-01-04 | Update pickup request status & reschedule timeline | Supplier | WHEN a supplier modifies or cancels an eligible pickup in Requested status, THEN the system SHALL update the pickup status and applicable appointment timeline as specified by AD-UC-04. | [AD-UC-04](../activity/AD-UC-04.md) `s4` | Must | EI |
| FR-01-18 | FR-01-04 | Update central pickup schedule & logs | Supplier | WHEN an eligible pickup request has been changed, THEN the system SHALL update the central pickup schedule and retain the change in its logs. | [AD-UC-04](../activity/AD-UC-04.md) `s5` | Must | EI |
| FR-01-19 | FR-01-04 | Receive dispatch schedule update alert | Supplier | WHEN an eligible pickup request has been changed, THEN the system SHALL provide a dispatch-schedule update alert to UCO Collectors in parallel with the central schedule update. | [AD-UC-04](../activity/AD-UC-04.md) `c1` | Must | EI |
| FR-01-20 | FR-01-05 | Fetch real-time tracking & driver ETA | Supplier | WHEN a supplier selects View status, THEN the system SHALL retrieve tracking and driver ETA as drawn in AD-UC-04 and present the current pickup status, scheduled collection time, assigned driver/vehicle when available and last-update timestamp. | [AD-UC-04](../activity/AD-UC-04.md) `s2` | Must | EQ |

### F-02 — Collection Operations & Handover

| L2 ID | Legacy family | L2 function name | Primary actor | EARS acceptance criterion | Action-node / baseline trace | Initial priority | FP family type |
| --- | --- | --- | --- | --- | --- | --- | --- |
| FR-02-01 | FR-02-01 | Group pending pickups within vehicle capacity | ReFeed Staff | WHEN ReFeed Staff selects an area and requests a route plan, THEN the system SHALL group pending pickups without exceeding the selected vehicle capacity. | [AD-UC-05](../activity/AD-UC-05.md) `s1` | Must | EO |
| FR-02-02 | FR-02-01 | Obtain optimized visiting order from map/route data | ReFeed Staff | WHEN capacity-constrained pickup groups are available, THEN the system SHALL obtain an optimized visiting order from map/route data. | [AD-UC-05](../activity/AD-UC-05.md) `s2` | Must | EO |
| FR-02-03 | FR-02-01 | Display proposed collection route | ReFeed Staff | WHEN an optimized visiting order is available, THEN the system SHALL display the proposed collection route for staff review. | [AD-UC-05](../activity/AD-UC-05.md) `s3` | Must | EO |
| FR-02-04 | FR-02-01 | Assign driver and vehicle to route | ReFeed Staff | WHEN ReFeed Staff confirms the proposed route, THEN the system SHALL assign the driver and vehicle to that route. | [AD-UC-05](../activity/AD-UC-05.md) `s4` | Must | EO |
| FR-02-05 | FR-02-01 | Save and dispatch collection route | ReFeed Staff | WHEN the confirmed route has a driver and vehicle, THEN the system SHALL save and dispatch that collection route. | [AD-UC-05](../activity/AD-UC-05.md) `s5` | Must | EO |
| FR-02-06 | FR-02-01 | Update included pickups to Driver Assigned | ReFeed Staff | WHEN the collection route has been dispatched, THEN the system SHALL update its included pickups to Driver Assigned. | [AD-UC-05](../activity/AD-UC-05.md) `s6` | Must | EO |
| FR-02-07 | FR-02-01 | Notify assigned driver | ReFeed Staff | WHEN included pickups have been updated to Driver Assigned, THEN the system SHALL notify the assigned driver of the dispatched route. | [AD-UC-05](../activity/AD-UC-05.md) `s7` | Must | EO |
| FR-02-08 | FR-02-02 | Display assigned route and pickup points | Driver / Collector | WHEN a driver opens their assigned collection route, THEN the system SHALL display that route and its assigned pickup points. | [AD-UC-06](../activity/AD-UC-06.md) `s1` | Must | EI |
| FR-02-09 | FR-02-02 | Update selected pickup status to Arrived | Driver / Collector | WHEN the assigned driver selects a supplier point and confirms arrival, THEN the system SHALL update that pickup to Arrived. | [AD-UC-06](../activity/AD-UC-06.md) `s2` | Must | EI |
| FR-02-10 | FR-02-02 | Validate actual quantity, GPS, timestamp and supplied evidence | Driver / Collector | WHEN UCO is ready and the driver submits collection details, THEN the system SHALL validate actual quantity, GPS, timestamp and any supplied optional photo/notes using the AD-UC-06 draft rules. | [AD-UC-06](../activity/AD-UC-06.md) `s3` | Must | EI |
| FR-02-11 | FR-02-02 | Return validation feedback | Driver / Collector | WHEN required collection data are missing or invalid, THEN the system SHALL return validation feedback for correction, save no collection details and retain Arrived status. | [AD-UC-06](../activity/AD-UC-06.md) `s4` | Must | EI |
| FR-02-12 | FR-02-02 | Save actual quantity, GPS, timestamp and supplied evidence against pickup | Driver / Collector | WHEN collection details are valid, THEN the system SHALL save actual quantity, GPS, timestamp and supplied evidence against the assigned pickup. | [AD-UC-06](../activity/AD-UC-06.md) `s5` | Must | EI |
| FR-02-13 | FR-02-02 | Cancel current pickup and update status to Cancelled | Driver / Collector | WHEN the assigned driver reports that UCO is not ready at an Arrived pickup, THEN the system SHALL set that pickup to Cancelled without creating a collection record. | [AD-UC-06](../activity/AD-UC-06.md) `s6` | Must | EI |
| FR-02-14 | FR-02-03 | Validate actual quantity, handover time, and evidence | Driver / Collector | WHEN a driver submits handover information for an assigned pickup, THEN the system SHALL validate actual quantity, handover time and required evidence as specified by AD-UC-07. | [AD-UC-07](../activity/AD-UC-07.md) `a2` | Must | EI |
| FR-02-15 | FR-02-03 | Store handover record against assigned pickup | Driver / Collector | WHEN handover information is valid, THEN the system SHALL store the handover record against the assigned pickup. | [AD-UC-07](../activity/AD-UC-07.md) `a3` | Must | EI |
| FR-02-16 | FR-02-03 | Calculate transaction amount | Driver / Collector | WHEN the validated handover record is stored, THEN the system SHALL calculate the transaction amount using the actual quantity, base price and supplier current loyalty bonus rate. | [AD-UC-07](../activity/AD-UC-07.md) `a4` | Must | EI |
| FR-02-17 | FR-02-03 | Generate draft agreement | Driver / Collector | WHEN the transaction amount has been calculated, THEN the system SHALL generate the draft handover agreement. | [AD-UC-07](../activity/AD-UC-07.md) `a5` | Must | EI |
| FR-02-18 | FR-02-03 | Reject and prompt for correction | Driver / Collector | WHEN handover information is invalid, THEN the system SHALL reject the submission without saving a handover record and prompt the driver to correct it. | [AD-UC-07](../activity/AD-UC-07.md) `a7` | Must | EI |

### F-03 — UCO Transaction

| L2 ID | Legacy family | L2 function name | Primary actor | EARS acceptance criterion | Action-node / baseline trace | Initial priority | FP family type |
| --- | --- | --- | --- | --- | --- | --- | --- |
| FR-03-01 | FR-03-01 | Retrieve supplier draft agreement | Supplier | WHEN a signed-in supplier opens a transaction awaiting signature, THEN the system SHALL retrieve the unsigned draft belonging to that supplier, including the recorded handover and calculated amount. | [AD-UC-08](../activity/AD-UC-08.md) `s1`; [SD-UC-08](../use-case/SD-UC-08.md) | Must | EI |
| FR-03-02 | FR-03-01 | Display handover and calculated amount | Supplier | WHEN an owned unsigned draft is available, THEN the system SHALL display its recorded quantity, base price, loyalty bonus and calculated amount as read-only values. | [AD-UC-08](../activity/AD-UC-08.md) `s2`; [SD-UC-08](../use-case/SD-UC-08.md) | Must | EI |
| FR-03-03 | FR-03-01 | Display no transaction awaiting signature | Supplier | WHEN no unsigned draft belonging to the signed-in supplier is available, THEN the system SHALL display the no-transaction-awaiting-signature state without exposing another supplier transaction. | [AD-UC-08](../activity/AD-UC-08.md) `s3`; [SD-UC-08](../use-case/SD-UC-08.md) | Must | EI |
| FR-03-04 | FR-03-01 | Validate signature and confirmation | Supplier | WHEN the supplier submits an electronic signature for the confirmed transaction, THEN the system SHALL validate that the signature is nonempty and the agreement confirmation is present. | [AD-UC-08](../activity/AD-UC-08.md) `s4`; [SD-UC-08](../use-case/SD-UC-08.md) | Must | EI |
| FR-03-05 | FR-03-01 | Return signing validation feedback | Supplier | WHEN the submitted signature is missing or the agreement confirmation is unchecked, THEN the system SHALL return correction feedback without locking the agreement, storing a signature or completing the pickup. | [AD-UC-08](../activity/AD-UC-08.md) `s5`; [SD-UC-08](../use-case/SD-UC-08.md) | Must | EI |
| FR-03-06 | FR-03-01 | Lock agreement and calculated amount | Supplier | WHEN the supplier signs the reviewed transaction with valid signing input, THEN the system SHALL lock the agreement document hash together with the calculated amount. | [AD-UC-08](../activity/AD-UC-08.md) `s6`; [SD-UC-08](../use-case/SD-UC-08.md) | Must | EI |
| FR-03-07 | FR-03-01 | Store electronic signature | Supplier | WHEN the supplier signs the reviewed transaction, THEN the system SHALL store the electronic signature and signing time against that transaction. | [AD-UC-08](../activity/AD-UC-08.md) `s7`; [SD-UC-08](../use-case/SD-UC-08.md) | Must | EI |
| FR-03-08 | FR-03-01 | Mark pickup as Completed | Supplier | WHEN the signed agreement and electronic signature are stored successfully, THEN the system SHALL mark the linked pickup as Completed. | [AD-UC-08](../activity/AD-UC-08.md) `s8`; [SD-UC-08](../use-case/SD-UC-08.md) | Must | EI |
| FR-03-09 | FR-03-01 | Return signing failure and prompt retry | Supplier | WHEN signing the transaction fails, THEN the system SHALL retain the unsigned agreement and previous pickup/balance state and return failure feedback prompting the supplier to sign again. | [AD-UC-08](../activity/AD-UC-08.md) `s9`; [SD-UC-08](../use-case/SD-UC-08.md) | Must | EI |
| FR-03-10 | FR-03-01 | Display signing success | Supplier | WHEN the transaction is signed successfully, THEN the system SHALL display the signing confirmation, locked signature, signing time and signed amount, with receipt and home navigation. | [AD-UC-08](../activity/AD-UC-08.md) `s10`; [SD-UC-08](../use-case/SD-UC-08.md) | Must | EI |
| FR-03-11 | FR-03-02 | Check receipt ownership and signed status | Supplier | WHEN a supplier requests a transaction receipt, THEN the system SHALL check that the transaction is signed and belongs to that supplier. | Original W1 L2 and L3; no activity node | Must | EQ |
| FR-03-12 | FR-03-02 | Retrieve receipt data | Supplier | WHEN receipt ownership and signed status are valid, THEN the system SHALL retrieve the transaction receipt data. | Original W1 L2 and L3; no activity node | Must | EQ |
| FR-03-13 | FR-03-02 | Render and provide electronic receipt PDF | Supplier | WHEN authorized signed-transaction receipt data are available, THEN the system SHALL render and provide the electronic receipt PDF for viewing or downloading. | Original W1 L2 and L3; no activity node | Must | EQ |
| FR-03-14 | FR-03-02 | Show warehouse verification status | Supplier | WHEN the supplier views an authorized signed-transaction receipt, THEN the system SHALL show warehouse verification status for information only. | Original W1 L2 and L3; no activity node | Must | EQ |

### F-04 — Warehouse & Traceability

| L2 ID | Legacy family | L2 function name | Primary actor | EARS acceptance criterion | Action-node / baseline trace | Initial priority | FP family type |
| --- | --- | --- | --- | --- | --- | --- | --- |
| FR-04-01 | FR-04-01 | Record received quantity | Warehouse Staff | WHEN Warehouse Staff receives collected UCO whose transaction has been signed, THEN the system SHALL record the received quantity against the collection. | [AD-UC-10](../activity/AD-UC-10.md) `s1` | Must | EI |
| FR-04-02 | FR-04-01 | Compare received quantity with collection record | Warehouse Staff | WHEN the received quantity has been recorded, THEN the system SHALL compare it with the quantity in the collection record using the configured tolerance limit. | [AD-UC-10](../activity/AD-UC-10.md) `s2` | Must | EI |
| FR-04-03 | FR-04-01 | Log discrepancy alert for internal reconciliation | Warehouse Staff | WHEN the quantity difference exceeds the tolerance limit, THEN the system SHALL log a discrepancy alert for internal reconciliation without halting receiving. | [AD-UC-10](../activity/AD-UC-10.md) `s3` | Must | EI |
| FR-04-04 | FR-04-01 | Keep signed transaction amount unchanged | Warehouse Staff | WHEN warehouse receiving is processed, whether within or outside tolerance, THEN the system SHALL preserve the signed transaction amount unchanged. | [AD-UC-10](../activity/AD-UC-10.md) `s4` | Must | EI |
| FR-04-05 | FR-04-01 | Update collection status to Received | Warehouse Staff | WHEN warehouse quantity comparison and any discrepancy alert are processed, THEN the system SHALL update the collection status to Received. | [AD-UC-10](../activity/AD-UC-10.md) `s5` | Must | EI |
| FR-04-06 | FR-04-02 | Validate collection inputs & batch parameters | Warehouse Staff | WHEN Warehouse Staff selects verified UCO collections and enters batch parameters, THEN the system SHALL validate those inputs and parameters, including invalid volume or duplicate inputs. | [AD-UC-11](../activity/AD-UC-11.md) `s1` | Must | EI |
| FR-04-07 | FR-04-02 | Prompt invalid parameters & request correction | Warehouse Staff | WHEN collection inputs or batch parameters are invalid, THEN the system SHALL display the invalid-parameter feedback and request correction. | [AD-UC-11](../activity/AD-UC-11.md) `s2` | Must | EI |
| FR-04-08 | FR-04-02 | Link collections & compute mass-balance metrics | Warehouse Staff | WHEN batch inputs and parameters are valid, THEN the system SHALL link the component collections and compute mass-balance metrics for the Created UCO batch. | [AD-UC-11](../activity/AD-UC-11.md) `s3` | Must | EI |
| FR-04-09 | FR-04-02 | Maintain linked collection inputs & update batch balance | Warehouse Staff | WHEN the UCO batch has been created, THEN the system SHALL maintain its linked collection inputs and update its batch balance. | [AD-UC-11](../activity/AD-UC-11.md) `s4` | Must | EI |
| FR-04-10 | FR-04-02 | Receive batch traceability & mass-balance record | Warehouse Staff | WHEN the UCO batch has been created, THEN the system SHALL make its batch traceability and mass-balance record available to Eco Oil System in parallel with internal balance maintenance. | [AD-UC-11](../activity/AD-UC-11.md) `e1` | Must | EI |
| FR-04-11 | FR-04-03 | Trace UCO through the supply chain | ReFeed Staff | WHEN authorised Refeed staff requests a trace, THEN the system SHALL display the traceable relationship from supplier and collection through warehouse batch to downstream handover. | Original W1 L2 and L3; no activity node | Must | EQ |

### F-05 — Supplier Value & ESG / Green Profile

| L2 ID | Legacy family | L2 function name | Primary actor | EARS acceptance criterion | Action-node / baseline trace | Initial priority | FP family type |
| --- | --- | --- | --- | --- | --- | --- | --- |
| FR-05-01 | FR-05-01 | Retrieve Loyalty Tier & Payout Balance | Supplier | WHEN an Active signed-in supplier requests the Loyalty & Payout overview, THEN the system SHALL retrieve their current loyalty tier, progress to the next tier and accumulated payout balance. | [AD-UC-12](../activity/AD-UC-12.md) `retrieve` | Must | EQ |
| FR-05-02 | FR-05-01 | Display Loyalty Tier & History Details | Supplier | WHEN the supplier chooses View tier and history details, THEN the system SHALL display the current tier, verified UCO volume, progress to the next tier and itemized base-payment, loyalty-bonus and tier-upgrade-bonus history. | [AD-UC-12](../activity/AD-UC-12.md) `displayTier` | Must | EQ |
| FR-05-03 | FR-05-01 | Add signed transaction payment to payout balance | Supplier | WHEN a UCO transaction is signed, THEN the system SHALL add its locked base payment and loyalty bonus to the supplier accumulated payout balance. | Original W1 FR-05-01; UC-12 post-signing requirement; not drawn in the current AD-UC-12 | Must | EQ |
| FR-05-04 | FR-05-01 | Add signed transaction quantity to verified UCO volume | Supplier | WHEN a UCO transaction is signed, THEN the system SHALL add its actual quantity to the supplier verified UCO volume. | Original W1 FR-05-01; UC-12 post-signing requirement; not drawn in the current AD-UC-12 | Must | EQ |
| FR-05-05 | FR-05-01 | Upgrade loyalty tier | Supplier | WHEN the supplier verified UCO volume reaches a higher tier threshold, THEN the system SHALL upgrade the supplier loyalty tier. | Original W1 FR-05-01; UC-12 post-signing requirement; not drawn in the current AD-UC-12 | Must | EQ |
| FR-05-06 | FR-05-01 | Award tier upgrade bonus | Supplier | WHEN the supplier loyalty tier is upgraded, THEN the system SHALL award the applicable tier upgrade bonus. | Original W1 FR-05-01; UC-12 post-signing requirement; not drawn in the current AD-UC-12 | Must | EQ |
| FR-05-07 | FR-05-02 | Validate Withdrawal Request | Supplier | WHEN a supplier requests a balance withdrawal, THEN the system SHALL validate the requested amount against available balance, the minimum withdrawal threshold and supplier/bank-profile eligibility. | [AD-UC-12](../activity/AD-UC-12.md) `validate`; [AD-UC-13](../activity/AD-UC-13.md) `s1` | Must | EI |
| FR-05-08 | FR-05-02 | Display Existing Balance & Withdrawal Rejection Reason | Supplier | WHEN a withdrawal request is ineligible, THEN the system SHALL display the existing balance and rejection reason without creating a withdrawal transaction or deducting balance. | [AD-UC-12](../activity/AD-UC-12.md) `display`; [AD-UC-13](../activity/AD-UC-13.md) `s3` | Must | EI |
| FR-05-09 | FR-05-02 | Create Withdrawal Transaction | Supplier | WHEN a withdrawal request is eligible, THEN the system SHALL create a withdrawal transaction record for external payout handling. | [AD-UC-12](../activity/AD-UC-12.md) `transaction`; [AD-UC-13](../activity/AD-UC-13.md) `s2` | Must | EI |
| FR-05-10 | FR-05-02 | Update Payout Status | Supplier | WHEN an eligible withdrawal is accepted, THEN the system SHALL update its payout status and deduct the accepted amount from available payout balance; WHEN the request is rejected, THEN the system SHALL retain the existing balance and shall not mark a payout as completed. | [AD-UC-12](../activity/AD-UC-12.md) `update`; [AD-UC-13](../activity/AD-UC-13.md) `s4` | Must | EI |
| FR-05-11 | FR-05-02 | Display Withdrawal Result & Updated Payout Status and Balance | Supplier | WHEN withdrawal transaction creation and payout-status update have both completed, THEN the system SHALL display the withdrawal result, updated payout status and balance. | [AD-UC-12](../activity/AD-UC-12.md) `displayUpdated` | Must | EI |
| FR-05-12 | FR-05-03 | View Green Profile and UCO contribution history | Supplier | WHEN a supplier opens the Green Profile, THEN the system SHALL display verified UCO contribution and collection history. | Original W1 L2 and L3; no activity node | Should | EQ |
| FR-05-13 | FR-05-04 | Generate ESG / environmental contribution report | Supplier | WHEN a supplier requests an ESG or environmental contribution report, THEN the system SHALL generate the report using verified collection data. | Original W1 L2 and L3; no activity node | Should | EO |
| FR-05-14 | FR-05-04 | Provide ESG / environmental contribution report | Supplier | WHEN the requested ESG or environmental contribution report is generated, THEN the system SHALL provide it to the supplier. | Original W1 L2 and L3; no activity node | Should | EO |

### F-06 — Operations, Audit & Compliance

| L2 ID | Legacy family | L2 function name | Primary actor | EARS acceptance criterion | Action-node / baseline trace | Initial priority | FP family type |
| --- | --- | --- | --- | --- | --- | --- | --- |
| FR-06-01 | FR-06-01 | Monitor operations and compliance status | ReFeed Staff | WHEN authorised staff opens the monitoring view, THEN the system SHALL display relevant suppliers, pickups, collections, transactions and compliance statuses for operational oversight. | Original W1 L2 and L3; no activity node | Must | EQ |
| FR-06-02 | FR-06-02 | Retrieve and display audit history | ReFeed Staff | WHEN authorized ReFeed Staff requests audit information, THEN the system SHALL retrieve and display historical changes. | Original W1 L2 and L3; no activity node | Must | EO |
| FR-06-03 | FR-06-02 | Generate compliance report | ReFeed Staff | WHEN authorized ReFeed Staff requests compliance information, THEN the system SHALL generate the applicable compliance report. | Original W1 L2 and L3; no activity node | Must | EO |
| FR-06-04 | FR-06-03 | Check role permission for protected action | System | WHEN an authenticated user attempts a protected action, THEN the system SHALL permit it only when the assigned role grants the required permission. | Original W1 L2 and L3; no activity node | Must | NC |
| FR-06-05 | FR-06-03 | Record denied access attempt | System | WHEN a protected action is denied by the permission check, THEN the system SHALL record the denied attempt. | Original W1 L2 and L3; no activity node | Must | NC |
| FR-06-06 | FR-06-04 | Run automated unit tests | System | WHEN code is merged into the release branch, THEN the delivery pipeline SHALL run automated unit tests. | Original W1 L2 and L3; no activity node | Must | NC |
| FR-06-07 | FR-06-04 | Run automated API tests | System | WHEN code is merged into the release branch, THEN the delivery pipeline SHALL run automated API tests. | Original W1 L2 and L3; no activity node | Must | NC |
| FR-06-08 | FR-06-04 | Run automated acceptance tests | System | WHEN code is merged into the release branch, THEN the delivery pipeline SHALL run automated acceptance tests. | Original W1 L2 and L3; no activity node | Must | NC |
| FR-06-09 | FR-06-04 | Block deployment on test failure | System | WHEN any required automated unit, API or acceptance test fails, THEN the delivery pipeline SHALL block deployment. | Original W1 L2 and L3; no activity node | Must | NC |
| FR-06-10 | FR-06-05 | Deploy approved release | System | WHEN a release is approved, THEN the delivery pipeline SHALL deploy it to the target environment. | Original W1 L2 and L3; no activity node | Must | NC |
| FR-06-11 | FR-06-05 | Expose service monitoring signals | System | WHEN the release is deployed, THEN the system SHALL expose health, error and latency signals for operational monitoring. | Original W1 L2 and L3; no activity node | Must | NC |
| FR-06-12 | FR-06-06 | Create scheduled operational and audit backup | System | WHEN the scheduled backup window occurs, THEN the system SHALL create a recoverable backup of operational and audit data. | Original W1 L2 and L3; no activity node | Must | NC |
| FR-06-13 | FR-06-06 | Record backup result | System | WHEN a scheduled backup attempt finishes, THEN the system SHALL record its result. | Original W1 L2 and L3; no activity node | Must | NC |
| FR-06-14 | FR-06-06 | Verify backup recovery | System | WHEN recovery verification is run for a saved backup, THEN the system SHALL verify that the backed-up operational and audit records can be recovered. | Original W1 L3 FR-06-06-BE: scheduled backup and recovery verification; no activity node | Must | NC |

## Requirement-family crosswalk

These original families are historical reference and costing groups, not current L2 function IDs. The last column lists the current functions under the example's numbering scheme. The original UC/CTX/CVCA/architecture traces are retained here alongside the action-node traces above. Legacy sequence labels are preserved as source history; the exact current activity file is the authority for the decomposition.

| Legacy family (original source IDs) | Existing scope / source trace | Current L2 IDs |
| --- | --- | --- |
| FR-01-01 | Register/Login as a UCO supplier — UC:UC-01; CTX:supplier-registration; CVCA:supplier-flow | FR-01-01, FR-01-02, FR-01-03, FR-01-04, FR-01-05, FR-01-06 |
| FR-01-02 | Manage Point-of-Origin and compliance profile — UC:UC-02; CTX:supplier-profile; CVCA:compliance-flow | FR-01-07, FR-01-08 |
| FR-01-03 | Request and schedule a UCO pickup — UC:UC-03; CTX:supplier-profile; CVCA:compliance-flow | FR-01-09, FR-01-10, FR-01-11, FR-01-12, FR-01-13, FR-01-14 |
| FR-01-04 | Manage an existing pickup request — UC:UC-04; SEQ:S-01; CTX:pickup-status | FR-01-15, FR-01-16, FR-01-17, FR-01-18, FR-01-19 |
| FR-01-05 | Track pickup status — UC:UC-04; SEQ:S-01; CTX:pickup-status | FR-01-20 |
| FR-02-01 | Plan and dispatch a UCO collection route — UC:UC-05; SEQ:S-01; CTX:dispatch-route; CVCA:logistics-flow | FR-02-01, FR-02-02, FR-02-03, FR-02-04, FR-02-05, FR-02-06, FR-02-07 |
| FR-02-02 | View assigned route and complete a UCO collection — UC:UC-06; SEQ:S-01; CTX:collection-confirmation; CVCA:collection-flow | FR-02-08, FR-02-09, FR-02-10, FR-02-11, FR-02-12, FR-02-13 |
| FR-02-03 | Record UCO handover information — UC:UC-07; SEQ:S-01; CTX:collection-confirmation; CVCA:collection-flow | FR-02-14, FR-02-15, FR-02-16, FR-02-17, FR-02-18 |
| FR-03-01 | Confirm and sign a UCO transaction — UC:UC-08; SEQ:SD-UC-08; AD:AD-UC-08; CTX:transaction-flow | FR-03-01, FR-03-02, FR-03-03, FR-03-04, FR-03-05, FR-03-06, FR-03-07, FR-03-08, FR-03-09, FR-03-10 |
| FR-03-02 | View or download transaction receipt — UC:UC-09; SEQ:S-02; CTX:transaction-flow | FR-03-11, FR-03-12, FR-03-13, FR-03-14 |
| FR-04-01 | Receive and verify collected UCO at warehouse — UC:UC-10; SEQ:S-01; CTX:warehouse-flow | FR-04-01, FR-04-02, FR-04-03, FR-04-04, FR-04-05 |
| FR-04-02 | Manage UCO batch and mass-balance records — UC:UC-11; SEQ:S-01; CTX:warehouse-batch-flow | FR-04-06, FR-04-07, FR-04-08, FR-04-09, FR-04-10 |
| FR-04-03 | Trace UCO through the supply chain — UC:UC-11; SEQ:S-02; CTX:traceability-flow; CVCA:traceability-flow | FR-04-11 |
| FR-05-01 | View loyalty status and accumulated payout balance — UC:UC-12; CTX:loyalty-payout-flow; CVCA:supplier-value-flow | FR-05-01, FR-05-02, FR-05-03, FR-05-04, FR-05-05, FR-05-06 |
| FR-05-02 | Withdraw accumulated payout balance — UC:UC-13; CTX:payment-flow; CVCA:supplier-value-flow | FR-05-07, FR-05-08, FR-05-09, FR-05-10, FR-05-11 |
| FR-05-03 | View Green Profile and UCO contribution history — UC:UC-14; CTX:ESG-report-flow; CVCA:environmental-value-flow | FR-05-12 |
| FR-05-04 | Access ESG / environmental contribution reports — UC:UC-15; SEQ:S-03; CTX:ESG-report-flow; CVCA:environmental-value-flow | FR-05-13, FR-05-14 |
| FR-06-01 | Monitor operations and compliance status — UC:UC-16; CTX:admin-monitoring-flow; CVCA:operations-flow | FR-06-01 |
| FR-06-02 | Review audit trail and generate compliance reports — UC:UC-17; SEQ:S-03; CTX:audit-report-flow; CVCA:compliance-flow | FR-06-02, FR-06-03 |
| FR-06-03 | Enforce role-based access control — ARCH:access-control; ADR:authorisation; C5:hidden-work | FR-06-04, FR-06-05 |
| FR-06-04 | Run automated regression and acceptance tests — ARCH:ci-pipeline; ADR:test-strategy; C5:hidden-work | FR-06-06, FR-06-07, FR-06-08, FR-06-09 |
| FR-06-05 | Deploy and monitor the service — ARCH:deployment; ADR:observability; C5:hidden-work | FR-06-10, FR-06-11 |
| FR-06-06 | Back up and recover operational records — ARCH:data-recovery; ADR:backup; C5:hidden-work | FR-06-12, FR-06-13, FR-06-14 |

## L3 work items

Each row below implements one L2 function in one layer (FE, BE or INT). Input capture and correction belong to the FE task of the validation or update function; display-only actions are separate FE tasks. No L3 row groups different L2 functions.

**Legacy estimate package** points to one of the original 43 L3 estimates retained in the next section. Several refined tasks can share a package: its hours apply once to the package, not once per task. The original estimates do not establish a separate S/M/L estimate for each new task; task-level allocation must be agreed before a new costing baseline is signed off. Splitting the requirements therefore does not automatically multiply the budget.

| L3 ID | L2 ID | Layer | L3 work item | Legacy estimate package | Hidden work? |
| --- | --- | --- | --- | --- | --- |
| FR-01-01-FE | FR-01-01 | FE | Capture and submit supplier credentials or registration/compliance data | FR-01-01-FE | No |
| FR-01-01-BE | FR-01-01 | BE | API/service: Validate Credentials and Compliance Data | FR-01-01-BE | No |
| FR-01-02-BE | FR-01-02 | BE | API/service: Create Supplier Account with Unique Supplier_ID | FR-01-01-BE | No |
| FR-01-03-FE | FR-01-03 | FE | UI: Display Account Already Exists & Redirect to Login | FR-01-01-FE | No |
| FR-01-04-FE | FR-01-04 | FE | UI: Display Dashboard & Authentication Result | FR-01-01-FE | No |
| FR-01-05-FE | FR-01-05 | FE | UI: Display Error Message & Prompt Retry | FR-01-01-FE | No |
| FR-01-06-BE | FR-01-06 | BE | API/service: Authenticate returning supplier | FR-01-01-BE | No |
| FR-01-07-FE | FR-01-07 | FE | Capture Point-of-Origin and compliance fields for validation | FR-01-02-FE | No |
| FR-01-07-BE | FR-01-07 | BE | API/service: Validate Point-of-Origin and compliance information | FR-01-02-BE | No |
| FR-01-08-BE | FR-01-08 | BE | API/service: Store Point-of-Origin and compliance profile | FR-01-02-BE | No |
| FR-01-09-FE | FR-01-09 | FE | Submit pickup request to the supplier-profile completeness check | FR-01-03-FE | No |
| FR-01-09-BE | FR-01-09 | BE | API/service: Check that supplier profile is complete | FR-01-03-BE | No |
| FR-01-10-FE | FR-01-10 | FE | UI: Reject request and prompt profile update | FR-01-03-FE | No |
| FR-01-10-BE | FR-01-10 | BE | API/service: Reject request and prompt profile update | FR-01-03-BE | No |
| FR-01-11-FE | FR-01-11 | FE | Open the pickup form and capture/correct location, estimated volume, preferred time window and notes | FR-01-03-FE | No |
| FR-01-11-BE | FR-01-11 | BE | API/service: Validate pickup location, estimated volume and time window | FR-01-03-BE | No |
| FR-01-12-FE | FR-01-12 | FE | UI: Return validation feedback | FR-01-03-FE | No |
| FR-01-12-BE | FR-01-12 | BE | API/service: Return validation feedback | FR-01-03-BE | No |
| FR-01-13-BE | FR-01-13 | BE | API/service: Create pickup request with status Requested and record the time window | FR-01-03-BE | No |
| FR-01-14-FE | FR-01-14 | FE | UI: Return pickup request ID | FR-01-03-FE | No |
| FR-01-14-BE | FR-01-14 | BE | API/service: Return pickup request ID | FR-01-03-BE | No |
| FR-01-15-FE | FR-01-15 | FE | Select an existing pickup request and request its status and applicable rules | FR-01-04-FE | No |
| FR-01-15-BE | FR-01-15 | BE | API/service: Fetch pickup request status & validation rules | FR-01-04-BE | No |
| FR-01-16-FE | FR-01-16 | FE | UI: Display modification error: driver already dispatched | FR-01-04-FE | No |
| FR-01-17-FE | FR-01-17 | FE | Capture an eligible pickup change or cancellation and show the update confirmation | FR-01-04-FE | No |
| FR-01-17-BE | FR-01-17 | BE | API/service: Update pickup request status & reschedule timeline | FR-01-04-BE | No |
| FR-01-18-BE | FR-01-18 | BE | API/service: Update central pickup schedule & logs | FR-01-04-BE | No |
| FR-01-19-BE | FR-01-19 | BE | Send the dispatch-schedule update alert to UCO Collectors | FR-01-04-BE | No |
| FR-01-20-FE | FR-01-20 | FE | Show pickup tracking, driver ETA and available status/appointment details | FR-01-05-FE | No |
| FR-01-20-BE | FR-01-20 | BE | API/service: Fetch real-time tracking & driver ETA | FR-01-05-BE | No |
| FR-02-01-FE | FR-02-01 | FE | Select the planning area and submit the collection-route planning request | FR-02-01-FE | No |
| FR-02-01-BE | FR-02-01 | BE | API/service: Group pending pickups within vehicle capacity | FR-02-01-BE | No |
| FR-02-02-INT | FR-02-02 | INT | Integration: Obtain optimized visiting order from map/route data | FR-02-01-INT | No |
| FR-02-03-FE | FR-02-03 | FE | UI: Display proposed collection route | FR-02-01-FE | No |
| FR-02-04-FE | FR-02-04 | FE | Capture staff route confirmation and driver/vehicle selection | FR-02-01-FE | No |
| FR-02-04-BE | FR-02-04 | BE | API/service: Assign driver and vehicle to route | FR-02-01-BE | No |
| FR-02-05-BE | FR-02-05 | BE | API/service: Save and dispatch collection route | FR-02-01-BE | No |
| FR-02-06-BE | FR-02-06 | BE | API/service: Update included pickups to Driver Assigned | FR-02-01-BE | No |
| FR-02-07-BE | FR-02-07 | BE | API/service: Notify assigned driver | FR-02-01-BE | No |
| FR-02-08-FE | FR-02-08 | FE | UI: Display assigned route and pickup points | FR-02-02-FE | No |
| FR-02-08-BE | FR-02-08 | BE | Retrieve the driver assigned route and pickup points | FR-02-02-BE | No |
| FR-02-09-FE | FR-02-09 | FE | Select an assigned supplier point and submit arrival confirmation | FR-02-02-FE | No |
| FR-02-09-BE | FR-02-09 | BE | API/service: Update selected pickup status to Arrived | FR-02-02-BE | No |
| FR-02-10-FE | FR-02-10 | FE | Capture/correct actual quantity and optional collection evidence for validation | FR-02-02-FE | No |
| FR-02-10-BE | FR-02-10 | BE | API/service: Validate actual quantity, GPS, timestamp and supplied evidence | FR-02-02-BE | No |
| FR-02-11-FE | FR-02-11 | FE | UI: Return validation feedback | FR-02-02-FE | No |
| FR-02-11-BE | FR-02-11 | BE | API/service: Return validation feedback | FR-02-02-BE | No |
| FR-02-12-BE | FR-02-12 | BE | API/service: Save actual quantity, GPS, timestamp and supplied evidence against pickup | FR-02-02-BE | No |
| FR-02-13-FE | FR-02-13 | FE | Report UCO not ready at an Arrived pickup and show its cancellation | FR-02-02-FE | No |
| FR-02-13-BE | FR-02-13 | BE | API/service: Cancel current pickup and update status to Cancelled | FR-02-02-BE | No |
| FR-02-14-FE | FR-02-14 | FE | Capture/submit actual quantity, handover time and evidence; allow correction | FR-02-03-FE | No |
| FR-02-14-BE | FR-02-14 | BE | API/service: Validate actual quantity, handover time, and evidence | FR-02-03-BE | No |
| FR-02-15-BE | FR-02-15 | BE | API/service: Store handover record against assigned pickup | FR-02-03-BE | No |
| FR-02-16-BE | FR-02-16 | BE | API/service: Calculate transaction amount | FR-02-03-BE | No |
| FR-02-17-FE | FR-02-17 | FE | Display the generated draft handover agreement | FR-02-03-FE | No |
| FR-02-17-BE | FR-02-17 | BE | API/service: Generate draft agreement | FR-02-03-BE | No |
| FR-02-18-FE | FR-02-18 | FE | UI: Reject and prompt for correction | FR-02-03-FE | No |
| FR-02-18-BE | FR-02-18 | BE | API/service: Reject and prompt for correction | FR-02-03-BE | No |
| FR-03-01-FE | FR-03-01 | FE | Request the signed-in supplier own unsigned draft agreement | FR-03-01-FE | No |
| FR-03-01-BE | FR-03-01 | BE | Retrieve the owned unsigned draft, handover and calculated amount | FR-03-01-BE | No |
| FR-03-02-FE | FR-03-02 | FE | Display the reviewed handover and read-only pricing/amount | FR-03-01-FE | No |
| FR-03-03-FE | FR-03-03 | FE | Show the existing empty state and transaction-history navigation | FR-03-01-FE | No |
| FR-03-04-FE | FR-03-04 | FE | Capture/clear the signature, capture the confirmation checkbox and open the confirmation modal | FR-03-01-FE | No |
| FR-03-04-BE | FR-03-04 | BE | Validate the submitted signature and explicit agreement confirmation | FR-03-01-BE | No |
| FR-03-05-FE | FR-03-05 | FE | Display signing validation feedback and allow input correction | FR-03-01-FE | No |
| FR-03-05-BE | FR-03-05 | BE | Return signature/confirmation validation feedback without signing writes | FR-03-01-BE | No |
| FR-03-06-BE | FR-03-06 | BE | Lock the reviewed agreement document hash and calculated amount | FR-03-01-BE | No |
| FR-03-07-BE | FR-03-07 | BE | Store the electronic signature and signing time against the transaction | FR-03-01-BE | No |
| FR-03-08-BE | FR-03-08 | BE | Mark the linked pickup Completed after successful signing | FR-03-01-BE | No |
| FR-03-09-FE | FR-03-09 | FE | Show signing-failure feedback and allow a fresh signature submission | FR-03-01-FE | No |
| FR-03-09-BE | FR-03-09 | BE | Return signing failure with the unsigned agreement and previous pickup state retained | FR-03-01-BE | No |
| FR-03-10-FE | FR-03-10 | FE | Show the locked signature and signing-success summary with receipt/home navigation | FR-03-01-FE | No |
| FR-03-11-FE | FR-03-11 | FE | Request an authorized signed-transaction receipt | FR-03-02-FE | No |
| FR-03-11-BE | FR-03-11 | BE | API/service: Check receipt ownership and signed status | FR-03-02-BE | No |
| FR-03-12-BE | FR-03-12 | BE | API/service: Retrieve receipt data | FR-03-02-BE | No |
| FR-03-13-FE | FR-03-13 | FE | UI: Render and provide electronic receipt PDF | FR-03-02-FE | No |
| FR-03-13-BE | FR-03-13 | BE | API/service: Render and provide electronic receipt PDF | FR-03-02-BE | No |
| FR-03-14-FE | FR-03-14 | FE | UI: Show warehouse verification status | FR-03-02-FE | No |
| FR-04-01-FE | FR-04-01 | FE | Identify the collected UCO and submit the warehouse received quantity | FR-04-01-FE | No |
| FR-04-01-BE | FR-04-01 | BE | API/service: Record received quantity | FR-04-01-BE | No |
| FR-04-02-FE | FR-04-02 | FE | Present the received-versus-collected quantity comparison | FR-04-01-FE | No |
| FR-04-02-BE | FR-04-02 | BE | API/service: Compare received quantity with collection record | FR-04-01-BE | No |
| FR-04-03-FE | FR-04-03 | FE | Show the logged discrepancy alert for internal reconciliation | FR-04-01-FE | No |
| FR-04-03-BE | FR-04-03 | BE | API/service: Log discrepancy alert for internal reconciliation | FR-04-01-BE | No |
| FR-04-04-BE | FR-04-04 | BE | API/service: Keep signed transaction amount unchanged | FR-04-01-BE | No |
| FR-04-05-FE | FR-04-05 | FE | Show the collection receiving-status result | FR-04-01-FE | No |
| FR-04-05-BE | FR-04-05 | BE | API/service: Update collection status to Received | FR-04-01-BE | No |
| FR-04-06-FE | FR-04-06 | FE | Select verified collections and enter/correct batch parameters | FR-04-02-FE | No |
| FR-04-06-BE | FR-04-06 | BE | API/service: Validate collection inputs & batch parameters | FR-04-02-BE | No |
| FR-04-07-FE | FR-04-07 | FE | UI: Prompt invalid parameters & request correction | FR-04-02-FE | No |
| FR-04-08-BE | FR-04-08 | BE | API/service: Link collections & compute mass-balance metrics | FR-04-02-BE | No |
| FR-04-09-FE | FR-04-09 | FE | Show the batch creation and updated mass-balance summary | FR-04-02-FE | No |
| FR-04-09-BE | FR-04-09 | BE | API/service: Maintain linked collection inputs & update batch balance | FR-04-02-BE | No |
| FR-04-10-BE | FR-04-10 | BE | Provide the batch traceability and mass-balance record to Eco Oil System | FR-04-02-BE | No |
| FR-04-11-FE | FR-04-11 | FE | UI: Trace UCO through the supply chain | FR-04-03-FE | No |
| FR-04-11-BE | FR-04-11 | BE | API/service: Trace UCO through the supply chain | FR-04-03-BE | No |
| FR-05-01-FE | FR-05-01 | FE | Open the Loyalty & Payout overview and show retrieved tier/balance | FR-05-01-FE | No |
| FR-05-01-BE | FR-05-01 | BE | API/service: Retrieve Loyalty Tier & Payout Balance | FR-05-01-BE | No |
| FR-05-02-FE | FR-05-02 | FE | UI: Display Loyalty Tier & History Details | FR-05-01-FE | No |
| FR-05-02-BE | FR-05-02 | BE | Retrieve itemized loyalty-tier and transaction-history details | FR-05-01-BE | No |
| FR-05-03-BE | FR-05-03 | BE | API/service: Add signed transaction payment to payout balance | FR-05-01-BE | No |
| FR-05-04-BE | FR-05-04 | BE | API/service: Add signed transaction quantity to verified UCO volume | FR-05-01-BE | No |
| FR-05-05-BE | FR-05-05 | BE | API/service: Upgrade loyalty tier | FR-05-01-BE | No |
| FR-05-06-BE | FR-05-06 | BE | API/service: Award tier upgrade bonus | FR-05-01-BE | No |
| FR-05-07-FE | FR-05-07 | FE | Capture withdrawal amount and bank details and submit for eligibility validation | FR-05-02-FE | No |
| FR-05-07-BE | FR-05-07 | BE | API/service: Validate Withdrawal Request | FR-05-02-BE | No |
| FR-05-08-FE | FR-05-08 | FE | UI: Display Existing Balance & Withdrawal Rejection Reason | FR-05-02-FE | No |
| FR-05-08-BE | FR-05-08 | BE | Return the withdrawal rejection reason and existing balance | FR-05-02-BE | No |
| FR-05-09-BE | FR-05-09 | BE | API/service: Create Withdrawal Transaction | FR-05-02-BE | No |
| FR-05-10-BE | FR-05-10 | BE | API/service: Update Payout Status | FR-05-02-BE | No |
| FR-05-11-FE | FR-05-11 | FE | UI: Display Withdrawal Result & Updated Payout Status and Balance | FR-05-02-FE | No |
| FR-05-12-FE | FR-05-12 | FE | UI: View Green Profile and UCO contribution history | FR-05-03-FE | No |
| FR-05-12-BE | FR-05-12 | BE | API/service: View Green Profile and UCO contribution history | FR-05-03-BE | No |
| FR-05-13-BE | FR-05-13 | BE | API/service: Generate ESG / environmental contribution report | FR-05-04-BE | No |
| FR-05-14-FE | FR-05-14 | FE | UI: Provide ESG / environmental contribution report | FR-05-04-FE | No |
| FR-06-01-FE | FR-06-01 | FE | UI: Monitor operations and compliance status | FR-06-01-FE | No |
| FR-06-01-BE | FR-06-01 | BE | API/service: Monitor operations and compliance status | FR-06-01-BE | No |
| FR-06-02-FE | FR-06-02 | FE | UI: Retrieve and display audit history | FR-06-02-FE | No |
| FR-06-02-BE | FR-06-02 | BE | API/service: Retrieve and display audit history | FR-06-02-BE | No |
| FR-06-03-FE | FR-06-03 | FE | Request and display the generated compliance report | FR-06-02-FE | No |
| FR-06-03-BE | FR-06-03 | BE | API/service: Generate compliance report | FR-06-02-BE | No |
| FR-06-04-BE | FR-06-04 | BE | API/service: Check role permission for protected action | FR-06-03-BE | Yes |
| FR-06-05-BE | FR-06-05 | BE | API/service: Record denied access attempt | FR-06-03-BE | Yes |
| FR-06-06-BE | FR-06-06 | BE | Test automation: Run automated unit tests | FR-06-04-BE | Yes |
| FR-06-07-BE | FR-06-07 | BE | Test automation: Run automated API tests | FR-06-04-BE | Yes |
| FR-06-08-BE | FR-06-08 | BE | Test automation: Run automated acceptance tests | FR-06-04-BE | Yes |
| FR-06-09-BE | FR-06-09 | BE | Test automation: Block deployment on test failure | FR-06-04-BE | Yes |
| FR-06-10-INT | FR-06-10 | INT | Integration: Deploy approved release | FR-06-05-INT | Yes |
| FR-06-11-INT | FR-06-11 | INT | Integration: Expose service monitoring signals | FR-06-05-INT | Yes |
| FR-06-12-BE | FR-06-12 | BE | Backup/recovery: Create scheduled operational and audit backup | FR-06-06-BE | Yes |
| FR-06-13-BE | FR-06-13 | BE | Backup/recovery: Record backup result | FR-06-06-BE | Yes |
| FR-06-14-BE | FR-06-14 | BE | Backup/recovery: Verify backup recovery | FR-06-06-BE | Yes |

## Retained estimate packages

S = 4 h, M = 8 h, L = 16 h. These are the original package estimates, not a new estimate of each refined task. Historical package IDs remain available to W2/W4/W5 and original diagram citations such as `FR-02-03-BE`. They are legacy references, distinct from the renumbered L3 task IDs. Their descriptions now name the package scope rather than pretending each package is one atomic function.

| Legacy estimate-package ID | Legacy family | Layer | Package scope | Size | Base hours (once per package) | EARS precise? (legacy) | Hidden work? |
| --- | --- | --- | --- | --- | --- | --- | --- |
| FR-01-01-FE | FR-01-01 | FE | Register/Login as a UCO supplier | M | 8 | Yes | No |
| FR-01-01-BE | FR-01-01 | BE | Register/Login as a UCO supplier | M | 8 | Yes | No |
| FR-01-02-FE | FR-01-02 | FE | Manage Point-of-Origin and compliance profile | M | 8 | Yes | No |
| FR-01-02-BE | FR-01-02 | BE | Manage Point-of-Origin and compliance profile | M | 8 | Yes | No |
| FR-01-03-FE | FR-01-03 | FE | Request and schedule a UCO pickup | M | 8 | Yes | No |
| FR-01-03-BE | FR-01-03 | BE | Request and schedule a UCO pickup | M | 8 | Yes | No |
| FR-01-04-FE | FR-01-04 | FE | Manage an existing pickup request | M | 8 | Yes | No |
| FR-01-04-BE | FR-01-04 | BE | Manage an existing pickup request | M | 8 | Yes | No |
| FR-01-05-FE | FR-01-05 | FE | Track pickup status | S | 4 | Yes | No |
| FR-01-05-BE | FR-01-05 | BE | Track pickup status | S | 4 | Yes | No |
| FR-02-01-FE | FR-02-01 | FE | Plan and dispatch a UCO collection route | M | 8 | Yes | No |
| FR-02-01-BE | FR-02-01 | BE | Plan and dispatch a UCO collection route | L | 16 | Yes | No |
| FR-02-01-INT | FR-02-01 | INT | Plan and dispatch a UCO collection route | M | 8 | Yes | No |
| FR-02-02-FE | FR-02-02 | FE | View assigned route and complete a UCO collection | M | 8 | Yes | No |
| FR-02-02-BE | FR-02-02 | BE | View assigned route and complete a UCO collection | M | 8 | Yes | No |
| FR-02-03-FE | FR-02-03 | FE | Record UCO handover information | M | 8 | Yes | No |
| FR-02-03-BE | FR-02-03 | BE | Record UCO handover information | M | 8 | Yes | No |
| FR-03-01-FE | FR-03-01 | FE | Confirm and sign a UCO transaction | M | 8 | Yes | No |
| FR-03-01-BE | FR-03-01 | BE | Confirm and sign a UCO transaction | M | 8 | Yes | No |
| FR-03-02-FE | FR-03-02 | FE | View or download transaction receipt | S | 4 | Yes | No |
| FR-03-02-BE | FR-03-02 | BE | View or download transaction receipt | S | 4 | Yes | No |
| FR-04-01-FE | FR-04-01 | FE | Receive and verify collected UCO at warehouse | M | 8 | Yes | No |
| FR-04-01-BE | FR-04-01 | BE | Receive and verify collected UCO at warehouse | M | 8 | Yes | No |
| FR-04-02-FE | FR-04-02 | FE | Manage UCO batch and mass-balance records | M | 8 | Yes | No |
| FR-04-02-BE | FR-04-02 | BE | Manage UCO batch and mass-balance records | L | 16 | Yes | No |
| FR-04-03-FE | FR-04-03 | FE | Trace UCO through the supply chain | M | 8 | Yes | No |
| FR-04-03-BE | FR-04-03 | BE | Trace UCO through the supply chain | L | 16 | Yes | No |
| FR-05-01-FE | FR-05-01 | FE | View loyalty status and accumulated payout balance | M | 8 | Yes | No |
| FR-05-01-BE | FR-05-01 | BE | View loyalty status and accumulated payout balance | M | 8 | Yes | No |
| FR-05-02-FE | FR-05-02 | FE | Withdraw accumulated payout balance | M | 8 | Yes | No |
| FR-05-02-BE | FR-05-02 | BE | Withdraw accumulated payout balance | M | 8 | Yes | No |
| FR-05-03-FE | FR-05-03 | FE | View Green Profile and UCO contribution history | S | 4 | Yes | No |
| FR-05-03-BE | FR-05-03 | BE | View Green Profile and UCO contribution history | S | 4 | Yes | No |
| FR-05-04-FE | FR-05-04 | FE | Access ESG / environmental contribution reports | M | 8 | Yes | No |
| FR-05-04-BE | FR-05-04 | BE | Access ESG / environmental contribution reports | M | 8 | Yes | No |
| FR-06-01-FE | FR-06-01 | FE | Monitor operations and compliance status | M | 8 | Yes | No |
| FR-06-01-BE | FR-06-01 | BE | Monitor operations and compliance status | M | 8 | Yes | No |
| FR-06-02-FE | FR-06-02 | FE | Review audit trail and generate compliance reports | M | 8 | Yes | No |
| FR-06-02-BE | FR-06-02 | BE | Review audit trail and generate compliance reports | L | 16 | Yes | No |
| FR-06-03-BE | FR-06-03 | BE | Enforce role-based access control | M | 8 | Yes | Yes |
| FR-06-04-BE | FR-06-04 | BE | Run automated regression and acceptance tests | M | 8 | Yes | Yes |
| FR-06-05-INT | FR-06-05 | INT | Deploy and monitor the service | M | 8 | Yes | Yes |
| FR-06-06-BE | FR-06-06 | BE | Back up and recover operational records | S | 4 | Yes | Yes |

## Activity-node coverage

Counts include system action nodes plus the two boundary-receipt nodes `c1` (UCO Collectors) and `e1` (Eco Oil System). Receipt in an external lane is represented by the corresponding ReFeed notification/export responsibility; W1 does not specify the partner system implementation.

| Activity diagram | Action nodes covered | Node → L2 function |
| --- | --- | --- |
| [AD-UC-01](../activity/AD-UC-01.md) | 5 | `validateAuth` → `FR-01-01`; `createAccount` → `FR-01-02`; `authenticateUser` → `FR-01-03`; `displayAuthSuccess` → `FR-01-04`; `displayAuthError` → `FR-01-05` |
| [AD-UC-03](../activity/AD-UC-03.md) | 6 | `s1` → `FR-01-09`; `s2` → `FR-01-10`; `s3` → `FR-01-11`; `s4` → `FR-01-12`; `s5` → `FR-01-13`; `s6` → `FR-01-14` |
| [AD-UC-04](../activity/AD-UC-04.md) | 6 | `s1` → `FR-01-15`; `s3` → `FR-01-16`; `s4` → `FR-01-17`; `s5` → `FR-01-18`; `c1` → `FR-01-19`; `s2` → `FR-01-20` |
| [AD-UC-05](../activity/AD-UC-05.md) | 7 | `s1` → `FR-02-01`; `s2` → `FR-02-02`; `s3` → `FR-02-03`; `s4` → `FR-02-04`; `s5` → `FR-02-05`; `s6` → `FR-02-06`; `s7` → `FR-02-07` |
| [AD-UC-06](../activity/AD-UC-06.md) | 6 | `s1` → `FR-02-08`; `s2` → `FR-02-09`; `s3` → `FR-02-10`; `s4` → `FR-02-11`; `s5` → `FR-02-12`; `s6` → `FR-02-13` |
| [AD-UC-07](../activity/AD-UC-07.md) | 5 | `a2` → `FR-02-14`; `a3` → `FR-02-15`; `a4` → `FR-02-16`; `a5` → `FR-02-17`; `a7` → `FR-02-18` |
| [AD-UC-08](../activity/AD-UC-08.md) | 10 | `s1` → `FR-03-01`; `s2` → `FR-03-02`; `s3` → `FR-03-03`; `s4` → `FR-03-04`; `s5` → `FR-03-05`; `s6` → `FR-03-06`; `s7` → `FR-03-07`; `s8` → `FR-03-08`; `s9` → `FR-03-09`; `s10` → `FR-03-10` |
| [AD-UC-10](../activity/AD-UC-10.md) | 5 | `s1` → `FR-04-01`; `s2` → `FR-04-02`; `s3` → `FR-04-03`; `s4` → `FR-04-04`; `s5` → `FR-04-05` |
| [AD-UC-11](../activity/AD-UC-11.md) | 5 | `s1` → `FR-04-06`; `s2` → `FR-04-07`; `s3` → `FR-04-08`; `s4` → `FR-04-09`; `e1` → `FR-04-10` |
| [AD-UC-12](../activity/AD-UC-12.md) | 7 | `retrieve` → `FR-05-01`; `displayTier` → `FR-05-02`; `validate` → `FR-05-07`; `display` → `FR-05-08`; `transaction` → `FR-05-09`; `update` → `FR-05-10`; `displayUpdated` → `FR-05-11` |
| [AD-UC-13](../activity/AD-UC-13.md) | 4 | `s1` → `FR-05-07`; `s3` → `FR-05-08`; `s2` → `FR-05-09`; `s4` → `FR-05-10` |

## Reconciliation notes and preserved boundaries

| Source issue | Treatment in this W1 |
|---|---|
| AD-UC-01 and AD-UC-12 prose/node-trace tables use `s1`, `s2`, etc. while Mermaid uses names such as `validateAuth` and `retrieve`. | Use the actual Mermaid IDs in every L2 trace and in the coverage table. The AD files are preserved. |
| UC-01 existing-supplier branch ends after an account-exists message and login redirect, although W1 and its metadata/acceptance text also require authentication/dashboard. | Preserve the five drawn actions. Retain the original returning-login requirement separately as `FR-01-06`, explicitly labelled as lacking an authentication action in the diagram. Reconcile the AD success path before implementation; do not silently drop login or treat redirection as authentication. |
| AD-UC-04 `s2` is tagged `FR-01-04`, while the original W1/W4 use `FR-01-05` for tracking. | Map this single tracking node to `FR-01-20`, preserve the tracking requirement, and inherit the tracking family's effective W4 priority. Do not duplicate it under modification. Real-time tracking/ETA comes from the AD and remains subject to the team's integration decisions. |
| The original UC-07 L2 duplicated UC-08's `FR-03-01`; AD-UC-07 `a5` cites the original L3 package `FR-02-03-BE`. | Keep `FR-02-03` as the corrected Legacy family for handover; its current functions are `FR-02-14` to `FR-02-18`, and `a5` maps to `FR-02-17`. Keep `FR-02-03-BE` as the legacy estimate-package reference. Supplier signing now uses current functions `FR-03-01` to `FR-03-10`, traced to AD-UC-08 and SD-UC-08. |
| AD-UC-03 request-field rules and AD-UC-06 collection-field rules are draft defaults. | Preserve their separate validation and feedback functions, without inventing minimum quantities, evidence file limits or offline behaviour. Existing open questions remain with their source diagrams. |
| UC-06 cancellation node `s6` has no FR in its label. | Give it explicit function `FR-02-13`, preserving the baseline requirement: assigned pickup, Arrived, UCO not ready, Cancelled, no collection record. The remaining-pickup loop is control flow and adds no route-completed state. |
| UC-06 and UC-07 both validate quantity/evidence. | Keep collection capture separate from handover validation, amount calculation and draft-agreement generation as the two existing ADs do. Do not merge the use cases. |
| AD-UC-10 ends at Received; AD-UC-11 starts from Verified_At_Warehouse. | Keep both source state names. No transition between them or extra verification function is invented. Reconcile the warehouse lifecycle before implementation. No numeric tolerance is fixed by this edit. |
| AD-UC-11 draws Eco Oil receipt `e1`, while the original W1 estimates contain no separate Eco Oil INT package. | Expose the existing diagram boundary action as `FR-04-10`, trace its work to the existing batch BE package, and retain the source sync/retry open question. The deferred end-to-end trace query is current function `FR-04-11` (original family `FR-04-03`). |
| UC-12 repeats withdrawal validation, creation and status update from UC-13. | Reuse `FR-05-07`, `FR-05-09` and `FR-05-10`; no duplicate work items or budget. Preserve UC-12's fork/join for creation/status update and UC-13's sequential path as source flows; execution order still needs reconciliation. |
| AD-UC-13 `s3` says "Define the handling of an ineligible withdrawal request"; its tests already require rejection and an error message. Its node-trace text for `s2` also mentions updating status although Mermaid gives that to `s4`. | Map `s3` to the UC-12 rejection-display function using those existing tests. Map `s2` only to transaction creation and `s4` only to payout-status update. On rejection, create no transaction and deduct no balance. |
| AD-UC-12's open question suggests automatic banking API processing, but W0/ADR-003 and AD-UC-13 place actual money movement outside Release 1. | Include eligibility, withdrawal record/status and result visibility only. Add no banking gateway or transfer-execution function. Keep W4's withdrawal priority and release-scope ambiguity visible. |
| Signed-transaction balance, volume, tier and tier-bonus updates already exist as `FR-05-03` through `FR-05-06`. | Retain them under UC-12 from the original W1. The supplied activity profile requires one use case per AD, so they are not actions in AD-UC-08 or SD-UC-08. The current AD-UC-12 does not draw these updates; no new UC-12 flow is invented here. |
| AD-UC-08 and SD-UC-08 now cover supplier review and signing only. UC-02 still has no AD; UC-09, UC-14, UC-15, UC-16 and UC-17 have no activity diagrams either. | Retain and, where necessary, split only the behaviours already written in W1. Platform controls likewise retain their architecture/baseline source, not a fabricated AD trace. |
| UC-08 was missing an AD/SD, while SD-UC-07 included the supplier-signing tail and S-F03-01 already specified empty, validation, modal, failure and success behaviour. | Add AD-UC-08 and SD-UC-08; keep UC-07 ending at the generated draft. Split signing into current `FR-03-01`–`FR-03-10`; renumber the existing receipt functions to `FR-03-11`–`FR-03-14` in the same feature sequence. Preserve original signing/receipt package IDs as legacy costing references. |
| W2/W4/W5 still use the original 23-family / 43-package granularity. | Retain the original package IDs explicitly as legacy references and the 348-hour historical estimate; new L2/L3 counts describe decomposition only. Existing cost/FP totals are not recalculated or re-approved by this edit. Size each refined task before producing a new bottom-up estimate. |

## Verification summary

- 23 legacy requirement families retained across 6 features; 91 current L2 IDs follow `FR-xx-yy` consecutively within each feature, and all L3 IDs append the matching layer. No duplicate L2 or L3 IDs.
- 91 L2 functions: 62 mapped to activity actions and 29 retained/decomposed from W1 where no corresponding action exists.
- 66 action-node references across 11 activity diagrams; every system/boundary action maps once. Four equivalent withdrawal actions are shared across UC-12 and UC-13.
- 132 refined L3 tasks; each references one existing L2 function, one layer and one valid legacy estimate package.
- 43 original estimate packages retained, totaling 348 base hours counted once; maximum package size 16 hours. Task-level re-estimation is not claimed.
- Requirement-family traces, original initial priorities, hidden work and deferred scope are retained. NC technical families remain zero-FP work; node decomposition is not a new FP count.

Source snapshot: repository commit `2fe0442cdf1a0a235bdb7bdfa786c2e7d77151d2`. AD-UC-08 and SD-UC-08 now supply the missing signing flow; the other existing ADs remain the team's source evidence. Related indexes and UC-08/UC-09 references are aligned with this addition.
