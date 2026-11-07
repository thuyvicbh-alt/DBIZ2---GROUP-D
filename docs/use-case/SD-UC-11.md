# Sequence Diagram: SD-UC-11 Manage UCO batch and mass-balance records

> Analysis level sequence diagram for ReFeed System. The Warehouse Staff aggregates verified collections into UCO batches, validates mass-balance metrics, updates inventory, and syncs batch records with Eco Oil System.

| Field | Value |
|---|---|
| Use case | UC-11 Manage UCO batch and mass-balance records |
| Version | v1 |
| FRs | FR-04-02, FR-04-03 |

```mermaid
sequenceDiagram
  actor WS as Warehouse Staff
  participant W as Web app
  participant B as Batch & Traceability service
  participant D as ReFeed database
  participant E as Eco Oil System
  WS->>W: select verified collections & enter batch parameters
  W->>B: create UCO batch (collections, parameters)
  B->>D: read verified collection records & quantities
  D-->>B: collection records & QA metrics
  B->>B: validate parameters & compute mass-balance (BR-04-MASS-BALANCE)
  alt batch parameters invalid
    B-->>W: refused: invalid parameters, prompt correction (FR-04-02)
  else batch parameters valid
    B->>D: create UCO batch record [Created] & link collections
    D-->>B: batch created OK
    B->>D: update warehouse inventory balance (FR-04-02)
    B->>E: transmit batch traceability & mass-balance record (FR-04-02)
    E-->>B: sync ACK
    B-->>W: batch created & mass-balance updated (FR-04-02)
  end
  W-->>WS: batch operation result summary
```