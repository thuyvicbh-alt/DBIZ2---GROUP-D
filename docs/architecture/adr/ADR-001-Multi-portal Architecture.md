# ADR-001: Multi-portal Architecture

> Worked example for teaching (ReFeed). Not a student submission and not a real product.

| Field | Value |
|---|---|
| Status | Accepted |
| Date | Lesson 4 |

**Context.** Suppliers need a simple way to request pickups from home or business locations. Drivers are on the road, needing reliable GPS, camera access for evidence, and sign-on-glass capabilities. Admins and warehouse staff work on desktop computers to monitor operations and manage large batches.

**Decision.** Build separate interfaces tailored to user environments: a responsive Web App for Suppliers, a Native or PWA (Progressive Web App) Field App for Drivers, and a Web Portal for Admins/Warehouse Staff.

**Consequences.** Drivers get the necessary hardware access (camera, GPS) and offline resilience. The user experience is optimized for each role. However, this requires maintaining three separate frontend codebases, demanding more development effort.
