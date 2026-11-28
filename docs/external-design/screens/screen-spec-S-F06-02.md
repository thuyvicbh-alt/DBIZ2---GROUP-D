# Screen Spec: S-F06-02 Audit trail and compliance reports

| Field | Value |
|---|---|
| Screen ID | S-F06-02 |
| Screen name | Audit trail and compliance reports (`Nhật ký kiểm toán & báo cáo tuân thủ`) |
| Actor | ReFeed Staff (`Nhân viên ReFeed`, `RF-004`) |
| Priority | Must |
| Feature | F-06 Operations, Audit & Compliance |
| Use cases | UC-16 (`Review audit trail and generate compliance reports`; note: `AD-UC-16` is not among the 10 remaining activity diagrams in W3, UI actions follow Screen List & `FR-06-02`/`FR-06-03`) |
| Mockup images | `img/S-F06-02-default.png` (default), five numbered state images and four state images (section 2) |
| Frame width | 370px screen inside a 390 × 844 device frame (exported at 3x with a 50px shadow margin, 1470 × 2832 pixels) |
| Figma frame | ReFeed Mobile App – ReFeed Staff Flow (`06012.pdf`, Page 2) |
| Status | Draft |

## 1. Purpose

**Shown when:** authorized ReFeed operations staff (`RF-004`, verified via `FR-06-04`) drills down from `S-F06-01` ("Xem báo cáo" `#11` or Compliance alerts tile `#8`) to review historical changes in the audit trail (collection handovers, warehouse reconciliation approvals/discrepancies, dispatch changes, and batch closing records) and generate/export compliance reports (`.CSV / ISCC`).

**The user leaves this screen when:** the staff member taps Back (`#1`) to return to `S-F06-01`, or taps the export button (`#4`) to generate and download a compliance report file to the device.

## 2. Mockup

![S-F06-02](img/S-F06-02-default.png)

**Numbered:** each number is the # of the element inventory in section 3. Elements 12–13 (Empty state message and "Xóa bộ lọc") appear only in the empty state, element 14 (Loading skeleton) only in the loading state, element 15 (Error banner) only in the error state, and element 16 (Success export card) only in the success state, so each is numbered on that state's image. Across the states, shared elements keep their numbers: the header (1–4) appears on all five images, the search input (5) appears on default, empty, and success, the filter chips (6) and log cards (8–10) appear on default, error, and success, and the SHA-256 integrity strip (7) and ISCC trace card (11) appear on default and success.

| Default, numbered | Empty, numbered | Loading, numbered | Error, numbered | Success, numbered |
| :---: | :---: | :---: | :---: | :---: |
| ![S-F06-02 default numbered](img/S-F06-02-default-numbered.png) | ![S-F06-02 empty numbered](img/S-F06-02-empty-numbered.png) | ![S-F06-02 loading numbered](img/S-F06-02-loading-numbered.png) | ![S-F06-02 error numbered](img/S-F06-02-error-numbered.png) | ![S-F06-02 success numbered](img/S-F06-02-success-numbered.png) |

| State | Image |
|---|---|
| Default | `img/S-F06-02-default.png` |
| Empty | `img/S-F06-02-empty.png` |
| Loading | `img/S-F06-02-loading.png` |
| Error | `img/S-F06-02-error.png` (sync failed because the connection was lost; cached snapshot from `10:12` shown) |
| Success | `img/S-F06-02-success.png` |

