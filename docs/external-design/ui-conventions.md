# UI Conventions: ReFeed System

## 1. Language and tone
- **Interface language:** Vietnamese by default, because every end user (supplier, driver / collector, warehouse staff, ReFeed staff) works in Vietnamese; English is available in user settings. Screen IDs, FR IDs and record IDs (`SUP-00125`, `PK-00125`, `COL-00891`) are never translated.
- **Form of address:** Address the user directly as "bạn" (English: "you"). Never "quý khách" or third person.
- **Tone:** Short, clear and operational. Use plain words instead of internal field names (e.g., "Khối lượng thực tế" instead of `Actual_Quantity`; "Điểm phát sinh (Point-of-Origin)" the first time the term appears on a screen).
- **Button labels:** Always a specific **verb + object** that states the outcome (e.g., "Đặt lịch thu gom", "Mở điểm thu gom", "Xác nhận & ký điện tử", "Rút tiền", "Quét mã thu gom", "Quản lý lô hàng"). Never generic labels like "OK", "Gửi", "Có" or "Xác nhận" on its own. The only short label allowed is "Thử lại" on a retry action inside an error message.

## 2. Formats
| Item | Format | Example |
|---|---|---|
| Date | `dd/MM/yyyy` | `25/09/2026` |
| Time | `HH:mm` (24-hour) | `10:24` |
| Date and time of a record | `HH:mm · dd/MM/yyyy` | `10:24 · 25/09/2026` |
| Pickup window | `dd/MM/yyyy · HH:mm–HH:mm`; on a stop card or screen header where the date is already shown: `HH:mm - HH:mm` | `25/09/2026 · 08:00 - 10:00` · `08:00 - 10:00` |
| UCO quantity (actual, verified) | 1 decimal + ` kg`; decimal comma in Vietnamese, decimal point in English | `32,5 kg` (VI) · `32.5 kg` (EN) |
| Estimated volume (pickup request) | range in whole kg | `30 - 50 kg` |
| Money | integer, `.` as thousands separator + ` đ` | `334.750 đ` |
| Unit price and loyalty bonus | integer + ` đ/kg`; a bonus always carries `+` | `10.000 đ/kg` · `+300 đ/kg` |
| Loyalty tier | `Hạng` + number · bonus | `Hạng 2 · +400 đ/kg` |
| Supplier ID | `SUP-` + 5 digits | `SUP-00125` |
| Pickup request ID | `PK-` + 5 digits | `PK-00125` |
| Collection ID | `COL-` + 5 digits | `COL-00891` |
| Warehouse ID | `WT-` + 5 digits | `WT-01234` |
| Driver ID | `D` + 3 digits | `D018` |
| Vehicle | Vietnamese licence plate | `29C-123.45` |
| Batch ID | `B` + 3 digits | `B001` |
| Evidence photo | `IMG-` + 3 digits | `IMG-001` |
| GPS position | latitude, longitude, 5 decimals; accuracy as `±` + whole m | `21.02080, 105.85070 · ±8 m` |

Warehouse identifiers always keep the exact `WT-` + five-digit format, including the hyphen and zero padding (for example, `WT-01234`) wherever `warehouse.id` appears.

## 3. Components
All components live in the Figma file *ReFeed – S-F05-01* (page 1 "Components", page "Trang chủ theo actor" and page "Trang chủ v2 (lời chào & lối tắt)", which holds the current home screens); colors come from the variable collection `ReFeed colors`, typography is Inter only. The four driver and supplier screens with five states each (`S-F02-03`, `S-F02-04`, `S-F02-05`, `S-F03-01`) are on page "4 màn × 5 trạng thái" of the Figma file *ReFeed – 4 màn × 5 trạng thái* and use the same variables (section 6).

