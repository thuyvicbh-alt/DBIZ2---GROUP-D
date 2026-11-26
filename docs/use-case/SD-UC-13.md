# Sequence Diagram: SD-UC-13 Withdraw accumulated payout balance

Lesson 4, analysis level: how the parts of ReFeed talk to each other when a supplier submits a withdrawal request for their accumulated payout balance. The lifelines are the elements of the physical design. The supplier's choice (to request a withdrawal) is shown in the activity diagram AD-UC-13, not here.

| Field | Value |
|---|---|
| Use case | UC-10 Receive and verify collected UCO at warehouse |
| Feature | F-04 Warehouse & Traceability |
| Version | v1.0 (from AD-UC-10 v0.1) |
| FRs | FR-04-01, FR-04-02, FR-04-03, FR-04-04, FR-04-05 |
| Business Rules | BR-04-01 |

```mermaid
sequenceDiagram
  actor W as Warehouse Staff
  participant P as Web/Mobile portal
  participant WService as Warehouse service
  participant D as ReFeed database

  W->>P: Submit received UCO quantity (Batch_ID, Received_Volume)
  P->>WService: receiveAndVerifyUCO(Batch_ID, Received_Volume)
  WService->>D: Record received quantity & fetch collection record by Batch_ID (FR-04-01)
  D-->>WService: Collection record (Collected_Volume, Signed_Amount, Tolerance_Threshold)
  
  WService->>WService: Compare received quantity with collection record & check tolerance (FR-04-02, BR-04-01)

  alt difference exceeds tolerance limit (BR-04-01)
    WService->>D: Log discrepancy alert for internal reconciliation (FR-04-03)
    D-->>WService: Discrepancy alert logged
    WService->>D: Keep signed transaction amount unchanged (FR-04-04) & update status to [Received] (FR-04-05)
    D-->>WService: Record updated
    WService-->>P: Success with discrepancy alert notification (FR-04-03, FR-04-05)
    P-->>W: Display receiving confirmation & discrepancy alert
  else difference within tolerance limit
    WService->>D: Keep signed transaction amount unchanged (FR-04-04) & update status to [Received] (FR-04-05)
    D-->>WService: Record updated
    WService-->>P: Success confirmation (FR-04-05)
    P-->>W: Display receiving verification success result
  end
```

The alt fragments here are outcomes decided inside the system (eligibility evaluation per BR-PAY-01 and BR-PAY-02), so they belong in a sequence diagram. The supplier's choice to submit a withdrawal request is shown in the activity diagram instead.
