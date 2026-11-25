# Sequence Diagram: SD-UC-08 Confirm and sign a UCO transaction

Lesson 4, analysis level. Supplier review/confirmation choices are in [AD-UC-08](../activity/AD-UC-08.md). This sequence shows the interactions between the Supplier Web App, Transaction & Agreement Service and ReFeed database from the [physical design](../architecture/architecture.md).

| Field | Value |
|---|---|
| Use case | UC-08 Confirm and sign a UCO transaction |
| Primary actor | Supplier |
| Version / status | v1.1 · Draft |
| Inputs | [AD-UC-08](../activity/AD-UC-08.md) · [W1](../function-list/w1-function-list.md) FR-03-01–FR-03-10 · [SD-UC-07](SD-UC-07.md) · [S-F03-01](../external-design/screens/screen-spec-S-F03-01.md) |
| Preconditions | Supplier is signed in. UC-07 has prepared the draft agreement with its calculated amount. |
| Success end state | Reviewed agreement/amount locked, signature/signing time stored, pickup Completed. |
| Other results | No own unsigned draft, invalid signing input or a signing failure; the supplier may leave without submitting a signature. |

```mermaid
sequenceDiagram
  actor S as Supplier
  participant P as Supplier Web App
  participant T as Transaction & Agreement Service
  participant D as ReFeed database

  S->>P: Open handover transaction
  P->>T: Retrieve supplier draft agreement (FR-03-01)
  T->>D: Read unsigned draft for signed-in Supplier_ID
  D-->>T: Own unsigned draft or no eligible draft
  alt No own unsigned draft available
    T-->>P: No transaction awaiting signature
    P-->>S: Display no transaction awaiting signature (FR-03-03)
  else Own unsigned draft available
    T-->>P: Recorded handover and calculated amount
    P-->>S: Display handover and calculated amount (FR-03-02)
    Note over S,P: Supplier confirmation or leaving unsigned is in AD-UC-08 d2.
    S->>P: Submit electronic signature and confirmation
    P->>T: Sign the reviewed transaction
    T->>T: Validate signature and confirmation (FR-03-04)
    alt Signing input invalid
      T-->>P: Return signing validation feedback (FR-03-05)
      P-->>S: Correct signing input and resubmit
    else Signing input valid
      Note over T,D: Keep the amount calculated by UC-07. Signing stores the agreement hash and signing time, following S-F03-01.
      T->>D: Lock agreement and calculated amount (FR-03-06)
      T->>D: Store electronic signature (FR-03-07)
      T->>D: Mark pickup as Completed (FR-03-08)
      alt Signing successful
        D-->>T: Signed agreement and Completed pickup
        T-->>P: Successful signing result
        P-->>S: Display signing success (FR-03-10)
      else Signing failed
        D-->>T: Failure with original unsigned agreement and pickup state retained
        T-->>P: Return signing failure and prompt retry (FR-03-09)
        P-->>S: Signing failed, submit a new signature
      end
    end
  end
```

## Interaction trace

| AD action | Current W1 function | Interaction |
|---|---|---|
| s1 | FR-03-01 | Retrieve the supplier's own unsigned draft. |
| s2 | FR-03-02 | Display the recorded handover and calculated amount read-only. |
| s3 | FR-03-03 | Display that no transaction awaits signature. |
| s4 | FR-03-04 | Validate the signature and explicit confirmation. |
| s5 | FR-03-05 | Return correction feedback for invalid signing input. |
| s6 | FR-03-06 | Lock the agreement hash with the reviewed amount. |
| s7 | FR-03-07 | Store the signature and signing time against the transaction. |
| s8 | FR-03-08 | Mark the linked pickup Completed on successful signing. |
| s9 | FR-03-09 | Return the existing signing-failure/retry result. |
| s10 | FR-03-10 | Display signing success. |

The three system-outcome alternatives correspond to AD-UC-08 d1 (draft availability), d3 (input validity) and d4 (signing result). Supplier confirmation d2 remains an actor decision in the AD.

The agreement lock, signature storage and pickup completion form one signing operation: the failed result must leave the unsigned agreement and original pickup state unchanged, as required by S-F03-01 SR-007. Database transaction mechanics belong to implementation; the AD contains only the externally observable business result.

UC-07 supplies the handover and calculated amount. Receipt retrieval belongs to UC-09. The existing post-signing balance, verified-volume and tier updates remain in UC-12 / FR-05-03–FR-05-06; they are not additional UC-08 actions or new functions.
