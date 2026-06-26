# Implementation Plan: Unique Cat-Style Cartoon Tarot Card Illustrations

**Branch**: `002-unique-cat-style` | **Date**: 2026-06-26 | **Spec**: `specs/002-unique-cat-style/spec.md`

---

## Summary

Replace the 22 placeholder tarot card images in `apps/tarot/assets/cards/` with unique, hand-crafted SVG illustrations — each card gets its own distinct cat character with a cute cartoon style, bright colorful palette, and a scene that reflects the card's meaning.

---

## Technical Context

**Language/Version**: SVG (inline, XML-based) + HTML/CSS/JS (existing app unchanged)

**Primary Dependencies**: None — pure SVG, no external libraries or tools

**Storage**: `apps/tarot/assets/cards/` — 22 SVG files replacing current placeholders

**Testing**: Manual browser testing — open each card SVG file and visually verify uniqueness and style

**Target Platform**: Modern browsers (Chrome, Firefox, Safari, Edge) — SVG rendering is universal

**Project Type**: Art asset replacement (no code changes)

**Performance Goals**: SVG files should be under 10KB each for fast loading

**Constraints**: All illustrations must use the same cartoon style, proportions, and resolution. No external image URLs.

**Scale/Scope**: 22 unique SVG illustrations

---

## Constitution Check

- [x] Artifacts conform to the canonical templates in `.specify/templates/`
- [x] Plan version follows `MAJOR.MINOR` format and is recorded in the plan header
- [ ] Any deviation from template structure is documented and justified below

*(No deviations.)*

---

## Project Structure

### Source Code

```text
apps/tarot/assets/cards/
├── card-00.svg  ← The Cat's Eye         (UNIQUE illustration)
├── card-01.svg  ← The Magician (Cat)     (UNIQUE illustration)
├── card-02.svg  ← The High Priestess    (UNIQUE illustration)
...
└── card-21.svg  ← The World (Cat)       (UNIQUE illustration)
```

---

## Illustration Design System (Per Card)

Each SVG uses a consistent cartoon aesthetic:
- **Cat character**: Round head, large expressive eyes, small nose, curved mouth, rounded ears, fluffy body
- **Style**: Flat colors with subtle shading, thick dark outlines (~2-3px), rounded strokes
- **Background**: Circular or rounded-rect frame with a theme-appropriate gradient or solid color
- **Text**: Card name in a cute rounded font at the top of the card
- **Palette**: Each card gets its own 3-4 color palette matching its mood
- **Size**: 300×480 viewBox (matching existing cards)

---

## Card-by-Card Illustration Concepts

| Card | Scene Concept | Palette |
|------|---------------|---------|
| 0 - The Cat's Eye | Mystical cat with a large glowing eye (third eye), stars around | Purple + gold + teal |
| 1 - The Magician (Cat) | Cat at a magical table with floating sparkles and a wand | Red + gold + white |
| 2 - The High Priestess (Cat) | Cat seated on a crescent moon, mysterious aura | Navy + silver + purple |
| 3 - The Empress (Cat) | Cat surrounded by flowers, butterflies, soft garden | Green + pink + yellow |
| 4 - The Emperor (Cat) | Cat wearing a crown and cape on a throne | Deep blue + gold + red |
| 5 - The Hierophant (Cat) | Cat in robes with ancient symbols, wise look | Brown + gold + cream |
| 6 - The Lovers (Cats) | Two cats facing each other with a heart between them | Pink + red + white |
| 7 - The Chariot (Cat) | Cat riding a tiny chariot pulled by moths | Blue + silver + navy |
| 8 - Strength (Cat) | Cat gently holding a glowing orb, serene expression | Orange + warm yellow + white |
| 9 - The Hermit (Cat) | Cat with a lantern on a hilltop under stars | Dark grey + gold + deep blue |
| 10 - Wheel of Fortune (Cat) | Cat atop a giant wheel covered in stars | Purple + pink + gold |
| 11 - Justice (Cat) | Cat holding a large golden scales | Teal + gold + white |
| 12 - The Hanged Cat | Cat hanging upside down by its tail, smiling | Yellow + lime + light blue |
| 13 - The Cat's Death | Cat as a friendly skeleton with flowers | Black + dark green + white |
| 14 - Temperance (Cat) | Cat mixing a colorful potion with two cups | Turquoise + purple + orange |
| 15 - The Cat Devil | Cat with tiny horns and pitchfork, playful smirk | Dark red + black + orange |
| 16 - The Cat Tower | Tower crumbling with cats falling out, dramatic | Dark purple + grey + gold |
| 17 - The Star (Cat) | Cat gazing at a giant glowing star in the sky | Light blue + silver + yellow |
| 18 - The Moon (Cat) | Cat howling at a giant crescent moon, bats around | Dark indigo + silver + purple |
| 19 - The Sun (Cat) | Cat in a sunflower field, bright sun behind | Yellow + orange + green |
| 20 - Judgement (Cat) | Cat rising from a golden cloud with wings | White + gold + light pink |
| 21 - The World (Cat) | Cat inside a wreath of flowers and stars | Green + gold + pink + teal |

---

## Implementation Phases

### Phase 1 — Design the Cat Cartoon Template

Create a reusable base SVG structure:
1. Standard card frame (rounded rect with border)
2. Standard cat character template (head, ears, eyes, nose, mouth, whiskers, body)
3. Standard text placement for card name and number
4. Define common `<defs>` (gradients, reusable shapes) for consistency

### Phase 2 — Create Card 0 (Reference) — The Cat's Eye

Build card-00.svg as the first fully detailed illustration:
- Mystical cat with glowing third eye
- Stars and cosmic elements
- Sets the quality benchmark for all other cards

### Phase 3 — Create Cards 1–11 (First Half of Deck)

Replace cards 01 through 11 with unique illustrations following the design system.

### Phase 4 — Create Cards 12–21 (Second Half of Deck)

Replace cards 12 through 21 with unique illustrations.

### Phase 5 — Verify & Test

1. Open `apps/tarot/index.html` in browser
2. Draw all 22 cards — confirm each illustration loads and is unique
3. Test image load error fallback (spec FR-007)
4. Verify responsive scaling on mobile

---

## Complexity Tracking

No complexity violations. Art asset replacement only — no architectural changes, no code changes.
