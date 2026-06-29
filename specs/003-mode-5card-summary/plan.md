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
  "ตัวตน": {
    icon: "🌟",
    outcomes: [19, 17, 14, 12, 2],  // score 0–2, 3–4, 5–6, 7–8, 9–10
    questions: [ /* 10 objects */ ]
  },
  "ครอบครัว": {
    icon: "🏠",
    outcomes: [7, 8, 6, 3, 12],  // score 0–2, 3–4, 5–6, 7–8, 9–10
    questions: [ /* 10 objects */ ]
  },
  "การงาน": {
    icon: "⚔️",
    outcomes: [7, 1, 6, 4, 9],  // score 0–2, 3–4, 5–6, 7–8, 9–10
    questions: [ /* 10 objects */ ]
  },
  "ความรัก": {
    icon: "💞",
    outcomes: [6, 8, 14, 12, 2],  // score 0–2, 3–4, 5–6, 7–8, 9–10
    questions: [ /* 10 objects */ ]
  },
  "การเงิน": {
    icon: "💰",
    outcomes: [19, 1, 10, 4, 11],  // score 0–2, 3–4, 5–6, 7–8, 9–10
    questions: [ /* 10 objects */ ]
  }
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

Each category has `outcomes` array indexed by outcome index (0–4):
- Index 0 = score 0–2 (A all / A-heavy — spectrum A side)
- Index 1 = score 3–4 (leaning A)
- Index 2 = score 5–6 (balanced)
- Index 3 = score 7–8 (leaning B)
- Index 4 = score 9–10 (B all / B-heavy — spectrum B side)

#### ตัวตน — Spectrum: A = กล้าเป็นตัวเอง → B = ซ่อนตัวเพื่อสังคม

| Score | ความหมาย | ไพ่แนะนำ | ข้อดี | ข้อเสีย |
|-------|----------|-----------|--------|---------|
| 0–2 | เปิดเผยมาก กล้าจนอาจ intense | The Sun (19) | กล้าแสดงตัวตน มีพลังงานสดใส คนรอบข้างรู้สึกได้ถึงความจริงใจ | อาจ overwhelm คนอื่น บางทีเปิดเผยเกินจนไม่มีพื้นที่ส่วนตัว |
| 3–4 | เริ่มเป็นตัวเอง แต่ยังลังเล | The Star (17) | มีความหวังและทิศทาง กำลังค้นพบตัวเองอย่างสวยงาม | ยังไม่มั่นคงพอ อาจถูกสั่นคลอนได้ง่ายจากความคิดเห็นคนอื่น |
| 5–6 | สมดุลระหว่างตัวเองและสังคม | Temperance (14) | ปรับตัวได้ดี รู้จักผสมผสานตัวตนกับบริบทรอบข้าง | อาจไม่ชัดเจนว่าตัวเองต้องการอะไรจริงๆ ในบางสถานการณ์ |
| 7–8 | เริ่มซ่อนตัว ปรับตามคนอื่น | The Hanged Cat (12) | อ่านห้องเก่ง ไม่สร้างความขัดแย้ง คนรอบข้างสบายใจ | เริ่มสูญเสียเสียงของตัวเอง ทำตามคนอื่นจนลืมว่าตัวเองอยากได้อะไร |
| 9–10 | ซ่อนตัวมาก lost ไม่รู้ตัวเองคือใคร | The High Priestess (2) | เก็บความลึกไว้ในตัว มีโลกภายในที่ซับซ้อน | ไม่มีใครเห็นตัวตนจริงๆ รวมถึงตัวเองด้วย อาจรู้สึกโดดเดี่ยวโดยไม่รู้สาเหตุ |

**outcomes: [19, 17, 14, 12, 2]**

#### ครอบครัว — Spectrum: A = กล้าแสดงความต้องการตัวเอง → B = เสียสละจน lose self

