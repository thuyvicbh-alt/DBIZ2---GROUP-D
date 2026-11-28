# Sequence Diagram: SD-UC-12 Manage loyalty status and accumulated payout balance

Lesson 4, analysis level: how the parts of ReFeed talk to each other when a signed UCO transaction
updates the supplier's payout balance, verified volume and loyalty tier, and when the supplier later opens
the Loyalty & Payout section. The lifelines are the elements of the
[physical design](../architecture/architecture.md). The supplier's actions are shown in the activity
diagram [AD-UC-12](../activity/AD-UC-12.md).

| Field | Value |
|---|---|
| Use case | UC-12 Manage loyalty status and accumulated payout balance |
| Primary actor | Supplier |
| Version / status | v2.0 · Rebuilt on the current W1 |
| Inputs | [AD-UC-12](../activity/AD-UC-12.md) · [W1](../function-list/w1-function-list.md) FR-05-01 to FR-05-06 · [SD-UC-08](SD-UC-08.md) |
| FRs | FR-05-01, FR-05-02, FR-05-03, FR-05-04, FR-05-05, FR-05-06 (each appears once) |
| Preconditions | The supplier is signed in and the account is `Active`. A UCO transaction has been signed in UC-08. |
| Success end state | Payment and quantity are added, the tier is upgraded with a bonus when a higher threshold is reached, and the supplier sees the loyalty status and payout balance. |
| Other results | No tier upgrade when the verified volume stays below the next threshold. There is no withdrawal and no bank API. |

```mermaid
sequenceDiagram
  actor S as Supplier
  participant P as Supplier Web App
  participant L as Loyalty & Payout Service
  participant D as ReFeed database
  S->>P: sign the UCO transaction (UC-08)
  P->>L: transaction signed(locked base payment, loyalty bonus, actual quantity)
  L->>D: Add signed transaction payment to payout balance (FR-05-03)
  L->>D: Add signed transaction quantity to verified UCO volume (FR-05-04)
  D-->>L: updated balance and verified volume
  alt verified volume reaches a higher tier threshold (BR-05)
    L->>D: Upgrade loyalty tier (FR-05-05)
    L->>D: Award tier upgrade bonus (FR-05-06)
    D-->>L: new tier and bonus
  else below the next threshold
    L-->>L: tier unchanged
  end
  S->>P: open Loyalty and Payout section
  P->>L: get loyalty overview(supplier)
  L->>D: Retrieve Loyalty Tier & Payout Balance (FR-05-01)
  D-->>L: tier, progress to next tier, accumulated payout balance
  L-->>P: loyalty overview
  alt tier and history details requested
    P->>L: get history(supplier)
    L->>D: read verified volume and payment history
    D-->>L: base payment, loyalty bonus and tier upgrade bonus history
    L-->>P: details
    P-->>S: Display Loyalty Tier & History Details (FR-05-02)
  else overview only
    P-->>S: loyalty tier, progress and payout balance
  end
```

The `alt` fragments are outcomes decided inside the system or by what the supplier asks for, as in the
activity diagram. Updating the balance, volume and tier happens automatically after UC-08. There is no
withdrawal in this sequence because ReFeed has no bank API. Each FR is written on one message only, with
its exact W1 name, in the same place as the matching action in AD-UC-12.
