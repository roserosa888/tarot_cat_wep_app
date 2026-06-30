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
- **US6 — Donation Page (Post-Result Screen)** (P2): After viewing the result card, a sad tarot card is displayed. Tapping it reveals a QR code (flip animation). Tapping again shows a "Save QR Image" button. Tapping Save flips to reveal a happy tarot card. A skip option navigates back to category selection.

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

## Donation Page (Post-Result Screen)

### Flow

1. **Sad Card (Initial State)**: After quiz results, display the "sad" tarot card as the default state.
2. **Tap 1 — Flip to QR**: User taps the sad card → card flips (rotate animation) → back side reveals the QR code image.
3. **Tap 2 — Show Save Button**: User taps the QR code again → a "Save QR Image" button appears below/over the QR code.
4. **Tap Save → Unlock Happy Card**: User taps "Save QR Image" → this single action (no transfer verification needed) triggers the card to flip again → reveals the "happy" tarot card, shown as a single static image (no further flip/back side).
5. **Skip Option**: Below the QR code image, show small, thin, low-emphasis text: "ไม่สะดวกโอน" (Not convenient to transfer now). Tapping this text navigates the user back to the category selection page, ending the flow without showing the happy card.

### Card States Summary

| State | Description |
|---|---|
| State A | Sad card (front) — default initial state |
| State B | Sad card flipped → QR code (back) |
| State C | QR code + "Save QR Image" button visible |
| State D | Happy card (single image, final state) |
| Exit | "ไม่สะดวกโอน" text link → back to category selection (available from State B/C) |

### Acceptance Criteria

- **AC-D01**: Given the result screen shows the sad card, when the user taps it, then the card flips to show the QR code.
- **AC-D02**: Given the QR code is shown, when the user taps it again, then a "Save QR Image" button appears.
- **AC-D03**: Given the "Save QR Image" button is visible, when the user taps it, then the happy card is displayed as a single static image — no payment/transfer verification is performed or required.
- **AC-D04**: Given the QR code or save-button state is shown, when the user taps "ไม่สะดวกโอน," then the app navigates back to the category selection screen.
- **AC-D05**: The QR code image is static and pre-uploaded (no dynamic generation, no backend check).

### Assets Required

- `assets/donation/sad-card.png` — sad tarot card (front of flip card)
- `assets/donation/happy-card.png` — happy tarot card (final reward state)
- `assets/donation/qr-code.png` — static QR code image (back of flip card)

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
