# Feature Specification: Thai-Language Tarot 10-Question Fork Reading

**Feature Branch**: `003-mode-5card-summary`
**Created**: 2026-06-26 | **Status**: Active
**Updated**: 2026-06-29 | Supersedes previous spec

## Overview

A Thai-language tarot web app where users answer 10 binary (A/B) fork questions in one of 5 life categories. Each question presents a real-life dilemma with two choices — A or B — where each carries a trade-off, not a right or wrong answer. After all 10 questions, the app calculates a score and maps it to one of 5 tarot card outcomes per category. The final card is revealed with an animated reveal and a personalized narrative synthesized from the user's answer pattern.

## User Stories

- **US1 — Choose a Life Category** (P1): User sees 5 category buttons and taps one to begin a 10-question reading. Acceptance: 5 categories visible; clicking starts question flow; disclaimer always visible.
- **US2 — Answer 10 Fork Questions** (P1): Each question shows a real-life dilemma with two choices — A or B. No right or wrong answer. Progress bar shows Q1/10 through Q10/10. Acceptance: 10 questions per category; progress bar increments; A/B buttons respond.
- **US3 — Receive a Single Outcome Card** (P1): After Q10, score 0–10 maps to one of 5 outcome cards per category. Card image (existing PNG asset) reveals with animation. Acceptance: exactly 1 card shown; correct card for score range; animated reveal.
- **US4 — Read a Personalized Narrative** (P2): The narrative synthesizes the user's answer pattern (streaks, balance, dominant choice) into a life-reading paragraph for the category. Acceptance: narrative references actual A/B pattern; unique per user.
- **US5 — Restart and Try Another Category** (P3): User can restart at any time and choose a different category. Acceptance: restart button visible on summary; returns to category select.
- **US6 — Donate Section** (P2): After viewing the result card, a Death tarot card is displayed below the summary. Tapping it flips to a QR code (3D rotate animation). Hovering over the QR shows a clickable "บันทึกรูปเพื่อแสดงการ์ตีใจ" button. Tapping it saves the QR to device and flips the card back to show a happy tarot card as the final reward state. A "ไม่สะดวกโอน" skip link is available from the QR state to return to category selection.

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

---

### Card States (Single Card Component, 4 States)

| State | What User Sees |
|---|---|
| A | Death Card (front face) |
| B | QR Code (back face, after flip) |
| C | QR Code + hover text visible |
| D | Happy Card (front face, final state) |

---

### State A — Death Card (Initial)

- Display the Death tarot card image as the default state
- On hover: show text **"สแกนเพื่อส่งต่อพลังบวกให้ผู้สร้างแอป"** ABOVE the card image
  - Fade-in transition: opacity 0 → 1
  - Text disappears when mouse leaves
- On click: trigger card flip animation → transition to State B

---

### State B / C — QR Code (Back Face)

- Card flips (3D rotate animation) to reveal a static pre-uploaded QR code image on the back
- On hover over QR code image:
  - Display clickable text: **"บันทึกรูปเพื่อสแกน"** (the QR code image will be saved to your computer or phone) overlaid on or above the QR image
  - Fade-in transition: opacity 0 → 1
  - This text is a BUTTON, not decorative label
- On click of hover text: trigger TWO sequential actions:
  1. Save / download the QR code image to user's device
  2. Immediately transition to State D (Happy Card) — card flips back to front face
- Below the QR image: small, thin, low-emphasis text link **"ไม่สะดวกโอน"**
  - On click: navigate user back to category selection page
  - Ends the flow without showing the Happy Card

---

### State D — Happy Card (Final State)

- Happy tarot card displayed as a single static image on the front face
- No further flip or interaction required
- No back face — this is the terminal state of the donate section

---

### Skip / Exit Flow

- "ไม่สะดวกโอน" text is available in State B and C only
- Styled as: small font size, thin font weight, low opacity — not a prominent button
- On click: return to category selection page, donate flow ends

---

### QR Code Asset

- Static, pre-uploaded image (no dynamic generation)
- No backend payment verification
- No slip upload required
- Saving the QR image is the sole trigger to unlock the Happy Card (trust-based)

---

### Acceptance Criteria

- Given State A, when user hovers the Death Card, then hover text appears above the image with fade-in
- Given State A, when user clicks the Death Card, then card flips to show QR code (State B)
- Given State B, when user hovers the QR image, then clickable text "บันทึกรูปเพื่อสแกน" appears
- Given State C, when user clicks the hover text, then QR is saved to device AND Happy Card is shown (State D)
- Given State B or C, when user clicks "ไม่สะดวกโอน", then app navigates back to category selection
- Given State D, the Happy Card is shown as a single static image with no further interaction needed
- The QR code is a fixed static asset — no payment gateway, no verification, no slip upload

### Assets Required

- `assets/donation/death-card.png` — Death tarot card (front face, State A)
- `assets/donation/qr-code.png` — static QR code image (back face, State B/C)
- `assets/donation/happy-card.png` — happy tarot card (final reward state, State D)

## Key Entities

```
Deck        → { id, name, meaning }
Category    → { icon, outcomes[5], questions[10] }
Question    → { text, context, A: {path, trade}, B: {path, trade} }
DrawnCard  → { choiceLabel: 'A'|'B', questionText }  (internal — not shown to user)
Analysis    → { isClustered, isAlternating, dominantChoice }  (no score disclosed)
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
