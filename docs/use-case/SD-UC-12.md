# Sequence Diagram: SD-UC-12 Manage loyalty status and accumulated payout balance

Lesson 4, analysis level: how the parts of ReFeed talk to each other when a supplier opens the
Loyalty & Payout section, views the loyalty tier and history, and requests a withdrawal. The
lifelines are the elements of the [physical design](../architecture/architecture.md). The supplier's
choice (view tier and history, or request a withdrawal) is shown in the activity diagram
[AD-UC-12](../activity/AD-UC-12.md), not here.

| Field | Value |
|---|---|
| Use case | UC-12 Manage loyalty status and accumulated payout balance |
| Version | v1 (from AD-UC-12 v1.0) |
| FRs | FR-05-01, FR-05-02 |

```mermaid
sequenceDiagram
  actor S as Supplier
  participant P as Web/Mobile portal
  participant L as Loyalty and payout service
  participant D as ReFeed database
  S->>P: open Loyalty and Payout section
  P->>L: get loyalty overview(supplier)
  L->>D: read tier, verified volume, payout balance, transaction history
  D-->>L: loyalty and payout data
  L-->>P: tier, progress to next tier, balance, history (FR-05-01)
  P-->>S: loyalty tier and history details
  S->>P: request withdrawal(amount)
  P->>L: create withdrawal(supplier, amount)
  L->>D: read payout balance and bank account
  D-->>L: balance and bank account
  alt balance below minimum or bank account invalid
    L-->>P: rejected: insufficient balance or invalid bank account (FR-05-02, BR-05)
    P-->>S: current balance and rejection reason
  else withdrawal eligible
    par record the withdrawal
      L->>D: save withdrawal transaction [Created] (FR-05-02)
    and update the balance
      L->>D: update payout status and balance [Deducted] (FR-05-02)
    end
    L-->>P: withdrawal result, updated payout status and balance
    P-->>S: withdrawal result and updated balance
  end
```

The `alt` fragment here is an outcome decided inside the system, so it belongs in a sequence
diagram. The `par` fragment is the fork and join of the activity diagram: the two saves run in
parallel and the result is shown after both finish. The supplier's choice between viewing and
withdrawing would be a decision in the activity diagram instead.

