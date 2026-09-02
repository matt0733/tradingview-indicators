# SMT Divergence

Marks Smart Money Technique divergence between the chart symbol and one or two
comparison markets. A bearish SMT is drawn when the chart takes a previous swing
high and the comparison market fails to take its matching high; a bullish SMT
when the chart takes a low and the comparison market holds.

Lines connect the two chart pivots. The label at the midpoint names the market
that disagreed.

## Read this first

Two settings **delete signals after the fact**, using information from after the
signal fired. Both are on by default because they are useful live, and both make
chart history look far more accurate than the indicator was in real time.

| Filter | Signals shown | Hit rate |
|---|---|---|
| No filter — what you actually saw live | 2,720 | 51.7% |
| Kept by **Hide Inside SMTs** | 1,279 | 63.3% |
| Deleted by Hide Inside | 1,441 | 41.5% |
| Survived **Remove on Invalidation** | 76 | 95.8% |
| Removed as invalidated | 2,644 | 50.4% |

Remove on Invalidation deletes, by definition, every SMT that price traded
through. Scrolling back through history with it on shows 76 of 2,720 signals at
an apparent 95.8% hit rate.

**Turn both off when judging the tool. Leave them on when trading it.**

## Settings

| Setting | Default | Notes |
|---|---|---|
| Symbol 1 / Symbol 2 | `CME_MINI:ES1!`, `CBOT_MINI:YM1!` | Both enabled |
| Inverse Corr. | off | For markets that move opposite the chart |
| Pivot A Strength | 3 | Bars each side required for the older pivot |
| Sync Tolerance | 1 | How far apart matching pivots may sit |
| Max SMT Span | 180 | Max bars between the two pivots |
| Crossing Tolerance | 3 | How far price may pierce the line, in 0.1 × ATR steps |
| Min Divergence (ATR) | 1.0 | Smallest divergence that counts |
| Max SMTs per side | 15 | Combined across both comparison symbols |
| Remove on Invalidation | on | See warning above |
| Hide Inside SMTs | on | See warning above |
| Label Style | Text | Text, Badge or Marker |

### Do not tune the detection parameters for accuracy

Ninety configurations of Pivot A Strength, Sync Tolerance, Max Span and Crossing
Tolerance were grid-searched over 22 days and split train/test. **Correlation
between train-half and test-half performance: −0.055.** Settings that look best
in one period tell you nothing about the next. Tuning these is fitting noise.

The defaults are chosen for signal legibility and data quality, not backtested
edge, with two exceptions noted below.

### Crossing Tolerance is measured in ATR

It was a percentage of price before v1.1.0, which meant a fixed point value and
wildly different strictness per timeframe. At the old default it worked out to
5.88 points on MNQ — **57% of an average 1-minute bar**, but only 7% of an hourly
bar. Each step is now 0.1 × ATR(14), so one setting is equally strict everywhere.

### Min Divergence rejects noise

Without it, any miss counts — including the comparison market missing its high by
a quarter point, which is measurement noise rather than a liquidity event.
Requiring the divergence to be at least 1× the comparison market's own ATR(14)
improved hit rate in **4 of 4 independent periods, mean +2.25 percentage points**,
at the cost of roughly half the signals. Live on a 2,800-bar window it cut 165
signals to 100.

### Sync Tolerance 0 is stricter than it looks

A less liquid comparison symbol has minutes with no trades. Those bars carry the
previous value forward, which shifts where its pivots land. At tolerance 0 those
shifted pivots are simply missed. YM benefits about 2.4× more than ES from
loosening it, which is exactly the staleness signature.

## Choosing comparison symbols

Micros are not worth switching to. MES tracks the same index as ES and arbitrage
holds them within a tick, so the divergence carries the same information —
**73% of signals are literally identical** between ES+YM and MES+MYM. Measured
edge differences between the pairings sit inside the noise.

Data quality is the only real differentiator, and it does not favour full-size
across the board:

| | Stale (forward-filled) bars on 1m |
|---|---|
| ES1! | 0.01% |
| MES1! | 0.00% |
| YM1! | 2.93% |
| MYM1! | **1.68%** |

If YM ever produces gappy pivots during thin overnight hours, MYM is the cleaner
Dow feed.

## Validation

- **Structural correctness** — 100 signals audited against raw MNQ/ES/YM bars:
  endpoints are real swing highs (bearish) or lows (bullish), both pivots
  confirmed, comparison market genuinely diverges, divergence clears the ATR
  threshold, span and crossing and cap limits respected. 100/100, zero failures.
- **No repainting** — a forced full recalculation reproduces identical line
  geometry and identical label text.
- **Algorithm fidelity** — an independent reconstruction from raw bars matched
  the live output 37/37 on 1-minute and 13/13 on hourly.
- **Inverse correlation** — feeding a perfectly inverted series with Inverse Corr.
  on produces a bit-identical signal set to the original with it off (54 = 54).

Against a control of *every* swing pivot with no divergence requirement, SMT
signals run roughly 2–5 percentage points better on hit rate, varying by period.

## Known limits

- Testing is MNQ against ES and YM, 1-minute, across roughly a month. Other
  instruments and timeframes are untested.
- Bar Replay is unverified — TradingView only materialises drawings near the
  viewport there, so signal counts in replay could not be trusted.
- Opposing-side signals can sit on the chart together. That is a range with
  failed breaks at both ends, not a contradiction, but nothing indicates which
  is more recent.
- Label collision avoidance only knows about this script's own labels. It cannot
  see other indicators' drawings.
