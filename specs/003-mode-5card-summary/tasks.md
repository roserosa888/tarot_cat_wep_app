# Tasks: Mode 5-Card Summary with Point-Based Level Scoring

---

## Task 1 — Add scoring & level helper functions
**Status**: pending

Add `computeScore(cards)` and `computeLevel(score)` to `app.js`.

- `computeScore`: sum A answers (+1 each), B = 0. Range 0–10.
- `computeLevel`:
  - 0–2 → 1
  - 3–5 → 2
  - 6–7 → 3
  - 8–10 → 4

**Files**: `apps/tarot/app.js`

---

## Task 2 — Add 5-card spread selection logic
**Status**: pending

Add `select5CardSpread(cards, category, level)` to `app.js`. Returns array of 5 card indices based on pattern analysis. See plan Phase 3 for full mapping table.

**Files**: `apps/tarot/app.js`

---

## Task 3 — Add per-card reasoning generation
**Status**: pending

Add `generateCardReasoning(cardIndex, position, analysis, level, score)` to `app.js`. Each of the 5 positions (0–4) gets distinct reasoning that references the user's actual pattern data.

**Files**: `apps/tarot/app.js`

---

## Task 4 — Add POSITION_LABELS and POSITION_CONTEXTS constants
**Status**: pending

Add these at the top of the section in `app.js` alongside `computeScore`:

```js
const POSITION_LABELS = ['แกนหลัก', 'ความท้าทาย', 'รากฐาน', 'สนับสนุน', 'ผลลัพธ์'];
const POSITION_CONTEXTS = [
  'นี่คือไพ่ที่สะท้อนแกนหลักของการอ่านดวงคุณ',
  'นี่คือไพ่แห่งความท้าทายที่ต้องตระหนัก',
  'นี่คือไพ่แห่งรากฐานที่คอยหล่อเลี้ยงคุณ',
  'นี่คือไพ่แห่งการสนับสนุนที่คอยเสริมแรงคุณ',
  'นี่คือไพ่แห่งผลลัพธ์ที่คุณกำลังมุ่งไป',
];
```

**Files**: `apps/tarot/app.js`

---

## Task 5 — Replace `showSummary()` with new 5-card version
**Status**: pending

Replace the existing `showSummary()` function with the new version that:
- Computes score + level
- Renders 5 spread cards
- Renders pattern strip
- Renders 5 reasoning paragraphs
- Updates score/level display

**Files**: `apps/tarot/app.js`

---

## Task 6 — Update summary HTML in `index.html`
**Status**: pending

Replace the summary screen `<div id="screen-summary">` contents with the new structure (score-row, card-spread container, pattern-strip, reasoning-list).

**Files**: `apps/tarot/index.html`

---

## Task 7 — Add CSS for new summary UI
**Status**: pending

Add CSS for:
- `.score-level-row`, `.score-display`, `.level-display`
- `.card-spread`, `.spread-card`, `.spread-card-img`, `.spread-card-label`
- `.pattern-strip`
- `.reasoning-list`, `.card-reasoning`, `.card-reasoning-header`, `.card-reasoning-context`, `.card-reasoning-body`

**Files**: `apps/tarot/styles.css`

---

## Task 8 — Remove old single-card summary elements
**Status**: pending

Remove old elements from `index.html` that are no longer needed:
- `#summary-card-img` (single card image)
- `#summary-card-name`
- `#summary-reason`
- `#summary-pattern`

**Files**: `apps/tarot/index.html`

---

## Task 9 — Test all scenarios in browser
**Status**: pending

Verify:
1. Score = 10 (all A) → Level 4 ✓
2. Score = 0 (all B) → Level 1 ✓
3. Score = 5 → Level 2 ✓
4. Score = 7 → Level 3 ✓
5. Score = 9 → Level 4 ✓
6. 5 cards displayed in spread ✓
7. Each card has reasoning with correct pattern data ✓
8. Restart button works ✓
