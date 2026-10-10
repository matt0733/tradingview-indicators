# Key Levels

Marks the highs and lows of previous months, weeks and days, the current
monthly and weekly opens, and the last monthly close, as horizontal lines, each labelled with the period it
came from. Labels sit past the current bar so
they can be lined up after Opening Gaps' labels.

## Levels

| Type | Label | Period |
|---|---|---|
| Monthly | `September 2026 Monthly High` | Calendar month |
| Weekly | `Previous Weekly High`, older weeks `09/20 Previous Weekly High` | Week; older weeks are dated by their open (Sunday 18:00 New York for CME futures) |
| Daily | `Thu 10/01 Daily High` | Trading day; the session opening Wednesday 18:00 is Thursday's |

Each type has its own on/off and **Show Last** count (1–12). Only completed
periods are drawn, so Show Last 1 is the last full month, week or day.

Periods follow TradingView's own bars for the symbol, which is why the weekly date
is the Sunday open and the monthly name comes from the trading days in the month
(a monthly bar opening Aug 31 18:00 is still September).

## Opens

| Type | Label | Shown on |
|---|---|---|
| Monthly Open | `October 2026 Opening Price` | Every timeframe |
| Weekly Open | `Weekly Open` | Weekly and lower; monthly too with Show Weekly Open on Monthly Chart (on by default) |
| Monthly Close | `September 2026 Closing Price` | Every timeframe |
| Previous Day Close | `Previous Day Close` | Daily and lower |

Each is the opening price of the month or week in progress, with its own on/off.
Monthly Open has a **Previous** count that adds earlier months (0-5, default 3);
Weekly Open is always just the current week. On a monthly chart the Weekly Open
starts on the month containing the week. Months and weeks follow the same
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

**Previous Day Close** is the last completed trading day's close, one level
only, with its own line and text settings.

**Close Price** (end of the Opens section) picks the price both closes use:

| Close Price | Price | Line starts on |
|---|---|---|
| Settlement (default) | The daily/monthly candle close; for CME futures the settlement, set around 16:00 New York | The last candle of the period that traded through that price (searching up to 3000 bars back), or the closest one if none did |
| Last Trade | The close of the period's last candle | That candle |

The settlement often isn't the last traded price (CME trades on to 17:00), so
anchoring it to the day's last candle left the line floating beside that
candle. On daily and higher charts, and for closes older than the loaded
history, both options use the candle close. For FX the two are the same price.

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
  line stops 3 bars short and a leader in the line's color, style and width runs from the
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
| Monthly | yes | optional | hidden |

**Show on Monthly Chart** (Weekly section, off by default) draws the previous
weekly highs and lows on a monthly chart too. A monthly candle spans several
weeks, so the levels and their takes come from the weekly candles: with Stop at
take a line ends at the monthly candle containing the week that took it.

On charts whose candles are longer than a level's period start (or take) time,
lines start on the candle that contains that time, e.g. a weekly level on a
monthly chart starts on the month containing that week, and a monthly open on a
weekly chart starts on the week containing the month's first session.

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
| *Type* on/off, Show Last | on; Monthly 3, Weekly 1, Daily 3 | One per type; Show Last 1–12. Weekly also has Show on Monthly Chart (on) |
| High Line / Low Line | Highs blue `#2962FF`; lows red `#B22833` (Weekly and Daily lows `#801922`); width 1; Solid (Weekly Dotted) | Color, style, width; set separately for highs and lows of each type |
| High Text / Low Text | Same colors as the lines, Small, bold | Color, size (Tiny–Large), Bold; set separately for highs and lows of each type |
| When Taken | Keep for every type | Per type: Keep, Stop at take, Remove |
| Monthly Open on/off, Previous | on, 3 | Previous 0-5 |
| Weekly Open on/off, Show Weekly Open on Monthly Chart | on, on | Current week only |
| Open Line | Black, Dashed, 1 | Color, style, width |
| Open Text | Black, Small, bold | Color, size (Tiny-Large), Bold |
| Monthly Close on/off, Previous | on, 3 | Previous 0-5; line Black Dashed 1, text Black Small bold |
| Previous Day Close on/off | on | Line Black Dashed 1, text Black Small bold |
| Close Price | Settlement | Settlement or Last Trade, for Monthly Close and Previous Day Close |

Inputs are hidden from the chart status line.

## Validation

On MNQZ2026 (CME_MINI), 2026-10-02:

- **Values**: every monthly, weekly and daily high and low matched TradingView's
  own M, W and D bars, with Show Last 1 and 12 (72 levels).
- **Names** (1.0.0 wording): September for a monthly bar opening Aug 31 18:00; `09/20 Prev Week`
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
- **Weekly Open** (1.13.0, 2026-10-09): on MNQZ2026, MESZ2026, BTCUSD and AAPL
  at 1m, 10m, 1h, 4h, daily, weekly and monthly, `Weekly Open` matched the current
  weekly candle's open and started on its first candle (on monthly, the month
  containing the week); turning Weekly Open or the monthly option off hid it.
  EURUSD matched on every timeframe except 1m, where the line started on Friday's
  last candle because no 1m bar exists at the Sunday 17:00 open (known limit).