| Score | ความหมาย | ไพ่แนะนำ | ข้อดี | ข้อเสีย |
|-------|----------|-----------|--------|---------|
| 0–2 | กล้า clash ตั้งขอบเขต | The Chariot (7) | รู้จักปกป้องตัวเอง มีขอบเขตชัดเจน ไม่ให้ใครมาละเมิดได้ง่าย | อาจดูแข็งกระด้างในสายตาครอบครัว ความสัมพันธ์อาจตึงเครียด |
| 3–4 | เริ่มสมดุล แต่ยังตามคนอื่นบ้าง | Strength (8) | มีพลังในการดูแลและรับมือกับความซับซ้อนของครอบครัวด้วยความอ่อนโยน | บางครั้งยังยอมเกินไปเพราะไม่อยากเป็นคนสร้างปัญหา |
| 5–6 | กลางๆ ระหว่างตัวเองและครอบครัว | The Lovers (6) | เข้าใจว่าต้องเลือก รับรู้ทั้งสองฝั่ง ยังไม่สุดโต่งทางใดทางหนึ่ง | อยู่ในสภาวะลังเลนาน อาจทำให้ตัดสินใจอะไรไม่ได้สักที |
| 7–8 | เริ่มเสียสละตัวเองมากขึ้น | The Empress (3) | ดูแลคนในบ้านได้ดี ครอบครัวรู้สึกอบอุ่นและพึ่งพาได้ | ให้จนหมดตัวโดยไม่รู้ตัว ความต้องการตัวเองถูกเลื่อนออกไปเรื่อยๆ |
| 9–10 | เสียสละจน lose self โดยไม่รู้ตัว | The Hanged Cat (12) | เป็นที่พึ่งของทุกคน ครอบครัวรู้สึกมั่นคงเพราะมีเรา | ไม่รู้แล้วว่าตัวเองต้องการอะไร ชีวิตถูกนิยามโดยหน้าที่ ไม่ใช่ความต้องการ |

**outcomes: [7, 8, 6, 3, 12]**

#### การงาน — Spectrum: A = กล้าเสี่ยง/ตามความฝัน → B = มั่นคง/อยู่ในกรอบ

| Score | ความหมาย | ไพ่แนะนำ | ข้อดี | ข้อเสีย |
|-------|----------|-----------|--------|---------|
| 0–2 | กล้ามาก อาจ restless ไม่หยุดนิ่ง | The Chariot (7) | พลังงานสูง กล้าเปลี่ยน ไม่กลัวความไม่แน่นอน สร้างโอกาสได้เก่ง | อาจเปลี่ยนเร็วเกินไป ไม่อดทนรอผล ทิ้งสิ่งดีๆ ก่อนมันจะออกดอก |
| 3–4 | กล้าพอประมาณ มีทิศทางชัด | The Magician (1) | รู้จักใช้ทักษะและโอกาส มีความมั่นใจในการสร้างสิ่งใหม่ | อาจมั่นใจเกินจริงบางครั้ง ประเมินความยากต่ำกว่าความเป็นจริง |
| 5–6 | สมดุลระหว่างความฝันและความมั่นคง | The Lovers (6) | รับรู้ทั้งสองเส้นทาง ยังมีตัวเลือก ไม่ได้ปิดประตูทางใดทางหนึ่ง | อยู่ในสภาวะเลือกไม่ได้นาน อาจพลาดโอกาสเพราะรอให้ชัวร์เกินไป |
| 7–8 | เริ่มระวังตัว ชอบความมั่นคง | The Emperor (4) | วางแผนดี มีระเบียบ สร้างความมั่นคงได้จริง คนรอบข้างไว้วางใจได้ | อาจพลาดโอกาสดีๆ เพราะกลัวความเสี่ยง หรือติดอยู่กับแผนเดิมนานเกินไป |
| 9–10 | ระวังมาก stuck อยู่กับที่ | The Hermit (9) | ไตร่ตรองละเอียด ไม่พลาดพลั้ง รู้จักตัวเองดีในแบบที่เงียบๆ | อาจโดดเดี่ยวเกินไป ไม่กล้าออกจาก comfort zone จนชีวิตหยุดนิ่ง |

**outcomes: [7, 1, 6, 4, 9]**

#### ความรัก — Spectrum: A = เปิดใจ/กล้าให้ → B = ปกป้องตัวเอง/รักษาระยะ

