# Screen Spec: S-F03-02 Transaction receipt

| Field | Value |
|---|---|
| Screen ID | S-F03-02 |
| Screen name | Transaction receipt |
| Actor | Supplier |
| Priority | Must |
| Feature | F-03 Transaction & Balance Management |
| Activity diagrams | AD-UC-03 (actions "View transaction receipt", "Download receipt PDF") |
| Mockup image | `img/S-F03-02.png` |
| Status | Draft |

## 1. Purpose

**Shown when:** the supplier selects a completed UCO collection transaction from the transaction history (S-F03-01) or immediately after signing a collection handover to view and download the receipt.

**The user leaves this screen when:** the supplier taps Back, navigates to Transaction History ("Về lịch sử giao dịch"), navigates to Accumulated Balance ("Về số dư tích lũy" in the success state), or opens the downloaded PDF file externally.

## 2. Mockup

![S-F03-02](img/S-F03-02.png)

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | Back | Button | static | Yes | none |
| 2 | Screen Title | Header | static: "Biên nhận giao dịch" | Yes | |
| 3 | Supplier Subtitle | Text | `supplier.name` · `supplier.code` (e.g., "Quán Phở Hàng Bài · SUP-00125") | Yes | |
| 4 | Error Banner | Banner | static: "Không tải được biên nhận" + "Mất kết nối khi tạo tệp PDF. Kiểm tra kết nối mạng và thử lại." (Error state only) | No | |
| 5 | Receipt Card Title | Subheader | static: "Chi tiết giao dịch" / "Biên nhận giao dịch UCO" | Yes | |
| 6 | Transaction & Collection ID | Text | `transaction.code` · `collection.code` (e.g., "TX-00891 · COL-00891") | Yes | |
| 7 | Signing Timestamp | Key-value row | `transaction.signed_at` (format: `HH:mm · DD/MM/YYYY`) | Yes | |
| 8 | Collection Location | Key-value row | `collection.address` (e.g., "12 Hàng Bài, Hoàn Kiếm") | Yes | |
| 9 | Actual Weight | Key-value row | `collection.actual_weight` in kg (format: `0,0 kg`) | Yes | `> 0` |
| 10 | Base Price | Key-value row | `transaction.base_price` (format: `0.000 đ/kg`) | Yes | |
| 11 | Tier Bonus | Key-value row | `transaction.tier_bonus` + `supplier.tier_name` (format: `+000 đ/kg`) | Yes | |
| 12 | Applied Price | Key-value row | `transaction.applied_price` (`base_price` + `tier_bonus`) | Yes | |
| 13 | Total Amount | Highlight box | `transaction.total_amount` (`actual_weight` * `applied_price`, format: `000.000 đ`) | Yes | |
| 14 | Warehouse Status Card | Card | Header: "Trạng thái kho"; Badge: `collection.warehouse_status` (e.g., "Chờ kho xác minh"); Note: "Thông tin mang tính tham khảo, không ảnh hưởng đến số dư." | Yes | |
| 15 | Download PDF | Primary button | Label: "Tải biên nhận (PDF)" (Default/Empty/Loading) or "Thử lại tải PDF" (Error) | Yes | Disabled in Empty and Loading states |
| 16 | View History | Secondary button | Label: "Về lịch sử giao dịch" (Default & Error states) | Yes | |
| 17 | Legal Disclaimer | Footer text | static: "Chứng từ giao dịch, không thay thế hóa đơn GTGT" | Yes | |
| 18 | Empty State Card | Card | Icon + "Chưa có biên nhận" + "Biên nhận được tạo sau khi bạn ký giao dịch. Giao dịch đã ký sẽ hiển thị ở đây." + "Xem lịch sử giao dịch" button (Empty state only) | No | |
| 19 | Success Card | Card | Check icon + "Tải biên nhận thành công" + summary `collection.code` · `actual_weight` · `total_amount` (Success state only) | No | |
| 20 | File Metadata Card | Card | `file.name` (e.g., `COL-00891_bien-nhan.pdf`), `file.size` (e.g., `128 KB`), `file.downloaded_at` (Success state only) | No | |
| 21 | Open PDF / Go to Balance | Buttons | Primary: "Mở tệp PDF"; Secondary: "Về số dư tích lũy" (Success state only) | No | |

## 4. States

