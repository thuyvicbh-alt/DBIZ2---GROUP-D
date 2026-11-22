# Screen Spec: S-F01-02 Point-of-Origin and compliance profile

| Field | Value |
|---|---|
| Screen ID | S-F01-02 |
| Screen name | Point-of-Origin and compliance profile |
| Actor | Supplier |
| Priority | Must |
| Feature | F-01 Supplier Portal & Account Management |
| Activity diagrams | AD-UC-03 (action "See prompt to update supplier profile" after `d1` [No: GPS or compliance data missing] leads to this screen; the update itself is UC-02, which has no activity diagram of its own) |
| Mockup images | `img/S-F01-02-default.png` (default), `img/S-F01-02-default-numbered.png`, four state images and four numbered state images (section 2) |
| Frame width | 390 × 844 px device frame (minimum supported width 360 px) |
| Figma frame | Figma file "S-F01-02 · Point-of-Origin and compliance profile" |
| Status | Draft |

## 1. Purpose

**Shown when:** the supplier opens "Profile" from the Supplier main menu, or taps the profile-incomplete prompt on S-F01-03 ("Complete profile").

**The user leaves this screen when:** the supplier taps Back, or the profile is saved and the supplier goes back to the main menu.

## 2. Mockup

![S-F01-02](img/S-F01-02-default.png)

**Numbered:** each number is the # of the element inventory in section 3. Element 4 (eligibility pill) appears in the default and success states only. Element 12 (banner) appears in the empty state as a warning and in the error state as an error banner. Element 13 ("Lấy vị trí hiện tại") appears only in the empty state, inside the greyed map. Element 14 (loading text) appears only in the loading state and element 15 (toast) only in the success state. In the loading state the fields and the map are replaced by skeleton blocks, so only 1, 2, 3, 11 and 14 are numbered.

| Default, numbered | Empty, numbered | Loading, numbered | Error, numbered | Success, numbered |
|---|---|---|---|---|
| ![S-F01-02 default numbered](img/S-F01-02-default-numbered.png) | ![S-F01-02 empty numbered](img/S-F01-02-empty-numbered.png) | ![S-F01-02 loading numbered](img/S-F01-02-loading-numbered.png) | ![S-F01-02 error numbered](img/S-F01-02-error-numbered.png) | ![S-F01-02 success numbered](img/S-F01-02-success-numbered.png) |

| State | Image |
|---|---|
| Default | [`img/S-F01-02-default.png`](img/S-F01-02-default.png) |
| Empty | [`img/S-F01-02-empty.png`](img/S-F01-02-empty.png) (no GPS and no contact yet, AD-UC-03 `d1` [No]) |
| Loading | [`img/S-F01-02-loading.png`](img/S-F01-02-loading.png) |
| Error | [`img/S-F01-02-error.png`](img/S-F01-02-error.png) (network error when saving) |
| Success | [`img/S-F01-02-success.png`](img/S-F01-02-success.png) |

