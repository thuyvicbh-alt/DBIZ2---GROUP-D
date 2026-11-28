# Activity Diagram: AD-UC-02 Manage Point-of-Origin and Compliance Profile

| Field | Value |
|---|---|
| AD ID | AD-UC-02 |
| Use case | UC-02 Manage Point-of-Origin and compliance profile |
| Feature | F-01 Supplier Onboarding & Pickup Management |
| Primary actor | Supplier |
| Lanes | Supplier · ReFeed System |
| Inputs (L3-L5) | Context Diagram · Use Case Diagram · CVCA · Architecture · W1 rows `FR-01-07` to `FR-01-08` (`FR-01-07-FE`, `FR-01-07-BE`, `FR-01-08-BE`) |
| Trigger | The supplier taps "Thông tin điểm thu gom" or "Bổ sung hồ sơ" on `S-F01-06` to open `S-F01-02` and enter or update Point-of-Origin and compliance information. |
| Preconditions | The supplier has an `Active` registered account with a unique `Supplier_ID` and is signed in (`UC-01` / `FR-01-01`, `FR-01-02`, `FR-01-06`). |
| Success end state | The submitted Point-of-Origin (address, GPS coordinates, contact details) and compliance information are validated (`FR-01-07`) and stored against the `Supplier_ID` (`FR-01-08`), updating the profile to `Complete` ("✓ Đầy đủ" on `S-F01-06`) so the supplier can request UCO pickups (`UC-03`). |
| Failure end state | If submitted profile or compliance fields are missing or invalid, validation fails (`FR-01-07`) without overwriting stored profile data; the supplier remains on `S-F01-02` to correct and resubmit. |
| FRs covered | `FR-01-07`, `FR-01-08` (3 L3 work items: `FR-01-07-FE`, `FR-01-07-BE`, `FR-01-08-BE`) |
| Business rules applied | `BR-01` (Tax ID / compliance data validation); Profile-completeness rule (`FR-01-09`, `FR-01-10`): GPS location and compliance data must be stored before a pickup request can be accepted. |
| Version / status | v1.0 · Aligned with Refined W1 Function List & `S-F01-06` |
| Author | ReFeed Group D |

## 1. Diagram

Notation follows the DBIZ2 activity profile (`mermaid-activity-patterns.md`). Rendered with `swimlane-beta TB`. Every action node maps 1-to-1 to a distinct L3 work item in W1 (`FR-01-07-FE`, `FR-01-07-BE`, `FR-01-08-BE`); control nodes (`start1`, `m1`, `d1`, `stop1`) and the object node (`o1`) do not add functions.

```mermaid
swimlane-beta TB
  subgraph SUP["Supplier"]
    start1@{ shape: sm-circ }
    m1{" "}
    a1("Capture or correct Point-of-Origin and compliance fields (FR-01-07-FE)")
  end
  subgraph SYS["ReFeed System"]
    s1("Validate Point-of-Origin and compliance information (FR-01-07-BE)")
    d1{"Are profile fields valid?"}
    s2("Store Point-of-Origin and compliance profile (FR-01-08-BE)")
    o1["Supplier Profile & Compliance [Complete]"]
    stop1@{ shape: fr-circ }
  end

  start1 --> m1 --> a1 --> s1 --> d1
  d1 -->|"[Invalid]"| m1
  d1 -->|"[Valid]"| s2 --> o1 --> stop1
```

### Reading notes

- **Strict 1-to-1 mapping with W1:** `UC-02` contains 2 L2 functions (`FR-01-07`, `FR-01-08`) decomposed into 3 L3 work items in W1. Each action box carries one unique L3 ID: `a1` (`FR-01-07-FE`), `s1` (`FR-01-07-BE`), and `s2` (`FR-01-08-BE`).
- **Validation retry loop:** Unlike `UC-03` or `UC-07`, W1 does not define a separate L3 row for returning validation errors in `UC-02`; field error feedback and retry are handled directly within `S-F01-02` (`FR-01-07-FE`). Therefore, `d1 [Invalid]` routes directly back through merge node `m1` into `a1`.
- **Navigation context:** The supplier enters this flow from `S-F01-06` (Supplier profile overview) into `S-F01-02` (Point-of-Origin form), and upon reaching `stop1` returns to `S-F01-06` showing the green confirmation banner ("Đã cập nhật hồ sơ") and the `"✓ Đầy đủ"` badge required by `UC-03` (`FR-01-09`).

