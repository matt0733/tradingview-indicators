# Indicator Notes

Scratchpad for indicator ideas, design decisions, and references. Claude reads this file when you start a session here, so anything you jot down becomes context for the next ask.

## Ideas

- Example: "VWAP with session high/low bands and labeled deviations"
- Example: "ATR-scaled trailing stop overlay"

## Conventions I want Claude to follow

- Default to `@version=6`
- Use `log.info()` liberally so I can read output via `pine_get_console`
- Always add inputs with sensible defaults and grouped tooltips
- Match my color preferences: (add here)

## Indicators in progress

_(none)_

## Released

### Opening Gaps 1.0.0 — `opening_gaps/` (2026-09-15)

NWOG, NDOG and RTH opening gaps drawn as lines with mid and quarter levels. See `opening_gaps/README.md` for settings and validation.

Design decisions:
- Gaps are lines, not boxes. Settings layout copies the reference graphic: per-gap checkbox + Show Last (1–10), then Line / Text / Mid Line / Quarters rows.
- Percent levels run from the open toward the prior close (25% = a quarter filled). Inferred from the reference screenshots, where 25% sat next to the open in both a gap up and a gap down.
- High/Low lines start at the bar that set each price; mid and quarters start at the open.
- RTH gap only on timeframes that divide 15 minutes; other bar sizes straddle 16:15.
- Futures holidays (Globex halts at 13:00, no RTH session): the day's RTH gap is dropped at the reopen and the next day measures from the last real 16:14 close. Early-close days run to 13:15 and still count.
- Zero-size gaps are skipped.

Possible follow-ups (not requested yet):
- Stop extending lines, or fade them, once a gap is filled.
- Stagger labels when all five levels of a small gap overlap.

## References / inspiration

- (paste TradingView script URLs here)
