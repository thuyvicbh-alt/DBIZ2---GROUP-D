# Sequence Diagram: SD-UC-13 Withdraw accumulated payout balance

Lesson 4, analysis level: how the parts of ReFeed talk to each other when a supplier submits a withdrawal request for their accumulated payout balance. The lifelines are the elements of the physical design. The supplier's choice (to request a withdrawal) is shown in the activity diagram AD-UC-13, not here.

| Field | Value |
|---|---|
| Use case | UC-13 Withdraw accumulated payout balance |
| Feature | F-05 Supplier Value & ESG / Green Profile |
| Version | v1.0 (from AD-UC-13 v0.1) |
| FRs | FR-05-02 |
| Business Rules | BR-PAY-01, BR-PAY-02 |

```mermaid
sequenceDiagram
  actor S as Supplier
  participant P as Web/Mobile portal
  participant PService as Payout service
  participant D as ReFeed database
  participant G as External Payment Gateway / Bank

  S->>P: Submit withdrawal request (Amount, Bank_Account_Details)
  P->>PService: submitWithdrawalRequest(Supplier_ID, Amount, Bank_Account_Details)
  PService->>D: Fetch supplier balance & verification status (FR-05-02)
  D-->>PService: Supplier profile (Accumulated_Balance, Verification_Status)

  PService->>PService: Validate eligibility (Amount >= Threshold & Status == Verified) (BR-PAY-01, BR-PAY-02)

  alt ineligible request (insufficient balance or unverified profile)
    PService->>D: Log rejected withdrawal attempt (FR-05-02)
    D-->>PService: Audit record saved
    PService-->>P: Rejection response with error details (BR-PAY-01, BR-PAY-02)
    P-->>S: Display error message (e.g., Insufficient balance / Verification required)
  else valid withdrawal request
    PService->>D: Create Withdrawal Transaction [Created] (FR-05-02)
    D-->>PService: Transaction_ID
    PService->>D: Update Payout Status to [Requested/Processing] & adjust available balance (FR-05-02)
    D-->>PService: Status updated
    PService->>G: Queue payout transfer request (out-of-scope / manual gateway transfer)
    G-->>PService: Queue confirmation received
    PService-->>P: Success response (FR-05-02)
    P-->>S: Display withdrawal request submitted successfully
  end
```

The alt fragments here are outcomes decided inside the system (eligibility evaluation per BR-PAY-01 and BR-PAY-02), so they belong in a sequence diagram. The supplier's choice to submit a withdrawal request is shown in the activity diagram instead.
