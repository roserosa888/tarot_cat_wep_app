# Feature Specification: Thai-Language Tarot 10-Question Fork Reading

**Feature Branch**: `003-mode-5card-summary`
**Created**: 2026-06-26 | **Status**: Active
**Updated**: 2026-07-02 | Added US7 — save 5-card summary image | Clarified State A start period button visibility

## Overview

A Thai-language tarot web app where users answer 10 binary (A/B) fork questions in one of 5 life categories. Each question presents a real-life dilemma with two choices — A or B — where each carries a trade-off, not a right or wrong answer. After all 10 questions, the app calculates a score and maps it to one of 5 tarot card outcomes per category. The final card is revealed with an animated reveal and a personalized narrative synthesized from the user's answer pattern.

## User Stories

- **US1 — Choose a Life Category** (P1): User sees 5 category buttons and taps one to begin a 10-question reading. Acceptance: 5 categories visible; clicking starts question flow; disclaimer always visible.
- **US2 — Answer 10 Fork Questions** (P1): Each question shows a real-life dilemma with two choices — A or B. No right or wrong answer. Progress bar shows Q1/10 through Q10/10. Acceptance: 10 questions per category; progress bar increments; A/B buttons respond.
- **US3 — Receive a Single Outcome Card** (P1): After Q10, score 0–10 maps to one of 5 outcome cards per category. Card image (existing PNG asset) reveals with animation. Acceptance: exactly 1 card shown; correct card for score range; animated reveal.
- **US4 — Read a Personalized Narrative** (P2): The narrative synthesizes the user's answer pattern (streaks, balance, dominant choice) into a life-reading paragraph for the category. Acceptance: narrative references actual A/B pattern; unique per user.
- **US5 — Restart and Try Another Category** (P3): User can restart at any time and choose a different category. Acceptance: restart button visible on summary; returns to category select.
- **US6 — Donate Section** (P2): After viewing the result card, a sad tarot card (`sad-card.png`) is displayed below the summary. Tapping it flips to a QR code (3D rotate animation). Hovering over the QR shows a clickable `qr-hover-btn` ("บันทึกรูปเพื่อสแกน"). Tapping it saves the QR to device and flips the card back to show a happy tarot card (`card-03.png`) as the final reward state. A "ไม่สะดวกโอน" skip link is available from the QR state to return to category selection.
- **US7 — Save 5-Card Summary Image** (P1): When user taps "บันทึกรูปเพื่อสแกน QR code" on the 5-card summary result screen, the system simultaneously saves the summary image to the user's device and shows a happy card on the frontend without a page reload. On desktop browsers, the image downloads to the user's Downloads folder. On mobile (iOS Safari / Android Chrome), the image saves to the device gallery; if direct save is unsupported, a fallback opens the image for long-press save with user guidance.

## Scoring System

- **A choice = 0 points** (cautious, reflective path)
- **B choice = 1 point** (action, change, risk-taking path)
- **Maximum score = 10** (all B choices)

### Score Ranges → Outcome Card

| Score Range | Outcome Index | Interpretation |
|---|---|---|
| 0–2 | ผลลัพธ์ที่ 1 | Very cautious / reflective path |
| 3–4 | ผลลัพธ์ที่ 2 | Balanced — leaning toward caution |
| 5–6 | ผลลัพธ์ที่ 3 | Balanced — leaning toward action |
| 7–8 | ผลลัพธ์ที่ 4 | Action-oriented path |
| 9–10 | ผลลัพธ์ที่ 5 | Bold, fully action-oriented path |

### Score → Card Mapping Per Category

| Category | 0–2 | 3–4 | 5–6 | 7–8 | 9–10 |
|---|---|---|---|---|---|
| การงาน | Hermit (9) | Emperor (4) | Chariot (7) | Magician (1) | Star (17) |
| ความรัก | Hanged Cat (12) | Lovers (6) | Strength (8) | Sun (19) | World (21) |
| การเงิน | Justice (11) | Wheel (10) | Emperor (4) | Temperance (14) | Sun (19) |
| ตัวตน | Priestess (2) | Cat's Death (13) | Magician (1) | Star (17) | Sun (19) |
| ครอบครัว | Hierophant (5) | Empress (3) | Lovers (6) | Emperor (4) | World (21) |

## Categories & Questions

**การงาน**: staying vs leaving, passion vs opportunity, autonomy vs stability, growth vs comfort, effort vs result, collaboration vs solo, recognition vs meaning, risk vs security, short-term vs long-term, leading vs following.

