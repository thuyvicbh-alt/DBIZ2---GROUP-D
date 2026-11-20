# Screen Spec: S-F05-04 ESG contribution reports

| Field | Value |
|---|---|
| Screen ID | S-F05-04 |
| Screen name | ESG contribution reports |
| Actor | Supplier |
| Priority | Should |
| Feature | F-05 Supplier Value & ESG / Green Profile |
| Use cases / Sequence | UC-15 · SEQ-S-03 (no separate activity diagram; query/report flow from S-F05-03) |
| Mockup image | `img/S-F05-04-default.png` (annotated: `img/S-F05-04-default-numbered.png`) |
| Status | Draft |

## 1. Purpose

**Shown when:** the supplier opens the ESG reporting view from the Green Profile (`S-F05-03` "Reports") or selects the "Báo cáo" (Reports) navigation tab to inspect verified environmental impact metrics and generate a downloadable ESG contribution certificate (PDF) for a chosen reporting period.

**The user leaves this screen when:** the supplier downloads or shares the generated ESG report PDF (staying on the success state or opening the system file/share sheet), or navigates to another supplier screen via the bottom navigation bar (`S-F05-01`, `S-F01-04`, `S-F05-03`, `S-F01-02`).

## 2. Mockup

![S-F05-04](img/S-F05-04-default.png)

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | Supplier initials badge | Avatar badge | Derived initials from `supplier.business_name` (e.g., `"NH"`) | Yes | none |
| 2 | Quick export icon button | Icon button | Static export icon; triggers PDF generation/download for active `period` | Yes | disabled in Loading, Empty, or Error states |
| 3 | Title | Header | Static: `"Báo cáo đóng góp ESG"` (ESG contribution reports) | Yes | none |
| 4 | Period & supplier line | Subtitle text | `period.label` + `" · Đối tác "` + `supplier.supplier_id` (e.g., `Quý 3/2026 · Đối tác S042`) | Yes | formats from `ui-conventions` |
| 5 | Reporting period selector | Segmented filter | Static options: `"Tháng này"` (This month), `"Quý 3/2026"` (Current quarter), `"Năm 2026"` (Current year) | Yes | exactly one period selected at a time; defaults to current quarter |
| 6 | ESG impact summary card | KPI card | `esg_report.verified_uco_volume` (Litres), `esg_report.period_delta_pct`, `esg_report.co2_reduction_tonnes` (Tonnes), `esg_report.saf_equivalent_litres` (Litres) | Yes | computed strictly from verified collections (`collection.status = Verified`) |
| 7 | Monthly collection breakdown | Bar chart card | `esg_report.monthly_breakdown[].month_label` and `esg_report.monthly_breakdown[].verified_litres` | Yes | unit fixed to Litres (`Đơn vị: Lít`); bar heights scaled relative to max monthly volume in selected period |
| 8 | Export ESG certificate | Primary button | Static label: `"Xuất chứng nhận ESG (PDF)"` (updates to `"Đã xuất chứng nhận ESG (Hoàn tất)"` in Success state) | Yes | disabled while loading or when `verified_uco_volume = 0` |
| 9 | Status / confirmation banner | Banner | Success message with generated filename `esg_report.file_name` (e.g., `"Đã xuất chứng nhận ESG_Q3_2026.pdf"`) or inline export failure notice | No | shown only after export action completes or fails |
| 10 | Bottom navigation bar | Tab bar | Static tabs: `"Tổng quan"` (`S-F05-01`), `"Thu gom"` (`S-F01-04`), `"Báo cáo"` (`S-F05-03` / `S-F05-04`), `"Tài khoản"` (`S-F01-02`) | Yes | highlights active `"Báo cáo"` tab; badge numbered `9` when banner #9 is hidden, `10` when banner #9 is visible |

## 4. States

