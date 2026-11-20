# Screen Spec: S-F06-02 Audit trail and compliance reports

| Field | Value |
|---|---|
| Screen ID | S-F06-02 |
| Screen name | Audit trail and compliance reports |
| Actor | ReFeed Staff (Authorised Admin / Compliance Staff) |
| Priority | Must |
| Feature | F-06 Operations, Audit & Compliance |
| Use cases / Sequence | UC-17 · SEQ-S-03 · `audit-report-flow` · `compliance-flow` (no separate activity diagram; audit & compliance export flow) |
| Mockup image | `img/S-F06-02-default.png` (annotated: `img/S-F06-02-default-numbered.png`) |
| Status | Draft |

## 1. Purpose

**Shown when:** authorised ReFeed admin or compliance staff open the audit log from the staff main menu (`"Kiểm toán"` tab) or drill down from the `"Tỷ lệ tuân thủ"` KPI card on `S-F06-01` to inspect immutable UCO batch custody records, cryptographic document hashes, and generate ISCC EU compliance report packages.

**The user leaves this screen when:** staff export and download the ISCC compliance audit package (staying on the success state or opening the system download sheet), or navigate to another staff screen via the bottom navigation bar (`S-F06-01`, `S-F02-01`, `S-F00-01`).

## 2. Mockup

![S-F06-02](img/S-F06-02-default.png)

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | Compliance role initials badge | Avatar badge | Derived role initials from authenticated staff profile (e.g., `"KT"` for Kiểm toán) | Yes | none |
| 2 | Quick download icon button | Icon button | Static download icon; triggers ISCC compliance package export for current filter/period | Yes | disabled in Loading, Empty, or Error states |
| 3 | Title | Header | Static: `"Nhật ký kiểm toán"` (Audit trail) | Yes | none |
| 4 | Compliance standard & period subtitle | Subtitle text | `compliance.standard_label` + `" · "` + `period.label` (e.g., `Chuẩn ISCC EU · Tháng 09/2026`) | Yes | formats from `ui-conventions` |
| 5 | Search & record filter bar | Search input + filter chip | Search query input (`audit_query.keyword`, placeholder `"Tìm mã lô UCO, biển số xe..."`) and status filter pill (`"Tất cả ("` + `audit_log.total_count` + `")"`, e.g., `Tất cả (128)`) | Yes | max 64 chars; trims leading/trailing whitespace; accepts alphanumeric, `-`, `.`, and spaces |
| 6 | Audit log entry card #1 | Read-only custody card | `audit_entry.batch_id` (`Lô #UCO-2026-0925-01`), `audit_entry.verification_status` (`"Hợp lệ"`), `audit_entry.origin_name` -> `audit_entry.warehouse_name` & `audit_entry.volume_litres` (`320 L`), `vehicle.plate_number` (`29C-123.45`), `driver.driver_id` (`D018`), `audit_entry.document_hash` (`0x8f4c...e21a`), `audit_entry.recorded_at` (`09:42 25/09/2026`) | Yes | read-only immutable record; hash displayed in truncated checksum format |
| 7 | Audit log entry card #2 | Read-only custody card | Same schema as `#6` for next chronological record (`Lô #UCO-2026-0925-02`, `450 L`, `Xe 29H-456.78 · Tài xế D024`, `Hash: 0x3b9a...77c4 · 08:15 25/09/2026`) | No | shown when `audit_log.total_count >= 2` |
| 8 | Export ISCC audit package button | Primary button | Static label: `"Xuất gói hồ sơ kiểm toán ISCC"` (updates to `"Đã xuất hồ sơ kiểm toán ISCC"` in Success state) | Yes | disabled while loading or when `audit_log.total_count = 0` |
| 9 | Export confirmation banner | Banner | Confirmation message with generated archive filename `compliance_report.file_name` (e.g., `"Đã xuất hồ sơ kiểm toán ISCC_092026.zip"`) or inline export error notice | No | shown only in Success state after compliance report generation completes |
| 10 | Bottom navigation bar | Tab bar | Static tabs: `"Giám sát"` (`S-F06-01`), `"Tuyến xe"` (`S-F02-01`), `"Kiểm toán"` (`S-F06-02`), `"Cài đặt"` (`S-F00-01`) | Yes | highlights active `"Kiểm toán"` tab; badge numbered `9` when banner #9 is hidden, `10` when banner #9 is visible |

