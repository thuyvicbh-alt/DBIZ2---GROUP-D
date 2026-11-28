# Sequence Diagram: SD-UC-04 Manage and track an existing pickup status

> Analysis-level sequence diagram for ReFeed System (aligned with W0/W1 revision `08e751f` and Screen List). The UCO Supplier views real-time pickup status, tracking, and driver ETA, or submits an eligible request modification/cancellation.

| Field | Value |
|---|---|
| Use case | UC-04 Manage and track an existing pickup status |
| Feature | F-01 Supplier Onboarding & Pickup Management |
| Primary actor | Supplier (`UCO Supplier`) |
| Version | v2 (aligned with `08e751f`) |
| FRs | FR-01-15, FR-01-16, FR-01-17, FR-01-18, FR-01-19, FR-01-20 |
| Effective priority (W4) | Should (`FR-01-15`–`FR-01-19`); Could (`FR-01-20`) |
| Activity diagram map | AD-UC-04 (`s1` → `FR-01-15`, `s2` → `FR-01-20`, `s3` → `FR-01-16`, `s4` → `FR-01-17`, `s5` → `FR-01-18`, `c1` → `FR-01-19`) |
| Related screens | `S-F01-04` (My pickup requests), `S-F01-05` (Pickup request detail), `S-F01-03` (Pickup request form — Reschedule mode), Modal: *Cancel this pickup request?* |

```mermaid
sequenceDiagram
  actor S as Supplier
  participant A as ReFeed app
  participant P as Pickup service
  participant D as ReFeed database
  participant C as UCO Collector / Dispatch

  S->>A: select existing pickup request (S-F01-04 -> S-F01-05)
  A->>P: fetch pickup request status & validation rules (FR-01-15)
  P->>D: read pickup request, current status & modification rules
  D-->>P: request details, status & rules
  P-->>A: status & applicable modification rules (FR-01-15)

  alt action == view status (FR-01-20)
    A->>P: fetch real-time tracking & driver ETA (FR-01-20)
    P->>D: read tracking, scheduled time, driver/vehicle & last-update timestamp
    D-->>P: tracking, ETA & assigned driver/vehicle (when available)
    P-->>A: status, scheduled time, driver/vehicle & timestamp (FR-01-20)
    A-->>S: display pickup status, tracking & driver ETA (S-F01-05)
  else action == modify or cancel & driver already dispatched (FR-01-16)
    S->>A: submit reschedule (S-F01-03) or confirm cancel modal
    A->>P: request pickup change (request_id)
    P-->>A: modification-lock error: driver already dispatched (FR-01-16)
    A-->>S: display lock error & retain existing request/schedule (S-F01-05)
  else action == modify or cancel & status == Requested (FR-01-17..19)
    S->>A: submit reschedule (S-F01-03) or confirm cancel modal
    A->>P: update pickup request (request_id, new_timeline / cancel)
    P->>D: update pickup status & reschedule appointment timeline (FR-01-17)
    D-->>P: pickup updated OK
    par Central schedule & log update
      P->>D: update central pickup schedule & retain change in logs (FR-01-18)
      D-->>P: central schedule & logs updated OK
    and Parallel dispatch alert
      P-->>C: send dispatch-schedule update alert to UCO Collectors (FR-01-19)
    end
    P-->>A: pickup change & schedule update confirmed
    A-->>S: display update confirmation (S-F01-05)
  end
```
