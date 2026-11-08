# ADR-002: Google Maps API Integration

> Worked example for teaching (ReFeed). Not a student submission and not a real product.

| Field | Value |
|---|---|
| Status | Accepted |
| Date | Lesson 4 |

**Context.** Operations staff must plan efficient collection routes across multiple supplier points. Drivers need accurate navigation. The system must also validate addresses and capture precise GPS coordinates during UCO handover for compliance and traceability.

**Decision.** Integrate the external Google Maps API to handle routing, distance calculation, map display, and address validation, rather than building custom mapping and routing engines.

**Consequences.** Dramatically reduces backend development time and ensures high reliability for location services. It introduces an external operational cost (API usage fees) that scales with the volume of collections, and creates a dependency on a third-party service.
