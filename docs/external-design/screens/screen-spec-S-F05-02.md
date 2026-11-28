# Screen Spec: S-F05-02 Green Profile

| Field             | Value |
| ----------------- | ----- |
| Screen ID         | S-F05-02 |
| Screen name       | Green Profile (Vietnamese title: "Hồ sơ xanh") |
| Actor             | Supplier |
| Priority          | Should (Screen List and W1; the effective priority of FR-05-07 in W4 is Could, open question 11) |
| Feature           | F-05 Supplier Value & ESG / Green Profile |
| Use case          | UC-13 View Green Profile and UCO contribution history |
| Activity diagrams | Not needed: read-only query (W0 UC-13) |
| Mockup images     | `img/S-F05-02-default.png` (default), `img/S-F05-02-default-numbered.png`, four state images and four numbered state images (section 2) |
| Frame width       | 370px screen inside a 390 × 844 device frame, exported at 3x; state images are 1170 × 2532 pixels, numbered images are 1230 × 2592 pixels (30px transparent padding so that the number badges have room) |
| Figma frame       | Board "Green Profile - 5 stages", five frames, one per state; the board label for this screen is S-F05-02. The repository holds no Figma link for this board, so the images in `img/` are the reference |
| Status            | Draft |

**Scope change in this version.** The withdrawal screen was removed (no bank connection), so the former S-F05-03 "Green Profile" becomes **S-F05-02**. The content of the screen is unchanged; only the ID and the button "Về ví của tôi" (back to S-F05-01) matter for navigation.

## 1. Purpose

**Shown when:** the supplier taps "Xem hồ sơ xanh" on the wallet (S-F05-01), taps the card "UCO đã giao" or the bottom navigation tab "Xanh" on the supplier home (S-F00-02), or comes back from the ESG contribution reports (S-F05-03).

**The user leaves this screen when:** they tap the back button, tap "Xem báo cáo ESG", tap "Yêu cầu thu gom" (empty state), tap "Về ví của tôi", or tap "Tải lại hồ sơ xanh" after an error (the screen stays and reloads).

## 2. Mockup

[![S-F05-02](img/S-F05-02-default.png)](img/S-F05-02-default.png)

**Numbered:** each number is the # of the element inventory in section 3. An element keeps the same number in every state.

| State | Numbers shown on the image |
| ----- | -------------------------- |
| Default | 1–3, 5–11, 14, 17 |
| Empty | 1–3, 5–9, 12, 15, 17 |
| Loading | 1–3, 5, 9, 10, 13, 14 (5, 9 and 10 on the skeleton blocks) |
| Error | 1–8, 16, 17 |
| Success | 1–3, 5–11, 14, 17, 18 |

In the Loading image, the number on a grey block names the element the block holds the place of; the block shows no data and cannot be tapped.

| Default, numbered | Empty, numbered | Loading, numbered | Error, numbered | Success, numbered |
| --- | --- | --- | --- | --- |
| [![S-F05-02 default numbered](img/S-F05-02-default-numbered.png)](img/S-F05-02-default-numbered.png) | [![S-F05-02 empty numbered](img/S-F05-02-empty-numbered.png)](img/S-F05-02-empty-numbered.png) | [![S-F05-02 loading numbered](img/S-F05-02-loading-numbered.png)](img/S-F05-02-loading-numbered.png) | [![S-F05-02 error numbered](img/S-F05-02-error-numbered.png)](img/S-F05-02-error-numbered.png) | [![S-F05-02 success numbered](img/S-F05-02-success-numbered.png)](img/S-F05-02-success-numbered.png) |

| State   | Image |
| ------- | ----- |
| Default | [`img/S-F05-02-default.png`](img/S-F05-02-default.png) (one verified collection, COL-00836, and one waiting for verification, COL-00891) |
| Empty   | [`img/S-F05-02-empty.png`](img/S-F05-02-empty.png) (no collection verified yet) |
| Loading | [`img/S-F05-02-loading.png`](img/S-F05-02-loading.png) |
| Error   | [`img/S-F05-02-error.png`](img/S-F05-02-error.png) (the Green Profile could not be loaded) |
| Success | [`img/S-F05-02-success.png`](img/S-F05-02-success.png) (reloaded after the error; both collections verified) |

