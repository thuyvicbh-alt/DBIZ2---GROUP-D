# Sequence Diagram: SD-UC-06 Check scheduled collection route and Confirm Delivery to supplier points

Lesson 4, analysis level: how the parts of the ReFeed System talk to each other when a driver/collector
works through an assigned collection route at one supplier point. The lifelines are the elements of the
[physical design](../architecture/architecture.md). The driver/collector's choices (which supplier point
next, whether UCO is ready, correcting the details) are shown in the activity diagram
[AD-UC-06](../activity/AD-UC-06.md), not here.

| Field | Value |
|---|---|
| Use case | UC-06 Check scheduled collection route and Confirm Delivery to supplier points |
| Version | v1 |
| FRs | FR-02-08, FR-02-09, FR-02-10, FR-02-11, FR-02-12, FR-02-13 |

```mermaid
sequenceDiagram
  actor C as UCO Collector / Driver
  participant A as Driver app
  participant P as Collection service
  participant D as ReFeed database
  C->>A: open assigned collection route
  A->>P: get assigned route(driver)
  P->>D: read route and pickup points in visiting order
  D-->>P: route with pickup points
  P-->>A: route and pickup points (FR-02-08)
  C->>A: select supplier point, confirm arrival
  A->>P: confirm arrival(pickup)
  P->>D: set pickup [Arrived]
  D-->>P: saved
  P-->>A: pickup Arrived (FR-02-09)
  alt UCO not ready (reported by the driver/collector on site)
    C->>A: report UCO not ready
    A->>P: cancel pickup(pickup)
    P->>D: set pickup [Cancelled], no collection record
    D-->>P: saved
    P-->>A: pickup Cancelled (FR-02-13)
  else UCO ready
    C->>A: enter actual quantity and optional photo and notes, submit
    A->>P: record collection(pickup, quantity, GPS, timestamp, photo, notes)
    alt required data missing or invalid (quantity, GPS or timestamp) (FR-02-10)
      P-->>A: rejected: validation feedback (FR-02-11)
    else data valid (FR-02-10)
      P->>D: save actual quantity, GPS, timestamp and evidence against pickup
      D-->>P: saved
      P-->>A: collection saved (FR-02-12)
    end
  end
  A-->>C: result
```

The outer `alt` follows what the driver/collector reports on site: whether UCO is ready is the
driver/collector's judgement, so the choice itself is the decision `d0` in AD-UC-06, but cancelling the
pickup is done by the system (`FR-02-13`), so the messages that carry it out are shown here. The inner
`alt` is an outcome decided inside the system (validation of the collection data). Correcting the details
and resubmitting, and moving on to the next supplier point, are the loops through `m1` and `d2` in the
same activity diagram.