## 2. Node trace

Every action node cites a unique W1 L3 Function List ID. Control nodes (`start1`, `m1`, `d1`, `stop1`) and object node (`o1`) do not add functions.

| Node | Kind | Text | Traces to |
|---|---|---|---|
| `a1` | Actor action | Capture or correct Point-of-Origin and compliance fields | `FR-01-07-FE` |
| `m1` | Merge | Merge initial profile entry and post-validation correction loop | Control flow (no separate FR) |
| `s1` | Action | Validate Point-of-Origin and compliance information | `FR-01-07-BE`, `BR-01` |
| `d1` | Decision | Are profile fields valid? | `FR-01-07`, `BR-01` (validation rule) |
| `s2` | Action | Store Point-of-Origin and compliance profile | `FR-01-08-BE` |
| `o1` | Object node | Supplier Profile & Compliance `[Complete]` | `FR-01-08` (state value enabling `FR-01-09`) |

## 3. Branches and acceptance tests

One row per outgoing branch of every decision. DBIZ3 turns each row into at least one test.

| Branch | Given | When | Then |
|---|---|---|---|
| `d1` `[Valid]` | Supplier `SUP-00125` is signed in (`S-F01-06` shows `"! Còn thiếu"`) and enters valid Point-of-Origin address, GPS coordinates, contact details, and compliance fields on `S-F01-02` | They submit the profile form (`FR-01-07-FE`) | `FR-01-07-BE` validates the fields, `FR-01-08-BE` persists the profile against `SUP-00125`, and `S-F01-06` shows `"✓ Đầy đủ"` with the update confirmation banner. |
| `d1` `[Invalid]` | Supplier `SUP-00125` is signed in and submits missing GPS coordinates, an empty address, or an invalid tax ID / compliance value on `S-F01-02` | They submit the profile form (`FR-01-07-FE`) | `FR-01-07-BE` rejects the invalid fields without overwriting stored data; the supplier remains on `S-F01-02` to correct and resubmit. |

## 4. Object states used

| Object | States that appear in this diagram | Full state list (all diagrams) |
|---|---|---|
| Supplier Profile & Compliance | `Complete` | `Incomplete` (`! Còn thiếu` on `S-F01-06`; triggers `FR-01-10` rejection in `AD-UC-03`), `Complete` (`✓ Đầy đủ` on `S-F01-06`; passes `FR-01-09` check in `AD-UC-03`) |

## 5. Screens touched

Filled in during Lesson 8, from the Screen List.

| Supplier action | Screen ID |
|---|---|
| Tap "Thông tin điểm thu gom" / "Bổ sung hồ sơ" (entry) and view updated profile confirmation banner + `"✓ Đầy đủ"` badge (post-save) | `S-F01-06` (Supplier profile) |
| Capture or correct Point-of-Origin (address, GPS, contact) and compliance fields (`FR-01-07-FE`) | `S-F01-02` (Point-of-Origin & compliance profile form) |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | `[NEEDS CLARIFICATION: What are the exact validation rules and required compliance fields in FR-01-07 (e.g., whether tax ID and business licence are captured at registration S-F01-01 while address, GPS coordinates, and contact phone are required on S-F01-02)?]` (W1 marks `FR-01-07` `EARS precise? = No` for this reason.) | No | Address, GPS coordinates, contact name/phone, and valid compliance fields (`BR-01`) are required to mark the profile `Complete`. |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders on mermaid.live (`swimlane-beta TB`) | Pass | | |
| Every decision has guards on all outgoing flows | Pass (`d1`: `[Valid]`, `[Invalid]`) | | |
| No action has two incoming flows; merge node used | Pass (`m1` merges `start1` and `d1 [Invalid]` before `a1`) | | |
| Every fork has a matching join | N/A (no forks) | | |
| Every action node cites a unique W1 Function ID | Pass (3 of 3 L3 items: `FR-01-07-FE`, `FR-01-07-BE`, `FR-01-08-BE`) | | |
| Every FR of `UC-02` appears in the diagram | Pass (2 of 2 L2; 3 of 3 L3) | | |
| Lane names match the Context Diagram & W1 | Pass (`Supplier · ReFeed System`) | | |
| AI critic prompt run, findings resolved or logged | Pass, 1 finding → open question 1 (`FR-01-07` `EARS precise? = No`) | | |