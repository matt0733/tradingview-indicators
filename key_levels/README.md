# Key Levels

Marks the highs and lows of previous months, weeks and days, the current
monthly and weekly opens, and the last monthly close, as horizontal lines, each labelled with the period it
came from. Labels sit past the current bar so
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

## Opens

| Type | Label | Shown on |
|---|---|---|
| Monthly Open | `Oct Monthly Open` | Every timeframe |
| Weekly Open | `10/04 Weekly Open` | Weekly and lower (hidden on monthly) |
| Monthly Close | `Sep Monthly Close` | Every timeframe |

Each is the opening price of the month or week in progress, with its own on/off
and a **Previous** count that adds earlier opens (0-5 monthly, 0-3 weekly;
default 0, so only the current open shows). Months and weeks follow the same
trading periods as the highs and lows: for CME futures October opens with the
Sep 30 18:00 session and the week opens Sunday 18:00. Each open's line starts at
its period's first candle and always runs to its label, even after price trades
through it. Opens share the label column, Show Price and Label Spacing with the
highs and lows.

**Monthly Close** works the same way, but since the month in progress has no
close yet, it shows the last completed month's close (Previous 0-5 adds earlier
months). The price is TradingView's monthly candle close, and the line starts on
the month's last candle. For CME futures TradingView closes daily and monthly
candles at the settlement price, so it can differ from the last intraday candle's
close: September 2026 on MNQZ2026 closed at 30698.75, while the last 1m candle
(16:59 Sep 30) closed at 30726.25.

## How the levels are drawn

- Each line starts at the candle that set the price. If that candle is older than
  the chart's loaded history (for example a monthly high on a 1m chart), the line
  starts at the period's open instead, which is off the left edge of the chart.
- The price always comes from the monthly, weekly or daily candle. TradingView's
  intraday bars can differ from it by a tick (seen on FX:EURUSD: daily low
  1.12149, lowest 1h bar 1.12150). When the whole period is loaded, the line
  still starts at the chart's own high or low candle.
- Untaken lines run to their label, **Label Offset** bars past the current bar.
- A level is **taken** when a wick touches it. Each type has its own **When
  Taken** setting, so for example daily levels can stop at their take while
  monthly and weekly levels keep running:

| When Taken | Result |
|---|---|
| Keep (default) | Nothing changes; the line runs on to its label |
| Stop at take | The line ends at the candle that took it, and its label moves to the middle of the line, above a high and below a low |
| Remove | The line and label are deleted |

- **Close or shared prices**: every level keeps its own line and label. Labels
  closer together than **Label Spacing** (about one label's height, as a share
  of the visible price range) are spaced out evenly around their prices, highest
  price on top; at equal prices the longer period goes on top. Each moved label's
  line stops 3 bars short and a thin leader in the line's color runs from the
  line's end to its label, so every label stays tied to its own ray. Labels with
  room stay level with their line. The visible range is re-measured whenever you
  zoom or scroll, so the spacing holds on any timeframe.

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
With Extend Right at 10, an offset of 25-30 clears it at normal zoom on 1m. The
default of 10 suits a chart without Opening Gaps.

Both offsets are counted in bars, not pixels, so the two sets of labels stay
together as you scroll. Text has a fixed pixel width though, so zooming far out
squeezes the gap and the labels can overlap; zooming in widens it.

## Settings

| Setting | Default | Notes |
|---|---|---|
| Label Offset (bars) | 10 | 0–500 bars past the current bar |
| Show Price in Label | on | Appends the price in parentheses, as in Sessions, e.g. `July Monthly High (30861.25)` |
| Label Spacing (% of view) | 2 | About one label's height; labels closer than this are spaced out with leaders |
| *Type* on/off, Show Last | on; Monthly 3, Weekly 1, Daily 3 | One per type; Show Last 1–12 |
| High Line / Low Line | Black, Solid, 1 for every type | Color, style, width; set separately for highs and lows of each type |
| High Text / Low Text | Black, Small for every type | Color, size (Tiny–Large); set separately for highs and lows of each type |
| When Taken | Keep for every type | Per type: Keep, Stop at take, Remove |
| Monthly Open / Weekly Open on/off, Previous | on, 0 | Previous 0-5 monthly, 0-3 weekly |
| Open Line | Black, Dashed, 1 | Color, style, width |
| Open Text | Black, Small | Color, size (Tiny-Large) |
| Monthly Close on/off, Previous | on, 0 | Previous 0-5; line Black Dashed 1, text Black Small |

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
- **Per-type When Taken** (1.2.0, 2026-10-06, COMEX:GCZ2026 1h): with Daily on
  Stop at take and Monthly/Weekly on Keep, taken daily lines ended on the first
  1h bar that touched them (5 checked) and untaken ones ran to their labels,
  while taken monthly and weekly lines kept running. Weekly on Remove dropped the
  taken weekly low and kept the untaken high.
- **Opens** (1.3.0, 2026-10-06, MNQZ2026): the current Oct monthly open
  (30707.00, Sep 30 18:00 session) and 10/04 weekly open (31060.75) started on
  the first daily candle of their period and matched its open. With Previous at
  5 and 3, all six monthly opens (May-Oct) matched TradingView's M candles and
  all four weekly opens matched its W candles; weekly opens were hidden on the
  monthly chart.
- **High/low styles** (1.5.0, 2026-10-08, MNQZ2026 1h): with different
  colors, styles, widths and text sizes for the highs and lows of each type, every
  line and label (and leader) was drawn with its own settings, read back from
  TradingView's renderer.
- **Monthly Close** (1.4.0, 2026-10-08, MNQZ2026): with Previous at 5, all six
  closes (Apr-Sep) matched TradingView's M candles and started on each month's
  last daily candle, whose close equalled the level. On 30m and 1m the September
  line started on the last candle of Sep 30 (16:30 and 16:59).
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
- On a weekly chart, a Monthly Close appears one week late when the current
  weekly bar opened in the previous month (e.g. September's close is missing
  while the week of Sunday 9/27 is the current bar).
- Tested on MNQ only.

## Changes

| Version | Change |
|---|---|
| 1.5.0 | Separate line and text settings for highs and lows of each type; fixed changed colors not being drawn (settings are no longer captured once on the first bar) |
| 1.4.0 | Monthly Close in the Opens section: last completed month's close plus up to 5 earlier, with its own line and text settings |
| 1.3.0 | Opens section: current monthly and weekly opens, each with up to 5 / 3 previous opens and its own line and text settings |
| 1.2.1 | Price in labels is shown in parentheses, e.g. `July Monthly High (30861.25)` |
| 1.2.0 | When Taken is set per type (Monthly, Weekly, Daily) instead of once for all; default line width 1 |
| 1.1.0 | Crowded labels are spaced out with leader lines to their own ray (replaces above/below); new defaults: offset 10, prices shown, Monthly 3 / Weekly 1 / Daily 3, black Solid 2 lines |
| 1.0.1 | Lines start at the chart's own high/low candle when the higher-timeframe candle differs from intraday bars by a tick (FX) |
| 1.0.0 | First release |