Colours, fonts and corners must follow the design tokens in [ui-conventions.md](../ui-conventions.md#6-design-tokens). Pixel check of the five state images: white `color/surface` matches; `#F4F7F6` matches `color/background`. Other large areas differ from the tokens, for example `#F3F5F4` (token `color/background` `#F4F7F6`), `#E6F4EE` (`color/green-light` `#E6F6F1`), `#1B2B26` (`color/ink` `#14231E`), `#0F7A57` (`color/green-dark` `#0B7A5F`) and `#A9D6C4` (no close token). The tokens are the rule; the images need to be redrawn. The images are 1170 × 2532 pixels, while ui-conventions §6 states 1470 × 2832 pixels with a shadow margin.

## 3. Element inventory

Data field names are defined by this spec; the repository has no data dictionary.

| #  | Element | Type | Content / data source | Required | Validation |
| -- | ------- | ---- | --------------------- | -------- | ---------- |
| 1  | Back button | Icon button | chevron-left | Yes | none |
| 2  | Screen title | Header | static "Hồ sơ xanh" | Yes | |
| 3  | Context line | Text | `supplier.id` · `supplier.business_name` (example: "SUP-00125 · Quán Phở Hàng Bài") | Yes | `SUP-` + 5 digits |
| 4  | Error banner | Error banner | triangle icon; "Không tải được hồ sơ xanh."; "Kiểm tra kết nối mạng và thử lại." | No | shown only in the error state |
| 5  | Contribution card | Metric card (primary) | leaf icon; label "Đóng góp đã xác minh"; `uco.verified_weight` + " kg" (example: "25,5 kg"); error: "— kg" | Yes | 1 decimal with decimal comma (open question 6) |
| 6  | Card description | Text | static "Lượng dầu ăn đã được kho xác minh." | Yes | |
| 7  | Collection count | Stat box | label "Số lần thu gom"; `uco.verified_collection_count` + " lần" (example: "1 lần"); error: "—" | Yes | integer >= 0 |
| 8  | Current tier | Stat box | label "Hạng hiện tại"; "Hạng " + `loyalty.tier` (example: "Hạng 1"); error: "—" | Yes | tier is a whole number |
| 9  | History title | Section title | static "Lịch sử thu gom" | No | hidden in the error state |
| 10 | Collection row | List row | leaf icon; `collection.id` (example: "COL-00891"); `collection.date` (example: "25/09/2026"); `collection.actual_quantity` + " kg" (example: "32,5 kg") | No | `COL-` + 5 digits; `dd/MM/yyyy`; 1 decimal; newest first |
| 11 | Verification chip | Chip | `collection.verification_status`: "✓ Đã xác minh" (green) or "◷ Đang chờ xác minh" (amber) | Yes, on every row | enum: verified, waiting |
| 12 | Empty history card | Empty state card | leaf icon in a grey circle; "Chưa có lần thu gom nào được xác minh"; "Sau khi kho xác minh, lịch sử thu gom của bạn sẽ hiện ở đây." | No | shown only when no collection is verified |
| 13 | Loading text | Text | static "Đang tải hồ sơ xanh..." | No | shown only in the loading state |
| 14 | Xem báo cáo ESG | Primary button | static label | Yes (default, loading, success) | none; not tappable while loading |
| 15 | Yêu cầu thu gom | Primary button | static label; replaces button 14 in the empty state | No | none |
| 16 | Tải lại hồ sơ xanh | Primary button | static label; replaces button 14 in the error state | No | none |
| 17 | Về ví của tôi | Secondary button | static label | Yes | none; hidden while loading |
| 18 | Success toast | Toast | check icon + "Đã tải lại hồ sơ xanh." | No | shown after a successful reload; dismisses after 4 seconds (ui-conventions §3) |

## 4. States

| State | What the user sees | Trigger |
| ----- | ------------------ | ------- |
| Default | Header "Hồ sơ xanh", "SUP-00125 · Quán Phở Hàng Bài". Mint card: "Đóng góp đã xác minh", **25,5 kg**, "Lượng dầu ăn đã được kho xác minh.", boxes "Số lần thu gom 1 lần" and "Hạng hiện tại Hạng 1". Title "Lịch sử thu gom". Rows: "COL-00891 · 25/09/2026 · 32,5 kg" with chip "Đang chờ xác minh"; "COL-00836 · 18/09/2026 · 25,5 kg" with chip "Đã xác minh". Buttons "Xem báo cáo ESG" and "Về ví của tôi". | Screen opened with data |
| Empty (no data) | Mint card with **0,0 kg**, "0 lần", "Hạng 1". Title "Lịch sử thu gom" and the empty card "Chưa có lần thu gom nào được xác minh". Buttons "Yêu cầu thu gom" and "Về ví của tôi". | No collection of the supplier has been verified yet |
| Loading | Header (1–3) visible. Skeleton block for the contribution card (5), a short bar for the history title (9), a block for the history list (10). Centred text "Đang tải hồ sơ xanh..." (13). Only one button, "Xem báo cáo ESG", greyed out (14). No banner, no figures. | Initial screen load, or "Tải lại hồ sơ xanh" tapped, while fetching |
| Error | Red banner "Không tải được hồ sơ xanh." / "Kiểm tra kết nối mạng và thử lại.". Mint card with "— kg", "—", "—". No history. Buttons "Tải lại hồ sơ xanh" and "Về ví của tôi". | Network connection lost or API timeout when fetching the Green Profile |
| Success / confirmation | Toast "Đã tải lại hồ sơ xanh." above the buttons (18). Mint card **58 kg**, "2 lần", "Hạng 2". Rows: "COL-00891 · 25/09/2026 · 32,5 kg" and "COL-00836 · 18/09/2026 · 25,5 kg", both with chip "Đã xác minh". Buttons "Xem báo cáo ESG" and "Về ví của tôi". | The supplier tapped "Tải lại hồ sơ xanh" in the error state and the data loaded |

**Sample data check:** verified weight 25,5 + 32,5 = 58,0 kg; verified collections 1 + 1 = 2; tier 1 → 2 at 50,0 kg (S-F05-01: "Còn 24,5 kg nữa để lên Hạng 2 (50,0 kg)"). In Default only COL-00836 (verified) counts toward 25,5 kg and "1 lần"; COL-00891 is listed but not counted. The success image shows "58 kg"; the correct format is "58,0 kg" (open question 6).

## 5. Interactions and navigation

Target Screen IDs were checked against [screen-list.md](../screen-list.md) and [screen-transition.md](../screen-transition.md).

| #  | Element | User action | System response | Goes to screen |
| -- | ------- | ----------- | --------------- | -------------- |
| 1  | Back button (1) | tap | Returns to the previous screen; if there is none, opens the supplier home | previous screen (S-F05-01 or S-F00-02) |
| 2  | Xem báo cáo ESG (14) | tap | Opens the ESG reports of the supplier | S-F05-03 |
| 3  | Yêu cầu thu gom (15) | tap | Opens the pickup request form | S-F01-03 |
| 4  | Về ví của tôi (17) | tap | Opens the wallet with the balance and payout status | S-F05-01 |
| 5  | Tải lại hồ sơ xanh (16) | tap | Re-fetches the Green Profile | stays (loading, then default, empty, success or error state) |
| 6  | Contribution card (5), stat boxes (7, 8), chips (11) | tap | Display only | stays |
| 7  | Collection row (10) | tap | Display only (open question 8) | stays |
| 8  | Screen body | pull down | Re-fetches the data and shows the Loading state (open question 4) | stays |

## 6. Screen-level rules

| Rule ID | Rule | Source |
| ------- | ---- | ------ |
| SR-001 | The supplier only sees the collections, weight and tier that belong to their own `supplier.id`. | FR-06-04 |
| SR-002 | Only collections verified by the warehouse count toward the verified weight and the collection count; collections waiting for verification are listed with the chip "Đang chờ xác minh" but not counted. The tier is the value the system returns (open question 12). | FR-05-07; screen design |
| SR-003 | The screen is read-only and displays the values the system returns; it never calculates weight or tier itself. | FR-05-07 |
| SR-004 | The history is sorted newest first and every row shows exactly one verification chip. | Screen design |
| SR-005 | In the Empty state the primary button is "Yêu cầu thu gom" so the first booking can start from this screen. | Screen design |
| SR-006 | If fetching fails, the screen never shows zeros in place of real figures: it shows "—" and the error banner (4). | Offline resilience convention |
| SR-007 | While loading, skeleton blocks keep the layout of the loaded screen, show one line of loading text, cannot be tapped, and the buttons cannot be used. | ui-conventions §3 (Skeleton) |
| SR-008 | Banners and chips use an icon + text, never colour alone (✓ Đã xác minh, ◷ Đang chờ xác minh, triangle, check). | ui-conventions §5 |

## 7. Linked requirements

FR IDs and the function names in quotes are copied from [w1-function-list.md](../../function-list/w1-function-list.md).

| FR ID | What this screen does for it |
| ----- | ---------------------------- |
| FR-05-07 "View Green Profile and UCO contribution history" | Shows the supplier's verified UCO contribution and collection history, with the collection count and the tier |
| FR-05-05 "Upgrade loyalty tier" | Displays the current tier returned by the system (display only) |
| FR-06-04 "Check role permission for protected action" | Opens only for a role that has the supplier permission |

**Related functions, entry only** (carried out on the target screen): FR-01-11 "Validate pickup location, estimated volume and time window" (pickup form, S-F01-03), FR-05-01 "Retrieve Loyalty Tier & Payout Balance" (S-F05-01), FR-05-08 "Generate ESG / environmental contribution report" and FR-05-09 "Provide ESG / environmental contribution report" (S-F05-03).

## 8. Responsive and accessibility notes

- Designed on a 370 × 824 screen inside a 390 × 844 device frame; smallest supported width 360 px; the two stat boxes stay in one row with equal width; collection ID, date, weight and chip do not truncate.
- Vertical scroll is enabled when the history is longer than the screen; the button area stays fixed at the bottom.
- Touch targets at least 44 × 44 (ui-conventions §5): back button and both buttons.
- Chips use different icons alongside the text: check ("Đã xác minh"), clock ("Đang chờ xác minh"); the banner uses a triangle icon; text contrast at least 4.5:1, including amber text on the amber chip (ui-conventions §5).

## 9. Open questions

"Closed (source)" is settled by a repository document. "Closed for this spec" applies a design default so that the spec has no gap; the team or client has not confirmed it, and the last column says what is still to be done.

| # | Question | Blocking? | Status and decision |
| - | -------- | --------- | ------------------- |
| 1 | Is the Green Profile S-F05-02 and the ESG reports S-F05-03? | No | Closed (source: Screen List, Screen Transition, screen-spec-S-F05-03.md). Yes. usage-flow.md still uses the old numbering and needs to be updated. |
| 2 | COL-00891 is "Đang chờ xác minh" here but "đang chờ bạn ký" on S-F05-01. Which comes first? | No | Closed (source). Signature first (FR-04-01). Until the warehouse verifies it, the chip here stays "Đang chờ xác minh", including while the signature is pending; the wallet shows the signature action. |
| 3 | The Loading skeleton blocks are taller than the loaded content. | No | Closed (source: ui-conventions §3 Skeleton). Block heights match the loaded layout. The image still needs to be redrawn. |
| 4 | Is there a pull-to-refresh? | No | Closed for this spec: yes, pull down re-fetches and shows the Loading state. |
| 5 | Is a waiting collection counted in "Số lần thu gom"? | No | Closed for this spec: no, only verified collections are counted (FR-05-07 "verified UCO contribution"; SR-002). |
| 6 | Success shows "58 kg". | No | Closed (source: ui-conventions §2). The format is "58,0 kg". The image still needs to be redrawn. |
| 7 | In Empty, should waiting collections be listed? | No | Closed for this spec: the empty card stays; waiting collections appear after the first verified one. |
| 8 | Does tapping a collection row open its receipt? | No | Closed (source: Screen Transition has no path from S-F05-02 to S-F03-02). Rows are display only. |
| 9 | Do the mockup colours match the design tokens? | No | Closed: checked, not passed (section 2). Redraw the images with the tokens. |
| 10 | The frames have no bottom navigation, although the tab "Xanh" leads here. | No | Closed for this spec: no tab bar, as in the mockups. The team needs to align this with ui-conventions §3. |
| 11 | Which priority applies: Should or Could? | No | Closed for this spec: the header follows the Screen List (Should). W4 lowers FR-05-07 to Could, and ui-conventions and the Screen List do not record this; the Screen List owner needs to align them. |
| 12 | "Verified" means signed volume in FR-05-04 (used for the tier) but warehouse-verified volume on this screen. Which does the Green Profile use? | No | Closed for this spec: weight and count are warehouse-verified, as the mockup says ("được kho xác minh"); the tier comes from the system (FR-05-05). After signing and before warehouse verification, the figures can differ from S-F05-01. The warehouse state that counts as verified is open (W3 D-08; W5 question 5) and needs team confirmation. |

---

## Completion checklist

- [x] The mockup is a separate cropped image file, named with the Screen ID, at the frame width (`img/S-F05-02-<state>.png`).
- [x] The numbered images exist for all five states, and their numbers match the # column of the element inventory.
- [x] Every visible element in the mockup appears in the element inventory.
- [ ] Every colour in the mockup is a design token from ui-conventions.md. **Checked, not passed:** see section 2.
- [x] Every input element has a validation rule or an explicit "none".
- [x] All five states are filled in.
- [x] Every navigation target is a Screen ID that exists in the Screen List (S-F00-02, S-F01-03, S-F05-01, S-F05-03).
- [x] Every element that displays data names the field it displays.
- [x] Every linked FR exists in the Function List.
