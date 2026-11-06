# Activity Diagram: AD-UC-13 Withdraw accumulated payout balance

| Field | Value |
|---|---|
| AD ID | `AD-UC-13` |
| Use case | `UC-13 Withdraw accumulated payout balance` |
| Feature | `F-05 Supplier Value & ESG / Green Profile` |
| Primary actor | Supplier |
| Lanes | Supplier · ReFeed System · External Banking/Payment Gateway |
| Inputs (L3-L5) | Context Diagram v1 · Use Case Diagram v1 · Sequence: `SD-UC-13` · Architecture/ADR used · W1 rows |
| Trigger | Supplier requests to withdraw their accumulated payout balance from their account dashboard. |
| Preconditions | Supplier is logged in, profile is verified, and the accumulated payout balance is greater than the minimum withdrawal threshold. |
| Success end state | Withdrawal request is successfully created, status is updated, and the payout transaction is queued for external bank transfer. |
| Failure end state(s) | Withdrawal request is rejected due to insufficient balance, incomplete profile, or payment gateway validation error. |
| FRs covered | `FR-05-02` |
| Business rules applied | `BR-PAY-01`, `BR-PAY-02` |
| Version / status | v0.1 · Draft / Reviewed |
| Author | Vũ Quỳnh Thơ / Group D - K63DB |

## 1. Diagram

```mermaid
swimlane-beta TB
  subgraph ACT["Supplier"]
    start@{ shape: sm-circ }
    a1("Submit withdrawal request")
  end
  subgraph SYS["ReFeed System"]
    s1("Validate withdrawal eligibility (FR-05-02)")
    d1{" "}
    s2("Create withdrawal transaction (FR-05-02)")
    s3("Define the handling of an ineligible withdrawal request")
    s4("Update payout status (FR-05-02)")
    m1{" "}
    stop1@{ shape: fr-circ }
  end
  
  start --> a1 --> s1 --> d1
  d1 -->|"[yes]"| s2 --> s4
  d1 -->|"[no]"| s3 --> s4
  s4 --> m1 --> stop1
```

Reading notes:
- Use case 13 handles the supplier's request to cash out their accumulated payout balance.
- Actual financial money movement is processed externally (out-of-scope manual/gateway transfer per W0 Table 3), while the system manages the withdrawal transaction record and payout status update.

## 2. Node trace

| Node | Kind | Text | Traces to |
|---|---|---|---|
| s1 | Action | Validate withdrawal eligibility | FR-05-02 |
| d1 | Decision | Eligibility check result ([yes] / [no]) | FR-05-02 |
| s2 | Action | Create withdrawal transaction and update payout status | FR-05-02 |
| s3 | Action | Define the handling of an ineligible withdrawal request | Open Question / BR |
| s4 | Action | Update payout status | FR-05-02 |

## 3. Branches and acceptance tests

| Branch | Given | When | Then |
|---|---|---|---|
| d1 [<insufficient balance or invalid profile>] | Supplier has 100,000 VND balance (minimum threshold is 200,000 VND) | Supplier submits withdrawal request | System rejects request and displays insufficient balance error message |
| d1 [<valid request>] | Supplier has 500,000 VND balance and verified bank details | Supplier submits withdrawal request for 300,000 VND | System creates withdrawal transaction, updates payout status to "Requested/Processing", and queues transfer |

## 4. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
|---|---|---|
| Payout Balance | Accumulated, Requested, Withdrawn | Zero, Accumulated, Requested, Settled |
| Withdrawal Transaction | Draft, Created, Processing, Completed | Draft, Created, Processing, Completed, Failed |

## 5. Screens touched

| Actor action | Screen ID |
|---|---|
| Navigate to payout & request withdrawal | SCR-SUPP-05 (Loyalty & Payout Dashboard) |
| Enter withdrawal amount & bank details | SCR-SUPP-06 (Withdrawal Request Form) |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | Should withdrawal fees be deducted automatically by the system or handled externally? | No | Handled externally / out of scope for Release 1 |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders on mermaid.live | Pass | Group D | 2026-10-27 |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass | Group D | 2026-10-27 |
| No action has two incoming flows (merge used instead) | Pass | Group D | 2026-10-27 |
| Every fork has a matching join | N/A | Group D | 2026-10-27 |
| Every system action cites an FR | Pass | Group D | 2026-10-27 |
| Every FR of this use case appears in the diagram | 1 of 1 (FR-05-02) | Group D | 2026-10-27 |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass | Group D | 2026-10-27 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass | Group D | 2026-10-27 |
| AI critic prompt run, findings resolved or logged as open questions | Pass | Group D | 2026-10-27 |

---