- **Close Price** (1.12.0, 2026-10-09): on MNQZ2026, MESZ2026 and FX:EURUSD,
  1m, 5m, 10m, 1h, 4h and daily, with both Settlement and Last Trade (36 runs),
  every Previous Day Close and Closing Price line whose candle was loaded
  started on a candle whose range contained its price. Settlement on MNQ: 30969.50
  anchored to 15:59 (1m) / 15:00 (1h); Last Trade: 31021.50 at 16:59 (1m).
  With 1m history loaded back past Sep 30, the September closing price also
  touched on 1m: MNQ Settlement 30698.75 at 16:15, Last Trade 30726.25 at 16:59;
  MES Settlement 7715.50 at 16:09, Last Trade 7721.50 at 16:59.
- **Previous Day Close and names** (1.11.0, 2026-10-09, MNQZ2026): 30969.50
  matched the Oct 8 daily candle close; its line started on that day's last
  candle on 1m (16:59), 5m (16:55), 10m (16:50), 1h (16:00), 4h (14:00) and
  daily, and it was hidden on weekly. Month labels read e.g. `September 2026
  Monthly High`, `October 2026 Opening Price`, `September 2026 Closing Price`.
- **Leaders** (1.9.0, 2026-10-08, MNQZ2026 D): dashed opens/close had dashed
  leaders, solid levels solid ones, and a Weekly High set to Dotted 2 got a
  Dotted 2 leader.
- **Weekly names** (1.8.0, 2026-10-08, MNQZ2026): with Show Last 1 the weekly
  levels read `Previous Weekly High/Low`; with 4, the latest week stayed undated
  and 09/20, 09/13 and 09/06 were dated, on daily and with Show on Monthly Chart.
- **Weekly on monthly** (1.7.0, 2026-10-08, MNQZ2026): with Show on Monthly
  Chart on, all 12 previous weeks (07/12-09/27) matched TradingView's weekly
  candles; with Stop at take the 5 levels taken by later weeks ended on the right
  month and the 3 untaken ran to their labels. Line starts snapped to the
  containing candle on the monthly and weekly charts; EURUSD 1h daily levels
  (up to 3 ticks off the daily candle) still started on each day's own
  high/low candle.
- **Bold** (1.6.0, 2026-10-08, MNQZ2026 1h): with Bold on for Monthly High,
  Weekly Low, Daily High and Monthly Open only, exactly those labels were drawn
  bold and all others regular.
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
  Thursday 7/2 18:00, so (when not the latest week) it is labelled `07/02 Previous Weekly`.
- Label Spacing only sees Key Levels' own labels. It can't move them away from
  Opening Gaps' text at a nearby price; Label Offset is the fix for that.
- Using the visible range makes the script recalculate on every zoom and scroll.
- On a weekly chart, a Monthly Close appears one week late when the current
  weekly bar opened in the previous month (e.g. September's close is missing
  while the week of Sunday 9/27 is the current bar).
- On a weekly chart a monthly high or low set in a week that straddles two
  months (e.g. the week of 9/27, which includes Oct 1-2) can't be pinned to that
  week, so its line starts at the week containing the month's first session.
- On a 1m chart, a period that opens in a minute with no bar (EURUSD's Sunday
  17:00 weekly open) starts its line on the bar before the gap.
- Tested on MNQ, MES, MYM, AAPL, BTCUSD, ETHUSD and EURUSD.

## Changes

| Version | Change |
|---|---|
| 1.13.0 | Weekly Open is the current week only, labelled `Weekly Open`, with Show Weekly Open on Monthly Chart; defaults from the user's chart: all line widths 1, Weekly lows `#801922` |
| 1.12.0 | Close Price setting (Settlement / Last Trade) for Monthly Close and Previous Day Close; settlement closes start on a candle that traded at that price instead of floating beside the day's last candle |
| 1.11.0 | Previous Day Close in the Opens section; defaults taken from the user's chart (blue highs, red lows, width 2, bold, Weekly dotted and on the monthly chart, Monthly Open/Close Previous 3) |
| 1.10.0 | Month labels include the year: `September 2026 Monthly High`, `October 2026 Opening Price`, `September 2026 Closing Price` |
| 1.9.0 | Leaders use their line's style and width (were always solid, width 1) |
| 1.8.0 | Weekly labels: the latest week reads `Previous Weekly High/Low` with no date; older weeks read `09/27 Previous Weekly High/Low` |
| 1.7.0 | Show on Monthly Chart option for weekly highs/lows; lines start on the candle containing their time on higher-timeframe charts (were a candle late); monthly levels on a weekly chart no longer anchor to a week straddling two months |
| 1.6.0 | Bold option on every text row (high and low text of each type, Monthly Open, Weekly Open, Monthly Close) |
| 1.5.0 | Separate line and text settings for highs and lows of each type; fixed changed colors not being drawn (settings are no longer captured once on the first bar) |
| 1.4.0 | Monthly Close in the Opens section: last completed month's close plus up to 5 earlier, with its own line and text settings |
| 1.3.0 | Opens section: current monthly and weekly opens, each with up to 5 / 3 previous opens and its own line and text settings |
| 1.2.1 | Price in labels is shown in parentheses, e.g. `July Monthly High (30861.25)` |
| 1.2.0 | When Taken is set per type (Monthly, Weekly, Daily) instead of once for all; default line width 1 |
| 1.1.0 | Crowded labels are spaced out with leader lines to their own ray (replaces above/below); new defaults: offset 10, prices shown, Monthly 3 / Weekly 1 / Daily 3, black Solid 2 lines |
| 1.0.1 | Lines start at the chart's own high/low candle when the higher-timeframe candle differs from intraday bars by a tick (FX) |
| 1.0.0 | First release |
