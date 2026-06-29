# Implementation Plan: Thai-Language Tarot 10-Question Fork Reading

**Branch**: `003-mode-5card-summary` | **Date**: 2026-06-29
**Supersedes**: previous plan.md (2026-06-26) which covered the 5-card spread approach.

---

## Overview

Replace the existing `app.js` logic with a new fork-question flow:
- 5 life categories × 10 A/B questions
- A = 0 pts, B = 1 pt → score 0–10
- Score maps to 1 of 5 outcome cards per category (ranges: 0–2, 3–4, 5–6, 7–8, 9–10)
- Single animated card reveal + personalized narrative

---

## Phase 1 — New Data Structure

Replace `categories` in `app.js` with:

```js
const categories = function(question) {
const _cats = {
  "การงาน": {
    icon: "⚔️",
    outcomes: [9, 4, 7, 1, 17],  // score 0–2, 3–4, 5–6, 7–8, 9–10
    questions: [ /* 10 objects */ ]
  },
  // ...4 more categories...
};
return _cats;
};
```

Each question object:
```js
{
  text: "...",
  context: "...",
  A: { path: "...", trade: "..." },  // A = 0 pts
  B: { path: "...", trade: "..." }   // B = 1 pt
}
```

Each category has an `outcomes` array of 5 deck indices.

**DO NOT remove `const categories(question)`** — the variable name and function form must be preserved.

### Score → Card per Category

| Category | 0–2 | 3–4 | 5–6 | 7–8 | 9–10 |
|---|---|---|---|---|---|
| การงาน | Hermit (9) | Emperor (4) | Chariot (7) | Magician (1) | Star (17) |
| ความรัก | Hanged Cat (12) | Lovers (6) | Strength (8) | Sun (19) | World (21) |
| การเงิน | Justice (11) | Wheel (10) | Emperor (4) | Temperance (14) | Sun (19) |
| ตัวตน | Priestess (2) | Cat's Death (13) | Magician (1) | Star (17) | Sun (19) |
| ครอบครัว | Hierophant (5) | Empress (3) | Lovers (6) | Emperor (4) | World (21) |

---

## Phase 2 — Replace `app.js`

### Functions to add/replace

```js
// Score → outcome index (0–4)
function scoreToOutcomeIndex(score) {
  if (score <= 2) return 0;
  if (score <= 4) return 1;
  if (score <= 6) return 2;
  if (score <= 8) return 3;
  return 4; // 9–10
}

// Pattern analysis (reuse existing logic from current app.js)
function analyzePattern(cards) { ... }

// Narrative generation
function generateNarrative(category, cardIndex, cards) { ... }
```

### `selectChoice` — update scoring

```js
// OLD (A=+1, B=0)
drawnCards.push({ choiceLabel: choice, cardName: ..., cardMeaning: ..., ... });

// NEW (A=0, B=1)
drawnCards.push({
  choiceLabel: choice,
  questionText: question.text
});
// After 10 questions → call showSummary()
```

### `showSummary` — replace entirely

```js
function showSummary() {
  const score = drawnCards.reduce((s, c) => s + (c.choiceLabel === 'A' ? 0 : 1), 0);
  const outcomeIdx = scoreToOutcomeIndex(score);
  const cat = categories()[currentCategory];
  const cardIdx = cat.outcomes[outcomeIdx];

  $('summary-card-img').src = `assets/cards/card-${String(cardIdx).padStart(2,'0')}.png`;
  $('summary-card-name').textContent = deck[cardIdx].name;
  $('summary-narrative').innerHTML = generateNarrative(currentCategory, cardIdx, drawnCards);

  const aCount = drawnCards.filter(c => c.choiceLabel === 'A').length;
  const bCount = drawnCards.filter(c => c.choiceLabel === 'B').length;
  const pattern = drawnCards.map(c => c.choiceLabel).join('');
  $('summary-pattern').textContent = `รูปแบบ: ${pattern} · A: ${aCount} · B: ${bCount} · คะแนน: ${score}/10`;

  showScreen('summary');
}
```

---

## Phase 3 — Replace `index.html`

Replace the entire file with:
- Disclaimer bar (always visible)
- `#screen-select` — title + category grid (5 buttons)
- `#screen-question` — progress bar + Q card (A/B buttons)
- `#screen-summary` — card image + name + narrative + pattern + restart btn

Key HTML elements to keep IDs:
- `screen-select`, `screen-question`, `screen-summary` (screen switching)
- `stars` (star animation container)
- `progress-bar`, `progress-label`, `category-badge`
- `question-number`, `question-text`, `question-context`
- `choice-a`, `choice-b`
- `summary-card-img`, `summary-card-name`, `summary-narrative`, `summary-pattern`
- `restart-btn`
- `category-grid`, `category-badge`

New/renamed elements:
- `summary-card-wrap` (animation container around card image)

---

## Phase 4 — Replace `styles.css`

Keep all existing working styles:
- `:root` CSS variables
- `.star` / `.stars` (twinkle animation)
- `.disclaimer`
- `.screen`, `.hidden`, `.fadeUp`
- `.main-title`, `.main-subtitle`
- `.category-grid`, `.category-card`
- `.progress-bar`, `.progress-label`, `.category-badge`
- `.question-card`, `.question-text`, `.question-context`
- `.choice-a`, `.choice-b`, `.choice-label`, `.choice-path`, `.choice-trade`
- `.btn-glow`
- Mobile breakpoints

New additions:
- `.summary-card-wrap` — animation wrapper for card reveal
- `.summary-card-wrap.revealed` — triggers reveal animation
- `@keyframes cardReveal` — spin/flip then fade-in
- `.summary-narrative p` — paragraph styling in narrative

Remove unused styles:
- `.summary-crystal` / `.summary-orb-inner`
- `.summary-single-card-wrap`
- `.reveal-inner`, `.reveal-header`, `.card-reveal-wrap`, `.card-spinner`, `.spinning-card`, `.card-drawn`, `.card-name-reveal`, `.card-interpretation`
- `.crystal-orb`, `.orb-inner`, `.orb-ring`

---

## Phase 5 — Verification

1. Serve app: `npx serve apps/tarot -p 3333`
2. Open http://localhost:3333
3. Disclaimer visible at top ✓
4. Click การงาน → badge shows "การงาน" ✓
5. Progress bar shows Q1/10 ✓
6. Answer all 10 questions (try A-all → score 0 → Hermit card)
7. Answer all 10 questions (try B-all → score 10 → Star card for การงาน)
8. Narrative text references the actual pattern string ✓
9. Restart → back to category select ✓
10. Mobile: test at 375px width, no overflow ✓
