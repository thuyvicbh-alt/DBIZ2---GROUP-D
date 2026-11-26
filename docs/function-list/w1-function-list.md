# W1 Function List: ReFeed

> ReFeed Group D, K63DB, DBIZ II.

Lesson 5 (block W1). L2 = one system action from the team's activity diagrams, or a behaviour already present in the baseline where no activity node exists. L3 = one work item in one layer (FE, BE, INT), sized S = 4 h, M = 8 h, L = 16 h. Initial priority is set here; the effective priority comes from [W4](w4-priorities.md).

Sources swept: [W0](w0-scope.md), Use Case, Activity, Sequence, Context, CVCA, Architecture and ADR. The 2026-11-25 decomposition is preserved: actor actions contribute FE work; decisions, start/end, object, merge, fork and join nodes do not add functions. Equivalent withdrawal actions in UC-12 and UC-13 reuse the same IDs. Source trace identifies actual Mermaid node IDs; existing diagram labels still need the reference reconciliation recorded in [W3](w3-cross-review.md).

## L2 functions

| Feature | L2 ID | L2 function name | Actor | EARS acceptance criterion | Source trace | Initial priority | FP type |
|---|---|---|---|---|---|---|---|
| F-01 | FR-01-01 | Validate Credentials and Compliance Data | Supplier | WHEN a supplier submits credentials or registration data, THEN the system SHALL validate the submitted credentials and applicable compliance data and return the authentication-validity result. | UC:UC-01; [AD-UC-01](../activity/AD-UC-01.md) `validateAuth` | Must | NC |
| F-01 | FR-01-02 | Create Supplier Account with Unique Supplier_ID | Supplier | WHEN submitted registration data are valid and the supplier is new, THEN the system SHALL create an Active supplier account with a unique Supplier_ID. | UC:UC-01; [AD-UC-01](../activity/AD-UC-01.md) `createAccount` | Must | EI |
| F-01 | FR-01-03 | Display Account Already Exists & Redirect to Login | Supplier | WHEN the existing-supplier branch is selected, THEN the system SHALL display that the account already exists and redirect the supplier to login. | UC:UC-01; [AD-UC-01](../activity/AD-UC-01.md) `authenticateUser` | Must | NC |
| F-01 | FR-01-04 | Display Dashboard & Authentication Result | Supplier | WHEN a new supplier account has been created successfully, THEN the system SHALL display the dashboard and authentication result. | UC:UC-01; [AD-UC-01](../activity/AD-UC-01.md) `displayAuthSuccess` | Must | NC |
| F-01 | FR-01-05 | Display Error Message & Prompt Retry | Supplier | WHEN credentials or registration/compliance data are invalid, THEN the system SHALL display an error and prompt retry without creating an account or session. | UC:UC-01; [AD-UC-01](../activity/AD-UC-01.md) `displayAuthError` | Must | NC |
| F-01 | FR-01-06 | Authenticate returning supplier | Supplier | WHEN a returning supplier submits valid login credentials, THEN the system SHALL authenticate the supplier. | UC:UC-01; [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Must | EQ |
| F-01 | FR-01-07 | Validate Point-of-Origin and compliance information | Supplier | WHEN a supplier submits or updates Point-of-Origin or compliance information, THEN the system SHALL validate the submitted profile fields. | UC:UC-02; [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Must | NC |
| F-01 | FR-01-08 | Store Point-of-Origin and compliance profile | Supplier | WHEN submitted Point-of-Origin and compliance information is valid, THEN the system SHALL store the profile against the Supplier_ID. | UC:UC-02; [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Must | EI |
| F-01 | FR-01-09 | Check that supplier profile is complete | Supplier | WHEN a signed-in supplier submits a pickup request, THEN the system SHALL check that the supplier profile contains GPS and compliance data. | UC:UC-03; [AD-UC-03](../activity/AD-UC-03.md) `s1` | Must | NC |
| F-01 | FR-01-10 | Reject request and prompt profile update | Supplier | WHEN the supplier profile lacks GPS or compliance data, THEN the system SHALL reject the pickup request without creating a pickup and prompt the supplier to update the profile. | UC:UC-03; [AD-UC-03](../activity/AD-UC-03.md) `s2` | Must | NC |
| F-01 | FR-01-11 | Validate pickup location, estimated volume and time window | Supplier | WHEN the supplier profile is complete, THEN the system SHALL validate the submitted pickup location, estimated volume and preferred time window using the draft rules in AD-UC-03. | UC:UC-03; [AD-UC-03](../activity/AD-UC-03.md) `s3` | Must | NC |
| F-01 | FR-01-12 | Return validation feedback | Supplier | WHEN pickup-request details are missing or invalid, THEN the system SHALL return field-validation feedback for correction without creating a pickup. | UC:UC-03; [AD-UC-03](../activity/AD-UC-03.md) `s4` | Must | NC |
| F-01 | FR-01-13 | Create pickup request with status Requested and record the time window | Supplier | WHEN the supplier profile is complete and pickup details are valid, THEN the system SHALL create a pickup linked to Supplier_ID with status Requested and the submitted time window. | UC:UC-03; [AD-UC-03](../activity/AD-UC-03.md) `s5` | Must | EI |
| F-01 | FR-01-14 | Return pickup request ID | Supplier | WHEN the pickup request has been created, THEN the system SHALL return its request ID to the supplier. | UC:UC-03; [AD-UC-03](../activity/AD-UC-03.md) `s6` | Must | NC |
| F-01 | FR-01-15 | Fetch pickup request status & validation rules | Supplier | WHEN a supplier selects an existing pickup request, THEN the system SHALL fetch its current status and applicable modification rules. | UC:UC-04; [AD-UC-04](../activity/AD-UC-04.md) `s1` | Must | EQ |
| F-01 | FR-01-16 | Display modification error: driver already dispatched | Supplier | WHEN a supplier attempts to modify or cancel a pickup whose driver is already dispatched, THEN the system SHALL display the modification-lock error and retain the existing request and schedule. | UC:UC-04; [AD-UC-04](../activity/AD-UC-04.md) `s3` | Must | NC |
| F-01 | FR-01-17 | Update pickup request status & reschedule timeline | Supplier | WHEN a supplier modifies or cancels an eligible pickup in Requested status, THEN the system SHALL update the pickup status and applicable appointment timeline as specified by AD-UC-04. | UC:UC-04; [AD-UC-04](../activity/AD-UC-04.md) `s4` | Must | EI |
| F-01 | FR-01-18 | Update central pickup schedule & logs | Supplier | WHEN an eligible pickup request has been changed, THEN the system SHALL update the central pickup schedule and retain the change in its logs. | UC:UC-04; [AD-UC-04](../activity/AD-UC-04.md) `s5` | Must | NC |
| F-01 | FR-01-19 | Receive dispatch schedule update alert | Supplier | WHEN an eligible pickup request has been changed, THEN the system SHALL provide a dispatch-schedule update alert to UCO Collectors in parallel with the central schedule update. | UC:UC-04; [AD-UC-04](../activity/AD-UC-04.md) `c1` | Must | EO |
| F-01 | FR-01-20 | Fetch real-time tracking & driver ETA | Supplier | WHEN a supplier selects View status, THEN the system SHALL retrieve tracking and driver ETA as drawn in AD-UC-04 and present the current pickup status, scheduled collection time, assigned driver/vehicle when available and last-update timestamp. | UC:UC-04; [AD-UC-04](../activity/AD-UC-04.md) `s2` | Must | EO |
| F-02 | FR-02-01 | Group pending pickups within vehicle capacity | ReFeed Staff | WHEN ReFeed Staff selects an area and requests a route plan, THEN the system SHALL group pending pickups without exceeding the selected vehicle capacity. | UC:UC-05; [AD-UC-05](../activity/AD-UC-05.md) `s1` | Must | NC |
| F-02 | FR-02-02 | Obtain optimized visiting order from map/route data | ReFeed Staff | WHEN capacity-constrained pickup groups are available, THEN the system SHALL obtain an optimized visiting order from map/route data. | UC:UC-05; [AD-UC-05](../activity/AD-UC-05.md) `s2` | Must | NC |
| F-02 | FR-02-03 | Display proposed collection route | ReFeed Staff | WHEN an optimized visiting order is available, THEN the system SHALL display the proposed collection route for staff review. | UC:UC-05; [AD-UC-05](../activity/AD-UC-05.md) `s3` | Must | EO |
| F-02 | FR-02-04 | Assign driver and vehicle to route | ReFeed Staff | WHEN ReFeed Staff confirms the proposed route, THEN the system SHALL assign the driver and vehicle to that route. | UC:UC-05; [AD-UC-05](../activity/AD-UC-05.md) `s4` | Must | NC |
| F-02 | FR-02-05 | Save and dispatch collection route | ReFeed Staff | WHEN the confirmed route has a driver and vehicle, THEN the system SHALL save and dispatch that collection route. | UC:UC-05; [AD-UC-05](../activity/AD-UC-05.md) `s5` | Must | EI |
| F-02 | FR-02-06 | Update included pickups to Driver Assigned | ReFeed Staff | WHEN the collection route has been dispatched, THEN the system SHALL update its included pickups to Driver Assigned. | UC:UC-05; [AD-UC-05](../activity/AD-UC-05.md) `s6` | Must | NC |
| F-02 | FR-02-07 | Notify assigned driver | ReFeed Staff | WHEN included pickups have been updated to Driver Assigned, THEN the system SHALL notify the assigned driver of the dispatched route. | UC:UC-05; [AD-UC-05](../activity/AD-UC-05.md) `s7` | Must | EO |
| F-02 | FR-02-08 | Display assigned route and pickup points | Driver / Collector | WHEN a driver opens their assigned collection route, THEN the system SHALL display that route and its assigned pickup points. | UC:UC-06; [AD-UC-06](../activity/AD-UC-06.md) `s1` | Must | EQ |
| F-02 | FR-02-09 | Update selected pickup status to Arrived | Driver / Collector | WHEN the assigned driver selects a supplier point and confirms arrival, THEN the system SHALL update that pickup to Arrived. | UC:UC-06; [AD-UC-06](../activity/AD-UC-06.md) `s2` | Must | EI |
| F-02 | FR-02-10 | Validate actual quantity, GPS, timestamp and supplied evidence | Driver / Collector | WHEN UCO is ready and the driver submits collection details, THEN the system SHALL validate actual quantity, GPS, timestamp and any supplied optional photo/notes using the AD-UC-06 draft rules. | UC:UC-06; [AD-UC-06](../activity/AD-UC-06.md) `s3` | Must | NC |
| F-02 | FR-02-11 | Return validation feedback | Driver / Collector | WHEN required collection data are missing or invalid, THEN the system SHALL return validation feedback for correction, save no collection details and retain Arrived status. | UC:UC-06; [AD-UC-06](../activity/AD-UC-06.md) `s4` | Must | NC |
| F-02 | FR-02-12 | Save actual quantity, GPS, timestamp and supplied evidence against pickup | Driver / Collector | WHEN collection details are valid, THEN the system SHALL save actual quantity, GPS, timestamp and supplied evidence against the assigned pickup. | UC:UC-06; [AD-UC-06](../activity/AD-UC-06.md) `s5` | Must | EI |
| F-02 | FR-02-13 | Cancel current pickup and update status to Cancelled | Driver / Collector | WHEN the assigned driver reports that UCO is not ready at an Arrived pickup, THEN the system SHALL set that pickup to Cancelled without creating a collection record. | UC:UC-06; [AD-UC-06](../activity/AD-UC-06.md) `s6` | Must | EI |
| F-02 | FR-02-14 | Validate actual quantity, handover time, and evidence | Driver / Collector | WHEN a driver submits handover information for an assigned pickup, THEN the system SHALL validate actual quantity, handover time and required evidence as specified by AD-UC-07. | UC:UC-07; [AD-UC-07](../activity/AD-UC-07.md) `a2` | Must | NC |
| F-02 | FR-02-15 | Store handover record against assigned pickup | Driver / Collector | WHEN handover information is valid, THEN the system SHALL store the handover record against the assigned pickup. | UC:UC-07; [AD-UC-07](../activity/AD-UC-07.md) `a3` | Must | EI |
| F-02 | FR-02-16 | Calculate transaction amount | Driver / Collector | WHEN the validated handover record is stored, THEN the system SHALL calculate the transaction amount using the actual quantity, base price and supplier current loyalty bonus rate. | UC:UC-07; [AD-UC-07](../activity/AD-UC-07.md) `a4` | Must | NC |
| F-02 | FR-02-17 | Generate draft agreement | Driver / Collector | WHEN the transaction amount has been calculated, THEN the system SHALL generate the draft handover agreement. | UC:UC-07; [AD-UC-07](../activity/AD-UC-07.md) `a5` | Must | EO |
| F-02 | FR-02-18 | Reject and prompt for correction | Driver / Collector | WHEN handover information is invalid, THEN the system SHALL reject the submission without saving a handover record and prompt the driver to correct it. | UC:UC-07; [AD-UC-07](../activity/AD-UC-07.md) `a7` | Must | NC |
| F-03 | FR-03-01 | Retrieve supplier draft agreement | Supplier | WHEN a signed-in supplier opens a transaction awaiting signature, THEN the system SHALL retrieve the unsigned draft belonging to that supplier, including the recorded handover and calculated amount. | UC:UC-08; [AD-UC-08](../activity/AD-UC-08.md) `s1`; [SD-UC-08](../use-case/SD-UC-08.md) | Must | EQ |
| F-03 | FR-03-02 | Display handover and calculated amount | Supplier | WHEN an owned unsigned draft is available, THEN the system SHALL display its recorded quantity, base price, loyalty bonus and calculated amount as read-only values. | UC:UC-08; [AD-UC-08](../activity/AD-UC-08.md) `s2`; [SD-UC-08](../use-case/SD-UC-08.md) | Must | NC |
| F-03 | FR-03-03 | Display no transaction awaiting signature | Supplier | WHEN no unsigned draft belonging to the signed-in supplier is available, THEN the system SHALL display the no-transaction-awaiting-signature state without exposing another supplier transaction. | UC:UC-08; [AD-UC-08](../activity/AD-UC-08.md) `s3`; [SD-UC-08](../use-case/SD-UC-08.md) | Must | NC |
| F-03 | FR-03-04 | Validate signature and confirmation | Supplier | WHEN the supplier submits an electronic signature for the confirmed transaction, THEN the system SHALL validate that the signature is nonempty and the agreement confirmation is present. | UC:UC-08; [AD-UC-08](../activity/AD-UC-08.md) `s4`; [SD-UC-08](../use-case/SD-UC-08.md) | Must | NC |
| F-03 | FR-03-05 | Return signing validation feedback | Supplier | WHEN the submitted signature is missing or the agreement confirmation is unchecked, THEN the system SHALL return correction feedback without locking the agreement, storing a signature or completing the pickup. | UC:UC-08; [AD-UC-08](../activity/AD-UC-08.md) `s5`; [SD-UC-08](../use-case/SD-UC-08.md) | Must | NC |
| F-03 | FR-03-06 | Lock agreement and calculated amount | Supplier | WHEN the supplier signs the reviewed transaction with valid signing input, THEN the system SHALL lock the agreement document hash together with the calculated amount. | UC:UC-08; [AD-UC-08](../activity/AD-UC-08.md) `s6`; [SD-UC-08](../use-case/SD-UC-08.md) | Must | NC |
| F-03 | FR-03-07 | Store electronic signature | Supplier | WHEN the supplier signs the reviewed transaction, THEN the system SHALL store the electronic signature and signing time against that transaction. | UC:UC-08; [AD-UC-08](../activity/AD-UC-08.md) `s7`; [SD-UC-08](../use-case/SD-UC-08.md) | Must | EI |
| F-03 | FR-03-08 | Mark pickup as Completed | Supplier | WHEN the signed agreement and electronic signature are stored successfully, THEN the system SHALL mark the linked pickup as Completed. | UC:UC-08; [AD-UC-08](../activity/AD-UC-08.md) `s8`; [SD-UC-08](../use-case/SD-UC-08.md) | Must | NC |
| F-03 | FR-03-09 | Return signing failure and prompt retry | Supplier | WHEN signing the transaction fails, THEN the system SHALL retain the unsigned agreement and previous pickup/balance state and return failure feedback prompting the supplier to sign again. | UC:UC-08; [AD-UC-08](../activity/AD-UC-08.md) `s9`; [SD-UC-08](../use-case/SD-UC-08.md) | Must | NC |
| F-03 | FR-03-10 | Display signing success | Supplier | WHEN the transaction is signed successfully, THEN the system SHALL display the signing confirmation, locked signature, signing time and signed amount, with receipt and home navigation. | UC:UC-08; [AD-UC-08](../activity/AD-UC-08.md) `s10`; [SD-UC-08](../use-case/SD-UC-08.md) | Must | NC |
| F-03 | FR-03-11 | Check receipt ownership and signed status | Supplier | WHEN a supplier requests a transaction receipt, THEN the system SHALL check that the transaction is signed and belongs to that supplier. | UC:UC-09; [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Must | NC |
| F-03 | FR-03-12 | Retrieve receipt data | Supplier | WHEN receipt ownership and signed status are valid, THEN the system SHALL retrieve the transaction receipt data. | UC:UC-09; [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Must | NC |
| F-03 | FR-03-13 | Render and provide electronic receipt PDF | Supplier | WHEN authorized signed-transaction receipt data are available, THEN the system SHALL render and provide the electronic receipt PDF for viewing or downloading. | UC:UC-09; [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Must | EQ |
| F-03 | FR-03-14 | Show warehouse verification status | Supplier | WHEN the supplier views an authorized signed-transaction receipt, THEN the system SHALL show warehouse verification status for information only. | UC:UC-09; [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Must | NC |
| F-04 | FR-04-01 | Record received quantity | Warehouse Staff | WHEN Warehouse Staff receives collected UCO whose transaction has been signed, THEN the system SHALL record the received quantity against the collection. | UC:UC-10; [AD-UC-10](../activity/AD-UC-10.md) `s1` | Must | EI |
| F-04 | FR-04-02 | Compare received quantity with collection record | Warehouse Staff | WHEN the received quantity has been recorded, THEN the system SHALL compare it with the quantity in the collection record using the configured tolerance limit. | UC:UC-10; [AD-UC-10](../activity/AD-UC-10.md) `s2` | Must | NC |
| F-04 | FR-04-03 | Log discrepancy alert for internal reconciliation | Warehouse Staff | WHEN the quantity difference exceeds the tolerance limit, THEN the system SHALL log a discrepancy alert for internal reconciliation without halting receiving. | UC:UC-10; [AD-UC-10](../activity/AD-UC-10.md) `s3` | Must | NC |
| F-04 | FR-04-04 | Keep signed transaction amount unchanged | Warehouse Staff | WHEN warehouse receiving is processed, whether within or outside tolerance, THEN the system SHALL preserve the signed transaction amount unchanged. | UC:UC-10; [AD-UC-10](../activity/AD-UC-10.md) `s4` | Must | NC |
| F-04 | FR-04-05 | Update collection status to Received | Warehouse Staff | WHEN warehouse quantity comparison and any discrepancy alert are processed, THEN the system SHALL update the collection status to Received. | UC:UC-10; [AD-UC-10](../activity/AD-UC-10.md) `s5` | Must | NC |
| F-04 | FR-04-06 | Validate collection inputs & batch parameters | Warehouse Staff | WHEN Warehouse Staff selects verified UCO collections and enters batch parameters, THEN the system SHALL validate those inputs and parameters, including invalid volume or duplicate inputs. | UC:UC-11; [AD-UC-11](../activity/AD-UC-11.md) `s1` | Must | NC |
| F-04 | FR-04-07 | Prompt invalid parameters & request correction | Warehouse Staff | WHEN collection inputs or batch parameters are invalid, THEN the system SHALL display the invalid-parameter feedback and request correction. | UC:UC-11; [AD-UC-11](../activity/AD-UC-11.md) `s2` | Must | NC |
| F-04 | FR-04-08 | Link collections & compute mass-balance metrics | Warehouse Staff | WHEN batch inputs and parameters are valid, THEN the system SHALL link the component collections and compute mass-balance metrics for the Created UCO batch. | UC:UC-11; [AD-UC-11](../activity/AD-UC-11.md) `s3` | Must | EI |
| F-04 | FR-04-09 | Maintain linked collection inputs & update batch balance | Warehouse Staff | WHEN the UCO batch has been created, THEN the system SHALL maintain its linked collection inputs and update its batch balance. | UC:UC-11; [AD-UC-11](../activity/AD-UC-11.md) `s4` | Must | NC |
| F-04 | FR-04-10 | Receive batch traceability & mass-balance record | Warehouse Staff | WHEN the UCO batch has been created, THEN the system SHALL make its batch traceability and mass-balance record available to Eco Oil System in parallel with internal balance maintenance. | UC:UC-11; [AD-UC-11](../activity/AD-UC-11.md) `e1` | Must | EO |
| F-04 | FR-04-11 | Trace UCO through the supply chain | ReFeed Staff | WHEN authorised Refeed staff requests a trace, THEN the system SHALL display the traceable relationship from supplier and collection through warehouse batch to downstream handover. | UC:UC-11; [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Must | EQ |
| F-05 | FR-05-01 | Retrieve Loyalty Tier & Payout Balance | Supplier | WHEN an Active signed-in supplier requests the Loyalty & Payout overview, THEN the system SHALL retrieve their current loyalty tier, progress to the next tier and accumulated payout balance. | UC:UC-12; [AD-UC-12](../activity/AD-UC-12.md) `retrieve` | Must | EQ |
| F-05 | FR-05-02 | Display Loyalty Tier & History Details | Supplier | WHEN the supplier chooses View tier and history details, THEN the system SHALL display the current tier, verified UCO volume, progress to the next tier and itemized base-payment, loyalty-bonus and tier-upgrade-bonus history. | UC:UC-12; [AD-UC-12](../activity/AD-UC-12.md) `displayTier` | Must | EQ |
| F-05 | FR-05-03 | Add signed transaction payment to payout balance | Supplier | WHEN a UCO transaction is signed, THEN the system SHALL add its locked base payment and loyalty bonus to the supplier accumulated payout balance. | UC:UC-12; [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Must | NC |
| F-05 | FR-05-04 | Add signed transaction quantity to verified UCO volume | Supplier | WHEN a UCO transaction is signed, THEN the system SHALL add its actual quantity to the supplier verified UCO volume. | UC:UC-12; [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Must | NC |
| F-05 | FR-05-05 | Upgrade loyalty tier | Supplier | WHEN the supplier verified UCO volume reaches a higher tier threshold, THEN the system SHALL upgrade the supplier loyalty tier. | UC:UC-12; [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Must | NC |
| F-05 | FR-05-06 | Award tier upgrade bonus | Supplier | WHEN the supplier loyalty tier is upgraded, THEN the system SHALL award the applicable tier upgrade bonus. | UC:UC-12; [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Must | NC |
| F-05 | FR-05-07 | Validate Withdrawal Request | Supplier | WHEN a supplier requests a balance withdrawal, THEN the system SHALL validate the requested amount against available balance, the minimum withdrawal threshold and supplier/bank-profile eligibility. | UC:UC-13; [AD-UC-12](../activity/AD-UC-12.md) `validate`; [AD-UC-13](../activity/AD-UC-13.md) `s1` | Must | NC |
| F-05 | FR-05-08 | Display Existing Balance & Withdrawal Rejection Reason | Supplier | WHEN a withdrawal request is ineligible, THEN the system SHALL display the existing balance and rejection reason without creating a withdrawal transaction or deducting balance. | UC:UC-13; [AD-UC-12](../activity/AD-UC-12.md) `display`; [AD-UC-13](../activity/AD-UC-13.md) `s3` | Must | NC |
| F-05 | FR-05-09 | Create Withdrawal Transaction | Supplier | WHEN a withdrawal request is eligible, THEN the system SHALL create a withdrawal transaction record for external payout handling. | UC:UC-13; [AD-UC-12](../activity/AD-UC-12.md) `transaction`; [AD-UC-13](../activity/AD-UC-13.md) `s2` | Must | EI |
| F-05 | FR-05-10 | Update Payout Status | Supplier | WHEN an eligible withdrawal is accepted, THEN the system SHALL update its payout status and deduct the accepted amount from available payout balance; WHEN the request is rejected, THEN the system SHALL retain the existing balance and shall not mark a payout as completed. | UC:UC-13; [AD-UC-12](../activity/AD-UC-12.md) `update`; [AD-UC-13](../activity/AD-UC-13.md) `s4` | Must | NC |
| F-05 | FR-05-11 | Display Withdrawal Result & Updated Payout Status and Balance | Supplier | WHEN withdrawal transaction creation and payout-status update have both completed, THEN the system SHALL display the withdrawal result, updated payout status and balance. | UC:UC-13; [AD-UC-12](../activity/AD-UC-12.md) `displayUpdated` | Must | NC |
| F-05 | FR-05-12 | View Green Profile and UCO contribution history | Supplier | WHEN a supplier opens the Green Profile, THEN the system SHALL display verified UCO contribution and collection history. | UC:UC-14; [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Should | EQ |
| F-05 | FR-05-13 | Generate ESG / environmental contribution report | Supplier | WHEN a supplier requests an ESG or environmental contribution report, THEN the system SHALL generate the report using verified collection data. | UC:UC-15; [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Should | EO |
| F-05 | FR-05-14 | Provide ESG / environmental contribution report | Supplier | WHEN the requested ESG or environmental contribution report is generated, THEN the system SHALL provide it to the supplier. | UC:UC-15; [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Should | NC |
| F-06 | FR-06-01 | Monitor operations and compliance status | ReFeed Staff | WHEN authorised staff opens the monitoring view, THEN the system SHALL display relevant suppliers, pickups, collections, transactions and compliance statuses for operational oversight. | UC:UC-16; [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Must | EQ |
| F-06 | FR-06-02 | Retrieve and display audit history | ReFeed Staff | WHEN authorized ReFeed Staff requests audit information, THEN the system SHALL retrieve and display historical changes. | UC:UC-17; [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Must | EQ |
| F-06 | FR-06-03 | Generate compliance report | ReFeed Staff | WHEN authorized ReFeed Staff requests compliance information, THEN the system SHALL generate the applicable compliance report. | UC:UC-17; [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Must | EO |
| F-06 | FR-06-04 | Check role permission for protected action | System | WHEN an authenticated user attempts a protected action, THEN the system SHALL permit it only when the assigned role grants the required permission. | [Architecture](../architecture/architecture.md); [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Must | NC |
| F-06 | FR-06-05 | Record denied access attempt | System | WHEN a protected action is denied by the permission check, THEN the system SHALL record the denied attempt. | [Architecture](../architecture/architecture.md); [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Must | NC |
| F-06 | FR-06-06 | Run automated unit tests | System | WHEN code is merged into the release branch, THEN the delivery pipeline SHALL run automated unit tests. | [Architecture](../architecture/architecture.md); [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Must | NC |
| F-06 | FR-06-07 | Run automated API tests | System | WHEN code is merged into the release branch, THEN the delivery pipeline SHALL run automated API tests. | [Architecture](../architecture/architecture.md); [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Must | NC |
| F-06 | FR-06-08 | Run automated acceptance tests | System | WHEN code is merged into the release branch, THEN the delivery pipeline SHALL run automated acceptance tests. | [Architecture](../architecture/architecture.md); [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Must | NC |
| F-06 | FR-06-09 | Block deployment on test failure | System | WHEN any required automated unit, API or acceptance test fails, THEN the delivery pipeline SHALL block deployment. | [Architecture](../architecture/architecture.md); [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Must | NC |
| F-06 | FR-06-10 | Deploy approved release | System | WHEN a release is approved, THEN the delivery pipeline SHALL deploy it to the target environment. | [Architecture](../architecture/architecture.md); [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Must | NC |
| F-06 | FR-06-11 | Expose service monitoring signals | System | WHEN the release is deployed, THEN the system SHALL expose health, error and latency signals for operational monitoring. | [Architecture](../architecture/architecture.md); [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Must | NC |
| F-06 | FR-06-12 | Create scheduled operational and audit backup | System | WHEN the scheduled backup window occurs, THEN the system SHALL create a recoverable backup of operational and audit data. | [Architecture](../architecture/architecture.md); [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Must | NC |
| F-06 | FR-06-13 | Record backup result | System | WHEN a scheduled backup attempt finishes, THEN the system SHALL record its result. | [Architecture](../architecture/architecture.md); [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Must | NC |
| F-06 | FR-06-14 | Verify backup recovery | System | WHEN recovery verification is run for a saved backup, THEN the system SHALL verify that the backed-up operational and audit records can be recovered. | [Architecture](../architecture/architecture.md); [W1 baseline, 2026-11-25](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w1-function-list.md#l2-functions); no activity node | Must | NC |

## L3 work items

| L3 ID | L2 ID | Layer | L3 work item | Size | Base hours | EARS precise? | Hidden work? |
|---|---|---|---|---|---|---|---|
| FR-01-01-FE | FR-01-01 | FE | Capture and submit supplier credentials or registration/compliance data | M | 8 | No | No |
| FR-01-01-BE | FR-01-01 | BE | API/service: Validate Credentials and Compliance Data | M | 8 | No | No |
| FR-01-02-BE | FR-01-02 | BE | API/service: Create Supplier Account with Unique Supplier_ID | S | 4 | Yes | No |
| FR-01-03-FE | FR-01-03 | FE | UI: Display Account Already Exists & Redirect to Login | S | 4 | Yes | No |
| FR-01-04-FE | FR-01-04 | FE | UI: Display Dashboard & Authentication Result | S | 4 | Yes | No |
| FR-01-05-FE | FR-01-05 | FE | UI: Display Error Message & Prompt Retry | S | 4 | Yes | No |
| FR-01-06-BE | FR-01-06 | BE | API/service: Authenticate returning supplier | M | 8 | Yes | No |
| FR-01-07-FE | FR-01-07 | FE | Capture Point-of-Origin and compliance fields for validation | M | 8 | No | No |
| FR-01-07-BE | FR-01-07 | BE | API/service: Validate Point-of-Origin and compliance information | S | 4 | No | No |
| FR-01-08-BE | FR-01-08 | BE | API/service: Store Point-of-Origin and compliance profile | S | 4 | Yes | No |
| FR-01-09-FE | FR-01-09 | FE | Submit pickup request to the supplier-profile completeness check | S | 4 | Yes | No |
| FR-01-09-BE | FR-01-09 | BE | API/service: Check that supplier profile is complete | S | 4 | Yes | No |
| FR-01-10-FE | FR-01-10 | FE | UI: Reject request and prompt profile update | S | 4 | Yes | No |
| FR-01-10-BE | FR-01-10 | BE | API/service: Reject request and prompt profile update | S | 4 | Yes | No |
| FR-01-11-FE | FR-01-11 | FE | Open the pickup form and capture/correct location, estimated volume, preferred time window and notes | M | 8 | No | No |
| FR-01-11-BE | FR-01-11 | BE | API/service: Validate pickup location, estimated volume and time window | S | 4 | No | No |
| FR-01-12-FE | FR-01-12 | FE | UI: Return validation feedback | S | 4 | Yes | No |
| FR-01-12-BE | FR-01-12 | BE | API/service: Return validation feedback | S | 4 | Yes | No |
| FR-01-13-BE | FR-01-13 | BE | API/service: Create pickup request with status Requested and record the time window | S | 4 | Yes | No |
| FR-01-14-FE | FR-01-14 | FE | UI: Return pickup request ID | S | 4 | Yes | No |
| FR-01-14-BE | FR-01-14 | BE | API/service: Return pickup request ID | S | 4 | Yes | No |
| FR-01-15-FE | FR-01-15 | FE | Select an existing pickup request and request its status and applicable rules | S | 4 | Yes | No |
| FR-01-15-BE | FR-01-15 | BE | API/service: Fetch pickup request status & validation rules | S | 4 | Yes | No |
| FR-01-16-FE | FR-01-16 | FE | UI: Display modification error: driver already dispatched | S | 4 | Yes | No |
| FR-01-17-FE | FR-01-17 | FE | Capture an eligible pickup change or cancellation and show the update confirmation | M | 8 | Yes | No |
| FR-01-17-BE | FR-01-17 | BE | API/service: Update pickup request status & reschedule timeline | M | 8 | Yes | No |
| FR-01-18-BE | FR-01-18 | BE | API/service: Update central pickup schedule & logs | S | 4 | Yes | No |
| FR-01-19-BE | FR-01-19 | BE | Send the dispatch-schedule update alert to UCO Collectors | S | 4 | Yes | No |
| FR-01-20-FE | FR-01-20 | FE | Show pickup tracking, driver ETA and available status/appointment details | S | 4 | Yes | No |
| FR-01-20-BE | FR-01-20 | BE | API/service: Fetch real-time tracking & driver ETA | M | 8 | Yes | No |
| FR-02-01-FE | FR-02-01 | FE | Select the planning area and submit the collection-route planning request | S | 4 | No | No |
| FR-02-01-BE | FR-02-01 | BE | API/service: Group pending pickups within vehicle capacity | M | 8 | No | No |
| FR-02-02-INT | FR-02-02 | INT | Integration: Obtain optimized visiting order from map/route data | M | 8 | No | No |
| FR-02-03-FE | FR-02-03 | FE | UI: Display proposed collection route | M | 8 | Yes | No |
| FR-02-04-FE | FR-02-04 | FE | Capture staff route confirmation and driver/vehicle selection | S | 4 | Yes | No |
| FR-02-04-BE | FR-02-04 | BE | API/service: Assign driver and vehicle to route | S | 4 | Yes | No |
| FR-02-05-BE | FR-02-05 | BE | API/service: Save and dispatch collection route | M | 8 | Yes | No |
| FR-02-06-BE | FR-02-06 | BE | API/service: Update included pickups to Driver Assigned | S | 4 | Yes | No |
| FR-02-07-BE | FR-02-07 | BE | API/service: Notify assigned driver | S | 4 | Yes | No |
| FR-02-08-FE | FR-02-08 | FE | UI: Display assigned route and pickup points | M | 8 | Yes | No |
| FR-02-08-BE | FR-02-08 | BE | Retrieve the driver assigned route and pickup points | S | 4 | Yes | No |
| FR-02-09-FE | FR-02-09 | FE | Select an assigned supplier point and submit arrival confirmation | S | 4 | Yes | No |
| FR-02-09-BE | FR-02-09 | BE | API/service: Update selected pickup status to Arrived | S | 4 | Yes | No |
| FR-02-10-FE | FR-02-10 | FE | Capture/correct actual quantity and optional collection evidence for validation | M | 8 | No | No |
| FR-02-10-BE | FR-02-10 | BE | API/service: Validate actual quantity, GPS, timestamp and supplied evidence | M | 8 | No | No |
| FR-02-11-FE | FR-02-11 | FE | UI: Return validation feedback | S | 4 | Yes | No |
| FR-02-11-BE | FR-02-11 | BE | API/service: Return validation feedback | S | 4 | Yes | No |
| FR-02-12-BE | FR-02-12 | BE | API/service: Save actual quantity, GPS, timestamp and supplied evidence against pickup | M | 8 | Yes | No |
| FR-02-13-FE | FR-02-13 | FE | Report UCO not ready at an Arrived pickup and show its cancellation | S | 4 | Yes | No |
| FR-02-13-BE | FR-02-13 | BE | API/service: Cancel current pickup and update status to Cancelled | S | 4 | Yes | No |
| FR-02-14-FE | FR-02-14 | FE | Capture/submit actual quantity, handover time and evidence; allow correction | M | 8 | No | No |
| FR-02-14-BE | FR-02-14 | BE | API/service: Validate actual quantity, handover time, and evidence | S | 4 | No | No |
| FR-02-15-BE | FR-02-15 | BE | API/service: Store handover record against assigned pickup | M | 8 | Yes | No |
| FR-02-16-BE | FR-02-16 | BE | API/service: Calculate transaction amount | M | 8 | Yes | No |
| FR-02-17-FE | FR-02-17 | FE | Display the generated draft handover agreement | S | 4 | Yes | No |
| FR-02-17-BE | FR-02-17 | BE | API/service: Generate draft agreement | M | 8 | Yes | No |
| FR-02-18-FE | FR-02-18 | FE | UI: Reject and prompt for correction | S | 4 | Yes | No |
| FR-02-18-BE | FR-02-18 | BE | API/service: Reject and prompt for correction | S | 4 | Yes | No |
| FR-03-01-FE | FR-03-01 | FE | Request the signed-in supplier own unsigned draft agreement | S | 4 | Yes | No |
| FR-03-01-BE | FR-03-01 | BE | Retrieve the owned unsigned draft, handover and calculated amount | S | 4 | Yes | No |
| FR-03-02-FE | FR-03-02 | FE | Display the reviewed handover and read-only pricing/amount | S | 4 | Yes | No |
| FR-03-03-FE | FR-03-03 | FE | Show the existing empty state and transaction-history navigation | S | 4 | Yes | No |
| FR-03-04-FE | FR-03-04 | FE | Capture/clear the signature, capture the confirmation checkbox and open the confirmation modal | M | 8 | Yes | No |
| FR-03-04-BE | FR-03-04 | BE | Validate the submitted signature and explicit agreement confirmation | S | 4 | Yes | No |
| FR-03-05-FE | FR-03-05 | FE | Display signing validation feedback and allow input correction | S | 4 | Yes | No |
| FR-03-05-BE | FR-03-05 | BE | Return signature/confirmation validation feedback without signing writes | S | 4 | Yes | No |
| FR-03-06-BE | FR-03-06 | BE | Lock the reviewed agreement document hash and calculated amount | M | 8 | Yes | No |
| FR-03-07-BE | FR-03-07 | BE | Store the electronic signature and signing time against the transaction | M | 8 | Yes | No |
| FR-03-08-BE | FR-03-08 | BE | Mark the linked pickup Completed after successful signing | S | 4 | Yes | No |
| FR-03-09-FE | FR-03-09 | FE | Show signing-failure feedback and allow a fresh signature submission | S | 4 | Yes | No |
| FR-03-09-BE | FR-03-09 | BE | Return signing failure with the unsigned agreement and previous pickup state retained | M | 8 | Yes | No |
| FR-03-10-FE | FR-03-10 | FE | Show the locked signature and signing-success summary with receipt/home navigation | S | 4 | Yes | No |
| FR-03-11-FE | FR-03-11 | FE | Request an authorized signed-transaction receipt | S | 4 | Yes | No |
| FR-03-11-BE | FR-03-11 | BE | API/service: Check receipt ownership and signed status | S | 4 | Yes | No |
| FR-03-12-BE | FR-03-12 | BE | API/service: Retrieve receipt data | S | 4 | Yes | No |
| FR-03-13-FE | FR-03-13 | FE | UI: Render and provide electronic receipt PDF | S | 4 | Yes | No |
| FR-03-13-BE | FR-03-13 | BE | API/service: Render and provide electronic receipt PDF | M | 8 | Yes | No |
| FR-03-14-FE | FR-03-14 | FE | UI: Show warehouse verification status | S | 4 | Yes | No |
| FR-04-01-FE | FR-04-01 | FE | Identify the collected UCO and submit the warehouse received quantity | S | 4 | Yes | No |
| FR-04-01-BE | FR-04-01 | BE | API/service: Record received quantity | S | 4 | Yes | No |
| FR-04-02-FE | FR-04-02 | FE | Present the received-versus-collected quantity comparison | S | 4 | Yes | No |
| FR-04-02-BE | FR-04-02 | BE | API/service: Compare received quantity with collection record | S | 4 | Yes | No |
| FR-04-03-FE | FR-04-03 | FE | Show the logged discrepancy alert for internal reconciliation | S | 4 | Yes | No |
| FR-04-03-BE | FR-04-03 | BE | API/service: Log discrepancy alert for internal reconciliation | S | 4 | Yes | No |
| FR-04-04-BE | FR-04-04 | BE | API/service: Keep signed transaction amount unchanged | S | 4 | Yes | No |
| FR-04-05-FE | FR-04-05 | FE | Show the collection receiving-status result | S | 4 | Yes | No |
| FR-04-05-BE | FR-04-05 | BE | API/service: Update collection status to Received | S | 4 | Yes | No |
| FR-04-06-FE | FR-04-06 | FE | Select verified collections and enter/correct batch parameters | S | 4 | No | No |
| FR-04-06-BE | FR-04-06 | BE | API/service: Validate collection inputs & batch parameters | S | 4 | No | No |
| FR-04-07-FE | FR-04-07 | FE | UI: Prompt invalid parameters & request correction | S | 4 | Yes | No |
| FR-04-08-BE | FR-04-08 | BE | API/service: Link collections & compute mass-balance metrics | M | 8 | No | No |
| FR-04-09-FE | FR-04-09 | FE | Show the batch creation and updated mass-balance summary | S | 4 | No | No |
| FR-04-09-BE | FR-04-09 | BE | API/service: Maintain linked collection inputs & update batch balance | M | 8 | No | No |
| FR-04-10-INT | FR-04-10 | INT | Integration: provide the batch traceability and mass-balance record to Eco Oil System | M | 8 | No | No |
| FR-04-11-FE | FR-04-11 | FE | UI: Trace UCO through the supply chain | M | 8 | Yes | No |
| FR-04-11-BE | FR-04-11 | BE | API/service: Trace UCO through the supply chain | L | 16 | Yes | No |
| FR-05-01-FE | FR-05-01 | FE | Open the Loyalty & Payout overview and show retrieved tier/balance | S | 4 | Yes | No |
| FR-05-01-BE | FR-05-01 | BE | API/service: Retrieve Loyalty Tier & Payout Balance | S | 4 | Yes | No |
| FR-05-02-FE | FR-05-02 | FE | UI: Display Loyalty Tier & History Details | S | 4 | Yes | No |
| FR-05-02-BE | FR-05-02 | BE | Retrieve itemized loyalty-tier and transaction-history details | S | 4 | Yes | No |
| FR-05-03-BE | FR-05-03 | BE | API/service: Add signed transaction payment to payout balance | M | 8 | Yes | No |
| FR-05-04-BE | FR-05-04 | BE | API/service: Add signed transaction quantity to verified UCO volume | S | 4 | Yes | No |
| FR-05-05-BE | FR-05-05 | BE | API/service: Upgrade loyalty tier | S | 4 | Yes | No |
| FR-05-06-BE | FR-05-06 | BE | API/service: Award tier upgrade bonus | M | 8 | Yes | No |
| FR-05-07-FE | FR-05-07 | FE | Capture withdrawal amount and bank details and submit for eligibility validation | S | 4 | No | No |
| FR-05-07-BE | FR-05-07 | BE | API/service: Validate Withdrawal Request | S | 4 | No | No |
| FR-05-08-FE | FR-05-08 | FE | UI: Display Existing Balance & Withdrawal Rejection Reason | S | 4 | Yes | No |
| FR-05-08-BE | FR-05-08 | BE | Return the withdrawal rejection reason and existing balance | S | 4 | Yes | No |
| FR-05-09-BE | FR-05-09 | BE | API/service: Create Withdrawal Transaction | M | 8 | Yes | No |
| FR-05-10-BE | FR-05-10 | BE | API/service: Update Payout Status | M | 8 | Yes | No |
| FR-05-11-FE | FR-05-11 | FE | UI: Display Withdrawal Result & Updated Payout Status and Balance | S | 4 | Yes | No |
| FR-05-12-FE | FR-05-12 | FE | UI: View Green Profile and UCO contribution history | S | 4 | Yes | No |
| FR-05-12-BE | FR-05-12 | BE | API/service: View Green Profile and UCO contribution history | S | 4 | Yes | No |
| FR-05-13-BE | FR-05-13 | BE | API/service: Generate ESG / environmental contribution report | M | 8 | No | No |
| FR-05-14-FE | FR-05-14 | FE | UI: Provide ESG / environmental contribution report | S | 4 | Yes | No |
| FR-06-01-FE | FR-06-01 | FE | UI: Monitor operations and compliance status | M | 8 | No | No |
| FR-06-01-BE | FR-06-01 | BE | API/service: Monitor operations and compliance status | M | 8 | No | No |
| FR-06-02-FE | FR-06-02 | FE | UI: Retrieve and display audit history | S | 4 | No | No |
| FR-06-02-BE | FR-06-02 | BE | API/service: Retrieve and display audit history | M | 8 | No | No |
| FR-06-03-FE | FR-06-03 | FE | Request and display the generated compliance report | S | 4 | No | No |
| FR-06-03-BE | FR-06-03 | BE | API/service: Generate compliance report | M | 8 | No | No |
| FR-06-04-BE | FR-06-04 | BE | API/service: Check role permission for protected action | M | 8 | Yes | Yes |
| FR-06-05-BE | FR-06-05 | BE | API/service: Record denied access attempt | S | 4 | Yes | Yes |
| FR-06-06-BE | FR-06-06 | BE | Test automation: Run automated unit tests | M | 8 | Yes | Yes |
| FR-06-07-BE | FR-06-07 | BE | Test automation: Run automated API tests | M | 8 | Yes | Yes |
| FR-06-08-BE | FR-06-08 | BE | Test automation: Run automated acceptance tests | M | 8 | Yes | Yes |
| FR-06-09-BE | FR-06-09 | BE | Test automation: Block deployment on test failure | S | 4 | Yes | Yes |
| FR-06-10-INT | FR-06-10 | INT | Integration: Deploy approved release | M | 8 | Yes | Yes |
| FR-06-11-INT | FR-06-11 | INT | Integration: Expose service monitoring signals | M | 8 | Yes | Yes |
| FR-06-12-BE | FR-06-12 | BE | Backup/recovery: Create scheduled operational and audit backup | M | 8 | Yes | Yes |
| FR-06-13-BE | FR-06-13 | BE | Backup/recovery: Record backup result | S | 4 | Yes | Yes |
| FR-06-14-BE | FR-06-14 | BE | Backup/recovery: Verify backup recovery | M | 8 | Yes | Yes |

91 L2 functions, 132 L3 work items, 724 base hours across 6 features. No row is above the 16-hour ceiling. The 11 hidden technical work items total 76 base hours.

Sizing basis: these are task-level planning estimates for the refined list. S covers a bounded query, validation, state update or feedback view; M covers a multi-field form, signature capture, generated document, calculation, coordinated state change, external integration or substantial automation; the deferred supply-chain query uses L for its backend. They replace the earlier grouped estimate; they are not hours copied from a shared package. The team must review the estimates before accepting a delivery commitment.

FP type follows the workshop definitions: EI = input that changes data, EO = output with derived data, EQ = retrieval, NC = a rule/internal step or technical work inside another transaction. Count a boundary transaction once at its representative L2 row. For example, signing counts at FR-03-07; its validation, locking, pickup completion and balance/tier updates are NC. The receipt retrieves already calculated signed data and is EQ. Each NC row still carries its own L3 effort; only hidden platform work is added separately in W2's FP comparison.

EARS precision is No for 16 L2 functions (28 L3 items), with the reasons listed in [W3](w3-cross-review.md#ears-precision-review). A configured parameter can be tested with a fixture; its final business value may still be open. No tolerance, tier threshold, withdrawal minimum or external API contract is decided by this estimate.
