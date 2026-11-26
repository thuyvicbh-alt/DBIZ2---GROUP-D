# W3 Cross-team review: ReFeed

Lesson 5 (block W3). Read [W0](w0-scope.md), [W1](w1-function-list.md) and [W2](w2-cost.md), then carry evidenced defects into [W4](w4-priorities.md). Structure follows the Group D Cost Workshop, `W3_CrossReview!A11:H27`: eight criteria and a defect log.

| Field | Value |
|---|---|
| Owning team | Group D |
| Review status | AI-assisted pre-review, 2026-11-26; peer-team review and owning-team decisions pending |
| Scope reviewed | 6 features, 91 L2 functions, 132 L3 tasks; 11 activity diagrams |
| Evidence rule | Cite a file, section and current function/task ID. Findings against the previous revision link to that frozen commit. |

The assessments below record this document check. They do not represent a peer team's verdict or the client's acceptance. The original workbook's references to five features and its all-pass narrative are not reused as current findings.

## Checklist — eight quality criteria

| # | Criterion | Judged by | Machine signal / source evidence | Owning-team self-assessment | Pre-review finding | Pre-review pass? | Note / peer action |
|---|---|---|---|---|---|---|---|
| C1 | Two-way traceability | Machine + person | 91 unique L2 IDs; all 132 tasks have a valid parent; 66 action references map to 62 functions | Pending team confirmation | Forward node traces exist. Existing AD FR labels still use the earlier numbering; 29 baseline functions have no mapped activity node. | Partial | D-06; confirm diagram/screen reverse references. Coverage below is a node map, not a full screen audit. |
| C2 | Scope boundary explicit (Won't-have with a reason) | Person | W0 Table 3 excludes in-app transfers and chat; W4 excludes FR-04-11 from release totals | Pending team confirmation | Withdrawal tracking retains Should, but its previous rationale said Release 2. | Partial | D-05; confirm timing without introducing banking-transfer implementation. |
| C3 | MECE — no duplicates, no missing CRUD | Person | No duplicate L2/L3 IDs; four shared withdrawal actions are reused across UC-12/UC-13 | Pending team confirmation | The preserved node split adds no new CRUD. UC-06/UC-07 capture overlap and the two withdrawal execution orders still need team reconciliation. | Partial | D-09; do not infer missing delete/edit functions solely from a CRUD checklist. |
| C4 | State and time transitions have a trigger | Person | All 91 EARS rows name WHEN; FR-06-12 names the scheduled backup window | Pending team confirmation | Receiving ends at Received; batching requires Verified_At_Warehouse. No source defines the intervening transition. | Partial | D-08; confirm lifecycle, configured timing and tolerance. |
| C5 | Hidden technical work | Machine + person | 11 tasks; 69.4 p-h / 562.8 in-scope p-h = 12.3%, floor 10% | Pending team confirmation | Role control, denied-access logs, test automation, deployment, monitoring and backup/recovery are explicitly estimated. | Pass for current list | Do not confuse NC business steps with hidden technical work. |
| C6 | Granularity 4–16 hours | Machine | 132 tasks; 85 S, 46 M, 1 L; 0 outside the 4–16 h range | Pending team confirmation | Every task has its own size and hours; total 724 base h. Effort remains a planning estimate. | Pass for current list | D-01/D-03; developer/team sizing review remains required. |
| C7 | Layer label FE / BE / INT present | Machine + person | 132 valid layers; 0 missing; FR-04-10-INT identifies the external Eco Oil boundary | Pending team confirmation | Eco Oil delivery is counted as INT; partner-system implementation and in-app money transfer are not estimated. | Pass for current list | Confirm the integration contract before acceptance. |
| C8 | Independently testable EARS sentence | Person | 16 L2 criteria / 28 L3 tasks marked EARS precise? = No | Pending team confirmation | Incomplete validation, selection/recovery, batch formulas and report definitions remain visible; W2 applies the 0.40 verification factor. | Needs clarification | D-09 and the precision table below. No draft threshold is treated as an approved rule. |

## Defect log

| # | Location (current IDs) | Criterion | Evidence | Proposed bucket | Treatment in this revision | Owning-team decision | Person-hours affected |
|---|---|---|---|---|---|---|---|
| D-01 | W1 all 132 L3 tasks; W2 roll-up | C1 / C6 | [Previous W2](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w2-cost.md#roll-up-per-feature) reports "23", "43", "348", while the previous W1 lists 91 functions / 132 refined tasks and shared estimate packages. | Fix now | W1 has exactly the template columns and task-level S/M/L hours; W2 has one costing row per current task. | Pending review of new estimates | 580.4 all tasks; 562.8 in scope |
| D-02 | W4 all 91 functions; W5 Must seed | C1 | [Previous W5](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w5-handover.md#3-tasksmd-seed-effective-priority-must) labels the seed Must but includes old FR-01-04/FR-01-05 entries and "6.4" h where W2 uses 6.8 h. | Fix now | Rebuild W4 from current L2 IDs and rebuild the seed from exactly the 101 effective-Must tasks; all hours reconcile to W2. | Pending peer confirmation | 428.4 Must p-h |
| D-03 | W2 thresholds; W5 feasibility | Capacity | Current in-scope estimate is 562.8 p-h; fixed capacity is 360.0 p-h. Must alone is 428.4 p-h. | Review scope/capacity | Expose full-scope shortfall 202.8 p-h and Must shortfall 68.4 p-h; do not lower estimates to force a pass. | Pending team/client decision | 562.8 in-scope p-h |
| D-04 | W4 priority totals | Scope contingency | Must = 76.1% versus maximum 60%; Could = 2.5% versus target 20%. | Review priorities and dependencies | Retain prior effective priorities on their decomposed functions and carry the failed thresholds into W5. | Pending team/client decision | 428.4 Must p-h; 14.2 Could p-h |
| D-05 | FR-05-07 through FR-05-11 | C2 | [Previous W4](https://github.com/thuyvicbh-alt/DBIZ2-GroupD-Refeed-app/blob/8670e158dbeceae25d81e9c09f13eb7f64e66134/docs/function-list/w4-priorities.md) says "Should" and "Deferred to Release 2" in the same withdrawal row. W0 excludes actual in-app transfers. | Clarify release timing | Preserve Should for request/status tracking and expose the timing conflict. External manual money movement is unchanged. | Pending client confirmation | 28.4 |
| D-06 | AD-UC-03 s1–s6; W1 FR-01-09 through FR-01-14; equivalent older AD labels | C1 | [AD-UC-03](../activity/AD-UC-03.md) tags six different system actions "FR-01-03". W1 now assigns those actions six current IDs, while current FR-01-03 means account-exists feedback. | Reconcile references in owning diagrams/screens | W1 Source trace and the coverage table identify actual Mermaid nodes unambiguously. Diagram behaviour is preserved; reverse references remain open. | Pending integrator reconciliation | 35.4 for this example; other references may overlap |
| D-07 | W0 Table 4: Map & Optimized Route Data | FP cross-check | [W0](w0-scope.md#table-4-data-functions) classifies the map group EIF but assigns 10 FP. Workshop Parameters!A36:B36 assigns average EIF weight 7. | Confirm data weight | Preserve the source total 77 and flag it in W2; a correction would lower the comparison by 3 FP × 3 = 9 base h. | Pending W0 owner confirmation | 0 change to bottom-up p-h |
| D-08 | FR-04-02, FR-04-03, FR-04-05, FR-04-06 | C4 / C8 | [AD-UC-10](../activity/AD-UC-10.md) branch tests use "tolerance 5%"; previous W5 asks whether 2% is agreed. UC-10 ends at Received; [AD-UC-11](../activity/AD-UC-11.md) starts from Verified_At_Warehouse. | Clarify rule and lifecycle | Retain a configured tolerance and both source states; add no invented verification transition. W5 records both blockers. | Pending warehouse/client confirmation | 23.6 |
| D-09 | W1 No-precision criteria; FR-05-09, FR-05-10 | C3 / C8 | [AD-UC-12](../activity/AD-UC-12.md) forks withdrawal creation/status updates; [AD-UC-13](../activity/AD-UC-13.md) sequences them. Other rule gaps are cited individually below. | Clarify acceptance rules | Flag imprecise tasks honestly, cost their additional verification, and keep shared withdrawal functions counted once. | Pending rule owners; no new workflow assumed | 177.6 p-h across the 28 imprecise tasks; overlaps other findings. Withdrawal ordering is a separate consistency issue. |

Affected effort is the work under review, not an extra charge. Defect rows overlap and must not be summed. W4 is the owner of any later priority change; W1 owns any later estimate/criterion correction.

## EARS precision review

These No flags indicate incomplete acceptance definitions. Yes means the stated behaviour can be checked with known inputs/configured fixtures, not that all business parameter values have been approved.

| Current L2 ID | Reason for No | Affected L3 tasks | Verification factor |
|---|---|---|---|
| FR-01-01 | credential/compliance validation rules are not enumerated | FR-01-01-FE, FR-01-01-BE | 0.40 |
| FR-01-07 | Point-of-Origin/compliance field-validation rules are not enumerated | FR-01-07-FE, FR-01-07-BE | 0.40 |
| FR-01-11 | AD-UC-03 section 6 question 3 leaves detailed validation rules open | FR-01-11-FE, FR-01-11-BE | 0.40 |
| FR-02-01 | AD-UC-05 section 6 question 2 leaves pending/eligibility rules open | FR-02-01-FE, FR-02-01-BE | 0.40 |
| FR-02-02 | AD-UC-05 section 6 question 3 leaves optimisation failure handling open | FR-02-02-INT | 0.40 |
| FR-02-10 | AD-UC-06 section 6 question 3 leaves validation/GPS/offline rules open | FR-02-10-FE, FR-02-10-BE | 0.40 |
| FR-02-14 | AD-UC-07 section 6 question 1 leaves evidence constraints open | FR-02-14-FE, FR-02-14-BE | 0.40 |
| FR-04-06 | batch parameter constraints are incomplete; AD-UC-11 section 6 question 1 | FR-04-06-FE, FR-04-06-BE | 0.40 |
| FR-04-08 | the mass-balance metric formula is not specified in the criterion | FR-04-08-BE | 0.40 |
| FR-04-09 | the batch-balance update formula is not specified in the criterion | FR-04-09-FE, FR-04-09-BE | 0.40 |
| FR-04-10 | Eco Oil payload/acknowledgement/retry contract remains open | FR-04-10-INT | 0.40 |
| FR-05-07 | minimum withdrawal and bank-profile eligibility rules remain open | FR-05-07-FE, FR-05-07-BE | 0.40 |
| FR-05-13 | ESG report fields and environmental calculation method are not specified | FR-05-13-BE | 0.40 |
| FR-06-01 | the criterion says relevant statuses without a complete field/filter list | FR-06-01-FE, FR-06-01-BE | 0.40 |
| FR-06-02 | audit-history fields and query limits are not specified | FR-06-02-FE, FR-06-02-BE | 0.40 |
| FR-06-03 | the criterion says applicable compliance report without a report definition | FR-06-03-FE, FR-06-03-BE | 0.40 |

## Activity-node coverage

The preserved decomposition covers 66 action references across 11 ADs, mapping to 62 distinct L2 functions. Four withdrawal actions are shared across UC-12/UC-13. The other 29 functions retain their baseline source without claiming a drawn activity action. Boundary-receipt nodes c1/e1 represent ReFeed notification/export work, not implementation inside the partner's system.

Counts include system action nodes plus the two boundary-receipt nodes `c1` (UCO Collectors) and `e1` (Eco Oil System). Receipt in an external lane is represented by the corresponding ReFeed notification/export responsibility; W1 does not specify the partner system implementation.

| Activity diagram | Action nodes covered | Node → L2 function |
| --- | --- | --- |
| [AD-UC-01](../activity/AD-UC-01.md) | 5 | `validateAuth` → `FR-01-01`; `createAccount` → `FR-01-02`; `authenticateUser` → `FR-01-03`; `displayAuthSuccess` → `FR-01-04`; `displayAuthError` → `FR-01-05` |
| [AD-UC-03](../activity/AD-UC-03.md) | 6 | `s1` → `FR-01-09`; `s2` → `FR-01-10`; `s3` → `FR-01-11`; `s4` → `FR-01-12`; `s5` → `FR-01-13`; `s6` → `FR-01-14` |
| [AD-UC-04](../activity/AD-UC-04.md) | 6 | `s1` → `FR-01-15`; `s3` → `FR-01-16`; `s4` → `FR-01-17`; `s5` → `FR-01-18`; `c1` → `FR-01-19`; `s2` → `FR-01-20` |
| [AD-UC-05](../activity/AD-UC-05.md) | 7 | `s1` → `FR-02-01`; `s2` → `FR-02-02`; `s3` → `FR-02-03`; `s4` → `FR-02-04`; `s5` → `FR-02-05`; `s6` → `FR-02-06`; `s7` → `FR-02-07` |
| [AD-UC-06](../activity/AD-UC-06.md) | 6 | `s1` → `FR-02-08`; `s2` → `FR-02-09`; `s3` → `FR-02-10`; `s4` → `FR-02-11`; `s5` → `FR-02-12`; `s6` → `FR-02-13` |
| [AD-UC-07](../activity/AD-UC-07.md) | 5 | `a2` → `FR-02-14`; `a3` → `FR-02-15`; `a4` → `FR-02-16`; `a5` → `FR-02-17`; `a7` → `FR-02-18` |
| [AD-UC-08](../activity/AD-UC-08.md) | 10 | `s1` → `FR-03-01`; `s2` → `FR-03-02`; `s3` → `FR-03-03`; `s4` → `FR-03-04`; `s5` → `FR-03-05`; `s6` → `FR-03-06`; `s7` → `FR-03-07`; `s8` → `FR-03-08`; `s9` → `FR-03-09`; `s10` → `FR-03-10` |
| [AD-UC-10](../activity/AD-UC-10.md) | 5 | `s1` → `FR-04-01`; `s2` → `FR-04-02`; `s3` → `FR-04-03`; `s4` → `FR-04-04`; `s5` → `FR-04-05` |
| [AD-UC-11](../activity/AD-UC-11.md) | 5 | `s1` → `FR-04-06`; `s2` → `FR-04-07`; `s3` → `FR-04-08`; `s4` → `FR-04-09`; `e1` → `FR-04-10` |
| [AD-UC-12](../activity/AD-UC-12.md) | 7 | `retrieve` → `FR-05-01`; `displayTier` → `FR-05-02`; `validate` → `FR-05-07`; `display` → `FR-05-08`; `transaction` → `FR-05-09`; `update` → `FR-05-10`; `displayUpdated` → `FR-05-11` |
| [AD-UC-13](../activity/AD-UC-13.md) | 4 | `s1` → `FR-05-07`; `s3` → `FR-05-08`; `s2` → `FR-05-09`; `s4` → `FR-05-10` |