| Score | ความหมาย | ไพ่แนะนำ | ข้อดี | ข้อเสีย |
|-------|----------|-----------|--------|---------|
| 0–2 | เปิดใจมาก กล้าให้จน over-give | The Lovers (6) | รักเต็มที่ จริงใจ ทำให้คนอีกฝ่ายรู้สึกถึงความรักได้ชัดเจน | อาจให้จนตัวเองหมด หรือรักเร็วเกินไปจนเจ็บโดยไม่ทันตั้งตัว |
| 3–4 | เปิดใจแต่ยังมีสติ | Strength (8) | รักด้วยความกล้าและอ่อนโยน เปิดใจได้โดยไม่ทิ้งตัวเอง | บางครั้งยังลังเลว่าจะให้มากกว่านี้ได้ไหม อาจดูระมัดระวังเกินในสายตาคู่รัก |
| 5–6 | สมดุล ให้และรับได้พอๆ กัน | Temperance (14) | ความสัมพันธ์มีความสมดุล รู้จักทั้งการให้และการรับ | อาจดูเย็นชาหรือคำนวณเกินไปในช่วงที่ความรักต้องการความบ้าบิ่นบ้าง |
| 7–8 | เริ่มระวังตัว ตั้งการ์ด | The Hanged Cat (12) | ไม่เจ็บง่าย รู้จักสังเกตและรอให้มั่นใจก่อน | อาจทำให้คนที่รักรู้สึกว่าเข้าถึงไม่ได้ ความสัมพันธ์เดินหน้าช้า |
| 9–10 | ปิดตัวมาก กำแพงหนา | The High Priestess (2) | ปกป้องตัวเองได้ดีมาก ไม่ถูกหลอกง่าย มีพื้นที่ส่วนตัวที่แข็งแรง | แทบไม่ยอมให้ใครเข้ามาใกล้จริงๆ ความเหงาสะสมอยู่ข้างในโดยไม่รู้ตัว |

**outcomes: [6, 8, 14, 12, 2]**

#### การเงิน — Spectrum: A = ใช้สุรุ่ยสุร่าย ไม่คิด → B = อดออม ระวัดระวัง

| Score | ความหมาย | ไพ่แนะนำ | ข้อดี | ข้อเสีย |
|-------|----------|-----------|--------|---------|
| 0–2 | ใช้เงินตามอารมณ์ ไม่วางแผน หมดก็หมด | The Fool (0) | ใช้ชีวิตเต็มที่ในปัจจุบัน ไม่แบกความกังวลเรื่องเงิน มีความสุขง่าย | ไม่มีเงินสำรอง เสี่ยงกับอนาคต วิกฤตเล็กๆ อาจกลายเป็นปัญหาใหญ่ |
| 3–4 | ชอบใช้จ่าย แต่พอมีทิศทางบ้าง | The Sun (19) | เชื่อว่าเงินมีไว้ใช้ สร้างประสบการณ์ดีๆ ให้ตัวเอง คนรอบข้างรู้สึกว่าอยู่กับแกสนุก | ออมได้น้อย บางเดือนติดลบ พึ่งพาโชคมากกว่าแผน |
| 5–6 | สมดุล รู้จักใช้และเก็บพอกัน | The Wheel (10) | เข้าใจจังหวะของเงิน รู้ว่าเมื่อไหรควรใช้และเมื่อไหรควรเก็บ ไม่ตึงเกินไปทั้งสองด้าน | บางครั้งตัดสินใจช้าเพราะชั่งน้ำหนักนาน อาจพลาดทั้งโอกาสใช้และโอกาสออม |
| 7–8 | อดออมเก่ง มีวินัย | The Emperor (4) | มีวินัยทางการเงินสูง วางแผนละเอียด มีเงินสำรองเสมอ ไม่เป็นหนี้ | แข็งเกินไปจนพลาดความสุขเล็กๆ น้อยๆ คนรอบข้างอาจรู้สึกว่าตึงหรือขี้งก |
| 9–10 | ออมสุดขีด ใช้เงินกับตัวเองแทบไม่ได้ | The Hermit (9) | ทุกบาทมีเหตุผล ปลอดภัยทางการเงินสูงมาก ไม่มีทางเดือดร้อนเรื่องเงิน | กลัวขาดจนไม่กล้าใช้แม้แต่กับตัวเอง ชีวิตตึงและรู้สึกขาดแคลนทั้งที่มีเงิน |

**outcomes: [0, 19, 10, 4, 9]**

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
4. Click ตัวตน → badge shows "ตัวตน" ✓
5. Progress bar shows Q1/10 ✓
6. Answer all 10 questions (A-all → score 0 → The Sun (19) for ตัวตน)
7. Restart → pick การเงิน → B-all → score 10 → Justice (11) ✓
8. Restart → pick ความรัก → score 5–6 → Temperance (14) ✓
9. Narrative shows ข้อดี/ข้อเสีย from the new table ✓
10. Restart → back to category select ✓
11. Mobile: test at 375px width, no overflow ✓
