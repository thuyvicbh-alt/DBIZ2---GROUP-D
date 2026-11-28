# Activity Diagram: AD-UC-12 Supplier Value & ESG / Green Profile

| Field | Value |
|---|---|
| AD ID | AD-UC-12 |
| Use case | UC-12 Manage loyalty status and accumulated payout balance, followed through the F-05 flow (Green Profile, ESG report) |
| Feature | F-05 Supplier Value & ESG / Green Profile |
| Primary actor | Supplier |
| Lanes | Supplier · ReFeed system |
| Inputs (L3-L5) | Context Diagram v2 (Supplier) · Use Case Diagram v2 · Sequence: SD-UC-12 · Architecture v2 · W1 rows FR-05-01, FR-05-03, FR-05-04 |
| Trigger | (a) A UCO transaction is signed (UC-08), or (b) the supplier opens the Loyalty & Payout section on the portal |
| Preconditions | The supplier is signed in. The supplier account is active (`Active`). |
| Success end state | The accumulated payout balance and payout status are up to date and visible to the supplier. The supplier can open the Green Profile or obtain an ESG report. |
| Failure end state | None defined. The system does not execute payments or withdrawals; the supplier only views the balance and whether the payment has been received. |
| FRs covered | FR-05-01 View loyalty status and accumulated payout balance · FR-05-03 View Green Profile and UCO contribution history · FR-05-04 Access ESG / environmental contribution reports |
| Business rules applied | BR-05 (Loyalty Tier Thresholds) |
| Version / status | v1.4 · No withdrawal (no bank API); each FR appears once · FR-05-02 pending W1 update |
| Author | Nguyen Thuy Vi - 2411560050 |

## 1. Diagram

Notation follows the DBIZ2 activity profile (`mermaid-activity-patterns.md`). Rendered with
`swimlane-beta`, which needs Mermaid 11.16 or later.

```mermaid
swimlane-beta TB
  subgraph SUP["Supplier"]
    start@{ shape: sm-circ }
    a1("Open the Loyalty and Payout section")
    a2("Open the Green Profile")
    a3("Request ESG report")
    stop1@{ shape: fr-circ }
  end
  subgraph SYS["ReFeed system"]
    signed@{ shape: sm-circ }
    u1("Add signed payment, bonus and quantity to balance and verified volume")
    d0{"Higher tier<br>threshold reached?"}
    u2("Upgrade loyalty tier and award tier upgrade bonus")
    m0{" "}
    stop2@{ shape: fr-circ }
    s1("View loyalty status and accumulated payout balance (FR-05-01)")
    d1{"Next<br>action?"}
    s2("View Green Profile and UCO contribution history (FR-05-03)")
    s3("Access ESG / environmental contribution reports (FR-05-04)")
  end
  signed --> u1 --> d0
  d0 -->|"[yes]"| u2 --> m0
  d0 -->|"[no]"| m0
  m0 --> stop2
  start --> a1 --> s1 --> d1
  d1 -->|"[done]"| stop1
  d1 -->|"[green profile]"| a2 --> s2 --> stop1
  d1 -->|"[ESG report]"| a3 --> s3 --> stop1
```

Reading notes:

- Every action in the system lane that implements a requirement carries one FR with its exact W1 name, and each FR appears once: `s1` FR-05-01, `s2` FR-05-03, `s3` FR-05-04.
- **There is no withdrawal.** ReFeed cannot call a bank API, so the system does not move money. Payments are made outside the application, and the supplier only sees the accumulated balance and whether the payment has been received (the payout status in the history shown by `s1`).
- The supplier actions `a1` to `a3`, the update actions `u1` and `u2` carry no FR. `u1` and `u2` are the update half of FR-05-01; the view half is `s1`.
- The diagram has two start points. The system-side start is a signed transaction handed over by UC-08: the system adds the payment, bonus and quantity, and upgrades the tier with a bonus when a higher threshold is reached. The supplier-side start is opening the Loyalty and Payout section.
- `m0` is a **merge** node: the upgrade flow and the no-upgrade flow both enter it before the update flow ends.
- Three flows enter `stop1`. This is allowed: a final node is not an action.

## 2. Node trace

Every action in the system lane cites a requirement. Every decision cites the rule or requirement
that decides it.

| Node | Kind | Text | Traces to |
|---|---|---|---|
| u1 | Action | Add signed payment, bonus and quantity to balance and verified volume | FR-05-01 (update half) |
| d0 | Decision | Higher tier threshold reached? | FR-05-01, BR-05 |
| u2 | Action | Upgrade loyalty tier and award tier upgrade bonus | FR-05-01 (update half), BR-05 |
| s1 | Action | View loyalty status and accumulated payout balance | FR-05-01 |
| d1 | Decision | Next action? | FR-05-03, FR-05-04 |
| s2 | Action | View Green Profile and UCO contribution history | FR-05-03 |
| s3 | Action | Access ESG / environmental contribution reports | FR-05-04 |

## 3. Branches and acceptance tests

One row per outgoing branch of every decision. DBIZ3 turns each row into at least one test.

| Branch | Given | When | Then |
|---|---|---|---|
| d0 [yes] | a UCO transaction is signed and the verified UCO volume reaches a higher tier threshold | the system processes the signed transaction | balance and volume are updated, the tier is upgraded and the tier upgrade bonus is awarded |
| d0 [no] | a UCO transaction is signed and the volume stays below the next threshold | the system processes the signed transaction | balance and volume are updated and the tier stays the same |
| d1 [done] | the supplier has seen the loyalty status | they leave the section | the activity ends and nothing is changed |
| d1 [green profile] | the supplier has verified UCO contributions | the supplier opens the Green Profile | the system displays the verified contribution and collection history |
| d1 [ESG report] | verified collection data exists | the supplier requests an ESG or environmental contribution report | the system generates and provides the report |

## 4. Object states used

No object node appears in this diagram. The payout status shown to the supplier (for example whether a payment has been received) is read data, not a state this activity changes.

## 5. Screens touched

Filled in during Lesson 8, from the Screen List.

| Supplier action | Screen ID |
|---|---|
| Open the Loyalty and Payout section | S-F05-01 (Loyalty and payout balance) |
| Open the Green Profile | S-F05-02 (Green Profile) |
| Request ESG report | S-F05-03 (ESG contribution reports) |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: W1 still lists FR-05-02 Withdraw accumulated payout balance. What replaces it?] | Yes | FR-05-02 is left out of this diagram. W1 should either remove it or rename it to a view of the payout status (payment received or not). Update W1, W4 and W5 together so the cost stays consistent. |
| 2 | [NEEDS CLARIFICATION: how does the system learn that a payment has been received?] | No | Staff record it manually after the external bank transfer; the supplier sees the status in the history. |
| 3 | [NEEDS CLARIFICATION: does the tier upgrade bonus count towards the verified UCO volume?] | No | No. The bonus is added to the payout balance only. |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders (mermaid-cli 11.16.0) | Pass | | 28/11/2026 |
| Every decision has guards on all outgoing flows | Pass | | 28/11/2026 |
| No two flows enter an action directly (merge used) | Pass | | 28/11/2026 |
| Every system action that implements a requirement cites an FR | Pass (3 of 3) | | 28/11/2026 |
| Every FR of F-05 still in scope appears in the diagram | Pass (FR-05-01, FR-05-03, FR-05-04; FR-05-02 pending W1) | | 28/11/2026 |
| Each FR appears exactly once, with the exact W1 name | Pass | | 28/11/2026 |
| Lane names match the Context Diagram | Pass | | 28/11/2026 |
