# ADR-001: Multi-portal Architecture

> Worked example for teaching (ReFeed). Not a student submission and not a real product.

| Field | Value |
|---|---|
| Status | Accepted |
| Date | Lesson 4 |

**Context.** Suppliers need a simple way to request pickups. Drivers and warehouse staff handle the end-to-end physical flow of UCO (collecting and receiving) and need continuous handover data. Admin staff require a desktop environment to monitor overall operations and compliance.

**Decision.** Build three separate interfaces: a responsive Web App for Suppliers, a unified Operations App for field drivers and warehouse staff, and a Web Portal for Admins.

**Consequences.** Grouping drivers and warehouse staff into one Operations App streamlines the handover flow and reduces the number of apps to maintain. The user experience remains optimized for each specific role's environment.

**Consequences.** Drivers get the necessary hardware access (camera, GPS) and offline resilience. The user experience is optimized for each role. However, this requires maintaining three separate frontend codebases, demanding more development effort.
