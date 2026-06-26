# Implementation Plan: Mode 5-Card Summary with Point-Based Level Scoring

---

## Phase 1 — Scoring & Level System

### 1.1 Add scoring logic to `app.js`

In `selectChoice()`, record each answer. Then after 10 questions, compute:

```js
// In selectChoice — already pushes to drawnCards with choiceLabel
drawnCards.push({ choiceLabel: choice === 'A' ? 'A' : 'B', ... });
```

Add a helper after all 10 answers:

```js
function computeScore(cards) {
  return cards.reduce((sum, c) => sum + (c.choiceLabel === 'A' ? 1 : 0), 0);
}

function computeLevel(score) {
  if (score <= 2) return 1;
  if (score <= 5) return 2;
  if (score <= 7) return 3;
  return 4; // score 8–10
}
```

**Files**: `apps/tarot/app.js`

---

## Phase 2 — Pattern Analysis (reuse existing `analyzePattern`)

The existing `analyzePattern(cards)` already computes:
- `aCount`, `bCount`
- `maxAStreak`, `maxBStreak`
- `isAlternating`, `isClustered`, `isNearlyAllA`, `isNearlyAllB`
- `dominantChoice`

This function already exists in `app.js`. No new code needed here.

---

## Phase 3 — 5-Card Spread Selection Logic

Add a new function `select5CardSpread(cards, category)` that maps pattern analysis → 5 card indices.

**Mapping strategy** (deterministic, pattern-aware):

| Position | Condition | Card(s) from Deck |
|----------|-----------|-------------------|
| Card 0 (Primary) | `dominantChoice='A'` → score-driven | High-action card (7,10,13,19) |
| Card 0 (Primary) | `dominantChoice='B'` → reflection | Contemplative card (2,9,12) |
| Card 0 (Primary) | `dominantChoice='balanced'` | Balance card (14,21) |
| Card 1 (Challenge) | `isClustered` | Disruption/change card (13,16) |
| Card 1 (Challenge) | `!isClustered` | Caution card (18,15) |
| Card 2 (Foundation) | `maxAStreak > maxBStreak` | Card matching A energy (7,1,4) |
| Card 2 (Foundation) | `maxBStreak >= maxAStreak` | Card matching B energy (9,2,5) |
| Card 3 (Support) | Category-specific | Category-supportive card |
| Card 4 (Outcome) | `level >= 3` | Growth/expansion card (17,19,21) |
| Card 4 (Outcome) | `level <= 2` | Grounding/stability card (3,4,5) |

Category overrides for Card 3 (Support):

| Category | Card 3 Default |
|----------|---------------|
| การงาน | 1 (Magician — tools) |
| ความรัก | 6 (Lovers) |
| การเงิน | 10 (Wheel of Fortune) |
| ตัวตน | 17 (Star) |
| ครอบครัว | 3 (Empress) |

```js
function select5CardSpread(cards, category, level) {
  const analysis = analyzePattern(cards);
  const { aCount, bCount, maxAStreak, maxBStreak,
          isClustered, dominantChoice } = analysis;

  // Card 0 — Primary
  let card0;
  if (dominantChoice === 'A') {
    const highAction = [7, 10, 13, 19];
    card0 = highAction[aCount % highAction.length];
  } else if (dominantChoice === 'B') {
    const contemplative = [2, 9, 12];
    card0 = contemplative[bCount % contemplative.length];
  } else {
    const balanced = [14, 21];
    card0 = balanced[aCount % balanced.length];
  }

  // Card 1 — Challenge
  let card1 = isClustered ? [13, 16][aCount % 2] : [18, 15][bCount % 2];

  // Card 2 — Foundation
  let card2 = maxAStreak > maxBStreak
    ? [7, 1, 4][maxAStreak % 3]
    : [9, 2, 5][maxBStreak % 3];

  // Card 3 — Support (category-driven)
  const catSupport = { 'การงาน': 1, 'ความรัก': 6, 'การเงิน': 10, 'ตัวตน': 17, 'ครอบครัว': 3 };
  let card3 = catSupport[category] || 6;

  // Card 4 — Outcome
  let card4 = level >= 3
    ? [17, 19, 21][level % 3]
    : [3, 4, 5][level % 3];

  return [card0, card1, card2, card3, card4];
}
```

**Files**: `apps/tarot/app.js`

---

## Phase 4 — Per-Card Reasoning Generation

