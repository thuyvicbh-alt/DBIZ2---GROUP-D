# Activity Diagram: AD-UC-08 Confirm and sign a UCO transaction

| Field | Value |
|---|---|
| AD ID | `AD-UC-08` |
| Use case | `UC-08 Confirm and sign a UCO transaction` |
| Feature | `F-03 UCO Transaction` |
| Primary actor | Supplier |
| Lanes | Supplier · ReFeed System |
| Inputs (L3-L5) | [Context Diagram v2.1](../context/context-diagram.md) · [Use Case Diagram v2](../use-case/use-case-diagram.md), UC-08 in [W0](../function-list/w0-scope.md) · Sequence: [SD-UC-08](../use-case/SD-UC-08.md) · [Architecture v1](../architecture/architecture.md), SyR-04 · [W1](../function-list/w1-function-list.md) rows `FR-03-01`–`FR-03-10` |
| Trigger | The supplier opens a recorded UCO handover transaction for review and signature. |
| Preconditions | The supplier is signed in. UC-07 has recorded the handover and prepared the draft agreement with its calculated amount. |
| Success end state | The reviewed agreement and amount are locked, the electronic signature and signing time are stored, and the pickup is `Completed`. |
| Failure end state(s) | No own unsigned transaction is available: no signing occurs. The supplier leaves without confirming: the transaction remains unsigned. Invalid signing input or a failed signing attempt returns the supplier to signature submission without completing the transaction. |
| FRs covered | `FR-03-01`, `FR-03-02`, `FR-03-03`, `FR-03-04`, `FR-03-05`, `FR-03-06`, `FR-03-07`, `FR-03-08`, `FR-03-09`, `FR-03-10` |
| Business rules applied | Existing [S-F03-01](../external-design/screens/screen-spec-S-F03-01.md) SR-001–SR-005, SR-007–SR-008: own transaction; read-only handover/amount; required signature and confirmation; agreement locking; unchanged data on signing failure. No separate UC-08 BR ID is defined in W0. |
| Version / status | v1.3 · Draft |
| Author | Codex · draft for Group D review |

## 1. Diagram

Notation follows [mermaid-activity-patterns.md](../../templates/mermaid-activity-patterns.md), Pattern A, and the supplied [activity-diagram-template.md](../../templates/activity-diagram-template.md). `swimlane-beta` requires Mermaid 11.16 or later.

```mermaid
swimlane-beta TB
  subgraph ACT["Supplier"]
    start@{ shape: sm-circ }
    a1("Open handover transaction")
    a2("Review recorded handover and amount")
    d2{"Confirm transaction?"}
    m1{" "}
    a3("Submit electronic signature")
    a4("View signing result")
    stop1@{ shape: fr-circ }
    stop2@{ shape: fr-circ }
    stop3@{ shape: fr-circ }
  end
  subgraph SYS["ReFeed System"]
    s1("Retrieve supplier draft agreement (FR-03-01)")
    d1{"Own unsigned draft available?"}
    s2("Display handover and calculated amount (FR-03-02)")
    s3("Display no transaction awaiting signature (FR-03-03)")
    s4("Validate signature and confirmation (FR-03-04)")
    d3{"Signing input valid?"}
    s5("Return signing validation feedback (FR-03-05)")
    s6("Lock agreement and calculated amount (FR-03-06)")
    s7("Store electronic signature (FR-03-07)")
    s8("Mark pickup as Completed (FR-03-08)")
    d4{"Signing successful?"}
    s9("Return signing failure and prompt retry (FR-03-09)")
    o1["Pickup [Completed]"]
    s10("Display signing success (FR-03-10)")
  end
  start --> a1 --> s1 --> d1
  d1 -->|"[no]"| s3 --> stop3
  d1 -->|"[yes]"| s2 --> a2 --> d2
  d2 -->|"[no]"| stop2
  d2 -->|"[yes]"| m1
  m1 --> a3 --> s4 --> d3
  d3 -->|"[no]"| s5 --> m1
  d3 -->|"[yes]"| s6 --> s7 --> s8 --> d4
  d4 -->|"[no]"| s9 --> m1
  d4 -->|"[yes]"| o1 --> s10 --> a4 --> stop1
```

Reading notes:

- UC-07 supplies the handover and calculated amount. The supplier reviews them here; leaving or cancelling confirmation produces no signature. Confirmation-modal interactions remain in S-F03-01.
- `m1` merges the first signature submission and retries after invalid input or signing failure. `d4` represents the result of the signing operation; storage and failure handling are detailed in SD-UC-08, not as additional AD nodes.
- This diagram covers UC-08 only. Receipt access belongs to UC-09. The existing post-signing balance, volume and tier updates remain UC-12 / `FR-05-03`–`FR-05-06`, outside this activity.

## 2. Node trace