**ความรัก**: expressing vs holding back, fighting vs letting go, giving vs receiving, trust vs doubt, closeness vs space, commitment vs freedom, heart vs logic, past vs future, self vs partner, staying vs moving on.

**การเงิน**: spending vs saving, investing vs holding, giving vs keeping, risk vs safety, now vs later, need vs want, security vs growth, generosity vs self-care, stability vs opportunity, control vs flow.

**ตัวตน**: authentic vs accepted, change vs stay, speaking vs silence, solitude vs connection, dreaming vs doing, heart vs mind, old self vs new self, visible vs invisible, holding on vs letting go, leading vs following.

**ครอบครัว**: family expectation vs personal dream, giving vs receiving care, closeness vs boundaries, tradition vs change, duty vs desire, speaking truth vs keeping peace, staying vs leaving home, past wounds vs present love, sacrifice vs self-preservation, protecting vs releasing.

## Functional Requirements

- FR-001: 5 category buttons visible on initial load with icons and labels.
- FR-002: Clicking a category transitions to question screen with Q1.
- FR-003: Progress bar fills from 0% to 100% over 10 questions.
- FR-004: Question text, context, A choice, and B choice always visible.
- FR-005: A choice records 0 points; B choice records 1 point.
- FR-006: After Q10, a brief 400ms pause then summary screen shows.
- FR-007: Summary shows: category label, card image (PNG from `assets/cards/`), card name, narrative (no numeric score disclosed). Pattern line removed entirely.
- FR-008: Card image uses existing `card-XX.png` assets (not modified).
- FR-009: Restart button returns to category select screen.
- FR-010: Disclaimer text always visible: "ไพ่ทาโร่นี้มีไว้เพื่อสำรวจตัวเองเท่านั้น ไม่ใช่คำทำนายหรือคำแนะนำในการตัดสินใจ กรุณาใช้วิจารณญาณของตัวเอง"
- FR-011: Screen transitions use smooth fade animation.
- FR-012: Card reveal has flip/spin animation before final image appears.
- FR-013: "บันทึกรูปเพื่อสแกน QR code" button triggers image save and happy card display simultaneously.
- FR-014: Desktop save uses `<a download>` anchor; mobile save uses Web Share API Level 2 with `navigator.share()`.
- FR-015: On mobile browsers without share API support, open image in new view with Thai "กดค้างที่รูปเพื่อบันทึกลงอุปกรณ์" instruction and a close/back control.
- FR-016: Success shows toast "บันทึกรูปแล้ว"; errors show descriptive Thai messages without blocking happy card display.
- FR-017: Save button is disabled on first tap and re-enabled only after save completes or fails, preventing race conditions.

## Narrative Generation

`generateNarrative(category, cardIndex, cards)` produces 3 paragraphs:
1. **Pattern description**: describes the choice pattern (clustered, alternating, balanced, dominant choice)
2. **Outcome**: outcome index label, card name (no numeric score disclosed)
3. **Interpretation**: pattern-based description — uses `isClustered`, `isAlternating`, `dominantChoice`, not numeric score

Pattern display uses symbols (◆/◇) instead of A/B letters so the user cannot deduce their score from the pattern string.

Pattern detection uses:
- `isClustered` — streak ≥ 5
- `isAlternating` — exact ABAB... pattern
- `dominantChoice` — A-dominant, B-dominant, or balanced

## Donate Section

### Placement

- Positioned directly below the 5-card summary result on the same result page
- Not a separate route/page — renders as the next section after the summary within the same view
- Order on result screen:
  1. 5-card reading summary (per-category results)
  2. Donate section
- **Note**: The Donate section's `qr-hover-btn` ("บันทึกรูปเพื่อสแกน") is distinct from the US7 `summary-save-btn`. The `summary-save-btn` appears on the 5-card summary screen and saves the summary PNG to device; the `qr-hover-btn` is inside the donate section and saves the QR code image then flips to the Happy Card.

---

### Card States (Single Card Component, 4 States)

| State | What User Sees |
|---|---|
| A | Sad Card — front face (`sad-card.png`) |
| B | QR Code — back face, after flip |
| C | QR Code + `qr-hover-btn` visible on hover |
| D | Happy Card — front face (`card-03.png`), final state |

---

### State A — Sad Card (Initial, Start Period)

