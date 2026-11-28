# Activity Diagram: AD-UC-12 Manage loyalty status and accumulated payout balance

| Field | Value |
|---|---|
| AD ID | AD-UC-12 |
| Use case | UC-12 Manage loyalty status and accumulated payout balance |
| Feature | F-05 Supplier Value & ESG / Green Profile |
| Primary actor | Supplier |
| Lanes | Supplier · ReFeed system |
| Inputs (L3-L5) | Context Diagram v2 (Supplier) · Use Case Diagram v2 · Sequence: SD-UC-12 · Architecture v2 · W1 rows FR-05-01 to FR-05-06 |
| Trigger | The supplier signs a UCO transaction (UC-08) |
| Preconditions | The supplier is signed in. The supplier account is active (`Active`). |
| Success end state | The signed payment and quantity are added to the payout balance and verified volume, the tier is upgraded with a bonus when a higher threshold is reached, and the supplier sees the loyalty status and payout balance. |
| Failure end state | None defined. The system does not move money; payments are made outside the application. |
| FRs covered | FR-05-01 Retrieve Loyalty Tier & Payout Balance · FR-05-02 Display Loyalty Tier & History Details · FR-05-03 Add signed transaction payment to payout balance · FR-05-04 Add signed transaction quantity to verified UCO volume · FR-05-05 Upgrade loyalty tier · FR-05-06 Award tier upgrade bonus |
| Business rules applied | BR-05 (Loyalty Tier Thresholds) |
| Version / status | v2.0 · Rebuilt on the current W1 function list, each FR appears once |
| Author | Nguyen Thuy Vi - 2411560050 |

## 1. Diagram

Notation follows the DBIZ2 activity profile (`mermaid-activity-patterns.md`). Rendered with
`swimlane-beta`, which needs Mermaid 11.16 or later.

```mermaid
swimlane-beta TB
  subgraph SUP["Supplier"]
    start@{ shape: sm-circ }
    a1("Sign the UCO transaction")
    a2("Open the Loyalty and Payout section")
    stop1@{ shape: fr-circ }
  end
  subgraph SYS["ReFeed system"]
    s1("Add signed transaction payment to payout balance (FR-05-03)")
    s2("Add signed transaction quantity to verified UCO volume (FR-05-04)")
    d1{"Higher tier<br>threshold reached?"}
    s3("Upgrade loyalty tier (FR-05-05)")
    s4("Award tier upgrade bonus (FR-05-06)")
    m1{" "}
    s5("Retrieve Loyalty Tier & Payout Balance (FR-05-01)")
    d2{"Tier and history<br>details requested?"}
    s6("Display Loyalty Tier & History Details (FR-05-02)")
  end
  start --> a1 --> s1 --> s2 --> d1
  d1 -->|"[yes]"| s3 --> s4 --> m1
  d1 -->|"[no]"| m1
  m1 --> a2 --> s5 --> d2
  d2 -->|"[yes]"| s6 --> stop1
  d2 -->|"[no]"| stop1
```

Reading notes:

- Every action in the system lane carries one FR with its exact W1 name, and each FR appears once: `s5` FR-05-01, `s6` FR-05-02, `s1` FR-05-03, `s2` FR-05-04, `s3` FR-05-05, `s4` FR-05-06.
- The diagram has one start point. The supplier signs the transaction (UC-08), the system updates balance, volume and tier, and the supplier later opens the Loyalty and Payout section to see the result.
- Supplier actions carry no FR. Decisions follow the LabDesk example: the label has no tag, and section 2 gives the FR that decides each branch.
- `m1` is a **merge** node: the upgrade flow and the no-upgrade flow meet before the supplier opens the section.
- **There is no withdrawal.** ReFeed cannot call a bank API. The supplier only views the accumulated balance and the payment history.
- Two flows enter `stop1`. This is allowed: a final node is not an action.

## 2. Node trace

Every action in the system lane cites a requirement. Every decision cites the rule or requirement
that decides it.

| Node | Kind | Text | Traces to |
|---|---|---|---|
| s1 | Action | Add signed transaction payment to payout balance | FR-05-03 |
| s2 | Action | Add signed transaction quantity to verified UCO volume | FR-05-04 |
| d1 | Decision | Higher tier threshold reached? | FR-05-05, BR-05 |
| s3 | Action | Upgrade loyalty tier | FR-05-05, BR-05 |
| s4 | Action | Award tier upgrade bonus | FR-05-06 |
| s5 | Action | Retrieve Loyalty Tier & Payout Balance | FR-05-01 |
| d2 | Decision | Tier and history details requested? | FR-05-02 |
| s6 | Action | Display Loyalty Tier & History Details | FR-05-02 |

## 3. Branches and acceptance tests

One row per outgoing branch of every decision. DBIZ3 turns each row into at least one test.

| Branch | Given | When | Then |
|---|---|---|---|
| d1 [yes] | a UCO transaction is signed and the verified UCO volume reaches a higher tier threshold | the system processes the signed transaction | payment and quantity are added, the tier is upgraded and the tier upgrade bonus is awarded |
| d1 [no] | a UCO transaction is signed and the volume stays below the next threshold | the system processes the signed transaction | payment and quantity are added and the tier stays the same |
| d2 [yes] | an Active supplier has opened the Loyalty and Payout section | they choose View tier and history details | the system displays the current tier, verified UCO volume, progress to the next tier and the base-payment, loyalty-bonus and tier-upgrade-bonus history |
| d2 [no] | an Active supplier has opened the Loyalty and Payout section | they do not ask for details | the system shows the loyalty tier, progress to the next tier and accumulated payout balance |

## 4. Object states used

No object node appears in this diagram.

## 5. Screens touched

Filled in during Lesson 8, from the Screen List.

| Supplier action | Screen ID |
|---|---|
| Open the Loyalty and Payout section, see tier, balance and history | S-F05-01 (Loyalty and payout balance) |

The signing action `a1` is on S-F03-01 (UC-08). The balance, volume and tier updates are system-triggered and have no screen.

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: how does the supplier know a payment has been received?] | No | Payments are made outside the application and the supplier sees the history on S-F05-01; no bank API is used. |
| 2 | [NEEDS CLARIFICATION: does the tier upgrade bonus count towards the verified UCO volume?] | No | No. The bonus is added to the payout balance only. |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders (mermaid-cli 11.16.0) | Pass | | 28/11/2026 |
| Every decision has guards on all outgoing flows | Pass | | 28/11/2026 |
| Every system action cites an FR | Pass (6 of 6) | | 28/11/2026 |
| Every FR of UC-12 appears in the diagram | Pass (FR-05-01 to FR-05-06) | | 28/11/2026 |
| Each FR appears exactly once, with the exact W1 name | Pass | | 28/11/2026 |
| Lane names match the Context Diagram | Pass | | 28/11/2026 |
