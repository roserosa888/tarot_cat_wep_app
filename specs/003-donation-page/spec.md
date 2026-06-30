# SPEC: Donation Page (Post-Result Screen)

## 1. Overview

**What it does:** After the tarot result is displayed for a "sad" card (score tier 0), the card itself becomes the interactive surface — two taps flip it to reveal the QR code, then the save button, then the happy card.

**Why it exists:** Creates a tactile, card-flip experience that feels like interacting with a physical tarot card, with the "happy" card as an incentive to support the project.

---

## 2. Sad Card Determination

- **Trigger:** `scoreToOutcomeIndex(score)` returns `0` for scores 0–2 (the lowest tier). This maps to `outcomes[0]` for the selected category.
- A card is "sad" when `outcomeIdx === 0`.
- All other tiers (1–4) show the normal summary with no donation interaction.

---

## 3. Interaction Flow & State Machine

```
┌──────────────────────────────────────────────────────┐
│  State A — Sad card front (interactive, tappable)    │
│  "Tap card" → card flips 180° →                     │
│  State B — QR code back (tappable QR area)          │
│  "Tap QR code" → save button appears →              │
│  State C — Save button visible                       │
│  "Tap บันทึกรูป QR" → happy card shown →          │
│  State D — Happy card (final, static)                │
│                                                       │
│  [ไม่สะดวกโอน] available from State B & C → back to home
└──────────────────────────────────────────────────────┘
```

---

## 4. HTML Structure

The `#summary-card-wrap` in `index.html` is replaced with `#card-interaction-area`:

```html
<div class="card-interaction-area" id="card-interaction-area">
  <div class="card-face card-front" id="card-front">
    <img id="summary-card-img" src="" alt="ไพ่ทาโร่ต์" />
  </div>
  <div class="card-face card-back hidden" id="card-back">
    <div class="qr-info-wrap" id="qr-info-wrap">
      <div class="qr-wrap" id="qr-wrap">
        <img src="assets/qr-donation.png" alt="รหัสชำระเงิน" />
      </div>
      <div class="save-btn-wrap hidden" id="save-btn-wrap">
        <button class="btn-save-qr" id="save-qr-btn">บันทึกรูป QR</button>
      </div>
      <button class="skip-link" id="skip-link">ไม่สะดวกโอน</button>
    </div>
  </div>
</div>
```

In State D, the content of `#qr-info-wrap` is replaced (innerHTML) with:
- `#happy-final-img` (happy tarot card image)
- `.happy-thanks-heading` with "ขอบคุณที่สนับสนุน! ✨"
- `.happy-thanks-sub` with the uplifting message

---

## 5. CSS Notes

- **`.card-interaction-area`** — 3D perspective container, `perspective: 1000px`
- **`.card-face`** — shared `backface-visibility: hidden` + `transition: transform 0.7s cubic-bezier(0.34, 1.56, 0.64, 1)`
- **`.card-front`** — `transform: rotateY(0deg)`; `.flipped` → `rotateY(180deg) + opacity: 0`
- **`.card-back`** — `position: absolute; inset: 0; transform: rotateY(-180deg)`; `.flipped` → `rotateY(0deg)`
- **`.qr-info-wrap`** — flex column, centered, inside `.card-back`
- **`.skip-link`** — low-emphasis underlined text, `cursor: pointer`
- State D happy card is a single static image (no flip), directly injected as innerHTML

---

## 6. State Handling

- Session-only: no `localStorage` or backend required.
- `donationState` variable tracks `'A'|'B'|'C'|'D'|null`
- `resetToHome()` clears all state and returns to category select
- Click handlers are dynamically bound/unbound per state (no duplicate event listeners)

---

## 7. Asset Requirements

| Asset | Path | Notes |
|---|---|---|
| QR Code | `assets/qr-donation.png` | Static image, pre-uploaded. 300×300px recommended |
| Happy Card | `assets/cards/card-happy.png` | Tarot-style happy card image, same size as other cards |

> **Note:** QR code and happy card image files must be provided and placed in the assets directory before this feature is fully functional.

---

## 8. Acceptance Criteria

| # | Scenario | Expected result |
|---|---|---|
| AC1 | User finishes quiz with score 0–2 | Sad card shown, card is tappable (cursor: pointer) |
| AC2 | User on sad card taps the card | Card flips 180° to show QR code on back |
| AC3 | User on QR back taps the QR area | "บันทึกรูป QR" button fades in |
| AC4 | User taps "บันทึกรูป QR" | QR content replaced with happy card image + thank-you message |
| AC5 | User taps "ไม่สะดวกโอน" (from State B or C) | Returns to category selection, no happy card |
| AC6 | User finishes quiz with score 3+ | Normal summary, no donation interaction, card not tappable |
