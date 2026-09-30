# SMT Divergence

Marks Smart Money Technique divergence between the chart and one or two
comparison markets, using the standard definition: compare the **last two swing
highs** (or lows) in each market. If one market takes its previous swing and the
other doesn't, that's an SMT.

- Bearish SMT: a line joining the chart's last two swing highs.
- Bullish SMT: a line joining the chart's last two swing lows.
- The label at the midpoint names the market that disagreed. When both comparison
  markets disagree on the same swings it reads `ES1!/YM1!`.

## What to expect

SMT is a small edge, not a prediction. Measured on 1-minute MNQ against ES and YM,
entering the bar after a signal appears, with a 2 × ATR target and stop over 30 bars:

| | Signals/day | Hit rate | Same swings, no SMT condition |
|---|---|---|---|
| v2.0.0, Sept 10–30 2026 | ~7 | 58.5% | 51.2% |
| v2.0.0 on the live MNQZ2026 chart, Sept 7–30 | ~7 | 55.8% | 50.7% |
| v1.2.2 (old design), Sept 10–30 2026 | ~38 | 53.6% | 49.8% |

The v2 edge was positive in each of the three weeks (+5, +10, +5 points). Two
cautions: Swing Length 5 was chosen from a small grid on that same data, and three
weeks is a short sample. Forward results are the real test.

## Settings

| Setting | Default | Notes |
|---|---|---|
| Symbol 1 / Symbol 2 | `CME_MINI:ES1!`, `CBOT_MINI:YM1!` | Both enabled |
| Inverse | off | Compares chart highs with comparison lows, for markets that move opposite |
| Swing Length | 5 | Bars required each side of a swing. 5 tested clearly better than 3 or 8 |
| Sync Tolerance | 2 | How many bars apart matching swings may be |
| Max SMT Span | 30 | Max bars between the two swings compared |
| Min Divergence (ATR) | 1.0 | Comparison market's swings must differ by at least 1 × its ATR(14) |
| Show SMTs during | All | Optional session filter: NY AM, London, both, or custom (New York time) |
| Max SMTs shown | 10 | Oldest removed first |
| When invalidated | Fade | Fade, Delete or Keep |

Alerts: **Bearish SMT**, **Bullish SMT**, or *Any alert() function call* for a
message naming the chart and comparison symbol.

### Faded does not mean the trade failed

An SMT is invalidated once price closes beyond its extreme swing. Over days, price
trades through almost every swing eventually. On the validation window 114 of 119
SMTs ended up faded, including ones that moved well in their favour first. Fade is
there so history shows the misses honestly instead of deleting them. "Delete"
removes them and makes history look far more accurate than it was live.

### Signals print on closed bars only

Signals are only evaluated when a bar closes, so a line can't appear intrabar and
then vanish. A full recalculation over the same bars reproduces every signal
exactly. It has not yet been checked by recording signals during a live session
and comparing them after a reload.

## Why v2 was a rewrite

v1 searched every older swing within the span for *any* divergence, which fired
about 38 times a day. Three weeks of live data it was never tuned on (Sept 10–30)
showed a hit rate of 53.6% against 49.8% for ordinary swing pivots. That's close to
a coin flip, and the improvements measured in-sample in v1.2.x mostly didn't hold.
The detection was correct. The design produced too many weak signals.

## What was tested and didn't help

Written down before running, each scored against a control with the same filter,
on v1 signals Sept 10–30:

| Idea | Result |
|---|---|
| Only when the swing sweeps a 60-bar extreme | **Worse**: −1.8 points |
| London session only (2–5 ET) | No effect: +1.1 |
| Enter only after a structure break (MSS) | Absolute hit rate unchanged; the later entry costs what the filter gains |
| NY AM only (9:30–11:00 ET) | 60% on 46 signals, positive every week. Promising, too few to confirm. In v2 there are too few NY AM signals to judge yet |

## Choosing comparison symbols

Micros aren't worth switching to. MES tracks the same index as ES, and 73% of
signals were identical between ES+YM and MES+MYM, with edge differences inside
the noise. If YM gets gappy overnight, MYM is the cleaner Dow feed (1.68% stale
1-minute bars against YM's 2.93%).

YM will always disagree with MNQ more than ES does. Rolling correlation of
1-minute returns: MNQ↔ES median 0.884, MNQ↔YM 0.515. The Dow is 30 price-weighted
industrials and the Nasdaq-100 is tech-heavy, so some YM divergences are sector
rotation, not liquidity. Turn Symbol 2 off if YM is too noisy.

## Validation

- **Matches an independent reconstruction**: all 121 signals on the live chart
  (23,205 bars, Sept 7–30) matched a separately written model bar for bar, with
  none missing on either side, and 121/121 labels matched, including merged ones.
- **Fade logic**: all 114 invalidated SMTs drawn faded, all 5 intact ones solid.
- **Compiles clean** on TradingView's compiler: 0 errors, 0 warnings.
- **Full history**: runs without errors on 23,000+ bars.

## Known limits

- Tested on MNQ against ES and YM, 1-minute, three weeks. Other instruments and
  timeframes are untested.
- Comparison markets need real-time data. Without a subscription TradingView
  delays them, and live SMTs against a delayed market will be late or missing.
- Label placement only avoids this script's own labels; it can't see other
  indicators' drawings.