## 4. States

| State | What the user sees | Trigger |
|---|---|---|
| Default | Header with `"KT"` badge, download icon, title `"Nhật ký kiểm toán"`, and `"Chuẩn ISCC EU · Tháng 09/2026"`; search box and filter chip `"Tất cả (128)"`; immutable UCO audit cards (`#UCO-2026-0925-01` and `#UCO-2026-0925-02`) with green `"Hợp lệ"` pills and SHA-256 hash previews; solid green CTA `"Xuất gói hồ sơ kiểm toán ISCC"`; bottom navigation bar. | Screen opened by authorised staff and historical audit entries loaded (`total_count > 0`) |
| Empty (no data) | Empty-state card with green document icon, title `"Chưa có bản ghi kiểm toán nào"`, helper text explaining UCO batch traceability history and ISCC digital signatures are logged automatically, and outlined button `"Tải lại nhật ký kiểm toán"`. Search bar, audit cards, and export CTA are hidden. | No audit entries exist in the system or match the active search/filter criteria |
| Loading | Grey skeleton placeholders for header avatar/title, search/filter bar, audit summary/log cards, export CTA button, and bottom navigation tabs; all interactive controls disabled. | Initial audit history query, search/filter query, or compliance report generation is in progress |
| Error | Error card with red alert triangle icon, title `"Lỗi truy xuất chuỗi kiểm toán"`, message `"Không thể xác thực chữ ký dữ liệu lô UCO. Vui lòng tải lại để đồng bộ bản ghi."`, and solid green button `"Tải lại dữ liệu"`. | Backend audit query, hash verification check, or compliance report export fails / times out |
| Success / confirmation | Green confirmation banner `#9` (`"Đã xuất hồ sơ kiểm toán ISCC_092026.zip"`), audit log cards `#6`–`#7` preserved below, primary button `#8` updated to completed state `"Đã xuất hồ sơ kiểm toán ISCC"`, and ZIP compliance package triggered for download. | ISCC compliance report package generated and downloaded successfully (`FR-06-02`) |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Search input (`#5`) | type / submit | Filters historical audit trail entries by batch ID (`#UCO-...`), vehicle plate, or driver ID (`FR-06-02`) | stays |
| 2 | Status filter chip (`#5`) | tap | Cycles or opens status filter (`Tất cả`, `Hợp lệ`, `Cảnh báo sai lệch`) and refreshes matching count | stays |
| 3 | Export ISCC audit package button (`#8`) | tap | Generates the ISCC EU compliance report archive (`ZIP`/`PDF` + mass-balance & signature log) from verified historical records (`FR-06-02`), triggers download, and transitions to Success state | stays (success or error state) |
| 4 | Quick download icon button (`#2`) | tap | Same action as tapping Export ISCC audit package (`#8`) | stays (success or error state) |
| 5 | `"Tải lại nhật ký kiểm toán"` button (Empty state) | tap | Clears active search filters, enters Loading state, and re-queries audit records | stays |
| 6 | `"Tải lại dữ liệu"` button (Error state) | tap | Enters Loading state and retries audit log retrieval & hash verification | stays |
| 7 | Tab `"Giám sát"` (`#10`) | tap | Opens operations and compliance monitoring dashboard | S-F06-01 |
| 8 | Tab `"Tuyến xe"` (`#10`) | tap | Opens pending pickup dispatch queue | S-F02-01 |
| 9 | Tab `"Kiểm toán"` (`#10`) | tap | Stays on audit trail and compliance reports screen or scrolls to top | S-F06-02 |
| 10 | Tab `"Cài đặt"` / Staff avatar (`#10` / `#1`) | tap | Opens staff session / sign-out screen (`S-F00-01`) | S-F00-01 |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | Only authenticated users with an authorised Admin or Compliance Staff role may view audit trail entries or generate compliance reports; denied access attempts MUST be blocked and recorded. | FR-06-02, FR-06-03 |
| SR-002 | Audit trail records displayed on `S-F06-02` are strictly append-only and immutable; no UI action allows editing or deleting historical collection quantities, timestamps, or document hashes. | FR-06-02 |
| SR-003 | Every compliance report export action logs the requesting `staff_id`, filter scope, export timestamp, and output package checksum into the system audit trail. | FR-06-02 |
| SR-004 | Verification badges (`"Hợp lệ"`), hash strings, and status banners combine text labels, monospaced checksums, and icons with colour fills—never relying on colour alone. | `ui-conventions` §5 |

