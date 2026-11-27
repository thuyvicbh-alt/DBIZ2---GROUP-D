# W3 Cross-team review: ReFeed

Lesson 5 (block W3). Review [W0](w0-scope.md), [W1](w1-function-list.md) and [W2](w2-cost.md), then carry evidenced findings into [W4](w4-priorities.md) and [W5](w5-handover.md). The structure retains the workshop's eight quality criteria and defect log.

| Field | Value |
|---|---|
| Owning team | Group D |
| Review status | AI-assisted document pre-review, 2026-11-27; peer-team review and owning-team decisions pending |
| Scope reviewed | W0/W1 at `08e751f`: 16 use cases, 6 features, 86 L2 functions, 125 L3 tasks; 10 remaining activity diagrams |
| Source authority | W0/W1 define the current scope and decomposition; CVCA and architecture support it. Activity/sequence/screen evidence is checked for consistency and does not reintroduce removed scope. |
| Evidence rule | Cite current files, sections and IDs. Historical findings use a frozen revision. Findings are not a peer-team verdict or client acceptance. |

The withdrawal use case and seven tasks are removed. Current UC-13/14/15/16 mean Green Profile, ESG, monitoring and audit/compliance respectively. Current FR-05-07/08/09 mean Green Profile, ESG generation and ESG provision; these IDs must not inherit the old withdrawal meanings.

## Checklist — eight quality criteria

| # | Criterion | Judged by | Machine signal / source evidence | Owning-team self-assessment | Pre-review finding | Pre-review pass? | Note / peer action |
|---|---|---|---|---|---|---|---|
| C1 | Two-way traceability | Machine + person | 86 unique L2 IDs; 125 unique L3 IDs; every task has a valid parent | Pending team confirmation | W2/W4/W5 reconcile. W1 has one nonstandard task ID and two broken source links; several diagrams still use earlier FR meanings. | Partial | D-02/D-06; supplementary node map below is not a full screen audit. |
| C2 | Scope boundary explicit (Won’t-have with a reason) | Person | W0 Table 3 excludes in-app transfers and chat; W1 has no withdrawal; W4 defers FR-04-11 | Pending team confirmation | W2–W5 exclude withdrawal work. AD-UC-12 still contains an obsolete withdrawal branch and a banking-API default. | Partial | D-05; remove stale implementation instructions in the owning source. |
| C3 | MECE — no duplicates, no missing CRUD | Person | No duplicate L2/L3 IDs; 7 obsolete withdrawal tasks are absent from costing | Pending team confirmation | UC-06/UC-07 collection/handover capture may overlap. Do not infer missing edit/delete functions solely from CRUD. | Partial | D-09; clarify whether capture is reused or separate before changing estimates. |
| C4 | State and time transitions have a trigger | Person | All 86 EARS rows name WHEN; FR-06-12 names a scheduled backup window | Pending team confirmation | Receiving ends at Received; batching requires Verified_At_Warehouse. No explicit intervening transition is defined. | Partial | D-08; confirm the lifecycle and tolerance. |
| C5 | Hidden technical work | Machine + person | 11 tasks; 69.4 p-h / 534.4 in-scope p-h = 13.0%; floor 10% | Pending team confirmation | Role checks, denied-access logs, test automation, deployment, monitoring and backup/recovery remain estimated. | Pass for current list | NC business steps are not automatically hidden technical work. |
| C6 | Granularity 4–16 hours | Machine | 125 tasks: 80 S, 44 M, 1 L; 688 base h; 0 outside 4–16 h | Pending team confirmation | Every task has its own size and hours. The estimates remain a planning baseline. | Pass for current list | D-01/D-03; team sizing review remains required. |
| C7 | Layer label FE / BE / INT present | Machine + person | 125 valid layers; 0 missing; FR-04-10-INT identifies the Eco Oil boundary | Pending team confirmation | Only ReFeed integration work is counted; no partner implementation or bank gateway is estimated. | Pass for current list | D-09; confirm the external contract before integration acceptance. |
| C8 | Independently testable EARS sentence | Person | 15 L2 criteria / 26 L3 tasks have EARS precise? = No | Pending team confirmation | W2 uses the W1 flags and applies 0.40 verification to these tasks. Rule/report definitions remain incomplete. | Needs clarification | D-09; current affected IDs and reasons are listed below. |

