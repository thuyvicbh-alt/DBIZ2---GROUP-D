# Screen Spec: S-F01-04 My pickup requests

| Field | Value |
|---|---|
| Screen ID | S-F01-04 |
| Screen name | My pickup requests |
| Actor | Supplier |
| Priority | Could |
| Feature | F-01 Supplier Onboarding & Pickup Management |
| Activity diagrams | AD-UC-04 (select an existing request and choose an action) |
| Mockup image | `img/S-F01-04.svg` (390 × 844 mobile baseline) |
| Status | Draft — design baseline |

## 1. Purpose

**Shown when:** the supplier taps **My pickups** from the supplier home or chooses **View my requests** after creating a pickup request on S-F01-03.

**The user leaves this screen when:** they select a request (→ S-F01-05), open a receipt for a completed request (→ S-F03-02), request a new pickup (→ S-F01-03), or go back to the supplier home.

## 2. Visual direction

Use a calm, operational mobile layout: a white header, a soft green canvas, one horizontal filter control, and stacked request cards. The first card is the latest request and uses a status tint only as a supporting cue; the status label and icon carry the meaning. Keep the primary action fixed at the bottom so the list remains the focus.

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | Back | Icon button | Returns to supplier home | Yes | 44 × 44 touch area |
| 2 | Page title | Header | `Yêu cầu thu gom` | Yes | Static |
| 3 | Supplier context | Text | `supplier.business_name` · `supplier.id` | Yes | Supplier ID format from UI conventions |
| 4 | Filter control | Segmented control | `Tất cả`, `Đang xử lý`, `Đã xong`; counts from request query | Yes | One segment selected |
| 5 | Section label | Caption | `MỚI NHẤT` | Yes | Static |
| 6 | Request card | Card/list row | One card per `pickup_request`, newest first | Yes | Card is tappable |
| 7 | Request identity | Metadata | `pickup_request.id` · `created_at` | Yes | `REQ-` + 4 digits; `dd/MM/yyyy` |
| 8 | Status badge | Badge | Requested, Driver Assigned, Arrived, Completed, Cancelled | Yes | Text + icon; never colour alone |
| 9 | Schedule | Heading | `scheduled_date` · `time_window` | Yes | `dd/MM/yyyy · HH:mm–HH:mm` |
| 10 | Location and volume | Supporting text | `supplier.address` · `estimated_volume` | Yes | One decimal + `L`; wrap to two lines |
| 11 | Card action | Link/button | `Xem chi tiết` or `Xem biên nhận` for Completed | Yes | 44 px row height |
| 12 | Empty state | Card | Current filter + next action message | No | See state table |
| 13 | Error banner | Banner | Load failure + retry | No | Retry keeps selected filter |
| 14 | Loading skeleton | Skeleton | Header, filter and three cards | No | Non-interactive |
| 15 | New request | Primary button | `Yêu cầu thu gom mới` | Yes | Opens S-F01-03; fixed footer |

## 4. States

| State | What the user sees | Trigger |
|---|---|---|
| Default | Three request cards, with REQ-0102 selected as the newest `Đã yêu cầu`, REQ-0098 `Hoàn tất`, and REQ-0091 `Đã hủy`; `Tất cả (3)` selected. | Query succeeds with one or more requests |
| Empty | Filter-specific card: `Chưa có yêu cầu ở trạng thái này` and `Hãy tạo yêu cầu thu gom để bắt đầu.`; primary `Yêu cầu thu gom mới` remains available. | Query returns no rows |
| Loading | Grey skeleton for title context, segmented control and three cards; `Đang tải yêu cầu…`. | Initial query or filter change |
| Error | Red banner `Không tải được danh sách yêu cầu. Kiểm tra kết nối mạng và thử lại.` and button `Tải lại danh sách`; preserve the previous list if cached. | Query fails |
| Success / confirmation | After S-F01-03 creates a request, show the new request at the top with a green toast `Yêu cầu REQ-0102 đã được tạo.`; toast dismisses after 4 seconds. | Return from request creation |

## 5. Interactions and navigation

| Element | User action | System response | Goes to |
|---|---|---|---|
| Back | Tap | Return to supplier home | Supplier main menu |
| Filter control | Tap segment | Fetch/filter requests and show Loading | Stays |
| Request card | Tap | Open request details | S-F01-05 |
| Xem biên nhận | Tap on Completed row | Open signed receipt | S-F03-02 |
| Tải lại danh sách | Tap in Error | Fetch again | Stays |
| Yêu cầu thu gom mới | Tap | Open pickup form | S-F01-03 |
| List | Scroll | Reveal older requests; header/footer remain fixed | Stays |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | Only requests belonging to the signed-in `supplier.id` are displayed. | FR-06-03 |
| SR-002 | Requests are sorted newest first by `created_at`; status filters do not change the ordering. | FR-01-05 |
| SR-003 | A request card exposes only the action allowed by its current status; Completed uses `Xem biên nhận`, while eligible active requests use `Xem chi tiết`. | FR-01-04, FR-01-05 |
| SR-004 | Status uses text, icon and colour; colour alone is never meaningful. | UI conventions §3–§5 |
| SR-005 | The fixed primary action is always available, including Empty and Error states. | UI conventions §3 |

## 7. Linked requirements

| FR ID | What this screen does for it |
|---|---|
| FR-01-05 | Displays all own pickup requests with current status and latest update context |
| FR-01-04 | Provides entry to the detail screen where an eligible request can be changed or cancelled |
| FR-06-03 | Restricts the list to the signed-in supplier |

## 8. Responsive and accessibility notes

- Designed at 390 × 844; minimum supported width 360 px.
- The list scrolls inside the page; header, filter and footer action remain visible.
- Cards have at least 44 px interactive height; the primary button is 52 px high.
- Status badges include an icon and explicit Vietnamese text; all body text meets 4.5:1 contrast.
- At 360 px, schedule text may wrap but the request ID and status badge remain on the first row.

## 9. Open questions

| # | Question | Blocking? | Default |
|---|---|---|---|
| 1 | Should a completed request open S-F03-02 or a read-only version of S-F01-05 first? | No | `Xem biên nhận` opens S-F03-02 directly. |
| 2 | Should the filter include `Đã hủy` separately? | No | Keep it inside `Tất cả`; add a separate filter only if the request volume grows. |

## Completion checklist

- [x] Screen purpose, viewport and visual direction are defined.
- [x] All visible interactive elements have an inventory entry.
- [x] Default, Empty, Loading, Error and Success states are specified.
- [x] All navigation targets exist in the Screen List.
- [x] Data fields, formats and accessibility behavior are named.
- [ ] Figma component instances and final export are still to be placed in the shared Figma file.
