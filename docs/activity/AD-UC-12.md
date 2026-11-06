# Activity Diagram: AD-UC-12 Manage loyalty status and accumulated payout balance

| Field | Value |
|---|---|
| AD ID | AD-UC-12 |
| Use case | UC-12 Manage loyalty status and accumulated payout balance |
| Feature | F-05 Loyalty, Reward & ESG Transparency Management |
| Primary actor | Supplier |
| Lanes | Supplier · ReFeed system |
| Inputs (L3-L5) | Context Diagram v2 (Supplier) · Use Case Diagram v2 · Sequence: SD-UC-12 · Architecture v2 · W1 rows FR-05-01, FR-05-02 |
| Trigger | The supplier opens the Loyalty & Payout section on the portal |
| Preconditions | The supplier is signed in. The supplier account is active (`Active`). |
| Success end state | Supplier views their loyalty tier and payout history, OR a new `Withdrawal` in state `Completed`/`Processing` exists with updated payout balance. |
| Failure end state | No withdrawal transaction is created; the supplier sees why (insufficient balance or invalid bank account details). |
| FRs covered | FR-05-01, FR-05-02 |
| Business rules applied | BR-05 (Loyalty Tier Thresholds & Minimum Withdrawal Balance) |
| Version / status | v1.0 · Reviewed |
| Author | Nguyen Thuy Vi - 2411560050 |

## 1. Diagram

Notation follows the DBIZ2 activity profile (`mermaid-activity-patterns.md`). Rendered with
`swimlane-beta`, which needs Mermaid 11.16 or later.

```mermaid
---
config:
  flowchart:
    nodeSpacing: 60
    rankSpacing: 45
---


swimlane-beta TB
%% UC12: Manage loyalty status and accumulated payout balance
%% ReFeed group project
subgraph supplier ["Supplier"]
initial@{ shape: f-circ, label: " " }
request("Request Loyalty & Payout Overview")
choice{" "}
end


subgraph system ["ReFeed System"]
retrieve("Retrieve Loyalty Tier & Payout Balance (FR-05-01)")


displayTier("Display Loyalty Tier & History Details (FR-05-01)")
finalTier@{ shape: fr-circ, label: " " }


validate("Validate Withdrawal Request (FR-05-02)")
decision{" "}


display("Display Existing Balance & Withdrawal Rejection Reason (FR-05-02)")
finalRejected@{ shape: fr-circ, label: " " }


fork@{ shape: fork, label: " " }
transaction("Create Withdrawal Transaction (FR-05-02)")
update("Update Payout Status (FR-05-02)")
join@{ shape: fork, label: " " }
displayUpdated("Display Withdrawal Result & Updated Payout Status and Balance (FR-05-02)")
finalWithdrawal@{ shape: fr-circ, label: " " }
end


initial --> request
request --> retrieve
retrieve --> choice


choice -->|View Tier & History Details| displayTier
displayTier --> finalTier


choice -->|Request Balance Withdrawal| validate
validate --> decision


decision -->|No| display
display --> finalRejected


decision -->|Yes| fork
fork --> transaction
fork --> update
transaction --> join
update --> join
join --> displayUpdated
displayUpdated --> finalWithdrawal
```
Reading notes:

- The process starts when the **Supplier** accesses the Loyalty & Payout overview section on ReFeed.
- System action node `s1` retrieves the loyalty tier, progress to the next tier, and accumulated payout balance.
- Decision `d1` branches the flow based on the supplier's intent: View tier and history details (`s2`) or Request a payout withdrawal (`s3`).
- Decision `d2` validates the withdrawal request (checking minimum threshold limits and bank account eligibility). If ineligible, the system displays the rejection reason via `s4`. If valid, the system executes parallel processing to create the withdrawal transaction `s5` (object state `o1`) and update the payout status/balance `s6` via the Fork `f1` and Join `j1` pair.

## 2. Node trace

Every action in the system lane cites a requirement. Every decision cites the rule or requirement
that decides it.

