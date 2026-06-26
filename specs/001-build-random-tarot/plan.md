# Implementation Plan: Random Tarot Reading Web App

**Branch**: `001-build-random-tarot` | **Date**: 2026-06-26 | **Spec**: `specs/001-build-random-tarot/spec.md`

**Input**: Feature specification from `/specs/001-build-random-tarot/spec.md`

---

## Summary

A single-page web app that displays a random tarot card (cat-themed artwork) centered on the page when the user clicks a "Draw Card" button. The card shows its name and meaning. Built with plain HTML/CSS/JavaScript — no backend, no framework, fully client-side.

---

## Technical Context

**Language/Version**: HTML5, CSS3, JavaScript (ES6+)

**Primary Dependencies**: None — pure vanilla stack.

**Storage**: N/A (no persistence; session-only state)

**Testing**: Manual browser testing; automated checks via Playwright or similar if desired.

**Target Platform**: Modern desktop and mobile browsers (Chrome, Firefox, Safari, Edge)

**Project Type**: Single-page web application (SPA, no framework)

**Performance Goals**: Page loads in under 2 seconds; card draw responds within 500ms.

**Constraints**: Works offline once assets are loaded. No backend required.

**Scale/Scope**: Single page, single feature, 10–22 tarot cards.

---

## Constitution Check

- [x] Artifacts conform to the canonical templates in `.specify/templates/`
- [x] Plan version follows `MAJOR.MINOR` format and is recorded in the plan header
- [ ] Any deviation from template structure is documented and justified below

*(No deviations from template structure.)*

---

## Project Structure

### Documentation (this feature)

```text
specs/001-build-random-tarot/
├── spec.md          # Feature spec (done)
├── plan.md          # This file
├── research.md      # N/A — no external research needed for vanilla HTML/CSS/JS
├── data-model.md    # N/A — no persistent data model
└── tasks.md         # Task breakdown (to be created by /speckit.tasks)
```

### Source Code (repository root)

```text
apps/tarot/
├── index.html       # Main page — centered layout, button, card display
├── styles.css       # Page styling, centering, card display, animations
├── app.js           # Card deck data, random draw logic, DOM updates
└── assets/
    └── cards/       # Cat-style tarot card images (22 images, JPG/PNG)
```

**Structure Decision**: `apps/tarot/` as a self-contained app directory. No framework, no build step — open `index.html` directly in a browser.

---

## Implementation Phases

### Phase 1 — Project Scaffold & Card Data

1. Create `apps/tarot/` directory structure.
2. Create `app.js` with a hardcoded deck of 22 tarot cards (id, name, meaning, imageUrl).
3. Use placeholder images (e.g., colored rectangles or Unsplash cat photos) for now — replace with final cat-style art later.
4. Write `index.html` shell with button and empty card display area.
5. Link CSS and JS files.

### Phase 2 — Core Draw Logic

1. Implement `drawCard()` function in `app.js`:
   - Pick a random card from the deck.
   - Update the DOM to show the card image, name, and meaning.
2. Wire the button `onclick` to `drawCard()`.
3. Disable button during card reveal animation to prevent spam.
4. Show loading spinner while image loads (use a CSS animation or a hidden element).

### Phase 3 — Styling & Layout

1. Style the page: dark mystical theme (deep purple/navy background, gold accents).
2. Center the card image vertically and horizontally using Flexbox.
3. Style the "Draw Card" button: large, prominent, cat/mystical themed (gold border, hover effect).
4. Style the card name and meaning text below the image.
5. Make layout responsive: stack naturally on mobile; image scales down gracefully.

### Phase 4 — Card Artwork (Cat-Style)

1. Replace placeholder images with actual cat-themed tarot card illustrations.
2. Sources to consider: commission-free cat tarot art, public domain, or generated.
3. Ensure all 22 cards have consistent art style.
4. Optimize images for web (WebP with JPEG fallback, max 400KB per image).

### Phase 5 — Polish & Edge Cases

1. Add a fade-in animation when a card is drawn.
2. Handle image load error gracefully (fallback to card name only).
3. Add a "Draw Again" option or auto-re-enable the button after animation.
4. Test on mobile (iOS Safari, Android Chrome).
5. Add a page title and meta description for SEO/shareability.

---

## Complexity Tracking

No complexity violations. Single project, vanilla stack, no architectural overhead.

---

## Tarot Card Deck (22 Major Arcana)

| # | Name | Meaning |
|---|------|---------|
| 0 | The Cat's Eye | Curiosity leads to discovery. |
| 1 | The Magician (Cat) | You have all the tools you need. |
| 2 | The High Priestess (Cat) | Trust your intuition. |
| 3 | The Empress (Cat) | Nurture yourself and others. |
| 4 | The Emperor (Cat) | Take charge with wisdom. |
| 5 | The Hierophant (Cat) | Seek guidance from tradition. |
| 6 | The Lovers (Cats) | A choice about matters of the heart. |
| 7 | The Chariot (Cat) | Determination conquers obstacles. |
| 8 | Strength (Cat) | Courage and gentle power. |
| 9 | The Hermit (Cat) | A time for reflection. |
| 10 | Wheel of Fortune (Cat) | Fate turns in your favor. |
| 11 | Justice (Cat) | Fairness and truth prevail. |
| 12 | The Hanged Cat | Pause and see from another angle. |
| 13 | The Cat's Death | Transformation and new beginnings. |
| 14 | Temperance (Cat) | Balance in all things. |
| 15 | The Cat Devil | Temptation at play — watch your desires. |
| 16 | The Cat Tower | Let go of what no longer serves you. |
| 17 | The Star (Cat) | Hope and inspiration shine on you. |
| 18 | The Moon (Cat) | Things may not be as they seem. |
| 19 | The Sun (Cat) | Joy, success, and warmth. |
| 20 | Judgement (Cat) | A moment of awakening. |
| 21 | The World (Cat) | Completion and fulfillment. |
