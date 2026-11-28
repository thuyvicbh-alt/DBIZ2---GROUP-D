# Activity Diagram: AD-UC-12 Supplier Value & ESG / Green Profile

| Field | Value |
|---|---|
| AD ID | AD-UC-12 |
| Use case | UC-12 Manage loyalty status and accumulated payout balance, followed through the F-05 flow (withdrawal, Green Profile, ESG report) |
| Feature | F-05 Supplier Value & ESG / Green Profile |
| Primary actor | Supplier |
| Lanes | Supplier · ReFeed system |
| Inputs (L3-L5) | Context Diagram v2 (Supplier) · Use Case Diagram v2 · Sequence: SD-UC-12 · Architecture v2 · W1 rows FR-05-01 to FR-05-04 |
| Trigger | (a) A UCO transaction is signed (UC-08), or (b) the supplier opens the Loyalty & Payout section on the portal |
| Preconditions | The supplier is signed in. The supplier account is active (`Active`). |
| Success end state | The supplier has viewed the loyalty status and payout balance, and has withdrawn an eligible balance, viewed the Green Profile or obtained an ESG report. After a signed transaction, balance, volume and tier are up to date. |
| Failure end state | No withdrawal transaction is created; the supplier sees why (balance not eligible). |
| FRs covered | FR-05-01 View loyalty status and accumulated payout balance · FR-05-02 Withdraw accumulated payout balance · FR-05-03 View Green Profile and UCO contribution history · FR-05-04 Access ESG / environmental contribution reports |
| Business rules applied | BR-05 (Loyalty Tier Thresholds & Minimum Withdrawal Balance) |
| Version / status | v1.3 · LabDesk pattern: every system action carries one FR, each FR appears once |
| Author | Nguyen Thuy Vi - 2411560050 |

## 1. Diagram

Notation follows the DBIZ2 activity profile (`mermaid-activity-patterns.md`). Rendered with
`swimlane-beta`, which needs Mermaid 11.16 or later.

```mermaid
swimlane-beta TB
  subgraph SUP["Supplier"]
    start@{ shape: sm-circ }
    a1("Open the Loyalty and Payout section")
    a2("Choose to withdraw, view Green Profile or request ESG report")
    a3("Submit withdrawal request")
    stop1@{ shape: fr-circ }
    stop2@{ shape: fr-circ }
  end
  subgraph SYS["ReFeed system"]
    signed@{ shape: sm-circ }
    u1("Add signed payment, bonus and quantity to balance and verified volume")
    d0{"Higher tier<br>threshold reached?"}
    u2("Upgrade loyalty tier and award tier upgrade bonus")
    m0{" "}
    stop3@{ shape: fr-circ }
    s1("View loyalty status and accumulated payout balance (FR-05-01)")
    d1{"Which<br>action?"}
    s2("Withdraw accumulated payout balance (FR-05-02)")
    d2{"Withdrawal<br>eligible?"}
    o1["Withdrawal Transaction [Created]"]
    s3("View Green Profile and UCO contribution history (FR-05-03)")
    s4("Access ESG / environmental contribution reports (FR-05-04)")
  end
  signed --> u1 --> d0
  d0 -->|"[yes]"| u2 --> m0
  d0 -->|"[no]"| m0
  m0 --> stop3
  start --> a1 --> s1 --> a2 --> d1
  d1 -->|"[withdraw balance]"| a3 --> s2 --> d2
  d2 -->|"[no, show reason]"| stop1
  d2 -->|"[yes, payout status updated]"| o1 --> stop2
  d1 -->|"[green profile]"| s3 --> stop2
  d1 -->|"[ESG report]"| s4 --> stop2
```

Reading notes:

- Every action in the system lane that implements a requirement carries one FR with its exact W1 name, and each FR appears once: `s1` FR-05-01, `s2` FR-05-02, `s3` FR-05-03, `s4` FR-05-04.
- Supplier actions, the update actions `u1` and `u2`, and the object node `o1` carry no FR. `u1` and `u2` are the update half of FR-05-01; the view half is `s1`.
- The diagram has two start points. The system-side start is a signed transaction handed over by UC-08: the system adds the payment, bonus and quantity, and upgrades the tier with a bonus when a higher threshold is reached. The supplier-side start is opening the Loyalty and Payout section.
- `m0` is a **merge** node: the upgrade flow and the no-upgrade flow both enter it before the update flow ends.
- The creation of the withdrawal transaction and the update of its payout status are shown by `o1` and the guard of `d2`, and need no separate FR.
- Two flows enter `stop2`, and the three branches after `d1` all end there. This is allowed: a final node is not an action.

