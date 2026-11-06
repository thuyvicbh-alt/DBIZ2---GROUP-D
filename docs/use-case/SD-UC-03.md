# Sequence Diagram: SD-UC-03 Request and schedule a UCO pickup

Lesson 4, analysis level: how the parts of the ReFeed System talk to each other when a supplier
requests a UCO pickup. The lifelines are the elements of the [physical design](../architecture/architecture.md).
The supplier's own steps (entering the request details, correcting them, updating the profile)
are shown in the activity diagram [AD-UC-03](../activity/AD-UC-03.md), not here.

| Field | Value |
|---|---|
| Use case | UC-03 Request and schedule a UCO pickup |
| Version | v1 |
| FRs | FR-01-03 |

```mermaid
sequenceDiagram
  actor S as Supplier
  participant A as Supplier app
  participant P as Pickup service
  participant D as ReFeed database
  S->>A: enter location, estimated volume, preferred time window and notes, submit
  A->>P: create pickup request(supplier, location, volume, time window, notes)
  P->>D: read supplier profile (GPS, compliance data)
  D-->>P: supplier profile
  alt profile incomplete (GPS or compliance data missing)
    P-->>A: rejected: update the supplier profile first (FR-01-03)
  else request details missing or invalid
    P-->>A: rejected: validation errors (FR-01-03)
  else profile complete and details valid
    P->>D: save pickup request [Requested] with the time window, linked to Supplier_ID
    D-->>P: Pickup_Request_ID
    P-->>A: request created, Pickup_Request_ID (FR-01-03)
  end
  A-->>S: result
```

The `alt` fragments here are outcomes decided inside the system (profile check, validation), so
they belong in a sequence diagram. The supplier correcting the details and resubmitting is a loop
in the activity diagram AD-UC-03 instead.
