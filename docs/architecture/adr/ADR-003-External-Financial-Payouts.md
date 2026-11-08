# ADR-003: External Financial Payouts

> Worked example for teaching (ReFeed). Not a student submission and not a real product.

| Field | Value |
|---|---|
| Status | Accepted |
| Date | Lesson 4 |

**Context.** Suppliers accumulate financial value based on the volume of UCO they provide and their loyalty tier. Processing real-time cash transfers to bank accounts requires complex integrations with payment gateways, strict security audits, and financial regulatory compliance.

**Decision.** Actual financial payouts (bank transfers) will be handled externally by the finance team. The ReFeed system will only calculate, update, and display the "accumulated balance" for each supplier.

**Consequences.** Significantly reduces technical complexity, server costs, and legal risks for Release 1. Direct in-app payments are recorded as Won't-have in W0 Table 3. This requires manual operational effort from the business team to execute withdrawal requests.
