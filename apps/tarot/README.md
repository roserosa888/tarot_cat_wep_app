# Thai Tarot Reading App — Cat Edition

## How to Run

```bash
cd apps/tarot
npx serve . -p 3333
# Open http://localhost:3333
```
<!-- หากอยากเปลี่ยนเลข port  : npx serve . -p 8888 -->
## Project Structure

```
apps/tarot/
  index.html       # Main HTML — 4 screens (select, question, reveal, summary)
  styles.css       # Mystical dark purple/gold UI + star animations
  app.js           # All game logic: 50 questions, 5 categories, card data
  assets/cards/    # 22 cat tarot PNG images (card-00.png to card-21.png)
```

## App Flow

1. **Select** — Pick one of 5 life categories
2. **Question** (×10) — Each fork presents a dilemma, pick A or B
3. **Reveal** (×10) — Animated card spin → flip → show card + meaning
4. **Summary** — All 10 cards + synthesized narrative + restart

## Categories & Questions

| Category | Topics Covered |
|---|---|
| การงาน | staying/leaving, passion/stability, autonomy, growth, effort/result... |
| ความรัก | express/hold back, fight/let go, give/receive, trust, closeness... |
| การเงิน | spend/save, invest/hold, risk/safety, need/want, control/flow... |
| ตัวตน | authentic/accepted, change/stay, speak/silence, solitude/connection... |
| ครอบครัว | family/personal dream, give/receive care, duty/desire, truth/peace... |

## Tarot Deck

22 cards reused from the original cat tarot project, each with Thai meaning.

## Notes

- Port `3333` is set via `serve -p 3333` (CLI flag, not in code)
- No build step required — pure HTML/CSS/JS
- Disclaimer is always shown at the top: ไพ่ทาโร่นี้มีไว้เพื่อสำรวจตัวเองเท่านั้น...