| Component | When to use | Notes |
|---|---|---|
| **Primary button** (`Button / Primary`) | The single main action of a screen (e.g., "Đặt lịch thu gom", "Mở điểm thu gom", "Quét mã thu gom"). | At most one per screen; full width on mobile; 52 px high, 16 Bold, fill `color/green`. |
| **Secondary button** (`Button / Ghost`) | Alternative actions (e.g., "Đổi lịch thu gom", "Quản lý lô hàng", "Xem số dư"). | White fill with `color/green` outline; never competes with the Primary button. |
| **Home header** (home screen of each actor: supplier home, `S-F02-03`, warehouse home, `S-F06-01`) | Top bar with `Avatar / Initials` on the left (40 × 40, `color/green-light` circle, `color/green-dark` initials) and `Notification button` on the right (44 × 44, red count only when there is something unread); below it the user or site name (22 Extra Bold) and one context line (12 Regular, e.g. "SUP-00125 · 12 Hàng Bài, Hoàn Kiếm"). No greeting line. | Initials are 2 letters of the business or role name (PH, TX, KH, RF). The unread count sits on the top-right corner of the bell and must never be clipped by its parent frame. Unread counts appear where the screen-transition diagram has a notification entry point: supplier ("draft agreement ready" → `S-F03-01`) and driver ("dispatch notification" → `S-F02-03`). |
| **Screen header** (every other screen, including the staff main menu) | Title (18 Bold) + one line of context (12 Regular). | Text only, no buttons or avatars. |
| **Shortcut tile** (`Shortcut tile`) | Supplier home: one key figure that opens the screen behind it, following the screen-transition diagram (open requests → `S-F01-04`, next pickup → `S-F01-05`, UCO delivered → `S-F05-03`, latest receipt → `S-F03-02`). | 2 × 2 grid; icon, label, value (18 Extra Bold), short note; the whole tile is the tap target. Never repeats a bottom-nav entry as a plain link. |
| **Destructive button** (`Button / Destructive`) | Irreversible actions inside a modal only (e.g., "Hủy yêu cầu thu gom", "Hủy lần thu gom"). | Fill `color/danger`; never placed directly on a screen, always behind a confirmation modal. |
| **Status badge** (`Status badge`) | Status of a pickup or transaction: `Đã gửi yêu cầu` (Requested), `Đã phân tài xế` (Driver Assigned), `Đã đến` (Arrived), `Hoàn tất` (Completed), `Đã hủy` (Cancelled), `Chờ ký` (awaiting signature). | Always text + icon + color, never color alone. Text values map 1:1 to `pickup.status`. |
| **Pill** (`Pill`) | Neutral facts next to a value (e.g., "58 kg đã xác thực", "Hạng 2 · 58 kg đã xác thực"). | Not a status; no icon needed. |
| **Banner** (`Banner / Warning`) | Persistent, non-blocking warning with a fix (incomplete profile on `S-F01-03`, transaction awaiting signature on the supplier home, quantity discrepancy on `S-F04-01`, balance not updated on `S-F05-01`). | Stays until the condition is resolved; the whole banner is tappable and the action link names the fix. An error banner (`color/danger-light` + `color/danger`) is used for save and network errors. |
| **Card / Stat card / Menu row / Stop card** | Grouped content; key figures on home screens (driver, warehouse, ReFeed staff); drill-down entries on `S-F06-01` and the staff main menu; one pickup point on the driver's route. | White fill, `color/line` border, radius 14. The current stop is marked by a 2 px `color/green` border and its header text. |
| **Modal** | Confirm one critical action: "Hủy yêu cầu thu gom?" (`S-F01-05`), "Xác nhận tuyến và điều phối?" (`S-F02-02`), "UCO chưa sẵn sàng: hủy lần thu gom này?" (`S-F02-04`), "Xác nhận & ký điện tử" (`S-F03-01`), "Xác nhận rút tiền" (`S-F05-02`). | Overlay on the parent screen; never a multi-step form or navigation. |
| **Toast** | Short confirmation after an action that stays on the screen (e.g., "Đã lưu hồ sơ", "Đã tải biên nhận"). | Auto-dismisses after 4 seconds. |
| **Skeleton** | Loading state of any screen with data. | Same layout as the loaded screen, `color/skeleton` blocks, plus one line of text ("Đang tải số dư…"). |
| **Bottom nav** (`Bottom nav`) | Supplier app only: Trang chủ · Thu gom · Tích lũy · Xanh · Hồ sơ. | Variant `Active=…` marks the current tab with bold text + `color/green-dark`; each item at least 44 × 56 px. Because the bottom nav already links to these areas, the supplier home has no separate menu list; its free space holds shortcut tiles instead. Sign out lives under "Hồ sơ". |

