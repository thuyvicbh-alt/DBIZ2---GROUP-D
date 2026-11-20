# Screen Spec: S-F01-05 Pickup request detail

| Field | Value |
|---|---|
| Screen ID | S-F01-05 |
| Screen name | Pickup request detail |
| Actor | Supplier |
| Priority | Should |
| Feature | F-01 Supplier Onboarding & Pickup Management |
| Activity diagrams | AD-UC-04 (view status, modify/cancel eligible request, show locked error) |
| Mockup image | `img/S-F01-05.svg` (390 × 844 mobile baseline) |
| Modal | `Cancel this pickup request?` |
| Status | Draft — design baseline |

## 1. Purpose

**Shown when:** a supplier selects a request from S-F01-04. The screen is the single source of truth for the request's current status, appointment, location and assignment information.

**The user leaves this screen when:** they reschedule (→ S-F01-03 edit mode), cancel through the confirmation modal, return to the request list, or open a completed transaction receipt.

## 2. Visual direction

Lead with the status card, then show the appointment, origin and assignment sections in that order. The design should make “what happens next” obvious: a Requested request has two actions at the bottom; a Driver Assigned request shows tracking context and disables destructive changes; a Completed request points to the receipt.

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | Back link | Button | `Quay lại danh sách` | Yes | 44 × 44 touch area |
| 2 | Page title | Header | `Chi tiết yêu cầu` | Yes | Static |
| 3 | Request identity | Text | `pickup_request.id` · `created_at` | Yes | `REQ-` + 4 digits |
| 4 | Status card | Banner/card | Current status, latest status message and `updated_at` | Yes | Status + icon + text |
| 5 | Status timeline | Timeline | Requested → Driver Assigned → Arrived → Completed; Cancelled branch | Yes | Current node highlighted |
| 6 | Appointment section | Card | `scheduled_date`, `time_window` | Yes | Date/time conventions |
| 7 | Origin section | Card | `supplier.business_name`, `supplier.address`, `estimated_volume` | Yes | Volume > 0 L |
| 8 | Assignment section | Card | `driver.id`, `driver.name`, `vehicle.plate`; empty before assignment | Yes | Show explicit “not assigned” copy |
| 9 | Last update | Text | `pickup_request.last_status_update_at` | Yes | `dd/MM/yyyy · HH:mm` |
| 10 | Reschedule | Secondary button | `Đổi lịch` | Conditional | Enabled only while status is Requested |
| 11 | Cancel | Destructive button | `Hủy yêu cầu` | Conditional | Opens confirmation modal |
| 12 | Locked message | Banner | Explains why changes are unavailable after dispatch | Conditional | Must include next support action |
| 13 | Receipt action | Primary/secondary button | `Xem biên nhận` for Completed | Conditional | Opens S-F03-02 |
| 14 | Error banner | Banner | Update failure + retry | No | Preserve original request data |

## 4. States

| State | What the user sees | Trigger |
|---|---|---|
| Default / Requested | Status `Đã yêu cầu`; timeline at Requested; 17/11/2026 · 09:00–11:00; Quán Phở Hàng Bài, 30–50 L; no driver assigned; `Đổi lịch` and `Hủy yêu cầu` actions. | Request is eligible for change |
| Driver Assigned | Status `Đã phân tài xế`; driver and vehicle shown; timeline active at Driver Assigned; an informational banner says `Nhân viên đã điều phối tuyến. Bạn không thể đổi lịch hoặc hủy yêu cầu lúc này.`; action buttons disabled/hidden. | Driver has been dispatched |
| Loading | Skeleton status card, timeline and three detail cards; `Đang tải chi tiết yêu cầu…`. | Detail query in progress |
| Error | Banner `Không tải được chi tiết yêu cầu. Thử lại`; keep the request ID and a Back action. | Query or update fails |
| Success / confirmation | After reschedule: status `Đã cập nhật`, updated schedule visible and toast `Đã đổi lịch thu gom.`; after cancellation: status `Đã hủy` and a clear cancellation reason. | Eligible update completes |

## 5. Cancel modal

**Title:** `Hủy yêu cầu thu gom?`

**Body:** `Yêu cầu REQ-0102 sẽ không được đưa vào lịch điều phối. Bạn có thể tạo yêu cầu mới sau này.`

**Actions:** `Quay lại` (secondary) and `Hủy yêu cầu` (destructive). The destructive action is disabled while the request update is being submitted and shows an inline loading label.

## 6. Interactions and navigation

| Element | User action | System response | Goes to |
|---|---|---|---|
| Back | Tap | Return to own request list | S-F01-04 |
| Đổi lịch | Tap while Requested | Open pickup form in edit mode with current values | S-F01-03 |
| Hủy yêu cầu | Tap while Requested | Open confirmation modal | Same screen, modal |
| Modal: Hủy yêu cầu | Confirm | Cancel request, update central schedule and show Success | Same screen |
| Tracking context | Tap while Driver Assigned | Show latest driver ETA if available | Same screen / overlay |
| Xem biên nhận | Tap while Completed | Open signed receipt | S-F03-02 |
| Thử lại | Tap in Error | Fetch detail again | Same screen |

## 7. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | The supplier sees only the selected request if it belongs to their own `supplier.id`. | FR-06-03 |
| SR-002 | Reschedule and cancellation are allowed only when status is Requested. | BR-01-PICKUP-CANCELLATION, AD-UC-04 |
| SR-003 | Once a driver is dispatched, the screen explains the lock instead of presenting a misleading active action. | FR-01-04 |
| SR-004 | Cancellation always requires the confirmation modal. | Screen List modal table |
| SR-005 | Every status has an icon and text label. | UI conventions §3–§5 |
| SR-006 | A failed update never mutates the displayed original appointment until the server confirms success. | SD-UC-04 |

## 8. Linked requirements

| FR ID | What this screen does for it |
|---|---|
| FR-01-04 | Shows eligible reschedule/cancel actions, enforces the locked state and surfaces update results |
| FR-01-05 | Displays current status, scheduled time, assigned driver/vehicle and last update |
| FR-06-03 | Restricts the request detail to the signed-in supplier |

## 9. Responsive and accessibility notes

- Designed at 390 × 844; content scrolls while the action bar remains visible.
- Status timeline uses labels and icons in addition to colour.
- Destructive action is visually separated from the primary navigation and requires a second confirmation step.
- The disabled state must still explain why the action is unavailable; do not rely on opacity alone.
- Long addresses wrap without truncating the request ID or scheduled time.

## 10. Open questions

| # | Question | Blocking? | Default |
|---|---|---|---|
| 1 | What is the cancellation cutoff before dispatch? | No | Allow while status is Requested. |
| 2 | Should live driver ETA be shown in Release 1? | No | Show it only when provided by the pickup service; otherwise show assignment details. |

## Completion checklist

- [x] Status-dependent design and actions are defined.
- [x] Cancel modal copy and behavior are defined.
- [x] Five states and update failure behavior are specified.
- [x] Traceability to AD-UC-04 and FR-01-04/05 is complete.
- [ ] Figma component instances and final export are still to be placed in the shared Figma file.