| State | What the user sees | Trigger |
|---|---|---|
| Default (1/5) | Full transaction receipt details, warehouse verification badge ("Chờ kho xác minh"), enabled "Tải biên nhận (PDF)" button, "Về lịch sử giao dịch" button, and legal disclaimer footer. | Screen opened with a valid signed transaction ID. |
| Empty (2/5) | Empty state card showing "Chưa có biên nhận" with explanation text and an inline "Xem lịch sử giao dịch" button; bottom "Tải biên nhận (PDF)" button is disabled. | Accessed when no signed transaction exists or receipt data is unavailable. |
| Loading (3/5) | Grey skeleton placeholders for the receipt and warehouse cards, centered text "Đang tải biên nhận...", and a disabled bottom button. | Receipt details are being fetched from the server. |
| Error (4/5) | Red alert banner at the top ("Không tải được biên nhận — Mất kết nối khi tạo tệp PDF..."), full receipt details preserved below, and primary button changes to "Thử lại tải PDF". | PDF generation or download fails due to network timeout or server error. |
| Success / confirmation (5/5) | Confirmation card with green checkmark ("Tải biên nhận thành công"), file metadata card (filename, size, download timestamp), and action buttons "Mở tệp PDF" and "Về số dư tích lũy". | PDF receipt is successfully generated and saved to the device. |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Back | tap | Returns to the previous screen | S-F03-01 (or caller) |
| 2 | Tải biên nhận (PDF) | tap | Initiates PDF generation and download; transitions to Success (5/5) or Error (4/5) state | stays |
| 3 | Thử lại tải PDF | tap (Error state) | Retries PDF generation and download | stays |
| 4 | Về lịch sử giao dịch / Xem lịch sử giao dịch | tap | Navigates to the supplier's transaction history list | S-F03-01 |
| 5 | Mở tệp PDF | tap (Success state) | Opens the downloaded PDF file using the device's native PDF viewer | External OS Viewer |
| 6 | Về số dư tích lũy | tap (Success state) | Navigates to the supplier's accumulated balance overview | S-F03-03 |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | Total amount is strictly calculated as `actual_weight * applied_price` at the moment of signing and cannot be edited on this screen. | BR-05 |
| SR-002 | Warehouse verification status (`Chờ kho xác minh` / `Đã xác minh`) is informational only and does not retroactively alter the credited balance shown on the signed receipt. | BR-06 |
| SR-003 | The legal disclaimer ("Chứng từ giao dịch, không thay thế hóa đơn GTGT") must always be visible at the bottom of the Default state. | Compliance §2 |
| SR-004 | Downloaded PDF filenames must follow the convention `{collection.code}_bien-nhan.pdf`. | ui-conventions §4 |

## 7. Linked requirements

| FR ID | What this screen does for it |
|---|---|
| FR-03-03 | Displays itemized transaction receipt details including weight, base price, tier bonus, and total payout. |
| FR-03-04 | Displays real-time warehouse verification status without blocking supplier payout visibility. |
| FR-03-05 | Generates and downloads a local PDF copy of the transaction receipt with error handling and confirmation. |

## 8. Responsive and accessibility notes

- Smallest supported width: 360px; key-value rows keep left-aligned labels and right-aligned bold values without overlapping.
- On Error (4/5), the error banner uses both a red exclamation icon and distinct border/background fill so error status is not conveyed by color alone.
- Skeleton placeholders in Loading (3/5) mirror the exact card layout of Default (1/5) to prevent layout shift upon data load.
- Body text contrast ratio meets at least 4.5:1 against card surfaces.

## 9. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: Should "Mở tệp PDF" open an in-app PDF previewer or hand off directly to the native iOS/Android file viewer?] | No | Open. Default: native OS file viewer. |
| 2 | [NEEDS CLARIFICATION: Confirm exact Screen ID for "Về số dư tích lũy" (assumed S-F03-03).] | No | Open. |

---

## Completion checklist

- [x] The mockup is a separate cropped image file, named with the Screen ID (`img/S-F03-02.png`).
- [x] Every visible element across all 5 states in the mockup appears in the element inventory.
- [x] Every input/action element has a validation rule or an explicit state behavior.
- [x] All five states (Default, Empty, Loading, Error, Success) are filled in.
- [x] Every navigation target is mapped to a Screen ID or external system action.
- [x] Every element that displays data names the field it displays.
- [x] Every linked FR and user action is covered by an interaction row.
