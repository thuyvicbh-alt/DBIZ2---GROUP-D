# Sequence Diagram: SD-UC-10 Receive and verify collected UCO at warehouse

Lesson 4, analysis level: how the parts of ReFeed talk to each other when warehouse staff record and verify received UCO quantity against collection records. The lifelines are the elements of the physical design. The warehouse staff's choice (to initiate receiving and enter quantities) is shown in the activity diagram AD-UC-10, not here.

| Field | Value |
|---|---|
| Use case | UC-10 Receive and verify collected UCO at warehouse |
| Feature | F-04 Warehouse & Traceability |
| Version | v1.0 (from AD-UC-10 v0.1) |
| FRs | FR-04-01 |
| Business Rules | BR-04-01 |

```mermaid
sequenceDiagram
  actor W as Warehouse Staff
  participant P as Web/Mobile portal
  participant WService as Warehouse service
  participant D as ReFeed database

  W->>P: Submit received UCO quantity (Batch_ID, Received_Volume)
  P->>WService: receiveAndVerifyUCO(Batch_ID, Received_Volume)
  WService->>D: Fetch collection record by Batch_ID (FR-04-01)
  D-->>WService: Collection record (Collected_Volume, Signed_Amount, Tolerance_Threshold)
  
  WService->>WService: Calculate volume difference & check tolerance (BR-04-01)

  alt difference exceeds tolerance limit (BR-04-01)
    WService->>D: Log discrepancy alert for internal reconciliation (FR-04-01)
    D-->>WService: Discrepancy alert logged
    WService->>D: Preserve signed transaction amount unchanged & update status to [Received] (FR-04-01)
    D-->>WService: Record updated
    WService-->>P: Success with discrepancy alert notification (FR-04-01)
    P-->>W: Display receiving confirmation & discrepancy alert
  else difference within tolerance limit
    WService->>D: Preserve signed transaction amount unchanged & update status to [Received] (FR-04-01)
    D-->>WService: Record updated
    WService-->>P: Success confirmation (FR-04-01)
    P-->>W: Display receiving verification success result
  end
```

The alt fragments here are outcomes decided inside the system (tolerance evaluation per BR-04-01), so they belong in a sequence diagram. The actor's initial action of submitting received batch data is shown in the activity diagram instead.
