# Activity Diagram: AD-UC-05 Plan and dispatch a UCO collection route

| Field | Value |
|---|---|
| AD ID | `AD-UC-05` |
| Use case | UC-05 Plan and dispatch a UCO collection route |
| Feature | F-02 Collection Operations & Handover |
| Primary actor | Operations Staff (W0/W1), represented by `ReFeed Admin` in the Context Diagram |
| Lanes | ReFeed Admin · ReFeed System |
| Inputs (L3-L5) | [CVCA](../context/cvca.md) StR-02 · [Context Diagram v2.1](../context/context-diagram.md) · Cost Workshop W0 Table 1 and W1 rows 20–22 · [SD-UC-05](../use-case/SD-UC-05.md) |
| Trigger | Operations Staff requests route planning for a selected area. |
| Preconditions | Staff is authorised to plan and dispatch routes. Pending requests and the vehicle-capacity information needed for planning are available. |
| Success end state | ReFeed saves and dispatches the route with an assigned driver and vehicle, sets the included pickups to `Driver Assigned` and sends the assigned driver a notification. |
| Alternative end state | Without staff confirmation, this attempt ends before assignment, dispatch, pickup-status changes and driver notification. The draft lifecycle is open in section 6. |
| FRs covered | `FR-02-01` (FE, BE and INT work items) |
| Business rules applied | The included demand must fit vehicle capacity, and staff confirmation is required before assignment and dispatch (W1 `FR-02-01`). |
| Version / status | v1.0 · Draft for group review |
| Author | Nguyễn Đức Minh · ReFeed Group D |

## 1. Diagram

```mermaid
---
config:
  flowchart:
    curve: stepAfter
  swimlane:
    lineHops: false
---
swimlane-beta LR
  subgraph ACT["ReFeed Admin"]
    start@{ shape: sm-circ }
    a1("Choose area and request collection-route plan")
    a2("Review proposed route")
    d1{"Confirm proposed route?"}
    stop2@{ shape: fr-circ }
  end
  subgraph SYS["ReFeed System"]
    s1("Group pending pickups within vehicle capacity (FR-02-01)")
    s2("Obtain optimized visiting order from map/route data (FR-02-01)")
    s3("Display proposed collection route (FR-02-01)")
    s4("Assign driver and vehicle to route (FR-02-01)")
    s5("Save and dispatch collection route (FR-02-01)")
    s6("Update included pickups to Driver Assigned (FR-02-01)")
    o1["Pickup requests [Driver Assigned]"]
    s7("Notify assigned driver (FR-02-01)")
    stop1@{ shape: fr-circ }
  end
  start --> a1 --> s1 --> s2 --> s3 --> a2 --> d1
  d1 -->|"[yes]"| s4 --> s5 --> s6 --> o1 --> s7 --> stop1
  d1 -->|"[no]"| stop2
```

- The staff member makes the decision at `d1`. Capacity is a planning constraint at `s1`, while assignment is a committed action at `s4`.
- Google Maps API supports `s2` in the sequence diagram. The four-actor Context Diagram omits it, so the activity keeps route optimisation within the ReFeed System lane and records the discrepancy in section 6.
- The output from `s6` concerns pickups included in this route. The notification hands off to UC-06, where the driver opens the assigned route.

## 2. Node trace

| Node | Kind | Text | Traces to |
|---|---|---|---|
| `start`, `a1` | Initial, actor action | Choose an area and request a plan | W0 UC-05, W1 D21:D22 |
| `s1` | System action | Group pending pickups within vehicle capacity | `FR-02-01`, W1 D21:D22 and J21 |
| `s2` | System action | Obtain optimized visiting order | `FR-02-01`, W1 D21:D22 and J22 (Google Maps API) |
| `s3` | System action | Display proposed collection route | `FR-02-01`, W1 J20 |
| `a2`, `d1` | Actor action, decision | Review and decide whether to confirm | `FR-02-01`, W1 D21:D22 and J20 |
| `s4` | System action | Assign driver and vehicle | `FR-02-01`, W1 D21:D22 and J21 |
| `s5` | System action | Save and dispatch route | `FR-02-01`, W1 D21:D22 and J21 |
| `s6`, `o1` | System action, object | Set included pickup requests to `Driver Assigned` | `FR-02-01`, W1 J21 |
| `s7` | System action | Send notification to the assigned driver | `FR-02-01`, W1 D21:D22 and J21 |
| `stop1`, `stop2` | Activity finals | Dispatch complete or current attempt ended before confirmation | `FR-02-01` confirmation gate, with the draft lifecycle open in section 6 |

