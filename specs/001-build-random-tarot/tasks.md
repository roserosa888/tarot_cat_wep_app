---
description: "Task list for Random Tarot Reading Web App"
---

# Tasks: Random Tarot Reading Web App

**Constitution**: `.specify/memory/constitution.md` (Principle I: Template-Driven Consistency, Principle VI: Versioned Artifacts)

**Input**: Design documents from `/specs/001-build-random-tarot/`

**Prerequisites**: plan.md (done), spec.md (done)

**Version**: 1.0 | **Last Amended**: 2026-06-26

---

## Phase 1: Setup (Shared Infrastructure)

- [ ] T001 Create `apps/tarot/` directory structure with `index.html`, `styles.css`, `app.js`, and `assets/cards/` subfolder
- [ ] T002 Add placeholder card images in `assets/cards/` (22 files named `card-00.webp` through `card-21.webp`)

---

## Phase 2: Foundational (Blocking Prerequisites)

- [ ] T003 Write the 22-card Major Arcana deck array in `apps/tarot/app.js` with fields: `id`, `name`, `meaning`, `imageUrl`
- [ ] T004 Build `apps/tarot/index.html` skeleton: empty card display div, "Draw Card" button, link to CSS and JS
- [ ] T005 Implement `drawCard()` function in `apps/tarot/app.js` — picks random card, updates DOM with image, name, meaning

---

## Phase 3: User Story 1 — Draw a Random Tarot Card (P1) 🎯 MVP

**Goal**: User clicks button and sees a random cat-style card centered on the page

**Independent Test**: Load `index.html`, click button, verify a card image appears centered with no page scroll required on 1280px viewport

- [ ] T006 [P] [US1] Implement `drawCard()` random selection logic in `apps/tarot/app.js`
- [ ] T007 [P] [US1] Wire button `onclick` to `drawCard()` in `apps/tarot/index.html`
- [ ] T008 [US1] Disable button during card reveal animation (prevent spam clicks)
- [ ] T009 [US1] Show loading spinner while card image is fetching (CSS animation in `styles.css`)
- [ ] T010 [US1] Handle image load error gracefully — show card name and meaning without image

**Checkpoint**: Card draw works reliably, centered, responsive

---

## Phase 4: User Story 2 — Read the Card's Meaning (P2)

**Goal**: Card name and meaning text appear alongside the image after a draw

**Independent Test**: Draw a card and verify card name and meaning are visible without scrolling on 375px mobile viewport

- [ ] T011 [P] [US2] Add card name heading (h2) and meaning paragraph below card image in `apps/tarot/index.html`
- [ ] T012 [P] [US2] Update `drawCard()` in `apps/tarot/app.js` to inject name and meaning into DOM on each draw
- [ ] T013 [US2] Style card name and meaning text in `styles.css` (font, color, spacing, readable on dark background)

**Checkpoint**: Card name and meaning always display correctly after a draw on mobile and desktop

---

## Phase 5: User Story 3 — Enjoy the Cat-Style Aesthetic (P3)

**Goal**: All card artwork uses a consistent cat-themed illustration style

**Independent Test**: Draw 5+ cards and visually confirm each image has cats in a whimsical illustration style

- [ ] T014 [P] [US3] Replace placeholder images in `apps/tarot/assets/cards/` with final cat-style tarot illustrations
- [ ] T015 [P] [US3] Verify all 22 card images are present and named correctly in `assets/cards/`
- [ ] T016 [US3] Confirm consistent art style across the deck (no mixed styles)

**Checkpoint**: All 22 cards have consistent cat-themed art

---

## Phase 6: Polish & Cross-Cutting Concerns

- [ ] T017 [P] Style the page in `styles.css` — dark mystical theme (deep purple/navy background, gold text/borders)
- [ ] T018 [P] Style the "Draw Card" button — large, gold border, hover glow effect, centered
- [ ] T019 Add fade-in CSS animation (`@keyframes fadeIn`) when a card appears
- [ ] T020 Make layout responsive: card and text scale down gracefully on mobile (375px wide); no horizontal scroll
- [ ] T021 Add `<title>` and `<meta name="description">` to `index.html` for basic SEO
- [ ] T022 Test on mobile browsers: iOS Safari and Android Chrome (manual or Playwright)
- [ ] T023 Re-enable button after card reveal animation completes

---

## Dependencies & Execution Order

### Phase Dependencies

- **Phase 1 (Setup)**: No dependencies — start immediately
- **Phase 2 (Foundational)**: Depends on Phase 1 — blocks all user stories
- **Phase 3 (US1)**: Depends on Phase 2 — the MVP core
- **Phase 4 (US2)**: Depends on Phase 2 — can run in parallel with Phase 3
- **Phase 5 (US3)**: Depends on Phase 2 — can run in parallel with Phases 3 and 4
- **Phase 6 (Polish)**: Depends on all user stories complete

### MVP Strategy (Recommended)

1. Complete Phases 1 + 2
2. Complete Phase 3 (US1) → Test → **You have a working tarot draw!**
3. Add Phase 4 (US2) → meanings appear
4. Add Phase 5 (US3) → cat art added
5. Polish (Phase 6) → beautiful and responsive

---

## Notes

- No automated tests required for this scope — manual browser testing suffices
- Card images: use placeholder colored rectangles until final cat art is sourced
- No backend, no build step — open `index.html` directly in any browser
- The `apps/tarot/` folder is self-contained and portable