## Defect log

| # | Location (current IDs) | Criterion | Evidence | Proposed bucket | Treatment in this revision | Owning-team decision | Person-hours affected |
|---|---|---|---|---|---|---|---|
| D-01 | W2 all L3 rows; W4 all L2 rows; W5 Must seed | C1 / C6 | [Previous W2](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/aee45cb/docs/function-list/w2-cost.md) retains 91 L2 / 132 L3, 724 base h and 562.8 in-scope p-h. Current W1 has 86 / 125 and 688 base h. | Fix now | Rebuild W2/W4 from current W1; remove 36 base h, 28.4 p-h and 7.820.000 VND of withdrawal work. Rebuild W5 from the current Must subset. | Applied; peer verification pending | 552.0 all tasks; 534.4 in scope |
| D-02 | W1 L3 row F-05-02-FE; W2 row; W5 seed | C1 | W1 spells the task ID F-05-02-FE while its parent is FR-05-02 and other task IDs use FR-. | Preserve source; clarify identifier | Copy the literal ID to W2 and W5 and join through the explicit parent column. No invented FR-05-02-FE row or duplicate is added. | Pending W1 owner naming correction | 2.0; no cost change |
| D-03 | W2 thresholds; W5 feasibility | Capacity | Current full scope is 534.4 p-h; fixed capacity 360.0 p-h; Must alone is 428.4 p-h. | Review scope/capacity | Report full-scope shortfall 174.4 p-h and Must shortfall 68.4 p-h. | Pending team/client decision | 534.4 in-scope p-h |
| D-04 | W4 priority totals | Scope contingency | Must = 80.2% versus maximum 60%; Could = 2.7% versus target 20%. | Review priorities and dependencies | Retain surviving effective priorities by function meaning. Green Profile remains Could at its new FR-05-07 ID; ESG remains Should at FR-05-08/09. | Pending team/client decision | 428.4 Must p-h; 14.2 Could p-h |
| D-05 | W0/W1 F-05; AD-UC-12 withdrawal branch | C2 | [AD-UC-12](../activity/AD-UC-12.md) still draws validate, display, transaction, update, displayUpdated for withdrawal and defaults to banking-API processing in section 6. W0/W1 have removed withdrawal; ADR-003 keeps transfers external. | Reconcile obsolete source | No withdrawal request, eligibility check, balance deduction, result view or banking integration is costed or seeded. Current UC-13 is Green Profile; AD-UC-13/SD-UC-13 withdrawal files were deleted upstream. | Pending diagram/sequence owners; scope followed in W2–W5 | 0 current withdrawal p-h |
| D-06 | W1 Sources swept; remaining AD/screen reverse references | C1 | W1 links w0-scope-updated.md and cvca.md inside function-list; neither exists. Actual files are w0-scope.md and ../context/cvca.md. AD-UC-03/06/10 now cite current IDs, but AD-UC-04/05/07/11 and the AD-UC-12 displayTier label still use earlier FR meanings. | Reconcile links and reverse references | Use working relative links in W2–W5 and provide a behaviour-to-node map below. Preserve W0/W1 and owning diagrams in this scoped update. | Pending W1/source owners | 0 added p-h; reference repair is not a new function |
| D-07 | W0 Table 4: Map & Optimized Route Data | FP cross-check | W0 classifies map data EIF but gives 10 FP; retained workshop average EIF weight is 7. | Confirm data weight | Retain W0 total 77 FP. Current all-task cross-check is 132 transactional FP + 77 data FP, then 76 platform base h. | Pending W0 owner confirmation | 0 bottom-up p-h change; a correction would reduce the FP comparison by 9 base h |
| D-08 | FR-04-02, FR-04-03, FR-04-05, FR-04-06 | C4 / C8 | AD-UC-10 uses tolerance 5% in examples; historical W5 proposes 2%. W1/UC-10 end at Received, while AD-UC-11 requires Verified_At_Warehouse. | Clarify rule and lifecycle | Treat 5% as a test fixture, not an approved threshold. Preserve the configured-tolerance criterion and both source states; W5 records blockers. | Pending warehouse/client confirmation | 23.6 |
| D-09 | 15 No-precision L2 criteria; collection/handover capture | C3 / C8 | Current W1 flags 26 L3 tasks No. FR-02-10/12 and FR-02-14/15 both describe collection/handover quantity and evidence capture. | Clarify acceptance and reuse | W2 retains W1 task estimates and verification factors. Clarify distinct capture responsibilities and rules before changing the source decomposition; removed withdrawal ordering is no longer a current acceptance issue. | Pending rule owners | 170.2 across No-precision tasks; overlaps other findings |
| D-10 | FR-06-02/03 Must; FR-04-06 through FR-04-10 Should | Release dependencies | W4 retains audit/compliance outputs as Must and batch/mass-balance/export as Should. CVCA P-5 and StR-05 connect compliance value to batch traceability. | Confirm dependency closure | W5 calls the Must set a costed planning subset. Confirm the manual workaround or change dependent priorities together before accepting it as a viable backlog. | Pending feature owners/client | 23.6 Must audit/report p-h; 41.2 Should batch p-h |

