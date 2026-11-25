# Sequence Diagram: SD-UC-07 Input Handover Information

Lesson 4, analysis level. The driver submits the actual handover information and ReFeed prepares the draft agreement. Supplier review and signing continue in [SD-UC-08](SD-UC-08.md), matching the boundary of [AD-UC-07](../activity/AD-UC-07.md).

| Field | Value |
|---|---|
| Use case | UC-07 Input Handover Information |
| Version / status | v2 · Draft; signing separated into UC-08 on 2026-11-25 |
| Current W1 functions | FR-02-14–FR-02-18 |
| Legacy family | Original FR-02-03; see W1's legacy crosswalk |
| Lifelines | Driver; Field Staff App; Backend; Database, retained from the existing handover sequence |
| Verification | Rendered with mermaid-cli 11.16.0 / Mermaid 11.17.2 on 2026-11-25 |

```mermaid
sequenceDiagram
  actor Dr as Driver
  participant App as Field Staff App
  participant B as Backend
  participant D as Database

  Dr->>App: Input Actual_Quantity and handover time
  opt Capture evidence for UCO condition
    Dr->>App: Capture Photo_Evidence and Notes
  end
  App->>App: Auto-capture GPS and Timestamp (OS Level)
  Dr->>App: Click Generate E-Agreement
  App->>B: prepareTransaction(Pickup_ID, Quantity, GPS, Timestamp, Photo)
  B->>B: Validate actual quantity, handover time and evidence (FR-02-14)
  alt Handover information invalid
    B-->>App: Reject and prompt for correction (FR-02-18)
    App-->>Dr: Correct invalid handover information
  else Handover information valid
    B->>D: Store handover record against assigned pickup (FR-02-15)
    D-->>B: Saved handover reference
    B->>D: fetchSupplierLoyaltyData(Supplier_ID)
    D-->>B: Return Base_Price and Current_Bonus_Rate
    B->>B: Effective_Price = Base_Price + Current_Bonus_Rate
    B->>B: Calculate transaction amount (FR-02-16)
    Note over B,D: Total_Payment = Actual_Quantity times Effective_Price. Retain Base_Payment and Loyalty_Bonus separately.
    B->>B: Generate draft agreement (FR-02-17)
    B->>D: Save draft agreement, calculated amount and payment components
    D-->>B: Draft agreement reference
    B-->>App: Return Draft Handover Agreement
    App-->>Dr: Display draft agreement
    Note over App,D: Supplier review/signing continues in UC-08. The amount is locked with a document hash only when signing commits.
  end
```

The existing combined sequence previously included supplier signing, document locking and pickup completion after draft generation. Those interactions now belong to SD-UC-08. UC-07 retains handover validation/storage, the existing price and bonus calculation, and draft generation; it does not complete the pickup or credit the payout balance.

The system-validation `alt` corresponds to AD-UC-07 `d1`. Evidence requirements still follow AD-UC-07 and its existing open question; this edit does not define new photo/GPS limits.
