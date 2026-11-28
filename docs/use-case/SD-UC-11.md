# Sequence Diagram: SD-UC-11 Manage UCO batch and mass-balance records

> Analysis-level sequence diagram for ReFeed System (aligned with W0/W1 revision `08e751f` and Screen List). Warehouse Staff aggregates verified UCO collections into a batch, validates parameters, computes mass-balance metrics, maintains internal batch balances, and provides traceability records to Eco Oil System in parallel.

| Field | Value |
|---|---|
| Use case | UC-11 Manage UCO batch and mass-balance records |
| Feature | F-04 Warehouse & Traceability |
| Primary actor | Warehouse Staff (`Warehouse Staff` for `FR-04-06`–`FR-04-10`; `ReFeed Staff` for deferred `FR-04-11`) |
| Version | v2 (aligned with `08e751f`) |


```mermaid
sequenceDiagram
  actor WS as Warehouse Staff
  participant A as ReFeed app
  participant B as Batch & Traceability service
  participant D as ReFeed database
  participant E as Eco Oil System

  WS->>A: select verified collections & enter batch parameters (S-F04-02)
  A->>B: submit batch creation request (collection_ids, batch_params)
  B->>D: read selected collection records & verification status
  D-->>B: collection records & received/verified quantities
  B->>B: validate collection inputs & batch parameters (FR-04-06)

  alt collection inputs or batch parameters invalid (FR-04-07)
    B-->>A: invalid-parameter feedback & prompt correction (FR-04-07)
    A-->>WS: display validation error & request correction (S-F04-02)
  else batch inputs & parameters valid (FR-04-08..10)
    B->>D: create UCO batch [Created], link collections & store mass-balance metrics (FR-04-08)
    D-->>B: batch created & mass-balance metrics saved OK
    par Internal batch balance maintenance
      B->>D: maintain linked collection inputs & update batch balance (FR-04-09)
      D-->>B: batch balance updated OK
    and External traceability export (ReFeed boundary)
      B->>E: provide batch traceability & mass-balance record (FR-04-10)
    end
    B-->>A: batch creation & updated mass-balance summary (FR-04-09)
    A-->>WS: display batch creation & mass-balance summary (S-F04-02)
  end
```