Affected effort is the work under review, not an extra charge. Defect rows overlap and must not be summed. W4 owns later effective-priority decisions; W1 owns task/criterion/identifier changes. W0's Needed/not needed flags do not silently remove functions retained in W1; confirm the column's intended meaning if the team intends it to govern release scope.

## EARS precision review

These are the current W1 No flags, not newly assigned flags. Yes means the stated behaviour can be checked with configured fixtures; it does not approve the final business parameter values. The warehouse tolerance remains an open parameter in D-08 without changing W1's current flags.

| Current L2 ID | Reason for No | Affected L3 tasks | Verification factor |
|---|---|---|---|
| FR-01-01 | Credential/compliance validation rules are not enumerated in the W1 criterion. | FR-01-01-FE, FR-01-01-BE | 0.40 |
| FR-01-07 | Point-of-Origin/compliance field-validation rules are not enumerated in W1. | FR-01-07-FE, FR-01-07-BE | 0.40 |
| FR-01-11 | Pickup required fields, minimum volume and time/location restrictions remain open; AD-UC-03 section 6 question 3. | FR-01-11-FE, FR-01-11-BE | 0.40 |
| FR-02-01 | Pending-pickup eligibility/selection rules remain open; AD-UC-05 section 6 question 2. | FR-02-01-FE, FR-02-01-BE | 0.40 |
| FR-02-02 | Optimisation failure/recovery handling remains open; AD-UC-05 section 6 question 3. | FR-02-02-INT | 0.40 |
| FR-02-10 | Collection validation, GPS precision and offline handling remain open; AD-UC-06 section 6 question 3. | FR-02-10-FE, FR-02-10-BE | 0.40 |
| FR-02-14 | Required handover-evidence constraints remain open; AD-UC-07 section 6 question 1. | FR-02-14-FE, FR-02-14-BE | 0.40 |
| FR-04-06 | Batch parameter and tank-volume constraints remain open; AD-UC-11 section 6 question 1. | FR-04-06-FE, FR-04-06-BE | 0.40 |
| FR-04-08 | The mass-balance metric formula is not specified in the W1 criterion. | FR-04-08-BE | 0.40 |
| FR-04-09 | The batch-balance update formula is not specified in the W1 criterion. | FR-04-09-FE, FR-04-09-BE | 0.40 |
| FR-04-10 | The Eco Oil payload, acknowledgement and retry contract remain open; AD-UC-11 section 6 question 2. | FR-04-10-INT | 0.40 |
| FR-05-08 | ESG report fields and environmental calculation method are not specified in W1. | FR-05-08-BE | 0.40 |
| FR-06-01 | The criterion says relevant statuses without a complete field/filter list. | FR-06-01-FE, FR-06-01-BE | 0.40 |
| FR-06-02 | Audit-history fields and query limits are not specified in W1. | FR-06-02-FE, FR-06-02-BE | 0.40 |
| FR-06-03 | The criterion says applicable compliance report without a report definition. | FR-06-03-FE, FR-06-03-BE | 0.40 |