## 7. Linked requirements

| FR ID | What this screen does for it |
|---|---|
| FR-06-02 | Enables authorised admin and compliance staff to search and inspect historical custody changes, verify document hashes, and generate ISCC EU compliance report packages (`UC-17`). |
| FR-06-01 | Serves as the compliance drill-down target from `S-F06-01` when staff inspect underlying audit evidence for operational compliance rates (`UC-16`). |
| FR-06-03 | Enforces role-based access control over sensitive cross-supplier audit records and compliance report exports. |

## 8. Responsive and accessibility notes

- Smallest supported viewport width: `360px` (mockup baseline `430×912px`); the search input and filter chip (`#5`) stack cleanly if needed below `360px`, and origin-to-warehouse labels wrap to two lines without clipping volume or hash fields.
- On tablet/desktop viewports (`>= 768px`), audit log entries render as a structured multi-column data table (`Batch ID`, `Origin -> Warehouse`, `Volume`, `Driver / Vehicle`, `Document Hash`, `Timestamp`, `Status`) with sticky export controls.
- Cryptographic hashes (`0x8f4c...e21a`) include full-value tooltips/copy support and accessible labels so screen readers read batch IDs and verification statuses clearly.
- All body text, metadata lines, and status badges (`#0D7A5F` on `#E6F4F0`) maintain at least a `4.5:1` contrast ratio.

## 9. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: Should tapping an individual audit card (`#6` or `#7`) expand an inline diff/history timeline modal showing every state transition (`Requested` -> `Arrived` -> `Signed` -> `Warehouse Verified`), or remain a flat chronological list for Release 1?] | No | Open. Default: flat chronological list with exportable full history in the ZIP package for Release 1. |
| 2 | [NEEDS CLARIFICATION: Should the exported ISCC compliance package (`ISCC_092026.zip`) bundle both a human-readable PDF summary and machine-readable CSV/JSON mass-balance ledger?] | No | Open. Default: bundle PDF summary + CSV ledger in a single ZIP archive. |

---

## Completion checklist

- [x] The mockup is a separate cropped image file, named with the Screen ID (`img/S-F06-02-default.png` and `img/S-F06-02-default-numbered.png`).
- [x] Every visible element in the mockup appears in the element inventory (`#1`–`#10`).
- [x] Every input/interactive element has a validation rule or an explicit `"none"`.
- [x] All five states (`Default`, `Empty`, `Loading`, `Error`, `Success`) are filled in and match the generated PNG mockups.
- [x] Every navigation target (`S-F06-01`, `S-F02-01`, `S-F06-02`, `S-F00-01`) is a valid Screen ID in `screen-list.md`.
- [x] Every element that displays data names the field it displays.
- [x] Every linked FR (`FR-06-02`, `FR-06-01`, `FR-06-03`) exists in the Function List.