# Key Levels

Marks the highs and lows of previous months, weeks and days as horizontal lines,
each labelled with the period it came from. Labels sit past the current bar so
they can be lined up after Opening Gaps' labels.

## Levels

| Type | Label | Period |
|---|---|---|
| Monthly | `September Monthly High` | Calendar month |
| Weekly | `09/20 Prev Week High` | Week, dated by its open (Sunday 18:00 New York for CME futures) |
| Daily | `Thu 10/01 Daily High` | Trading day; the session opening Wednesday 18:00 is Thursday's |

Each type has its own on/off and **Show Last** count (1–12). Only completed
periods are drawn, so Show Last 1 is the last full month, week or day.

Periods follow TradingView's own bars for the symbol, which is why the weekly date
is the Sunday open and the monthly name comes from the trading days in the month
(a monthly bar opening Aug 31 18:00 is still September).

## How the levels are drawn

- Each line starts at the candle that set the price. If that candle is older than
  the chart's loaded history (for example a monthly high on a 1m chart), the line
  starts at the period's open instead, which is off the left edge of the chart.
- The price always comes from the monthly, weekly or daily candle. TradingView's
  intraday bars can differ from it by a tick (seen on FX:EURUSD: daily low
  1.12149, lowest 1h bar 1.12150). When the whole period is loaded, the line
  still starts at the chart's own high or low candle.
- Untaken lines run to their label, **Label Offset** bars past the current bar.
- A level is **taken** when a wick touches it. **When Taken** sets what happens:

| When Taken | Result |
|---|---|
| Keep (default) | Nothing changes; the line runs on to its label |
| Stop at take | The line ends at the candle that took it, and its label moves to the middle of the line, above a high and below a low |
| Remove | The line and label are deleted |

- **Close or shared prices**: every level keeps its own line and label. Labels
  closer together than **Label Spacing** (a share of the visible price range) are
  spread out: two sit above and below their lines, three sit above, level with
  and below. At equal prices the longer period goes on top. The visible range is
  re-measured whenever you zoom or scroll, so the spacing holds on any timeframe.
  A fourth label in the same cluster stays level and can still overlap.

## Timeframes

A level is drawn when its period is at least as long as the chart's bars.

| Chart | Monthly | Weekly | Daily |
|---|---|---|---|
| Intraday | yes | yes | yes |
| Daily | yes | yes | yes |
| Weekly | yes | yes | hidden |
| Monthly | yes | hidden | hidden |

## Lining up with Opening Gaps

Opening Gaps puts its labels **Extend Right** bars past the current bar. Set
Label Offset to that plus about 15 so Key Levels' labels start after Gaps' text.
With Extend Right at 10, the default offset of 30 clears it at normal zoom on 1m.

Both offsets are counted in bars, not pixels, so the two sets of labels stay
together as you scroll. Text has a fixed pixel width though, so zooming far out
squeezes the gap and the labels can overlap; zooming in widens it.

## Settings

| Setting | Default | Notes |
|---|---|---|
| Label Offset (bars) | 30 | 0–500 bars past the current bar |
| When Taken | Keep | Keep, Stop at take, Remove |
| Show Price in Label | off | Appends the price, e.g. `July Monthly High 30861.25` |
| Label Spacing (% of view) | 2 | Labels closer than this share of the visible price range are spread out; 0 = only identical prices |
| *Type* on/off, Show Last | on, 1 | One per type; Show Last 1–12 |
| Line | Monthly `#F23645` Solid 2 · Weekly `#4CAF50` Dashed 2 · Daily `#00BCD4` Dotted 1 | Color, style, width |
| Text | Same colors as the lines, Small | Color, size (Tiny–Large) |

Inputs are hidden from the chart status line.

## Validation

On MNQZ2026 (CME_MINI), 2026-10-02:

- **Values**: every monthly, weekly and daily high and low matched TradingView's
  own M, W and D bars, with Show Last 1 and 12 (72 levels).
- **Names**: September for a monthly bar opening Aug 31 18:00; `09/20 Prev Week`
  for the week opening Sunday 9/20 18:00; `Thu 10/01 Daily` for the session
  opening Wednesday 18:00.
- **Anchors**: lines started on the exact daily and 1m bars that set each price.
- **Takes**: with Stop at take, take times matched the first 1m bar after the
  period that touched each level; Remove left only the untaken levels.
- **Timeframes**: checked on 1m, 1h, D, W and M against the table above.
- **Other markets** (1.0.1, 2026-10-02): MESZ2026, MYMZ2026, NASDAQ:AAPL,
  COINBASE:BTCUSD and FX:EURUSD. Every level matched that symbol's own M, W and D
  candles, and with history loaded back to June on 1h, every line started on the
  candle that set its price (22 lines each). Stocks and crypto weeks start Monday;
  crypto days run midnight to midnight UTC and include weekends.

## Known limits

- On a weekly chart the current bar can open in the previous month (e.g. Sunday
  9/27 while it is now October). That month is added as completed once the
  calendar moves on, but it can't tell whether price took it within the current
  weekly bar, so it shows as untaken until the next bar.
- Takes of older levels that happened before the chart's loaded history are
  placed at the open of the period that took them, not the exact candle. These
  are off the left edge of the chart.
- Holiday weeks use TradingView's weekly bar open: the July 4th 2026 week opened
  Thursday 7/2 18:00, so it is labelled `07/02 Prev Week`.
- Label Spacing only sees Key Levels' own labels. It can't move them away from
  Opening Gaps' text at a nearby price; Label Offset is the fix for that.
- Using the visible range makes the script recalculate on every zoom and scroll.
- Tested on MNQ only.

## Changes

| Version | Change |
|---|---|
| 1.0.1 | Lines start at the chart's own high/low candle when the higher-timeframe candle differs from intraday bars by a tick (FX) |
| 1.0.0 | First release |