| State | What the user sees | Trigger |
|---|---|---|
| Default | Header with supplier badge and quick export icon; period filter pills (`Quý 3/2026` active); ESG KPI summary card (`1.480 Lít`, `+18% vs Q2`, `3,72 Tấn CO₂`, `1.184 Lít SAF`); 3-month bar chart; active green primary button `"Xuất chứng nhận ESG (PDF)"`; bottom navigation bar. | Screen opened and verified ESG metrics loaded for the selected period (`verified_uco_volume > 0`) |
| Empty (no data) | Empty-state card with leaf icon, title `"Chưa có dữ liệu báo cáo ESG"`, helper text explaining CO₂ and SAF metrics appear after the first verified UCO collection, and outlined button `"Tạo yêu cầu thu gom ngay"` (links to `S-F01-03`). KPI card, chart, and PDF export CTA are hidden. | Supplier has zero verified UCO collections in the system or selected period |
| Loading | Grey skeleton placeholders for header avatar/title, period filter bar, KPI card, monthly chart card, primary CTA button, and bottom navigation tabs; all interactive controls disabled. | Initial data fetch or period filter switch is in progress |
| Error | Error card with red alert icon, title `"Không thể tải báo cáo ESG"`, message `"Kết nối máy chủ truy xuất SAF bị gián đoạn. Vui lòng kiểm tra mạng và thử lại."`, and solid green retry button `"Thử lại"`. | Backend query for ESG metrics or PDF report generation fails / times out |
| Success / confirmation | Green confirmation banner `#9` at top of content area (`"Đã xuất chứng nhận ESG_Q3_2026.pdf"`), KPI and chart cards preserved below, primary button `#8` updated to completed state `"Đã xuất chứng nhận ESG (Hoàn tất)"`, and PDF file triggered for download. | ESG PDF report generated and delivered successfully (`FR-05-04`) |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Period filter pill (`#5`) | tap | Sets active reporting period, triggers loading state, and fetches updated verified ESG metrics and chart data (`FR-05-04`) | stays |
| 2 | Export ESG certificate button (`#8`) | tap | Requests PDF report generation from verified collection data for the active period (`FR-05-04`), triggers file download, and transitions to Success state (or Error state on failure) | stays (success or error state) |
| 3 | Quick export icon button (`#2`) | tap | Same action as tapping Export ESG certificate (`#8`) | stays (success or error state) |
| 4 | `"Tạo yêu cầu thu gom ngay"` button (Empty state) | tap | Opens the UCO pickup request form so the supplier can schedule their first collection | S-F01-03 |
| 5 | `"Thử lại"` button (Error state) | tap | Re-submits the ESG report data request and shows Loading state | stays |
| 6 | Tab `"Tổng quan"` (`#10`) | tap | Opens supplier loyalty tier and payout balance overview | S-F05-01 |
| 7 | Tab `"Thu gom"` (`#10`) | tap | Opens supplier pickup requests list | S-F01-04 |
| 8 | Tab `"Báo cáo"` (`#10`) | tap | Returns to Green Profile & verified UCO contribution history (or stays if already on `S-F05-04`) | S-F05-03 |
| 9 | Tab `"Tài khoản"` (`#10`) | tap | Opens Point-of-Origin and compliance profile | S-F01-02 |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | ESG metrics (`verified_uco_volume`, `co2_reduction_tonnes`, `saf_equivalent_litres`) and exported PDF certificates MUST be calculated strictly from warehouse-verified UCO collections (`collection.status = Verified`), excluding unverified, pending, or cancelled pickups. | FR-05-04 |
| SR-002 | CO₂ emission reduction and SAF conversion factors are system-configured constants applied uniformly on the backend; the supplier cannot edit calculated figures on this screen. | FR-05-04 |
| SR-003 | Access is restricted by role-based access control: an authenticated supplier can query and export ESG reports only for their own `Supplier_ID`. | FR-06-03 |
| SR-004 | Chart bars, growth pills, and status banners combine text labels, numeric values, and icons with colour fills—never relying on colour alone to convey meaning. | `ui-conventions` §5 |

## 7. Linked requirements

| FR ID | What this screen does for it |
|---|---|
| FR-05-04 | Lets the supplier select a reporting period, inspect verified CO₂ reduction and SAF conversion metrics, and request/download a formal ESG contribution report (PDF) built from verified collection data (`UC-15`). |
| FR-05-03 | Connects from the Green Profile (`S-F05-03`) so suppliers can turn verified UCO contribution history into shareable ESG documentation (`UC-14`). |
| FR-06-03 | Enforces supplier-level data scoping so each partner only views and exports ESG reports associated with their own `Supplier_ID`. |

## 8. Responsive and accessibility notes

- Smallest supported viewport width: `360px` (mockup baseline `430×912px`); period filter pills wrap or scroll horizontally without text truncation, and the two sub-metric tiles inside card `#6` stack vertically below `360px`.
- On tablet/desktop viewports (`>= 768px`), the KPI summary card and monthly bar chart align side-by-side in a two-column grid with the primary PDF export CTA anchored below the summary card.
- All numeric KPIs (`1.480 Lít`, `3,72 Tấn`, `1.184 Lít`) use tabular figures and explicit unit labels for screen-reader clarity.
- Body and secondary label text maintain a minimum contrast ratio of `4.5:1` against card backgrounds.

## 9. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: Should exported ESG PDF certificates include a verifiable QR code / SHA-256 document hash linking to ISCC batch mass-balance records (`FR-04-02`), or remain a summary attestation for Release 2?] | No | Open. Default: include `Supplier_ID`, reporting period, generation timestamp, and ReFeed attestation ID without exposing internal batch IDs. |
| 2 | [NEEDS CLARIFICATION: Should suppliers also be able to select a custom date range in addition to the three preset pills (Month / Quarter / Year)?] | No | Open. Default: preset Month / Quarter / Year pills only for Release 2. |

---

## Completion checklist

- [x] The mockup is a separate cropped image file, named with the Screen ID (`img/S-F05-04-default.png` and `img/S-F05-04-default-numbered.png`).
- [x] Every visible element in the mockup appears in the element inventory (`#1`–`#10`).
- [x] Every input/interactive element has a validation rule or an explicit `"none"`.
- [x] All five states (`Default`, `Empty`, `Loading`, `Error`, `Success`) are filled in and match the generated PNG mockups.
- [x] Every navigation target (`S-F01-03`, `S-F05-01`, `S-F01-04`, `S-F05-03`, `S-F01-02`) is a valid Screen ID in `screen-list.md`.
- [x] Every element that displays data names the field it displays.
- [x] Every linked FR (`FR-05-04`, `FR-05-03`, `FR-06-03`) exists in the Function List.