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

### Key Levels 1.1.0 - `key_levels/` (1.0.0 on 2026-10-02, 1.1.0 on 2026-10-04)

Previous monthly/weekly/daily highs and lows. See `key_levels/README.md`.

Design decisions (from the interview):
- Lines start at the candle that set the price; untaken lines stop at their label, which sits Label Offset bars past the current bar (default 30 in 1.0.x; 10 from 1.1.0, user's chart setting).
- When Taken is a setting: Keep / Stop at take (Sessions-style mid-line label) / Remove.
- Equal prices are NOT merged: user asked for every level to keep its own line and label (merging was built, then removed).
- Overlapping labels: 1.0.x spread them above / level / below; user found labels hard to tie to their ray on a daily chart, so 1.1.0 spaces them evenly with leader lines (user asked to be able to back out if it displayed badly; it didn't). Collision threshold is automatic: % of the visible price range via chart.left/right_visible_bar_time (default 2%).
- 1.1.0 defaults copied from the user's chart: offset 10, prices on, M3/W1/D3, all black Solid 2, Small text.
- 1.11.0: Previous Day Close (single level, daily candle close) under Opens; defaults copied from the user's chart.
- 1.10.0: month labels include the year; opens/closes renamed 'Opening Price' / 'Closing Price'.
- 1.9.0: leaders take their line's style and width (user request).
- 1.8.0: latest week labelled 'Previous Weekly High/Low' (no date), older weeks dated 'MM/DD Previous Weekly High/Low' (user request; replaces 'MM/DD Prev Week').
- 1.7.0: weekly highs/lows optionally on the monthly chart (rebuilt on the last bar from weekly candles; lower-TF request needs lookahead_off to get the latest week, lookahead_on returns the first). Found with it: xloc.bar_time puts a time inside a bar on the NEXT bar, so line x1/take x2 are snapped to the containing chart bar.
- 1.6.0: Bold checkbox per text row (user: larger sizes don't always display well). Uses label text_formatting.
- 1.5.0: separate High/Low line and text settings per type (on/off, Show Last, When Taken stay shared; user agreed). Bug found while testing: settings held in `var Cfg` built on bar 0 meant changed colors never reached the drawings (styles/widths did). Cfg objects are now built every bar.
- 1.4.0: Monthly Close under Opens, mirroring Monthly Open; shows the last completed month (current month has no close). Uses TradingView's M candle close (settlement for CME futures), not the last intraday trade.
- 1.3.0: Opens section. Count = current + Previous (user chose; max 5 previous monthly / 3 weekly, default 0). Opens never stop at a take. Default Dashed to stand apart from solid highs/lows.
- 1.2.0: When Taken is per type (user wants e.g. daily stopped at the take while monthly/weekly keep running); default line width 1.
- Completed periods only. A type shows when its period >= chart timeframe (M chart: monthly only; W: monthly + weekly; D and below: all).
- Week label = Sunday-open date; daily label = trading date.

Gotcha hit while building: `x != na` is false in Pine, so a tracker initialised to na never updated and the history rebuild ran every bar (RE10110 timeout / loop too long).

 — `smt_divergence/` (2026-09-30)

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

### Opening Gaps 1.1.0 — `opening_gaps/` (2026-09-16)

NWOG, NDOG and RTH opening gaps drawn as lines with mid and quarter levels. See `opening_gaps/README.md` for settings and validation.

1.1.0: every label names its gap and carries the open's date. (1.0.0 released 2026-09-15.)

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

### Macros 1.2.0 — `macros/` (2026-08-28)

Boxes over the 24 hourly :50–:10 macro windows with an M.O. (macro open) line. See `macros/README.md`.

### Sessions 1.10.0 — `sessions/` (2026-10-01)

Asia, London, NY AM, Lunch and NY PM boxes with high/low level lines that stop, or disappear, once taken. See `sessions/README.md`.

### Chart Label 1.2 — `chart_label/` (2026-08-26)

Four-line table with ticker/timeframe, day/date, trading mode and free text, for screenshots. See `chart_label/README.md`.

## References / inspiration

- (paste TradingView script URLs here)