- Display the sad tarot card image (`assets/donation/sad-card.png`) as the default state
- **Start period**: the initial moment when the donate section first renders. During this period, `qr-hover-btn` ("บันทึกรูปเพื่อสแกน") and "เริ่มใหม่" buttons are NOT rendered / NOT visible in the donate section. (These are separate from the summary screen's restart button.)
- **"สนับสนุนค่าขนม"** text displayed ABOVE the sad card via `donate-label-above` — always visible during the start period and remains visible until the card flip animation begins. It is hidden via JS in `onCardTap()` when flip begins.
- **Hover text**: `state-a-hover-text` element ("คลิกเพื่อสแกนเพื่อส่งต่อพลังบวกให้ผู้สร้างแอป") is **always in the DOM** (hardcoded in HTML). CSS handles show/hide: opacity 0 by default, transitions to opacity 1 on `.card-front:hover`. Text disappears when mouse leaves.
- On click (of the card): trigger card flip animation → transition to State B

---

### State B / C — QR Code (Back Face)

- Card flips (3D rotate animation) to reveal a static pre-uploaded QR code image (`assets/donation/qr-code.png`) on the back
- **Note**: `qr-hover-btn` ("บันทึกรูปเพื่อสแกน") is always in the DOM inside `#qr-actions`. CSS shows it on hover of `#qr-wrap`. It is a clickable BUTTON, not a decorative label.
- On click of `qr-hover-btn`: trigger TWO sequential actions:
  1. `downloadQR()` — save the QR code image to the user's device (canvas → anchor download)
  2. `showHappyCard()` — immediately transition to State D (Happy Card), replacing the front face content with the happy card
- Below the QR image: small, thin, low-emphasis text link **"ไม่สะดวกโอน"**
  - On click: navigate user back to category selection page via `resetToHome()`
  - Ends the flow without showing the Happy Card

---

### State D — Happy Card (Final State)

- Happy tarot card image (`assets/happy/card-03.png`) replaces the front face content via `showHappyCard()`
- Displays "ขอบคุณที่สนับสนุน! ✨" heading and sub-text below the image
- `donate-restart-btn` ("ปุ่มเริ่มต้นใหม่") rendered inside the donate section below the happy card image
- On click: calls `resetToHome()` → returns to category selection
- No further flip or interaction required beyond the restart button
- No back face — this is the terminal state of the donate section

---

### Skip / Exit Flow

- "ไม่สะดวกโอน" text is available in State B and C only
- Styled as: small font size, thin font weight, low opacity — not a prominent button
- On click: return to category selection page via `resetToHome()`, donate flow ends
- `donate-restart-btn` ("ปุ่มเริ่มต้นใหม่") is available in State D (after happy card appears)
- On click: return to category selection page via `resetToHome()`, donate flow ends

---

### QR Code Asset

- Static, pre-uploaded image (no dynamic generation)
- No backend payment verification
- No slip upload required
- Saving the QR image is the sole trigger to unlock the Happy Card (trust-based)

---

### Acceptance Criteria

- Given State A, when user is in the start period (before any interaction with the sad card), `qr-hover-btn` and "เริ่มใหม่" are NOT visible in the donate section
- Given State A, when user hovers the sad card, `state-a-hover-text` appears above the image via CSS opacity transition
- Given State A, when user clicks the sad card, then card flips to show QR code (State B)
- Given State B, when user hovers `#qr-wrap`, then `qr-hover-btn` is visible (CSS hover)
- Given State C, when user clicks `qr-hover-btn`, then QR is saved to device AND Happy Card is shown (State D)
- Given State B or C, when user clicks "ไม่สะดวกโอน", then app navigates back to category selection via `resetToHome()`
- Given State D, `donate-restart-btn` ("ปุ่มเริ่มต้นใหม่") is visible in the donate section below the happy card
- Given State D, when user clicks `donate-restart-btn`, then app navigates back to category selection via `resetToHome()`
- Given State D, the Happy Card is shown with no further interaction needed beyond the restart button
- The QR code is a fixed static asset — no payment gateway, no verification, no slip upload

## Save 5-Card Summary Image

### Trigger

- Button label: **"บันทึกรูปเพื่อสแกน QR code"**
- Available on the 5-card summary result screen, prominently placed
- Single tap triggers two simultaneous actions: save image + show happy card

---

### Action 1 — Save Image to Device

#### Desktop Browser (Chrome, Firefox, Edge, Safari)

- Use native `<a download>` anchor with the pre-rendered summary image blob/URL
- Filename format: `tarot-summary-{timestamp}.png`
- Browser handles download to Downloads folder automatically
- Fallback if anchor download fails: open image in new tab, user saves manually

#### Mobile Browser (iOS Safari, Android Chrome)

- Attempt direct save via `navigator.share()` with `files` option (Web Share API Level 2)
  - If successful: image saved to Photos/Gallery via OS native share sheet
- If `navigator.share` unavailable or fails: open image in a new fullscreen tab/window
  - Display overlay instruction text: **"กดค้างที่รูปเพื่อบันทึกลงอุปกรณ์"** (long-press the image to save to your device)
  - Provide a close/back button to return to the summary screen

---

### Action 2 — Show Happy Card on Frontend

- After image save is initiated (not necessarily completed), immediately render the happy tarot card on the same screen
- No page reload or navigation required
- Happy card replaces or overlays the summary view (exact layout defined in component spec)
- Final terminal state — no further interaction needed from this action

---

### User Feedback

#### Success State

- Display a toast/notification: **"บันทึกรูปแล้ว"** (Image saved)
- Toast auto-dismisses after 3 seconds
- Happy card is already visible as confirmation

#### Error States

| Error | Message Shown |
|---|---|
| Permission denied | "ไม่สามารถบันทึกรูปได้ กรุณาอนุญาตการเข้าถึงไฟล์" |
| Storage full | "พื้นที่เก็บข้อมูลเต็ม กรุณาลบไฟล์บางส่วนแล้วลองใหม่" |
| Unsupported browser | "เบราว์เซอร์นี้ไม่รองรับการบันทึกรูปโดยตรง กรุณาเปิดในเบราว์เซอร์อื่นหรือบันทึกรูปด้วยตนเอง" |
| Network error / image load fail | "เกิดข้อผิดพลาด กรุณาลองใหม่อีกครั้ง" |

- All error messages displayed as toast notifications with distinct styling (error color)
- Happy card is still shown after error — user is not blocked from the reward state

---

### Race Condition Prevention

- On button tap: immediately disable the button (prevent further clicks)
- Keep button disabled until the save action completes or fails
- If user taps multiple times before the first save completes, only the first trigger is honored
- After error recovery, re-enable the button so user can retry

---

### Summary Image Generation

- The 5-card summary image is pre-rendered as a static composite image at page load (no dynamic assembly on save)
- Image resolution: 1080×1920 px (portrait, optimized for mobile sharing)
- Stored as a blob URL or `data:` URL accessible for both download anchor and share API
- Asset path: `assets/summary/summary-5card.png` (pre-generated static asset)

---

### Save 5-Card Summary Acceptance Criteria

- [ ] On desktop browser, tapping "บันทึกรูปเพื่อสแกน QR code" downloads the summary PNG to the Downloads folder
- [ ] On iOS Safari, tapping the button opens the share sheet and saves the image to Photos
- [ ] On Android Chrome, tapping the button saves the image to Gallery via share sheet
- [ ] On mobile browsers where share API is unavailable, the image opens in a new view with Thai-language save instructions
- [ ] The happy card appears on screen immediately after the save action is triggered (no reload)
- [ ] A success toast "บันทึกรูปแล้ว" is displayed
- [ ] Tapping the button multiple times rapidly only triggers one save (no duplicate downloads)
- [ ] On permission denied, a descriptive Thai error toast is shown and the happy card is still displayed
- [ ] On storage full, a descriptive Thai error toast is shown and the happy card is still displayed
- [ ] On unsupported browser, fallback instructions are shown in Thai


### Assets Required

- `assets/donation/sad-card.png` — sad tarot card image (front face, State A)
- `assets/donation/qr-code.png` — static QR code image (back face, State B/C)
- `assets/happy/card-03.png` — happy tarot card (final reward state, State D)
- `assets/summary/summary-5card.png` — pre-rendered 5-card summary composite image (1080×1920 px)

## Key Entities

```
Deck        → { id, name, meaning }
Category    → { icon, outcomes[5], questions[10] }
Question    → { text, context, A: {path, trade}, B: {path, trade} }
DrawnCard  → { choiceLabel: 'A'|'B', questionText }  (internal — not shown to user)
Analysis    → { isClustered, isAlternating, dominantChoice }  (no score disclosed)
SaveResult  → { status: 'success'|'error', errorType?: 'permission_denied'|'storage_full'|'unsupported'|'network_error' }
```

## Success Criteria

- All 5 categories load and display 10 questions each
- Progress bar accurately reflects current question number
- Score correctly computed as sum of B choices (0–10)
- Correct card displayed for all 5 score range × 5 categories = 25 verified mappings
- Narrative varies meaningfully between different answer patterns
- Card reveal animation plays in browser
- Restart returns to clean state
- Disclaimer always visible on all screens
- Card images load from existing `assets/cards/card-XX.png` files (no new images needed)