## 3. Branches and acceptance tests

The values below are test fixtures. Use the same unit for estimated pickup demand and vehicle capacity.

| Branch | Given | When | Then |
|---|---|---|---|
| `d1` `[yes]` | Area A-01 has pending pickups P-01, P-02 and P-03 of 80, 70 and 50 units. The proposed group totals 200 units against 250 units of capacity, and an optimized visiting order is available. D-01 and V-01 are available for assignment. | Operations Staff confirms the proposed route. | ReFeed saves and dispatches a route for D-01 and V-01 in the proposed visiting order, sets P-01 through P-03 to `Driver Assigned`, and sends the route notification to D-01. |
| `d1` `[no]` | The same proposal is available for review. The pickups are still pending. | Operations Staff ends the attempt without confirming. | No assignment, route dispatch, pickup-status update or driver notification results from this attempt. Draft retention or revision remains open. |

| Constraint check | Given | Expected result |
|---|---|---|
| Capacity at the boundary | Proposed demand of 200 units and capacity of 200 units | The group meets the capacity constraint. |
| Capacity exceeded | Proposed demand of 210 units and capacity of 200 units | The three pickups cannot form one group for that vehicle. |
| Scope of the update | A confirmed route includes P-01 and P-02. P-03 is outside it. | Only P-01 and P-02 become `Driver Assigned` through this dispatch. |

## 4. Object states used

| Object | States in this diagram | Related states from W1 |
|---|---|---|
| Pickup request | `Driver Assigned` (written) | `Requested` (UC-03), `Driver Assigned` (UC-05), `Arrived` and `Cancelled` (UC-06), `Completed` (UC-08) |
| Collection route | Proposed, saved and dispatched are workflow milestones | The persisted route-status vocabulary is open. |

## 5. Screens touched

| Actor action | Screen ID |
|---|---|
| Choose an area and request route planning | `S-F02-01` · Dispatch queue |
| Review and confirm the proposed route | `S-F02-02` · Route review and dispatch |

Screen IDs follow the [Screen List](../external-design/screen-list.md) and [Screen Transition](../external-design/screen-transition.md). The [Usage Flow](../external-design/usage-flow.md) still references `SCR-ADM-01`. The driver handoff opens `S-F02-03` in UC-06.

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | What happens to a proposal when staff declines confirmation or leaves the review? | Yes for the draft lifecycle | The current attempt stops at `stop2` before any confirmation-dependent work. |
| 2 | Which pickup statuses qualify as pending, and who chooses eligible driver and vehicle candidates? | Yes for detailed selection rules | Read pending requests and capacity inputs. Treat candidate selection and availability as inputs for the successful scenario. |
| 3 | How should ReFeed handle no feasible group, an optimisation error, stale capacity or availability, and a notification failure? | Yes for failure and recovery tests | The diagrams specify the confirmation gate and successful path. Failure handling is open. |
| 4 | Should the Context Diagram include Google Maps API, and should `Operations Staff`, `ReFeed Staff` and `ReFeed Admin` use one shared label? | Yes for integrated review | Use the Context Diagram label in the activity and sequence. Preserve workbook and screen labels in their source references. |

## 7. Verification log

| Check | Result |
|---|---|
| Mermaid rendering | Passed on Mermaid Live v12.2.0 (2026-11-07). Both lanes and guards render, with straight or right-angle connectors. |
| Node and edge inventory | 14 nodes, 13 edges, two lanes and one decision. |
| Guards and merges | `[yes]` and `[no]` exhaust `d1`. Each action has one incoming edge. No fork or loop. |
| FR coverage | 1 of 1. All seven system actions trace to `FR-02-01`. |
| Context, sequence and screens | Context labels, sequence confirmation gate and screen IDs checked. Remaining source discrepancies are in section 6 and the [Activity Diagram Index](activity-index.md). |
| Review findings | AI review and source comparison are recorded in the [AI log](../project/ai-log.md). Peer review is pending. |