Replace the single-card `generateDynamicReason()` with per-card reasoning.

Each card gets its own reasoning based on the pattern analysis, card position, and level.

```js
const POSITION_LABELS = ['แกนหลัก', 'ความท้าทาย', 'รากฐาน', 'การสนับสนุน', 'ผลลัพธ์'];
const POSITION_CONTEXTS = [
  'นี่คือไพ่ที่สะท้อนแกนหลักของการอ่านดวงคุณ — ไพ่นี้บอกถึงพลังหลักที่ขับเคลื่อนคุณ',
  'นี่คือไพ่แห่งความท้าทาย — ไพ่นี้บอกถึงสิ่งที่คุณต้องเผชิญหรือให้ความสนใจ',
  'นี่คือไพ่แห่งรากฐาน — ไพ่นี้บอกถึงแรงผลักดันพื้นฐานของคุณ',
  'นี่คือไพ่แห่งการสนับสนุน — ไพ่นี้บอกถึงพลังที่คอยช่วยเหลือคุณอยู่',
  'นี่คือไพ่แห่งผลลัพธ์ — ไพ่นี้บอกถึงทิศทางที่คุณกำลังมุ่งไป',
];
```

Per-card reasoning logic uses the pattern analysis to inject:
- A count and percentage
- Streak behavior
- Level indicator

```js
function generateCardReasoning(cardIndex, position, analysis, level, score) {
  const { aCount, bCount, maxAStreak, maxBStreak, isClustered, isAlternating } = analysis;
  const aPct = Math.round((aCount / 10) * 100);
  const posLabel = POSITION_LABELS[position];
  const posContext = POSITION_CONTEXTS[position];
  const levelLabel = ['', 'ระดับ 1 — พลังต่ำ', 'ระดับ 2 — พลังปานกลาง', 'ระดับ 3 — พลังสูง', 'ระดับ 4 — พลังสูงสุด'][level];

  let dynamicPart = '';

  if (position === 0) {
    // Primary card reasoning
    if (aCount >= 8) {
      dynamicPart = `จากคำตอบของคุณ คุณเลือก A ถึง ${aCount} จาก 10 คำถาม (${aPct}%) — นี่คือ<strong>แนวโน้มที่ชัดเจนมาก</strong> ไพ่ ${deck[cardIndex].name} จึงเป็นตัวแทนของพลังหลักที่ขับเคลื่อนคุณในช่วงนี้`;
    } else if (aCount >= 5) {
      dynamicPart = `คุณเลือก A ${aCount} ครั้งจาก 10 คำถาม (${aPct}%) แสดงถึง<strong>ความสมดุลระหว่างความกล้าและความระมัดระวัง</strong> ไพ่ ${deck[cardIndex].name} จึงสะท้อนแกนกลางของคุณได้ดี`;
    } else {
      dynamicPart = `คุณเลือก A เพียง ${aCount} ครั้ง (${aPct}%) แสดงถึง<strong>แนวโน้มที่ระมัดระวังแต่มีจุดยืน</strong> ไพ่ ${deck[cardIndex].name} จึงตอบสนองพลังภายในของคุณได้อย่างเหมาะสม`;
    }
  } else if (position === 1) {
    // Challenge card
    if (isClustered) {
      dynamicPart = `รูปแบบคำตอบของคุณมี<strong>การรวมกลุ่มชัดเจน</strong> (A ติดกัน ${maxAStreak} ครั้ง หรือ B ติดกัน ${maxBStreak} ครั้ง) ซึ่งบ่งบอกถึง<strong>ช่วงเวลาของการเปลี่ยนผ่าน</strong> ไพ่ ${deck[cardIndex].name} จึงปรากฏเป็นความท้าทายที่ต้องตระหนัก`;
    } else if (isAlternating) {
      dynamicPart = `คุณสลับไปมาระหว่าง A และ B อย่างสม่ำเสมอ — นี่คือ<strong>ความสมดุลที่มีชีวิตชีวา</strong> ไพ่ ${deck[cardIndex].name} จึงเป็นความท้าทายในการรักษาจังหวะนี้`;
    } else {
      dynamicPart = `จากการวิเคราะห์รูปแบบ คุณมีแนวโน้มที่<strong>หลากหลายแต่มีทิศทาง</strong> ไพ่ ${deck[cardIndex].name} จึงปรากฏเป็นสิ่งที่ควรให้ความสนใจ`;
    }
  } else if (position === 2) {
    // Foundation
    if (maxAStreak > maxBStreak) {
      dynamicPart = `คุณมีการเลือก A ติดต่อกันสูงสุด ${maxAStreak} ครั้ง แสดงถึง<strong>ความมุ่งมั่นและพลังในการลงมือทำ</strong> ไพ่ ${deck[cardIndex].name} จึงเป็นรากฐานที่คอยหล่อเลี้ยงคุณ`;
    } else {
      dynamicPart = `คุณมีการเลือก B ติดต่อกันสูงสุด ${maxBStreak} ครั้ง แสดงถึง<strong>การไตร่ตรองและความระมัดระวัง</strong> ไพ่ ${deck[cardIndex].name} จึงเป็นรากฐานที่คอยประคองคุณ`;
    }
  } else if (position === 3) {
    // Support
    dynamicPart = `ไพ่นี้ทำหน้าที่เป็น<strong>พลังสนับสนุน</strong>ในการอ่านดวงของคุณ ในหมวด${analysis.category || 'นี้'} ไพ่ ${deck[cardIndex].name} จึงคอยเสริมแรงให้คุณในเส้นทางที่เลือก`;
  } else {
    // Outcome
    if (level >= 3) {
      dynamicPart = `ด้วยระดับ ${level} (score ${score}/10) คุณมี<strong>พลังและแรงผลักดันสูง</strong> ไพ่ ${deck[cardIndex].name} จึงเป็นผลลัพธ์ที่คาดหวังได้ — คุณกำลังมุ่งสู่ความสำเร็จและการเติบโต`;
    } else {
      dynamicPart = `ด้วยระดับ ${level} (score ${score}/10) คุณอยู่ใน<strong>ช่วงของการสะสมและเตรียมพร้อม</strong> ไพ่ ${deck[cardIndex].name} จึงเป็นผลลัพธ์ที่บ่งบอก<strong>เส้นทางแห่งการค่อยๆ เติบโตอย่างมั่นคง</strong>`;
    }
  }

  return `<div class="card-reasoning-header">${posLabel} — ${deck[cardIndex].name}</div>
