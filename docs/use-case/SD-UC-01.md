# Sequence Diagram: SD-UC-01 Register / Login as a UCO supplier


Lesson 4, analysis level: how the parts of ReFeed talk to each other when a supplier submits
registration data or credentials. The lifelines are the elements of the
[physical design](../architecture/architecture.md). The supplier's choice (to submit credentials or
registration data) is shown in the activity diagram [AD-UC-01](../activity/AD-UC-01.md), not here.

| Field | Value |
|---|---|
| Use case | UC-01 Register / Login as a UCO supplier |
| Version | v1 (from AD-UC-01 v1.0) |
| FRs | FR-01-01 |

```mermaid
sequenceDiagram
  actor S as Supplier
  participant P as Web/Mobile portal
  participant A as Account service
  participant D as ReFeed database
  S->>P: submit credentials or registration data
  P->>A: authenticate or register(credentials, compliance data)
  A->>D: read supplier by Tax ID and email (BR-01)
  D-->>A: matching supplier or none
  alt data invalid or credentials wrong
    A-->>P: rejected: invalid data (FR-01-01)
    P-->>S: error message, please retry
  else new registration with valid data
    A->>D: save supplier with unique Supplier_ID [Active] (FR-01-01, BR-01)
    D-->>A: Supplier_ID
    A-->>P: account created, session token issued
    P-->>S: dashboard and authentication result
  else supplier already exists
    A-->>P: rejected: account already exists (FR-01-01)
    P-->>S: account already exists, redirect to login
  end
```

The `alt` fragments here are outcomes decided inside the system, so they belong in a sequence
diagram. The supplier's choice between registering and logging in would be a decision in the
activity diagram instead.
