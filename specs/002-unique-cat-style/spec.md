# Feature Specification: Unique Cat-Style Cartoon Tarot Card Illustrations

**Feature Branch**: `002-unique-cat-style`

**Created**: 2026-06-26

**Status**: Draft

**Constitution**: `.specify/memory/constitution.md` (Principle I: Template-Driven Consistency)

**Input**: User description: "Add unique cat-style cartoon illustrations to each tarot card — every card must have a different picture, cat style, cartoon, cute, and colorful"

---

## User Scenarios & Testing *(mandatory)*

### User Story 1 - View a Unique Cat Illustration Per Card (Priority: P1)

As a user, when I draw a tarot card, I want each of the 22 cards to have its own unique, distinct cat-style cartoon illustration — not just a color swap of the same silhouette.

**Why this priority**: The current placeholder cards use the same cat silhouette with only color changes. Each card must now have its own character, pose, and scene — this is the core improvement.

**Independent Test**: Load `index.html`, draw all 22 cards, and confirm every card image is visually unique (different cat character/scene, not just a palette swap).

**Acceptance Scenarios**:

1. **Given** I draw card 0 (The Cat's Eye), **Then** the image shows a unique cat character (e.g., a mystical cat with a glowing eye) visually distinct from all other cards.
2. **Given** I draw card 6 (The Lovers — Cats), **Then** the image shows two cats together — unique among the deck.
3. **Given** I draw any two different cards, **Then** their illustrations are clearly different in character, pose, and scene.

---

### User Story 2 - Enjoy a Cute & Colorful Cartoon Style (Priority: P2)

As a user, all 22 card illustrations must use a cute, cartoon, colorful art style — bright colors, soft rounded shapes, expressive cat faces — so the app feels playful and charming.

**Why this priority**: This defines the art direction and ensures visual consistency across the entire deck.

**Independent Test**: Draw 5+ cards and verify each illustration uses bright colors, rounded shapes, and expressive cute cat faces.

**Acceptance Scenarios**:

1. **Given** any card is drawn, **When** the image loads, **Then** the illustration uses a cute cartoon style (not realistic, not abstract).
2. **Given** any card is drawn, **When** the image loads, **Then** the colors are bright and varied — each card has its own vibrant palette.

---

### User Story 3 - Visual Consistency Across the Deck (Priority: P3)

As a user, although each card is unique, the entire deck should feel like a cohesive collection — same art style, same level of detail, same character proportions.

**Why this priority**: Prevents individual cards from looking like they came from different artists or styles.

**Acceptance Scenarios**:

1. **Given** any two cards from the deck, **When** compared side by side, **Then** they share the same consistent cartoon style, line weight, and color saturation.
2. **Given** the card text and meaning, **When** I look at the illustration, **Then** the illustration meaningfully reflects the card's name/theme.

---

### Edge Cases

- What happens if an SVG card image fails to load? → Show fallback: card name and meaning displayed without the image.
- How to handle cards with similar themes (e.g., multiple mystical cards)? → Each must have a unique cat character and scene — no repeated compositions.

---

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: Each of the 22 tarot cards MUST have a unique, hand-crafted SVG illustration visually distinct from all other cards.
- **FR-002**: All illustrations MUST use a cute cartoon style — rounded shapes, expressive cat faces, soft edges.
- **FR-003**: All illustrations MUST be colorful — each card uses a vibrant palette appropriate to its theme.
- **FR-004**: The illustration for each card MUST meaningfully reflect the card's name (e.g., The Lovers shows two cats, The Hanged Cat shows an upside-down cat).
- **FR-005**: The 22 unique SVG illustrations MUST replace the current placeholder card images in `apps/tarot/assets/cards/`.
- **FR-006**: Each SVG illustration MUST be a separate SVG file, replacing the existing placeholder files.
- **FR-007**: The app MUST handle missing/failed card images gracefully (fallback text display).

---

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: All 22 cards in `apps/tarot/assets/cards/` have visually distinct SVG illustrations — zero identical or near-identical designs.
- **SC-002**: A user can draw all 22 cards and each illustration clearly matches the card's theme/name.
- **SC-003**: The app loads all 22 card images without errors in Chrome, Firefox, and Safari.
- **SC-004**: All illustrations maintain the cute cartoon aesthetic — verified by visual inspection.

---

## Assumptions

- **SVG format**: All illustrations are hand-crafted inline SVGs — scalable, lightweight, version-controllable.
- **No external assets**: No external image URLs — all art is embedded as SVG code.
- **Inline SVG preferred**: Each card SVG is a standalone file in `assets/cards/` that can be opened and edited directly.
