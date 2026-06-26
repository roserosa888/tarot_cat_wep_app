---
description: "Task list for Unique Cat-Style Cartoon Tarot Card Illustrations"
---

# Tasks: Unique Cat-Style Cartoon Tarot Card Illustrations

**Constitution**: `.specify/memory/constitution.md`

**Input**: Design documents from `/specs/002-unique-cat-style/`

**Version**: 1.0 | **Last Amended**: 2026-06-26

---

## Phase 1: Design System & Template

- [ ] T001 Design the cartoon cat base template (reusable SVG structure: round head, big eyes, ears, nose, mouth, whiskers, fluffy body)
- [ ] T002 Define standard card frame (300×480 viewBox, rounded rect, border, card name placement, card number placement)
- [ ] T003 Define color palette guide — 3-4 colors per card, consistent with each card's mood

---

## Phase 2: The Cat's Eye — Card 0 (Reference)

- [ ] T004 Create `card-00.svg` — The Cat's Eye: mystical cat with glowing third eye, stars, cosmic purple/gold/teal palette. Sets the quality benchmark.

---

## Phase 3: Cards 1–11

- [ ] T005 [P] Create `card-01.svg` — The Magician (Cat): cat at magical table, wand, sparkles, red/gold/white
- [ ] T006 [P] Create `card-02.svg` — The High Priestess (Cat): cat on crescent moon, mysterious aura, navy/silver/purple
- [ ] T007 [P] Create `card-03.svg` — The Empress (Cat): cat in garden, flowers, butterflies, green/pink/yellow
- [ ] T008 [P] Create `card-04.svg` — The Emperor (Cat): cat with crown & cape on throne, deep blue/gold/red
- [ ] T009 [P] Create `card-05.svg` — The Hierophant (Cat): cat in robes, ancient symbols, wise look, brown/gold/cream
- [ ] T010 [P] Create `card-06.svg` — The Lovers (Cats): two cats facing each other, heart between them, pink/red/white
- [ ] T011 [P] Create `card-07.svg` — The Chariot (Cat): cat riding chariot pulled by moths, blue/silver/navy
- [ ] T012 [P] Create `card-08.svg` — Strength (Cat): cat holding glowing orb, serene, orange/yellow/white
- [ ] T013 [P] Create `card-09.svg` — The Hermit (Cat): cat with lantern on hilltop, stars, dark grey/gold/deep blue
- [ ] T014 [P] Create `card-10.svg` — Wheel of Fortune (Cat): cat atop giant wheel with stars, purple/pink/gold
- [ ] T015 [P] Create `card-11.svg` — Justice (Cat): cat holding golden scales, teal/gold/white

---

## Phase 4: Cards 12–21

- [ ] T016 [P] Create `card-12.svg` — The Hanged Cat: cat hanging upside down by tail, smiling, yellow/lime/light blue
- [ ] T017 [P] Create `card-13.svg` — The Cat's Death: friendly skeleton cat with flowers, black/dark green/white
- [ ] T018 [P] Create `card-14.svg` — Temperance (Cat): cat mixing colorful potion with two cups, turquoise/purple/orange
- [ ] T019 [P] Create `card-15.svg` — The Cat Devil: cat with tiny horns & pitchfork, playful smirk, dark red/black/orange
- [ ] T020 [P] Create `card-16.svg` — The Cat Tower: tower crumbling, cats falling out, dramatic, dark purple/grey/gold
- [ ] T021 [P] Create `card-17.svg` — The Star (Cat): cat gazing at giant glowing star, light blue/silver/yellow
- [ ] T022 [P] Create `card-18.svg` — The Moon (Cat): cat howling at crescent moon, bats around, dark indigo/silver/purple
- [ ] T023 [P] Create `card-19.svg` — The Sun (Cat): cat in sunflower field, bright sun, yellow/orange/green
- [ ] T024 [P] Create `card-20.svg` — Judgement (Cat): cat rising from golden cloud with wings, white/gold/light pink
- [ ] T025 [P] Create `card-21.svg` — The World (Cat): cat inside wreath of flowers & stars, green/gold/pink/teal

---

## Phase 5: Verification

- [ ] T026 Open `apps/tarot/index.html` — draw all 22 cards, confirm each loads correctly
- [ ] T027 Verify all 22 illustrations are visually unique (no identical or near-identical designs)
- [ ] T028 Verify all illustrations reflect their card's name/theme meaningfully
- [ ] T029 Test image load error fallback (app shows card name/meaning if image fails)
- [ ] T030 Test responsive layout on mobile (375px wide) — no horizontal scroll

---

## Dependencies & Execution Order

- **Phase 1** → Phase 2 → Phase 3 → Phase 4 → Phase 5
- All `[P]` tasks in Phases 3 & 4 are independent and can run in parallel
- Cards in Phase 3 and Phase 4 can run in parallel once Phase 1 is done

---

## Notes

- All SVGs: 300×480 viewBox, cute cartoon style, thick outlines, no scary imagery
- No external image URLs — all art embedded as SVG code
- Each SVG file replaces the existing placeholder in `apps/tarot/assets/cards/`
