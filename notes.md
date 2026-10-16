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
- 1.14.0: roll at session close (user chose this, conditional on crypto being unaffected; it is, since crypto periods only close when the next opens). Closed-period levels are built on the last bar for display only (not stored), so nothing duplicates when the next period opens.
- 1.13.2: label offset in bars (xloc.bar_index) past the current bar; time offsets landing in a market break collapsed to the next open (user saw the offset do nothing on a Saturday). Lines are drawn in two parts: bar_time up to the current bar, bar_index after it.
- 1.13.0: Weekly Open = current week only, label 'Weekly Open', optional on monthly chart; defaults from the user's chart (widths 1, weekly lows #801922). EURUSD 1m weekly open started before the weekend gap; fixed in 1.13.1 (gap times snap to the next bar).
- 1.12.0: Close Price setting (user chose 'offer both'). Settlement closes anchor to the last candle of the period that traded through the price (CME settlement != last trade, so the line floated). Last Trade uses the last candle's close.
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

### Opening Gaps 1.8.3 — `opening_gaps/` (1.0.0 on 2026-09-15, 1.8.3 on 2026-10-15)

NWOG, NDOG and RTH opening gaps drawn as lines with mid and quarter levels. See `opening_gaps/README.md` for settings and validation.

Design decisions:
- 1.8.3: the user's changed colors didn't show on the chart (styles and widths did).
  - Cause, measured: Pine gives every `input.color` that reaches a drawing a palette slot, and drawings carry only the slot number. The chart draws from its own copy of the palette (`properties().palettes`), and that copy kept old colors even while the inputs and TradingView's computed palette (`metaInfo().defaults.palettes`) held the new ones.
  - Fix: colors are rebuilt with `live(c) => color.new(c, color.t(c))`. That leaves the script with no palette, and each drawing carries its color, the same way Sessions already worked.
  - 1.8.2 (building GapCfg every bar instead of `var`) was tried first and did not fix it.
  - Key Levels uses palette slots too (its 1.5.0 note above blames `var Cfg`), so it likely has the same latent issue. Untested.
- 1.8.1: user saw vertical leaders during the 17:00 halt. Future positions were time-based, and times in the halt collapsed onto the 18:00 bar (Extend Right squeezed to 1 bar too). Now everything past the current bar is in bars (xloc.bar_index), and each level is drawn in two parts meeting at the current bar. Same fix as Key Levels 1.13.2.
- 1.8.0: a Label checkbox on every Mid Line and Quarters row (12 total) to show lines only. The user chose separate switches for Mid Line and Quarters, defaults labelled on Current and lines only on Previous, and to replace Label High/Low Only rather than keep both (keeping it would override the new switches and make them look broken).
- 1.7.0: separate Bold for the Mid Line (50%) and Quarters (25/75%) labels in every block (12 new switches, at the end of those rows). User's choices: the Text-row Bold now covers High/Low only, and the new switches default off, so High/Low stand out over the plain percent levels.
- 1.6.0: **Bold** checkbox on each block's Text row (6 total), default on, copied from Key Levels. The user found the text the right size but hard to read even in the darker grays.
- 1.5.1 (shipped in the 1.6.0 commit): Previous defaults are solid grays. Order chosen by the user: RTH darkest `#4A4D57`, then NDOG `#6A6D78`, then NWOG lightest `#8A8D97`. Text matches its line. Replaces the 50% faded type colors.
- 1.5.0: overlapping labels are spread with leaders, the same block method as Key Levels. Prompted by RTH High 10/15 overwriting NDOG Low 10/14. The user found Key Levels' spread messy but asked to try it, chose "share of the screen" spacing (labels shift on zoom) over a fixed distance, and kept a Spread on/off switch.
  - Spread alone was clean up to 5m but a wall of labels on 30m/1h with previous gaps on. Three fixes were tested on all 11 timeframes. Only labelling High/Low on previous gaps (blank 25/50/75% labels, lines kept) was clean everywhere; first a per-type Label High/Low Only setting, replaced in 1.8.0 by per-row Label switches.
  - The user made it one setting per Previous block (NWOG, NDOG, RTH), on its own row under Text, default on. Max Label Move and Skip Spread When Crowded failed to help but the user chose to keep them, off by default.
  - Rejected first: separate label columns per gap type. They need retuning with zoom, and the user wants nothing to adjust regularly.
- 1.4.0: Gaps older than the chart's history are backfilled from a `request.security` context: NWOG and NDOG from 60m, RTH from 15m. Those are the same trades the chart uses: the 60m bar that ends at the 17:00 halt closes on the 16:59 trade, the 18:00 bar opens on the 18:00 trade, and 15m bars close at 16:15 and open at 09:30. Backfill merges once, on the last historical bar, matched on open time. The current gap still comes from the chart's own bars, so it isn't late. Measured on 2026-10-15:
  - A live 1m chart loads ~25.8k bars, back only to Sun 09/20, which gave 3 of 6 NWOGs.
  - A 60m security context reached back to Jan 2026 and a 15m context to 08/31.
  - After the change, all 11 timeframes from 15s to 1h show 6/6/6, and all 18 gaps match the raw 1m bars.
- 1.3.0 (shipped in the 1.4.0 commit): each gap type has a Current block (newest gap) and a Previous block (up to 5 before it). Interview answers:
  - One shared style for all previous gaps.
  - Independent switches; each one turns off everything under it.
  - Current = 1 gap, Previous = 1–5 more.
  - Settings grouped by gap type.
  - One Date checkbox per block, 6 in total.
  - Previous defaults to off. It started as the type colors at 50% transparency; replaced by grays in 1.5.1.
- 1.2.0: RTH draws on every intraday timeframe. It used to be hidden on 2m/10m/30m/1h, and the user ran into that. Off-grid timeframes read RTH from the largest boundary-safe timeframe at or below the chart's own (1/3/5/15m). The user's idea: that source's bars have already closed, so there's no delay.
- 1.1.0: labels name the gap and carry the open's date (`NWOG 75% 10/11`). The user couldn't tell the gap types apart by color alone.
- Gaps are lines, not boxes. The settings layout copies the reference graphic (『 』 headers, ▸ rows); keep those characters.
- Percent levels run from the open toward the prior close (25% = a quarter filled). This was inferred from the reference screenshots, where 25% sat next to the open in both a gap up and a gap down.
- High/Low lines start at the bar that set each price; mid and quarters start at the open.
- Futures holidays (Globex halts at 13:00, no RTH session): the day's RTH gap is dropped at the reopen, and the next day measures from the last real 16:14 close. Early-close days run to 13:15 and still count.
- Zero-size gaps are skipped (e.g. Mon 2026-10-12: 16:59 close = 18:00 open = 30877.5).

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
