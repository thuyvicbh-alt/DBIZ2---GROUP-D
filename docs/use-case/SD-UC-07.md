# Sequence Diagram: SD-UC-07 Input Handover Information

Lesson 4, analysis level: how the parts of ReFeed talk to each other when a driver arrives at a supplier's point to collect UCO and finalize the transaction. The lifelines represent the Supplier, Driver, Field Staff App (GUI), Backend System, and Database.

| Field | Value |
|---|---|
| Use case | UC-07 Input Handover Information |
| Version | v1 (from image_fc21fe.png) |
| FRs | F-02 |

```mermaid
sequenceDiagram
  actor S as Supplier
  actor Dr as Driver
  participant App as Field Staff App
  participant B as Backend
  participant D as Database

  B->>App: Send Push Notification (New Route Assigned)
  Dr->>App: Open App & Select "Assigned Route"
  App->>B: getAssignedRoute(Driver_ID)
  B->>D: fetchRouteDetails(Driver_ID)
  D-->>B: Return Route Data
  B-->>App: Display Assigned Route
  Dr->>App: Select specific Pickup Request upon arrival
  App->>B: confirmArrival(Pickup_ID, Driver_ID)
  B->>D: updatePickupStatus("Arrived")
  D-->>B: Status Updated
  B-->>App: Return Arrival Confirmed
  Dr->>App: Input Actual_Quantity
  opt Capture evidence for UCO condition
    Dr->>App: Capture Photo_Evidence & Notes
  end
  App->>App: Auto-capture GPS & Timestamp (OS Level)
  Dr->>App: Click "Generate E-Agreement"
  App->>B: prepareTransaction(Pickup_ID, Quantity, GPS, Timestamp, Photo)
  B->>B: validateData(Quantity > 0), validateAddress(GPS)
  Note over B, D: Amount is calculated with current Bonus_Rate and locked in the agreement once signed
  B->>D: fetchSupplierLoyaltyData(Supplier_ID)
  D-->>B: Return Base_Price, Current_Bonus_Rate
  B->>B: Effective_Price = Base_Price + Current_Bonus_Rate
  B->>B: Total_Payment = Actual_Quantity * Effective_Price
  B->>B: Base_Payment = Actual_Quantity * Base_Price, Loyalty_Bonus = Actual_Quantity * Current_Bonus_Rate
  B->>B: createElectronicAgreement(Supplier_Info, Pricing, GPS, Photos)
  B-->>App: Return Draft Handover Agreement
  App-->>Dr: Display Agreement on Screen
  Dr->>S: Hand over device for review
  S->>App: Review Transaction Details
  S->>App: Sign E-Transaction (Sign-on-Glass)
  App->>B: submitSignature(Signature_Data, Signature_Method)
  B->>B: lockDocument & generateDocumentHash()
  B->>D: saveAgreement(Agreement_ID, Signed_At, Signature_Method, Document_Hash, Base_Payment, Loyalty_Bonus, Total_Payment)
  B->>D: saveCollectionRecord(Collection_ID, Quantity, GPS, Timestamp, Photo)
  B->>D: updatePickupStatus("Completed")
  D-->>B: Return Saved OK
  Note over B, D: The locked Base_Payment and Loyalty_Bonus are passed to Diagram 4 (BE) to update accumulated balance, verified volume and tier. Nothing is recalculated there.
  B-->>App: Return Digital Receipt & Success Status
  ```

The `alt` fragments here are outcomes decided inside the system, so they belong in a sequence
diagram. The supplier's choice between registering and logging in would be a decision in the
activity diagram instead.
  App-->>Dr: Display Collection Success Message