<div class="card-reasoning-context">${posContext}</div>
<div class="card-reasoning-body">${dynamicPart} <strong>${levelLabel}</strong> · ${aCount} A · ${bCount} B</div>`;
}
```

**Files**: `apps/tarot/app.js`

---

## Phase 5 — UI: Summary Screen Overhaul

Update `showSummary()` to render 5 cards + reasoning + score/level.

### HTML changes (`index.html`)

Replace the summary screen body with:

```html
<div id="screen-summary" class="screen hidden">
  <div class="screen-inner summary-inner">
    <h2 class="summary-title">การอ่านดวงชะตาของคุณ</h2>
    <p class="summary-category-label" id="summary-category-label"></p>

    <!-- Score + Level -->
    <div class="score-level-row" id="score-level-row">
      <div class="score-display">
        <div class="score-number" id="score-number">0</div>
        <div class="score-label">คะแนน</div>
      </div>
      <div class="level-display">
        <div class="level-number" id="level-number">1</div>
        <div class="level-label">ระดับ</div>
      </div>
    </div>

    <!-- 5-Card Spread -->
    <div class="card-spread" id="card-spread"></div>

    <!-- Pattern + Reasoning -->
    <div class="pattern-strip" id="pattern-strip"></div>
    <div class="reasoning-list" id="reasoning-list"></div>

    <button class="btn-glow" id="restart-btn">เริ่มใหม่</button>
  </div>
</div>
```

### CSS additions (`styles.css`)