## Activity-node coverage

This supplementary reconciliation maps current W1 behaviour to the 10 remaining activity diagrams. W1 now cites W0, CVCA and architecture rather than Mermaid node IDs, so this table is not presented as W1's source-trace column. A matching node does not mean its existing FR label, business rule or screen reference has already been repaired.

There are **57 action references mapping to 57 distinct current L2 functions**; the remaining **29 L2 functions**, including the deferred FR-04-11, have no action mapped here. These counts describe coverage of the full W1 decomposition. Release effort still follows W4's effective priorities. Decisions, actor inputs, start/end, object, merge, fork and join nodes do not create L2 functions. Boundary receipt nodes c1/e1 represent ReFeed notification/export responsibility, not partner implementation.

Only retrieve and displayTier in AD-UC-12 map to current F-05 functions. The five obsolete withdrawal nodes validate, display, transaction, update and displayUpdated are deliberately excluded; they do not map to current FR-05-07/08/09. The deleted AD-UC-13 withdrawal diagram is not a current source for Green Profile UC-13.

| Activity diagram | Current action references mapped | Node → current L2 function |
|---|---|---|
| [AD-UC-01](../activity/AD-UC-01.md) | 5 | `validateAuth` → `FR-01-01`; `createAccount` → `FR-01-02`; `authenticateUser` → `FR-01-03`; `displayAuthSuccess` → `FR-01-04`; `displayAuthError` → `FR-01-05` |
| [AD-UC-03](../activity/AD-UC-03.md) | 6 | `s1` → `FR-01-09`; `s2` → `FR-01-10`; `s3` → `FR-01-11`; `s4` → `FR-01-12`; `s5` → `FR-01-13`; `s6` → `FR-01-14` |
| [AD-UC-04](../activity/AD-UC-04.md) | 6 | `s1` → `FR-01-15`; `s3` → `FR-01-16`; `s4` → `FR-01-17`; `s5` → `FR-01-18`; `c1` → `FR-01-19`; `s2` → `FR-01-20` |
| [AD-UC-05](../activity/AD-UC-05.md) | 7 | `s1` → `FR-02-01`; `s2` → `FR-02-02`; `s3` → `FR-02-03`; `s4` → `FR-02-04`; `s5` → `FR-02-05`; `s6` → `FR-02-06`; `s7` → `FR-02-07` |
| [AD-UC-06](../activity/AD-UC-06.md) | 6 | `s1` → `FR-02-08`; `s2` → `FR-02-09`; `s3` → `FR-02-10`; `s4` → `FR-02-11`; `s5` → `FR-02-12`; `s6` → `FR-02-13` |
| [AD-UC-07](../activity/AD-UC-07.md) | 5 | `a2` → `FR-02-14`; `a3` → `FR-02-15`; `a4` → `FR-02-16`; `a5` → `FR-02-17`; `a7` → `FR-02-18` |
| [AD-UC-08](../activity/AD-UC-08.md) | 10 | `s1` → `FR-03-01`; `s2` → `FR-03-02`; `s3` → `FR-03-03`; `s4` → `FR-03-04`; `s5` → `FR-03-05`; `s6` → `FR-03-06`; `s7` → `FR-03-07`; `s8` → `FR-03-08`; `s9` → `FR-03-09`; `s10` → `FR-03-10` |
| [AD-UC-10](../activity/AD-UC-10.md) | 5 | `s1` → `FR-04-01`; `s2` → `FR-04-02`; `s3` → `FR-04-03`; `s4` → `FR-04-04`; `s5` → `FR-04-05` |
| [AD-UC-11](../activity/AD-UC-11.md) | 5 | `s1` → `FR-04-06`; `s2` → `FR-04-07`; `s3` → `FR-04-08`; `s4` → `FR-04-09`; `e1` → `FR-04-10` |
| [AD-UC-12](../activity/AD-UC-12.md) | 2 | `retrieve` → `FR-05-01`; `displayTier` → `FR-05-02` (existing label needs correction); withdrawal branch excluded |