| Node | Kind | Text | Traces to |
|---|---|---|---|
| s1 | Action | Retrieve supplier draft agreement | FR-03-01; S-F03-01 SR-001 and empty state |
| d1 | Decision | Own unsigned draft available? | FR-03-01; S-F03-01 empty state |
| s2 | Action | Display handover and calculated amount | FR-03-02; S-F03-01 SR-002 |
| s3 | Action | Display no transaction awaiting signature | FR-03-03; S-F03-01 empty state |
| d2 | Decision | Confirm transaction? | FR-03-04; S-F03-01 confirmation/Back interactions |
| s4 | Action | Validate signature and confirmation | FR-03-04; S-F03-01 SR-003 |
| d3 | Decision | Signing input valid? | FR-03-04; S-F03-01 SR-003 |
| s5 | Action | Return signing validation feedback | FR-03-05; S-F03-01 required signature/confirmation fields |
| s6 | Action | Lock agreement and calculated amount | FR-03-06; original W1 signing criterion; S-F03-01 SR-005/SR-008 |
| s7 | Action | Store electronic signature | FR-03-07; original W1 signing criterion; S-F03-01 signature/signing-time fields |
| s8 | Action | Mark pickup as Completed | FR-03-08; original W1 signing criterion; S-F03-01 SR-005 |
| d4 | Decision | Signing successful? | FR-03-06–FR-03-09; S-F03-01 SR-007 |
| s9 | Action | Return signing failure and prompt retry | FR-03-09; S-F03-01 error state and SR-007 |
| s10 | Action | Display signing success | FR-03-10; S-F03-01 success state |

## 3. Branches and acceptance tests

| Branch | Given | When | Then |
|---|---|---|---|
| d1 [no] | SUP-00125 has no unsigned transaction awaiting signature, or COL-00891 is already signed. | The supplier opens transaction review. | Show no transaction awaiting signature; create no signature and leave the pickup/balance unchanged. |
| d1 [yes] | SUP-00125 has draft COL-00891 from UC-07. | The supplier opens the transaction. | Display the recorded 32.5 kg, base price 10,000 VND/kg, bonus 300 VND/kg and calculated total 334,750 VND read-only, as in S-F03-01. |
| d2 [no] | SUP-00125 is reviewing COL-00891. | The supplier leaves without confirming the transaction. | End without signing; retain the unsigned agreement and existing pickup state. Cancelling the confirmation modal alone also saves nothing and leaves review available on S-F03-01. |
| d2 [yes] | SUP-00125 accepts the recorded 32.5 kg and 334,750 VND. | The supplier confirms the transaction. | Continue to signature submission. |
| d3 [no] | A signing submission for COL-00891 has an empty signature or no explicit confirmation. | The system validates the signing input. | Return correction feedback; store no signature, lock no agreement and complete no pickup; retry through m1. |
| d3 [yes] | COL-00891 has a nonempty supplier signature and explicit confirmation. | The system validates the signing input. | Continue to agreement locking, signature storage and pickup completion. |
| d4 [no] | Signing COL-00891 fails, as in the existing S-F03-01 error state. | The system returns the signing result. | Keep the agreement unsigned/unlocked and the previous pickup/balance state unchanged; prompt a fresh signature attempt through m1. |
| d4 [yes] | Signing COL-00891 succeeds. | The system returns the signing result. | Lock the reviewed 334,750 VND with the agreement hash, store the signature/signing time, mark the linked pickup Completed and show signing success. |

## 4. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
|---|---|---|
| Pickup | `Completed` | `Requested`, `Driver Assigned`, `Arrived`, `Cancelled`, `Completed` (W1); `Updated` also appears in AD-UC-04. AD-UC-04 uses the spelling `Driver_Assigned`; this document retains the unambiguous `Completed` value and does not redefine the shared catalogue. |

## 5. Screens touched

| Actor action | Screen ID |
|---|---|
| Open handover transaction | S-F00-02 / S-F05-01 → S-F03-01 |
| Review recorded handover and amount; confirm or leave unsigned | S-F03-01, including the existing confirmation modal |
| Submit or correct an electronic signature | S-F03-01 |
| View signing result | S-F03-01 success/error/empty states |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: Does the supplier sign on their own device or the driver's phone?] | No | Existing S-F03-01 question 2: supplier's own device after the notification. The logical AD is device-independent. |
| 2 | [NEEDS CLARIFICATION: What happens if the supplier disagrees with the quantity or amount?] | No | Existing S-F03-01 question 3: leave unsigned and contact ReFeed by phone. No in-app rejection/dispute function is added. |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders on mermaid.live | Pass; checked in Mermaid Live and with mermaid-cli 11.16.0 / Mermaid 11.17.2 | Codex | 2026-11-25 |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass; four yes/no decisions, eight guarded branches and eight acceptance rows | Codex | 2026-11-25 |
| No action has two incoming flows (merge used instead) | Pass; m1 merges first submission, invalid-input retry and failed-signing retry | Codex | 2026-11-25 |
| Every fork has a matching join | N/A; no fork | Codex | 2026-11-25 |
| Every system action cites an FR | Pass; 10 system actions, each with one current W1 FR at the end of its label | Codex | 2026-11-25 |
| Every FR of this use case appears in the diagram | 10 of 10; FR-03-01–FR-03-10 | Codex | 2026-11-25 |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Supplier from W0 UC-08; ReFeed System from Context v2.1; no internal component lane | Codex | 2026-11-25 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass; SD system alternatives correspond to d1, d3 and d4; supplier confirmation remains d2 | Codex | 2026-11-25 |
| AI critic prompt run, findings resolved or logged as open questions | Profile-based self-critique completed: corrected fallback syntax, mixed-use-case scope and screen detail; existing questions retained in section 6; see AI log entry 6 | Codex | 2026-11-25 |

---

Template source: DBIZ2 Lesson 7, VJCBI College - FTU, [activity-diagram-template.md](../../templates/activity-diagram-template.md). Notation: OMG UML 2.5.1, Clause 15; [mermaid-activity-patterns.md](../../templates/mermaid-activity-patterns.md).