```css
/* Score & Level */
.score-level-row {
  display: flex;
  gap: 2rem;
  justify-content: center;
  align-items: center;
}

.score-display, .level-display {
  text-align: center;
}

.score-number, .level-number {
  font-size: 2.5rem;
  color: var(--gold);
  text-shadow: 0 0 16px var(--gold-glow);
  line-height: 1;
}

.score-label, .level-label {
  font-size: 0.75rem;
  color: var(--text-dim);
  letter-spacing: 0.1em;
  text-transform: uppercase;
  margin-top: 0.2rem;
}

/* 5-Card Spread */
.card-spread {
  display: flex;
  gap: 0.6rem;
  justify-content: center;
  flex-wrap: wrap;
  width: 100%;
}

.spread-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.4rem;
  animation: fadeUp 0.5s ease both;
}

.spread-card-img {
  width: 90px;
  height: auto;
  border-radius: 8px;
  border: 1.5px solid var(--gold);
  box-shadow: 0 0 12px rgba(245,200,66,0.2);
  transition: transform 0.3s ease;
}

.spread-card-img:hover { transform: translateY(-4px); }

.spread-card-label {
  font-size: 0.65rem;
  color: var(--text-dim);
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

/* Pattern strip */
.pattern-strip {
  font-size: 0.85rem;
  color: var(--text-muted);
  letter-spacing: 0.15em;
  background: rgba(139,92,246,0.08);
  border: 1px solid rgba(139,92,246,0.2);
  border-radius: 8px;
  padding: 0.5rem 1.2rem;
  font-family: monospace;
  letter-spacing: 0.3em;
}

/* Reasoning list */
.reasoning-list {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  width: 100%;
}

.card-reasoning {
  background: rgba(139,92,246,0.06);
  border: 1px solid rgba(139,92,246,0.18);
  border-radius: var(--radius-sm);
  padding: 1.2rem;
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
}

.card-reasoning-header {
  font-size: 0.9rem;
  color: var(--gold);
  font-weight: 600;
  letter-spacing: 0.03em;
}

.card-reasoning-context {
  font-size: 0.75rem;
  color: var(--purple-glow);
  letter-spacing: 0.05em;
}

.card-reasoning-body {
  font-size: 0.88rem;
  color: var(--text-muted);
  line-height: 1.7;
}

@media (max-width: 480px) {
  .card-spread { gap: 0.4rem; }
  .spread-card-img { width: 60px; }
  .score-number, .level-number { font-size: 2rem; }
}
```

### JS update (`app.js`)

Replace `showSummary()`:

```js
function showSummary() {
  const category = currentCategory;
  const score = computeScore(drawnCards);
  const level = computeLevel(score);
  const pattern = drawnCards.map(c => c.choiceLabel).join('');
  const analysis = analyzePattern(drawnCards);
  const spread = select5CardSpread(drawnCards, category, level);

  $('summary-category-label').textContent = `หมวด: ${category}`;

  // Score + Level
  $('score-number').textContent = score;
  $('level-number').textContent = level;

  // 5-Card Spread
  const spreadEl = $('card-spread');
  spreadEl.innerHTML = '';
  const posLabels = ['แกนหลัก', 'ความท้าทาย', 'รากฐาน', 'สนับสนุน', 'ผลลัพธ์'];
  spread.forEach((cardIdx, i) => {
    const padded = String(cardIdx).padStart(2, '0');
    const div = document.createElement('div');
    div.className = 'spread-card';
    div.style.animationDelay = `${i * 0.1}s`;
    div.innerHTML = `
      <img class="spread-card-img" src="assets/cards/card-${padded}.png" alt="${deck[cardIdx].name}" />
      <span class="spread-card-label">${posLabels[i]}</span>
    `;
    spreadEl.appendChild(div);
  });

  // Pattern strip
  $('pattern-strip').textContent = pattern;

  // Reasoning list
  const reasoningEl = $('reasoning-list');
  reasoningEl.innerHTML = '';
  spread.forEach((cardIdx, i) => {
    const reasoning = generateCardReasoning(cardIdx, i, analysis, level, score);
    const div = document.createElement('div');
    div.className = 'card-reasoning';
    div.style.animationDelay = `${0.3 + i * 0.12}s`;
    div.innerHTML = reasoning;
    reasoningEl.appendChild(div);
  });

  showScreen('summary');
}
```

**Files**: `apps/tarot/index.html`, `apps/tarot/styles.css`, `apps/tarot/app.js`

---

## Phase 6 — Verification

1. Load `index.html` in browser
2. Select a category (e.g., การงาน)
3. Answer 10 questions (vary A/B choices)
4. Verify summary shows: score number, level number, 5 spread cards, pattern strip, 5 reasoning paragraphs
5. Verify level matches score table:
   - All A → score 10 → level 4
   - 0 A → score 0 → level 1
   - 5 A → score 5 → level 2
   - 7 A → score 7 → level 3
   - 9 A → score 9 → level 4
6. Verify each card reasoning references actual pattern data (aCount, streaks, etc.)
7. Verify restart button resets to category select