## 4. Messages
| Situation | Pattern | Example |
|---|---|---|
| **Profile incomplete (`S-F01-03` / `FR-01-03`)** | what is missing + link to fix | `"Hồ sơ điểm thu gom của bạn còn thiếu vị trí GPS. Bổ sung hồ sơ để đặt lịch thu gom."` |
| **Invalid pickup request (`S-F01-03` / `FR-01-03`)** | what is wrong + valid rule | `"Khung giờ 08:00 - 10:00 ngày 20/09/2026 đã qua. Hãy chọn khung giờ từ hôm nay trở đi."` |
| **Pickup request created (`S-F01-03` / `FR-01-03`)** | confirmation + ID + next step | `"Đã tạo yêu cầu thu gom PK-00125. Xem yêu cầu của tôi."` |
| **Invalid collection data (`S-F02-04` / `FR-02-02`)** | what is wrong + valid rule | `"Khối lượng thực tế phải lớn hơn 0 kg."` |
| **UCO not ready (`S-F02-04`, modal)** | consequence of the action + destructive button | `"Lần thu gom PK-00125 sẽ chuyển sang Đã hủy và không ghi nhận khối lượng."` → "Hủy lần thu gom" / "Quay lại" |
| **Transaction awaiting signature (supplier home, `S-F03-01`)** | what waits + amount + action | `"1 giao dịch chờ bạn ký · COL-00891 · 32,5 kg · 334.750 đ. Ký giao dịch"` |
| **Quantity discrepancy (`S-F04-01` / `FR-04-01`)** | measured difference + reference value | `"COL-00891: kho nhận 32,3 kg, chênh −0,2 kg so với lúc thu gom (32,5 kg)."` The tolerance value is still to be confirmed by ReFeed. |
| **Balance not updated (`S-F05-01` / `FR-05-01`)** | what is late + since when + retry | `"COL-00891 đã ký nhưng chưa được cộng vào số dư. Số liệu bên dưới tính đến ngày 18/09. Thử lại"` |
| **Empty list (`S-F05-01`, `S-F01-04`)** | current state + next action | `"Chưa có giao dịch. Số dư sẽ tăng sau mỗi lần thu gom bạn ký xác nhận."` → "Đặt lịch thu gom" |
| **GPS missing (`S-F02-04` / `FR-02-02`)** | what is missing + what to do | `"Chưa lấy được vị trí GPS. Bật định vị rồi lưu lại."` |
| **Route not refreshed (`S-F02-03` / `FR-02-02`)** | what happened + time of the data shown + retry | `"Không tải được tuyến mới nhất. Mất kết nối mạng. Đang hiển thị dữ liệu lúc 10:12. Thử lại"` |
| **Amount not calculated (`S-F02-05` / `FR-02-03`)** | what failed + what is kept + retry | `"Chưa tính được thành tiền. Không lấy được giá gốc hoặc hạng tích lũy của nhà cung cấp. Thông tin bàn giao đã được lưu. Thử lại"` |
| **Signing failed (`S-F03-01` / `FR-03-01`)** | what happened + what did not change | `"Chưa ký được giao dịch. Mất kết nối khi gửi chữ ký. Biên bản chưa bị khóa và chưa cộng tiền vào số dư."` |
| **Network / save error** | what happened + retry instruction | `"Không lưu được thay đổi. Kiểm tra kết nối mạng và thử lại."` |

## 5. Accessibility minimums
- **Contrast:** Body text, button text and status badge text must reach at least `4.5:1` against their background (WCAG 2.2, 1.4.3). Measured on the current tokens: white on `color/green` `#0E8466` 4.7:1 · `color/green-dark` on `color/green-light` 4.7:1 · `color/muted` on `color/background` 4.9:1 · `color/pending` on white 5.1:1 · `color/info` on `color/info-light` 5.3:1 · `color/danger` on `color/danger-light` 5.8:1. (`color/green` was `#12A37F` at 3.2:1 and `color/pending` `#B7791F` at 3.6:1; both were darkened to pass.)
- **Non-color indicators:** Statuses (`Đã gửi yêu cầu`, `Đã phân tài xế`, `Đã đến`, `Hoàn tất`, `Đã hủy`, `Chờ ký`) and alerts never rely on color alone: every one has a text label and its own icon (WCAG 2.2, 1.4.1).
- **Touch targets:** Every button, list row, tab, shortcut tile, notification button and form input is at least `44 × 44` px (above the WCAG 2.2, 2.5.8 minimum of `24 × 24` px, because drivers and kitchen staff use the app standing, often with gloves). Known gap: the "Thử lại" link inside the balance banner on `S-F05-01` is below 44 px high; the whole banner becomes the tap target until it is redesigned.
- **Viewport:** All four actors use the mobile app in Release 1, designed on a `370 × 824` px screen inside a `390 × 844` px device frame (minimum supported width `360` px). Whether warehouse and ReFeed staff need a wider web dashboard is an open question for the client.

