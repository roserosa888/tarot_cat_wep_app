# Feature Specification: Mode 5-Card Summary with Point-Based Level Scoring

**Feature Branch**: `003-mode-5card-summary`

**Created**: 2026-06-26

**Status**: Draft

**Constitution**: `.specify/memory/constitution.md` (Principle I: Template-Driven Consistency)

**Input**: User description: "Add a mode where every question shows 5 card summary with reasoning — scoring: A=+1, B=0, sum all 10 answers, if total >10 is level 4"

---

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Play Reading Mode with 5-Card Summary (Priority: P1)

As a user, I want to answer 10 binary (A/B) questions and receive a 5-card tarot spread with reasoning for each card, so I get a rich personalized reading that explains why each card was chosen.

**Why this priority**: This is the core new mode — the entire value proposition of this feature. Without it, the feature does not exist.

**Independent Test**: Select category → answer all 10 questions → verify 5 cards appear in the summary with reasoning text for each card.

**Acceptance Scenarios**:

1. **Given** the user selects a category and answers all 10 questions, **When** they reach the summary screen, **Then** exactly 5 tarot cards are displayed in a spread layout.
2. **Given** the user is on the summary screen, **When** the cards load, **Then** each card has a unique reasoning paragraph explaining why that card was selected based on the user's answer pattern.
3. **Given** the user restarts and plays the same category, **When** they answer differently, **Then** the 5-card spread and reasoning differ accordingly.
4. **Given** the user completes a reading, **When** they see the summary, **Then** the 5 cards are visually arranged in a spread (e.g., row of 5 or a cross/triangle layout).

---

### User Story 2 - Point-Based Scoring & Level Calculation (Priority: P2)

As a user, I want my 10 answers to be scored (A = +1 point, B = 0 points) and displayed as a level (1–5), so I understand the intensity/energy of my overall reading.

**Why this priority**: The scoring gives users a quick numeric summary of their pattern. Without it, the 5-card spread feels less contextualized.

**Independent Test**: Answer known combinations of A/B, verify the displayed score and level match the expected values.

**Acceptance Scenarios**:

1. **Given** the user answers all A (10 A's, 0 B's), **Then** the score is 10 and the level displayed is **5** (max).
2. **Given** the user answers 0 A's and 10 B's, **Then** the score is 0 and the level displayed is **1** (min).
3. **Given** the user answers 6 A's and 4 B's (score = 6), **Then** the level displayed is **3** (score 6–7).
4. **Given** the user answers 9 A's and 1 B (score = 9), **Then** the level displayed is **4** (score 8–10).
5. **Given** the user answers 4 A's and 6 B's (score = 4), **Then** the level displayed is **2** (score 3–5).

**Level Table (scoring: A=+1, B=0, max 10)**:

| Score Range | Level |
|------------|-------|
| 0–2        | 1     |
| 3–5        | 2     |
| 6–7        | 3     |
| 8–10       | 4     |

---

### User Story 3 - Reasoning Support Per Card (Priority: P3)

As a user, each of the 5 cards in the spread should have reasoning that references specific questions/answers from my session, so I understand the connection between my choices and the card.

**Why this priority**: Reasoning makes the reading meaningful and personal — without it, cards feel random.

**Acceptance Scenarios**:

1. **Given** card 0 is displayed, **When** the reasoning renders, **Then** it references the user's specific answer pattern (e.g., "คุณเลือก A 7 ครั้งจาก 10 คำถาม แสดงถึง...").
2. **Given** the user answers with an alternating pattern (ABAB...), **When** the reasoning for any card is shown, **Then** it notes the alternating tendency and interprets it.
3. **Given** the user answers with a clustered pattern (e.g., AAAA then BBBB), **When** the reasoning is shown, **Then** it notes the clustering behavior.

---

### Edge Cases

- What if the user answers exactly 5 A's (score = 5)? → Level 2, because 5 falls in range 3–5.
- What if the user clicks "restart" mid-reading? → Reset state entirely, return to category select.
- What if the card reasoning is too long? → Text truncates or scrolls gracefully within its card container.
- Does the 5-card spread reuse the existing card images? → Yes, reuse existing `card-XX.png` files.

---

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: After 10 answered questions, the summary screen MUST display exactly **5 tarot cards** in a spread layout.
- **FR-002**: Each of the 5 cards MUST display a **unique reasoning paragraph** explaining why that card was selected for the user's specific answer pattern.
- **FR-003**: The system MUST calculate a **score** by summing: A answers = +1 each, B answers = 0. Max score = 10.
- **FR-004**: The system MUST display a **level (1–4)** based on the score table:
  - Score 0–2 → Level 1
  - Score 3–5 → Level 2
  - Score 6–7 → Level 3
  - Score 8–10 → Level 4
- **FR-005**: The 5-card selection logic MUST be **pattern-aware**: cards are chosen based on the user's A/B streak, count, and clustering patterns — not random.
- **FR-006**: Each card's reasoning MUST reference the user's actual answer pattern (streak length, A count, alternation tendency, etc.).
- **FR-007**: The summary screen MUST also display the **A/B pattern string** (e.g., "AABBAABABB") and **score + level**.
- **FR-008**: The 5-card spread layout MUST be visually distinct from the current single-card summary — a row of 5 cards or a defined spread pattern.

### Key Entities

- **ReadingSession**: Represents a complete 10-question reading. Attributes: `category`, `answers[]` (each: `{choiceLabel, questionIndex}`), `score` (0–10), `level` (1–4).
- **CardSpread**: Represents the 5-card result. Attributes: `cards[]` (5 deck indices), `reasoning[]` (5 strings keyed to pattern analysis).
- **PatternAnalysis**: Represents the analysis of 10 answers. Attributes: `aCount`, `bCount`, `maxAStreak`, `maxBStreak`, `pattern` (string), `isAlternating`, `isClustered`, `dominantChoice`.

---

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: A user can complete 10 questions and see exactly 5 cards on the summary screen — no more, no fewer.
- **SC-002**: Each of the 5 cards has a unique reasoning paragraph visible on the summary screen.
- **SC-003**: Score = sum of A answers (B=0). Level matches the level table above for all possible scores 0–10.
- **SC-004**: Pattern string (e.g., "AABBAABABB") and score/level are visible on the summary screen.
- **SC-005**: Playing the same category twice with different answers produces different cards and/or different reasoning.
- **SC-006**: The 5-card spread is visually arranged (e.g., 5-card row or cross spread) — not a single card.

---

## Assumptions

- **Existing deck reuse**: The 22-card deck and existing card images (`card-XX.png`) are reused — no new images needed.
- **Standalone mode**: The 5-card mode replaces the current single-card summary mode (or runs as the only summary mode). No coexistence with the old mode is required.
- **Pattern-based card selection**: Card 0–4 of the spread are determined by the pattern analysis — not by random pick from the deck. The mapping from pattern → 5 card indices is deterministic per session.
- **No persistence**: Reading results are not stored across sessions.
- **Thai language**: All reasoning text is in Thai, matching the existing app language.
