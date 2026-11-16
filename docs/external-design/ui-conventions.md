# UI Conventions: ReFeed System

## 1. Language and tone
- **Interface language:** English by default (matches all Screen Specs, FRs, and diagrams); Vietnamese toggle available in user settings.
- **Form of address:** Address the user directly as "you" (Vietnamese: "bạn").
- **Tone:** Clear, operational, and objective. Avoid technical jargon (e.g., use "Collected volume" instead of "UCO payload").
- **Button labels:** Always use specific **verb + object** labels that describe the exact outcome (e.g., "Request pickup", "Confirm route", "Sign agreement", "Withdraw balance"). Never use generic labels like "OK", "Submit", or "Yes".

## 2. Formats
| Item | Format | Example |
|---|---|---|
| Date | `dd/MM/yyyy` | `16/11/2026` |
| Time | `HH:mm` (24-hour) | `14:30` |
| Pickup window | `dd/MM/yyyy · HH:mm–HH:mm` | `17/11/2026 · 09:00–11:00` |
| Volume (UCO) | Number with 1 decimal place + `L` | `48.5 L` |
| Currency (Payout) | Integer with thousand separator + `VND` | `720,000 VND` |
| Supplier ID | `SUP-` + 3 digits | `SUP-001` |
| Pickup Request ID | `REQ-` + 4 digits | `REQ-0102` |
| Collection ID | `COL-` + 4 digits | `COL-0089` |
| Batch ID | `BAT-` + `yyyyMM` + `-` + 2 digits | `BAT-202611-04` |

## 3. Components
| Component | When to use | Notes |
|---|---|---|
| **Primary button** | The single main action of a screen (e.g., "Request pickup", "Confirm handover"). | Maximum one per screen; full width on mobile (`Supplier` and `Driver` apps). |
| **Secondary button** | Alternative or secondary actions (e.g., "Reschedule", "Download PDF", "Back"). | Outline style; never competes visually with Primary button. |
| **Destructive button** | Irreversible cancellation actions inside modals (e.g., "Cancel pickup", "Report UCO not ready"). | Red fill or red outline; always triggers a confirmation modal first. |
| **Status badge** | Display status of requests, routes, agreements, or batches (e.g., `Pending`, `Dispatched`, `Awaiting signature`, `Verified`). | Must pair color with an explicit text label and icon (never color alone). |
| **Banner** | Persistent, non-blocking warning or prerequisite prompt on a screen (e.g., incomplete profile prompt on `S-F01-03`, quantity discrepancy alert on `S-F04-01`). | Remains visible until the condition is resolved; includes an action link when applicable. |
| **Modal** | Confirm a critical or destructive single action (`Cancel this pickup request?`, `Confirm route and dispatch?`, `UCO not ready: cancel this pickup?`, `Confirm and e-sign`, `Confirm withdrawal`). | Overlay on parent screen; never used for multi-step forms or general page navigation. |
| **Toast** | Short confirmation after a non-redirecting action succeeds (e.g., "Profile saved", "Receipt downloaded"). | Auto-dismisses after 4 seconds. |

## 4. Messages
| Situation | Pattern | Example |
|---|---|---|
| **Profile incomplete (`S-F01-03` / `FR-01-03`)** | what is missing + link to fix | `"Your Point-of-Origin and compliance profile is incomplete. Complete profile to request a pickup."` |
| **Invalid request/collection data (`S-F01-03`, `S-F02-04`)** | what is wrong + valid rule | `"Estimated volume must be greater than 0 L. Please enter a valid amount."` |
| **Pickup request created (`S-F01-03` / `FR-01-03`)** | confirmation + ID + next step | `"Pickup request REQ-0102 created. View my requests."` |
| **Quantity discrepancy alert (`S-F04-01` / `FR-04-01`)** | measured difference + tolerance warning | `"Received volume (45.0 L) differs from collected volume (48.5 L) by 3.5 L, exceeding the 2% tolerance."` |
| **Empty dispatch queue / list (`S-F02-01`, `S-F01-04`)** | current state + next action | `"No pending pickup requests in this area. Check another area or refresh."` |
| **Network / save error** | what happened + retry instruction | `"Changes could not be saved. Check your internet connection and try again."` |

## 5. Accessibility and device minimums
- **Contrast:** Body text and status badge text must have a contrast ratio of at least `4.5:1` against their background (WCAG 2.2, 1.4.3).
- **Non-color indicators:** Statuses (e.g., `Pending`, `Completed`, `Cancelled`, `Discrepancy`) must never rely on color alone — always include a text label and a distinct icon (WCAG 2.2, 1.4.1).
- **Touch targets:** All interactive buttons, list rows, and form inputs on mobile screens (`Supplier` and `Driver`) must be at least `44 × 44` CSS pixels (exceeding WCAG 2.2, 2.5.8 minimum of `24 × 24` px for field usability).
- **Viewport breakpoints:**
  - **Mobile apps (`Supplier` & `Driver / Collector`):** Designed at `390 × 844` px (minimum supported width: `360` px).
  - **Web dashboards (`Warehouse Staff` & `ReFeed Staff`):** Designed at `1440 × 900` px (minimum supported width: `1024` px).