## 6. Design tokens

Every ReFeed mockup uses only these values; they are the variables of the Figma collection
`ReFeed colors`. The screens are drawn on a 370px screen inside a 390 × 844 device frame and
exported at 3x with a 50px shadow margin, so every image in `screens/img/` is 1470 × 2832 pixels.

| Token | Value | Used for |
|---|---|---|
| color/green | #0E8466 | primary button, current stop border, selected option, success check mark |
| color/green-dark | #0B7A5F | secondary button text, links, avatar initials, highlighted values |
| color/green-light | #E6F6F1 | avatar circle, selected option, total row, "Hoàn tất" badge background |
| color/ink | #14231E | titles and values |
| color/muted | #5F6F69 | context lines, labels, hints |
| color/line | #E3E9E6 | card borders, row separators, header border |
| color/field-line | #CFDAD5 | input and option borders |
| color/surface | #FFFFFF | cards, header, footer |
| color/background | #F4F7F6 | screen background |
| color/skeleton | #E4EBE8 | loading placeholders |
| color/track | #DDE7E3 | upload progress track |
| color/info | #1F5FBF | "Đã phân tài xế" and "Đã đến" badge text |
| color/info-light | #E8F0FB | "Đã phân tài xế" and "Đã đến" badge background |
| color/pending | #9A6100 | "Chờ ký" badge text |
| color/warning-bg | #FFF4E0 | warning banner and "Chờ ký" badge background |
| color/warning-border | #F0C27A | warning banner border |
| color/warning-title | #6A3F00 | warning banner title |
| color/warning-text | #7A4B06 | warning banner text |
| color/danger | #B42318 | error banner, field error border and text, unread count, destructive button |
| color/danger-light | #FDECEC | error banner background, "Đã hủy" badge background |
| font/family | Inter | all text |
| font/size | home title 22px, screen title 18px, body 13px, small 12px, badge 11px, button 16px, total 22px | |
| space/unit | 2px; screen padding 18px, card padding 14px, gap between cards 10px | padding, gaps |
| radius | button and banner 12px, card 14px, input 10px, avatar 20px, badge and pill 99px | corners |
| frame/width | 370px screen inside a 390 × 844 device frame (minimum supported 360px) | every mockup is drawn at this width |

Contrast, checked with the WCAG 2.2 formula:

| Pair | Ratio | Needed |
|---|---|---|
| ink #14231E on white | 16.3:1 | 4.5:1 (1.4.3) |
| muted #5F6F69 on white | 5.3:1 | 4.5:1 (1.4.3) |
| muted #5F6F69 on background #F4F7F6 | 4.9:1 | 4.5:1 (1.4.3) |
| white on green #0E8466 | 4.7:1 | 4.5:1 (1.4.3) |
| green-dark #0B7A5F on green-light #E6F6F1 | 4.7:1 | 4.5:1 (1.4.3) |
| info #1F5FBF on info-light #E8F0FB | 5.3:1 | 4.5:1 (1.4.3) |
| pending #9A6100 on white | 5.1:1 | 4.5:1 (1.4.3) |
| pending #9A6100 on warning-bg #FFF4E0 | 4.7:1 | 4.5:1 (1.4.3) |
| warning-text #7A4B06 on warning-bg #FFF4E0 | 6.8:1 | 4.5:1 (1.4.3) |
| danger #B42318 on danger-light #FDECEC | 5.8:1 | 4.5:1 (1.4.3) |
| green #0E8466 on white (secondary button outline) | 4.7:1 | 3:1 (1.4.11) |
| field-line #CFDAD5 on white (input border) | 1.4:1 | 3:1 (1.4.11) — known gap, see below |

The disabled primary button (faded green) is exempt: WCAG 1.4.3 does not apply to inactive
components. The input border #CFDAD5 does not reach 3:1; until it is darkened, every input has a
visible label above it, so the field is still identifiable. The blue numbered circles on
`*-numbered.png` images are notes for readers, not part of the product, so they are not a token.
