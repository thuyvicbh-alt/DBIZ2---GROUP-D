# Sequence Diagram: SD-UC-04 Manage and track an existing pickup status

> Analysis level sequence diagram for ReFeed System. The UCO Supplier views real-time pickup status, scheduled collection details, or submits a request modification/cancellation.

| Field | Value |
|---|---|
| Use case | UC-04 Manage and track an existing pickup status |
| Version | v1 |
| FRs | FR-01-04, FR-01-05 |

```mermaid
sequenceDiagram
  actor S as UCO Supplier
  participant W as Web app
  participant C as Pickup service
  participant D as ReFeed database
  S->>W: select pickup request & action (view / modify / cancel)
  W->>C: get pickup request details (supplier, request_id)
  C->>D: read pickup request & assigned driver/vehicle info
  D-->>C: request details & status
  alt action == view status
    C-->>W: pickup status, scheduled time & driver info (FR-01-05)
  else modify or cancel & status locked (driver dispatched)
    W->>C: update request (request_id, new_details / cancel)
    C->>C: check eligibility (BR-01-PICKUP-CANCELLATION)
    C-->>W: refused: request ineligible for change (FR-01-04)
  else modify or cancel & status eligible (Requested)
    W->>C: update request (request_id, new_details / cancel)
    C->>D: update pickup status and appointment info
    D-->>C: updated OK
    C-->>W: request status updated (FR-01-04)
  end
  W-->>S: request status or update result
```