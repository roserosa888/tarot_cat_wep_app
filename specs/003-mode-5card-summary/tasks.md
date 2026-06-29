# Tasks: Thai-Language Tarot 10-Question Fork Reading

**Version**: 2.0 | **Last Amended**: 2026-06-29

---

## Task 1 — Replace spec files
**Status**: in_progress

Replace `spec.md`, `plan.md`, `tasks.md` in `specs/003-mode-5card-summary/` with the new spec.

**Files**: `specs/003-mode-5card-summary/spec.md`, `plan.md`, `tasks.md`

---

## Task 2 — Replace `app.js`
**Status**: pending

Write a new `app.js` with:
- `deck` array (keep existing 22 cards, DO NOT modify)
- New `categories` as a `const categories = function(question) { ... }` wrapper with:
  - 5 categories × 10 questions (A/B objects, no A_card/B_card fields)
  - `outcomes: [5]` array per category (card indices for score 0–2, 3–4, 5–6, 7–8, 9–10)
  - `outcomeNames: [5]` array per category
- `scoreToOutcomeIndex(score)` — maps 0–10 to 0–4
- `analyzePattern(cards)` — existing logic from current file
- `generateNarrative(category, cardIndex, cards)` — pattern-synthesized narrative
- State: `currentCategory`, `currentQ`, `drawnCards` (now `{choiceLabel, questionText}`)
- `startReading(cat)` — reset + show question screen
- `loadQuestion()` — render Q text/context, A/B buttons; A=0pts, B=1pt
- `selectChoice(choice, question)` — push to drawnCards, advance or call showSummary
- `showSummary()` — compute score, map to outcome card, reveal with animation
- `showScreen(name)` — existing screen switching
- `initStars()` — existing star animation
- `$('restart-btn')` handler — reset to category select

**Files**: `apps/tarot/app.js`

---

## Task 3 — Replace `index.html`
**Status**: pending

Replace entire file. Keep IDs: `stars`, `screen-select`, `screen-question`, `screen-summary`, `category-grid`, `category-badge`, `progress-bar`, `progress-label`, `question-number`, `question-text`, `question-context`, `choice-a`, `choice-b`, `summary-card-img`, `summary-card-name`, `summary-narrative`, `summary-pattern`, `restart-btn`.

New structure:
- Disclaimer bar (always visible)
- `#screen-select` — title + subtitle + 5 category buttons
- `#screen-question` — progress header + question card
- `#screen-summary` — title + category label + card wrap + name + narrative + pattern + restart

**Files**: `apps/tarot/index.html`

---

## Task 4 — Replace `styles.css`
**Status**: pending

Replace entire file. Keep: `:root` vars, `.star`, `.stars`, `.disclaimer`, `.screen`, `.hidden`, `.fadeUp`, `.main-title`, `.main-subtitle`, `.category-grid`, `.category-card`, `.progress-bar`, `.progress-label`, `.category-badge`, `.question-card`, `.question-text`, `.question-context`, `.choice-a`, `.choice-b`, `.choice-label`, `.choice-path`, `.choice-trade`, `.btn-glow`, mobile breakpoints.

New additions:
- `.summary-card-wrap` — wrapper for animated card reveal
- `.summary-card-wrap.revealed` — visible/fade state
- `@keyframes cardReveal` — spin + fade
- `.summary-narrative` — narrative text container with `p`/`strong` styling

Remove: `.summary-crystal`, `.summary-single-card-wrap`, `.reveal-*`, `.card-spinner`, `.spinning-card`, `.card-drawn`, `.card-name-reveal`, `.card-interpretation`, `.crystal-orb`, `.orb-inner`, `.orb-ring`

**Files**: `apps/tarot/styles.css`

---

## Task 5 — Verify in browser
**Status**: pending

1. Serve: `npx serve apps/tarot -p 3333`
2. Open http://localhost:3333
3. Disclaimer visible at top ✓
4. All 5 category buttons visible ✓
5. Click การงาน → badge shows "การงาน" ✓
6. Q1 shows, progress bar at 0% ✓
7. Click A → advances to Q2 ✓
8. Progress bar increments each step ✓
9. Q10 → summary screen loads ✓
10. Card image shows (Hermit = all A) ✓
11. Narrative shows correct pattern string ✓
12. Restart → back to category select ✓
13. Test ความรัก category ✓
14. Test การเงิน category ✓
15. Test ตัวตน category ✓
16. Test ครอบครัว category ✓
17. Mobile at 375px — no overflow ✓