All colours, fonts and corners come from the design tokens in
[ui-conventions.md](../ui-conventions.md#6-design-tokens).

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | Back | Button | static | Yes | none |
| 2 | Title and context line | Header | static "Điểm thu gom" + `supplier.supplier_id` · `supplier.business_name` | Yes | formats from ui-conventions §2 |
| 3 | Card title | Header | static "Thông tin cơ sở" | Yes | none |
| 4 | Eligibility pill | Pill | static "Đủ điều kiện đặt lịch thu gom" with a check icon; shown when the profile holds GPS (FR-01-03) | No | text + icon (ui-conventions §5) |
| 5 | Pickup point address | Text input | `point_of_origin.address`; label "Điểm thu gom" | Yes | not empty |
| 6 | GPS | Text input with pin icon | `point_of_origin.gps` (lat, lng, 4 decimals); label "Vị trí GPS"; placeholder "Chưa có vị trí" | Yes | valid latitude and longitude |
| 7 | Map | Map preview | pin on `point_of_origin.gps`, with the coordinates shown on a chip; without GPS the map is greyed with "Chưa có vị trí GPS" | Yes | illustrative map; the pin is the location that is saved |
| 8 | Confirm-pin switch | Switch | label "Ghim đúng địa chỉ thực tế của cơ sở"; note "Bạn đã xác nhận vị trí này. Kéo bản đồ để chỉnh nếu cần." (on) or "Cần có vị trí GPS trước khi xác nhận." (off); `point_of_origin.location_confirmed` | Yes | can be turned on only when GPS exists |
| 9 | Contact person | Text input | `point_of_origin.contact_name`; label "Người liên hệ tại điểm thu gom"; placeholder "Nhập tên người liên hệ" | Yes | not empty |
| 10 | Contact phone | Text input | `point_of_origin.contact_phone`; label "Số điện thoại liên hệ"; placeholder "Nhập số điện thoại liên hệ"; shown as `0123 456 789` | Yes | valid phone number |
| 11 | Lưu | Primary button | static label | Yes | disabled while a required field is missing, while loading, and when nothing has changed |
| 12 | Banner | Banner | warning "Chi tiết điểm thu gom của bạn còn thiếu vị trí GPS." + link "Lấy vị trí GPS" (empty state); error "Không lưu được thay đổi. Kiểm tra kết nối mạng và thử lại." + link "Thử lại" (error state) | No | the whole banner is tappable; icon + text + colour (ui-conventions §3, §5) |
| 13 | Lấy vị trí hiện tại | Secondary button (outline) | static, inside the greyed map | No | none |
| 14 | Loading text | Text | static "Đang tải thông tin…" | No | none |
| 15 | Toast | Toast | static "Đã lưu thông tin" with a check icon | No | disappears after 4 seconds (ui-conventions §3) |

## 4. States

| State | What the user sees | Trigger |
|---|---|---|
| Default | Header "Điểm thu gom" / "SUP-00125 · Quán Phở Hàng Bài" with back button; card "Thông tin cơ sở" with the pill "Đủ điều kiện đặt lịch thu gom", the address "12 Hàng Bài, Hoàn Kiếm, Hà Nội", GPS "21.0245, 105.8412", the map with a green pin and the coordinate chip, the switch on, the contact "Trần Minh Anh" and the phone "0123 456 789"; footer button "Lưu" enabled. The mockup shows this state. | The profile is complete |
| Empty (no data) | Warning banner "Chi tiết điểm thu gom của bạn còn thiếu vị trí GPS." with "Lấy vị trí GPS"; the address filled, GPS "Chưa có vị trí", the map greyed with "Chưa có vị trí GPS" and the button "Lấy vị trí hiện tại", the switch off and greyed with "Cần có vị trí GPS trước khi xác nhận.", the contact fields showing the placeholders; no pill; footer button disabled | The profile has no GPS (opened from S-F01-03 after AD-UC-03 `d1` [No], or from the main menu) |
| Loading | Card "Thông tin cơ sở" with skeleton blocks instead of the address field, the map and the two contact fields, and the text "Đang tải thông tin…"; footer button disabled | The profile is being loaded |
| Error | Red banner "Không lưu được thay đổi. Kiểm tra kết nối mạng và thử lại." with "Thử lại"; all entered values kept; footer button enabled | The save request failed |
| Success / confirmation | The default content with the pill and the switch on, a dark toast "Đã lưu thông tin" above the footer; footer button disabled | The profile is saved (FR-01-02) |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Banner "Lấy vị trí GPS" and button "Lấy vị trí hiện tại" | tap | Asks the device for its current location, fills the GPS field and moves the pin on the map | stays |
| 2 | Map | drag | Moves the pin and updates the GPS value | stays |
| 3 | Switch "Ghim đúng địa chỉ thực tế của cơ sở" | tap | Turns the confirmation on or off; not available while there is no GPS | stays |
| 4 | Address, Contact person, Contact phone | type | Opens the keyboard | stays |
| 5 | Lưu | tap | Validates and saves the Point-of-Origin data (FR-01-02), then shows the success toast, or the error banner if the request fails | stays |
| 6 | Banner "Thử lại" (error state) | tap (the whole banner) | Repeats the save | stays |
| 7 | Back | tap | Returns without saving | Supplier main menu |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | A pickup request is accepted only when the profile holds GPS and the compliance data given at registration; the pill is shown when this holds, and the warning banner when GPS is missing. | FR-01-03; AD-UC-03 `d1` |
| SR-002 | The GPS value is set by "Lấy vị trí hiện tại" or by moving the pin, and the saved location is the pin shown on the map. | FR-01-02 |
| SR-003 | The location can be confirmed with the switch only when GPS exists. | FR-01-02 |
| SR-004 | If saving fails, the entered values stay on the screen so the supplier can retry. | ui-conventions §4 (network / save error) |
| SR-005 | Banners are tappable as a whole, and statuses and errors are shown with text and icon, never by colour alone. | ui-conventions §3, §5 |

## 7. Linked requirements

| FR ID | What this screen does for it |
|---|---|
| FR-01-02 | Lets the supplier view and update the Point-of-Origin data (address, GPS, confirmed location and on-site contact) stored against the Supplier_ID |
| FR-01-03 | Shows whether the profile is complete enough to request a pickup, and gives the way to fix a missing GPS |

## 8. Responsive and accessibility notes

- Smallest supported width: 360px; the map keeps its height and scales in width.
- The card scrolls when the content is taller than the screen; the footer button stays fixed at the bottom.
- Touch targets at least 44 × 44: back button 44 × 44, the inputs at least 44px high, "Lấy vị trí hiện tại" about 44px high, the footer button 52px high; the switch and its label form one row at least 44px high.
- Every field has a visible label above it; the placeholders are examples only.
- Status and errors use text plus icon; the pill and the banners are never colour alone.
- Body text contrast at least 4.5:1.

## 9. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: are the contact person and phone required for the profile to count as complete ("Đủ điều kiện đặt lịch thu gom")? FR-01-03 names only GPS and compliance data.] | No | Open. Default: contact fields are required to save; only GPS blocks a pickup request. |
| 2 | [NEEDS CLARIFICATION: S-F01-01 no longer collects the address, but the address is already filled in the empty state. Where does it come from the first time?] | No | Open. Default: the supplier types it on this screen; the mockup shows it pre-filled. |
| 3 | [NEEDS CLARIFICATION: the screen name still says "compliance", but tax ID and business licence are entered on S-F01-01; this screen holds the pickup point and the on-site contact.] | No | Open. Default: name kept as in the Screen List; rename to "Pickup point profile" when the Screen List is next updated. |
| 4 | [NEEDS CLARIFICATION: ui-conventions §1 asks for a verb + object on every button; the mockup says "Lưu".] | No | Open. Default: as in the mockup; suggested label "Lưu thông tin điểm thu gom". |

---

## Completion checklist

- [x] The mockup is a separate cropped image file, named with the Screen ID, at the frame width.
- [x] The numbered image exists, and its numbers match the # column of the element inventory.
- [x] Every visible element in the mockup appears in the element inventory.
- [ ] Every colour in the mockup is a design token from ui-conventions.md.
- [x] Every input element has a validation rule or an explicit "none".
- [x] All five states are filled in.
- [ ] Every navigation target is a Screen ID that exists in the Screen List (Supplier main menu has none, see S-F00-01 open question 3).
- [x] Every element that displays data names the field it displays.
- [ ] Every linked FR exists in the Function List, and every action of this screen in AD-UC-03 is
      covered by an interaction row.