| Node | Kind | Text | Traces to |
|---|---|---|---|
| s1 | Action | Retrieve loyalty tier & payout balance | FR-05-01 |
| d1 | Decision | Choose action | FR-05-01, FR-05-02 |
| s2 | Action | Display loyalty tier & history details | FR-05-01 |
| s3 | Action | Validate withdrawal request | FR-05-02, BR-05 |
| d2 | Decision | Withdrawal valid? | FR-05-02, BR-05 |
| s4 | Action | Display existing balance & withdrawal rejection reason | FR-05-02 |
| f1 | Fork | Parallel withdrawal processing | FR-05-02 |
| s5 | Action | Create withdrawal transaction | FR-05-02 |
| o1 | Object node | Withdrawal Transaction [Created] | FR-05-02 (state value) |
| s6 | Action | Update payout status & deduct balance | FR-05-02 |
| j1 | Join | Synchronize transaction completion | FR-05-02 |
| s7 | Action | Display withdrawal result & updated payout status and balance | FR-05-02 |

## 3. Branches and acceptance tests

One row per outgoing branch of every decision. DBIZ3 turns each row into at least one test.

| Branch | Given | When | Then |
|---|---|---|---|
| d1 [view tier & history] | a supplier holds an Active account with UCO contribution history | they request loyalty and payout overview | system displays the current tier, progress bar to next tier, and itemized transaction history (base payment, loyalty bonus, tier bonus) |
| d1 [request withdrawal] → d2 [no] | a supplier has a payout balance below the minimum withdrawal threshold | they submit a withdrawal request | no transaction is created; system displays current balance and explains rejection due to threshold restriction |
| d1 [request withdrawal] → d2 [yes] | a supplier has an eligible payout balance and valid bank profile | they submit a withdrawal request | a Withdrawal Transaction is created in state Created, payout balance is deducted, status is updated, and success result is shown |

## 4. Object states used

| Object | States that appear in this diagram | Full state list (all diagrams) |
|---|---|---|
| Payout Balance | Accumulated, Deducted | Accumulated, Deducted, Pending |
| Withdrawal Transaction | Created | Created, Processing, Completed, Failed |

These state names become the allowed values of `payout_balance.status` and `withdrawal_transaction.status` in the DBIZ3 data model. Keep the spelling identical everywhere.

## 5. Screens touched

Filled in during Lesson 8, from the Screen List.

| Supplier action | Screen ID |
|---|---|
| Request loyalty & payout overview, See loyalty tier & history details | S-F05-01 (Loyalty & Payout Dashboard) |
| Request balance withdrawal | S-F05-02 (Withdrawal Request Modal) |
| See existing balance & withdrawal rejection reason | S-F05-02 (Withdrawal Error State) |
| See withdrawal result & updated payout status and balance | S-F05-01 (Updated Dashboard & Transaction Status) |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: Is there an automated bank gateway integration for instant payout?] | No | Yes: The withdrawal transaction is created in `Created` state and queued for automated batch processing via banking API. |
| 2 | [NEEDS CLARIFICATION: Are withdrawal fees deducted from the accumulated balance or paid separately?] | No | Withdrawal fees are zero for standard loyalty tiers or automatically offset against tier bonuses (BR-05). |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders on mermaid.live | Pass |  | 06/11/2026 |
| Every decision has guards on all outgoing flows | Pass |  | 06/11/2026 |
| Every fork has a matching join | Pass |  | 06/11/2026 |
| Every system action cites an FR | Pass (FR-05-01, FR-05-02) |  | 06/11/2026 |
| Every FR of UC-12 appears in the diagram | Pass (2 of 2) |  | 06/11/2026 |
| Lane names match the Context Diagram | Pass |  | 06/11/2026 |
| AI critic prompt run, findings resolved or logged | Pass, 2 findings → open questions 1 and 2 |  | 06/11/2026 |
  f1 --> s5 --> o1 --> j1
  f1 --> s6 --> j1
  j1 --> s7 --> a5 --> stop3