## 2. Node trace

Every action in the system lane cites a requirement. Every decision cites the rule or requirement
that decides it.

| Node | Kind | Text | Traces to |
|---|---|---|---|
| u1 | Action | Add signed payment, bonus and quantity to balance and verified volume | FR-05-01 (update half) |
| d0 | Decision | Higher tier threshold reached? | FR-05-01, BR-05 |
| u2 | Action | Upgrade loyalty tier and award tier upgrade bonus | FR-05-01 (update half), BR-05 |
| s1 | Action | View loyalty status and accumulated payout balance | FR-05-01 |
| d1 | Decision | Which action? | FR-05-02, FR-05-03, FR-05-04 |
| s2 | Action | Withdraw accumulated payout balance | FR-05-02, BR-05 |
| d2 | Decision | Withdrawal eligible? | FR-05-02, BR-05 |
| o1 | Object node | Withdrawal Transaction [Created] | FR-05-02 (state value) |
| s3 | Action | View Green Profile and UCO contribution history | FR-05-03 |
| s4 | Action | Access ESG / environmental contribution reports | FR-05-04 |

## 3. Branches and acceptance tests

One row per outgoing branch of every decision. DBIZ3 turns each row into at least one test.

| Branch | Given | When | Then |
|---|---|---|---|
| d0 [yes] | a UCO transaction is signed and the verified UCO volume reaches a higher tier threshold | the system processes the signed transaction | balance and volume are updated, the tier is upgraded and the tier upgrade bonus is awarded |
| d0 [no] | a UCO transaction is signed and the volume stays below the next threshold | the system processes the signed transaction | balance and volume are updated and the tier stays the same |
| d1 [withdraw balance] → d2 [no] | the accumulated payout balance is not eligible for withdrawal | the supplier submits a withdrawal request | no transaction is created and the system shows the reason |
| d1 [withdraw balance] → d2 [yes] | the supplier has an eligible payout balance | the supplier submits a withdrawal request | one Withdrawal Transaction in state Created exists and its payout status is updated |
| d1 [green profile] | the supplier has verified UCO contributions | the supplier opens the Green Profile | the system displays the verified contribution and collection history |
| d1 [ESG report] | verified collection data exists | the supplier requests an ESG or environmental contribution report | the system generates and provides the report |

## 4. Object states used

| Object | States that appear in this diagram | Full state list (all diagrams) |
|---|---|---|
| Withdrawal Transaction | Created | Created, Processing, Completed, Failed |

This state name becomes an allowed value of `withdrawal_transaction.status` in the DBIZ3 data model. Keep the
spelling identical everywhere.

## 5. Screens touched

Filled in during Lesson 8, from the Screen List.

| Supplier action | Screen ID |
|---|---|
| Open the Loyalty and Payout section, submit withdrawal request, see reason | S-F05-01 (Loyalty and payout balance) |
| Choose Green Profile | S-F05-02 (Green Profile) |
| Request ESG report | S-F05-03 (ESG contribution reports) |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: is withdrawal (FR-05-02) in Release 1?] | No | W4 defers it to Release 2 (Should) and the payment itself is handled outside the application. It stays in this diagram so the F-05 flow is complete. |
| 2 | [NEEDS CLARIFICATION: does the tier upgrade bonus count towards the verified UCO volume?] | No | No. The bonus is added to the payout balance only. |
| 3 | [NEEDS CLARIFICATION: should the W1 criterion of FR-05-02 state the reason shown when a withdrawal is not eligible?] | No | It is treated as part of FR-05-02. Add it to the EARS wording when W1 is next revised. |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders (mermaid-cli 11.16.0) | Pass | | 28/11/2026 |
| Every decision has guards on all outgoing flows | Pass | | 28/11/2026 |
| No two flows enter an action directly (merge used) | Pass | | 28/11/2026 |
| Every system action that implements a requirement cites an FR | Pass (4 of 4) | | 28/11/2026 |
| Every FR of F-05 appears in the diagram | Pass (4 of 4) | | 28/11/2026 |
| Each FR appears exactly once, with the exact W1 name | Pass | | 28/11/2026 |
| Lane names match the Context Diagram | Pass | | 28/11/2026 |
