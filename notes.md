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

### SMT Divergence 2.0.0 — `smt_divergence/` (2026-09-30)

Rewrite of the LLM-written v1. See `smt_divergence/README.md` for measured results and settings.

Design decisions:
- Standard SMT definition: compare only the last two swings in each market. v1 searched every older swing in the span, firing ~38×/day at a near coin-flip hit rate out of sample.
- Swing Length 5, Sync 2, Span 30, Min Divergence 1.0 × comparison ATR. Swing Length picked from 3/5/8 on Sept 10–30 data; all four length-5 variants were positive every week.
- Evaluated on closed bars only, so signals can't appear and vanish intrabar.
- Invalidated SMTs fade by default rather than being deleted, so history shows the misses.
- One line per swing pair; label reads `ES1!/YM1!` when both comparison markets diverge.
- Session filter optional, default All. NY AM looked best on v1 but has too few v2 signals to judge.

Possible follow-ups (not requested yet):
- `log.info()` output per the conventions above (not in 2.0.0).
- Only check invalidation for N bars after the signal, so "faded" means it failed soon rather than eventually traded through.
- Re-measure on data after 2026-09-30 before trusting the edge or turning on the NY AM filter.

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