All colours, fonts and corners come from the design tokens in [ui-conventions.md](../ui-conventions.md#6-design-tokens).

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | Back | Button | static icon `<` (states 1–5) | Yes | none |
| 2 | Screen title | Header | static: "Nhật ký kiểm toán" (states 1–5) | Yes | none |
| 3 | Context subtitle | Text | `staff.code` + static scope label (`RF-004 · Truy vết thay đổi & ISCC`) (states 1–5) | Yes | none |
| 4 | Export compliance report button | Button | static download icon trigger for `.CSV / ISCC` export (states 1–5) | Yes | disabled while loading (state 3) |
| 5 | Search input | Search input | `filter.query` with placeholder "Tìm mã COL, lô B001, nhân viên..." (states 1, 2, 5; shows `COL-99999` in state 2) | No | max 64 characters; alphanumeric and `-` |
| 6 | Workflow filter chip group | Button | `filter.category` options: `Tất cả (35)`, `Đối soát`, `Điều phối`, `ISCC-EU` (states 1, 4, 5) | Yes | exactly one chip selected |
| 7 | SHA-256 integrity confirmation row | Status / row | check icon + `audit_batch.date` + `audit_batch.verified_count` (`25/09/2026 · 35 bản ghi toàn vẹn chữ ký SHA-256`) (states 1, 5) | Yes | all displayed records match valid SHA-256 chain |
| 8 | Reconciliation approval log card | List item | badge `DUYỆT ĐỐI SOÁT`, `event.timestamp` (`10:35 · 25/09`), `event.title` (`COL-00891 · Chấp nhận hao hụt -0,4 kg`), `event.detail`, `event.actor` + `event.sha256_short` (`#8f4c...e91a`) (states 1, 4, 5) | No | none |
| 9 | Warehouse alert log card | List item | badge `CẢNH BÁO KHO`, `event.timestamp` (`10:18 · 25/09`), `event.title` (`COL-00891 · Cân nhập kho 32,1 kg`), `event.detail`, `event.actor` (`WT-01234` / `WH-002`) + `event.sha256_short` (`#72b1...40c8`) (states 1, 4, 5) | No | none |
| 10 | Collection handover log card | List item | badge `THU GOM`, `event.timestamp` (`09:42 · 25/09`), `event.title` (`COL-00891 · Bún Chả Hàng Quạt (32,5 kg)`), `event.detail`, `event.actor` (`DRV-011`) + `event.sha256_short` (`#3a9d...11f2`) (states 1, 4, 5) | No | none |
| 11 | ISCC trace batch log card | List item | badge `TRUY VẾT ISCC`, `event.timestamp` (`08:50 · 25/09`), `event.title` (`BATCH-B001 · Khóa sổ chứng từ lô SAF`), `event.detail`, `event.actor` (`RF-004`) + `event.sha256_short` (`#19e0...9b7d`) (states 1, 5) | No | none |
| 12 | Empty state card | Empty state | shield-check icon + "Không tìm thấy bản ghi kiểm toán" + "Chưa có lịch sử truy vết nào khớp với mã hoặc nhóm nghiệp vụ bạn đang chọn." (state 2) | No | none |
| 13 | Clear filter button | Button | static: "Xóa bộ lọc" (state 2) | No | none |
| 14 | Loading skeleton list | Loading state | status line "Đang tải nhật ký & kiểm tra chữ ký SHA-256..." + three grey skeleton cards (state 3) | No | none |
| 15 | Offline error banner | Error banner | warning icon + "Chưa cập nhật được bản ghi mới" + "Đứt kết nối mạng. Đang hiển thị bản lưu lúc 10:12." + inline action "Thử lại" (state 4) | No | none |
| 16 | Report export success card | Success card | check icon + `export.file_name` (`Đã xuất báo cáo · audit_log_25092026.csv`) + `export.summary` ("35 bản ghi kiểm toán kèm chữ ký SHA-256 đã tải xuống thành công lúc 10:37 · 25/09/2026.") (state 5) | No | none |

## 4. States

| State | What the user sees | Trigger |
|---|---|---|
| Default | Header with Back (`#1`), title "Nhật ký kiểm toán" (`#2`), subtitle `RF-004 · Truy vết thay đổi & ISCC` (`#3`), export icon button (`#4`); search input (`#5`), filter chips (`#6`: `Tất cả (35)` selected), SHA-256 integrity strip (`#7`), and chronological audit cards (`#8`–`#11`). | Screen opened and audit history records are retrieved (`FR-06-02`). |
| Empty (no data) | Header (`#1`–`#4`), search input (`#5`) showing active query `COL-99999`, and central empty state card (`#12`: "Không tìm thấy bản ghi kiểm toán") with secondary button "Xóa bộ lọc" (`#13`). | Search query or selected workflow category filter matches zero audit records. |
| Loading | Header (`#1`–`#4`), status label "Đang tải nhật ký & kiểm tra chữ ký SHA-256..." above three grey skeleton placeholder cards (`#14`). | Screen initially retrieves audit history or verifies integrity hashes after refresh. |
| Error | Amber warning banner at top (`#15`: "Chưa cập nhật được bản ghi mới — Đứt kết nối mạng. Đang hiển thị bản lưu lúc 10:12. Thử lại"), filter chips (`#6`), and locally cached audit cards (`#8`–`#10`). | Network connection is lost while fetching new audit records; locally cached snapshot is preserved. |
| Success / confirmation | Green confirmation card at top (`#16`: "Đã xuất báo cáo · audit_log_25092026.csv — 35 bản ghi kiểm toán kèm chữ ký SHA-256 đã tải xuống thành công lúc 10:37 · 25/09/2026."), followed by search input (`#5`), filter chips (`#6`), SHA-256 integrity strip (`#7`), and audit cards (`#8`–`#11`). | Staff taps export (`#4`) and the `.CSV` compliance report downloads successfully (`FR-06-03`). |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Back (`#1`) | tap | Returns to the operations and compliance monitoring staff home | S-F06-01 |
| 2 | Export compliance report (`#4`) | tap | Generates and downloads `.CSV` compliance report with SHA-256 signatures (`FR-06-03`); shows Success card (`#16`) | stays (default $\rightarrow$ success) |
| 3 | Search input (`#5`) | type / submit | Filters audit log cards by `COL` code, batch ID (`B001`), or actor ID; transitions to Empty (`2`) if count $= 0$ | stays |
| 4 | Workflow filter chips (`#6`) | tap chip (`Tất cả (35)`, `Đối soát`, `Điều phối`, `ISCC-EU`) | Updates selected chip and filters displayed audit log cards | stays |
| 5 | Audit log card (`#8`–`#11`) | tap | Opens in-screen bottom sheet showing full event payload and SHA-256 digest (overlay, not a separate Screen ID) | stays |
| 6 | Xóa bộ lọc (`#13`) | tap (empty state) | Clears search input (`#5`) and resets filter chip (`#6`) to `Tất cả (35)` | stays (empty $\rightarrow$ default) |
| 7 | Thử lại (`#15`) | tap (error state) | Re-attempts network fetch and integrity check from cached state (`4`) | stays (loading $\rightarrow$ default/error) |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | Access to audit trail history and compliance report generation is restricted to authorized `ReFeed Staff` roles; unauthorized attempts are denied and recorded. | FR-06-02, FR-06-03, FR-06-04, FR-06-05 |
| SR-002 | Audit trail entries are append-only historical change records; banner `#7` confirms batch integrity (`35 bản ghi toàn vẹn chữ ký SHA-256`) whenever records are verified online (integrity hashing scheme subject to final audit field confirmation in W3 C8). | FR-06-02; W3 EARS precision review |
| SR-003 | Every audit card (`#8`–`#11`) displays a top row with workflow badge (`DUYỆT ĐỐI SOÁT`, `CẢNH BÁO KHO`, `THU GOM`, `TRUY VẾT ISCC`) and timestamp (`HH:mm · DD/MM`), an independent bold title line, description, and actor ID with truncated digest. | FR-06-02 |
| SR-004 | If network connectivity fails while syncing new entries, the screen displays banner `#15` with the last cached timestamp (`10:12`) and preserves cached audit cards (`#8`–`#10`) so compliance records remain inspectable offline. | FR-06-02 |
| SR-005 | Workflow category badges and status banners (`#15`, `#16`) pair distinct icons and text labels with semantic fills, never relying on colour alone. | ui-conventions §5 |

## 7. Linked requirements

| FR ID | What this screen does for it |
|---|---|
| FR-06-02 | Retrieves and displays chronological historical changes across collection handovers, warehouse reconciliation, dispatch, and batch records, with search and category filtering (`Retrieve and display audit history`). |
| FR-06-03 | Generates and exports the applicable compliance report (`.CSV / ISCC` format) and confirms download completion (`Generate compliance report`). |
| FR-06-04 | Enforces role permission check for authorized `ReFeed Staff` before exposing audit logs or generating compliance reports (`Check role permission for protected action`). |

## 8. Responsive and accessibility notes

- Standard frame width: `390px` (`370px` inner viewport with `10px` device bezel), scaling down to `360px` minimum width; filter chips (`#6`) scroll horizontally without wrapping or clipping labels.
- Card layout uses a dedicated top metadata row (status badge on the left, `HH:mm · DD/MM` timestamp on the right) above an independent title row so long titles (`COL-00891 · Bún Chả Hàng Quạt (32,5 kg)`) never collide with timestamps.
- Minimum font size across all captions, actor lines, and hashes is `12px`, maintaining at least `4.5:1` text contrast ratio per `ui-conventions.md`.

## 9. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| 1 | Confirm mandatory audit-history fields, query pagination/limits, and the exact compliance report schema/format (`.CSV` columns, SHA-256 signature inclusion, and ISCC-EU fields), as both `FR-06-02` and `FR-06-03` are flagged `EARS precise? = No` in W1. | Yes for final acceptance | Open (tracked in W3 EARS precision review & W5 Open Question #4). |
| 2 | Confirm release dependency closure between Must compliance reporting (`FR-06-02`, `FR-06-03`) and Should warehouse batch/mass-balance functions (`FR-04-06`–`FR-04-10`, e.g., `BATCH-B001` card `#11`), or approve a manual workaround for Release 1. | Yes for viable Must-only backlog | Open (tracked in W3 D-10 & W5 Open Question #9). |
| 3 | Should tapping an audit card (`#8`–`#11`) also allow copying the full 64-character SHA-256 digest to the clipboard from the detail bottom sheet? | No | Open. Default: open in-screen detail bottom sheet with full digest and copy action. |

---

## Completion checklist

- [x] The mockup is a separate cropped image file, named with the Screen ID, at the frame width (`390×844px` @2x Retina).
- [x] The numbered images exist for all five states, and their numbers match the `#` column of the element inventory (`1`–`16`).
- [x] Every visible element in the mockup appears in the element inventory.
- [x] Every colour in the mockup is a design token from `ui-conventions.md`.
- [x] Every input element has a validation rule or an explicit "none".
- [x] All five states (`Default`, `Empty`, `Loading`, `Error`, `Success`) are filled in.
- [x] Every navigation target is a Screen ID that exists in the Screen List (`S-F06-01`).
- [x] Every element that displays data names the field it displays.
- [x] Every linked FR exists in the Function List (`FR-06-02`, `FR-06-03`, `FR-06-04`), and every action of this screen is covered by an interaction row.