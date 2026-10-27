# Key Levels

Marks the highs and lows of previous months, weeks and days, the current
monthly, weekly and daily opens, the last monthly and daily closes, and the
07:00-09:00 premarket high and low and the midnight open, as horizontal lines, each labelled with the
period it came from. Labels sit past the current bar so
they can be lined up after Opening Gaps' labels.

## Levels

| Type | Label | Period |
|---|---|---|
| Monthly | `September 2026 Monthly High` | Calendar month |
| Weekly | `Previous Weekly High`, older weeks `09/20 Previous Weekly High` | Week; older weeks are dated by their open (Sunday 18:00 New York for CME futures) |
| Daily | `Thu 10/01 Daily High` | Trading day; the session opening Wednesday 18:00 is Thursday's |

Each type has its own on/off and **Show Last** count (1–12). Only completed
periods are drawn, so Show Last 1 is the last full month, week or day.

A period counts as completed as soon as its session closes, even if the next
one hasn't opened yet: over a weekend Friday is the previous day (and Previous
Day Close is Friday's close) and the week just ended is the Previous Week, while
the Weekly Open stays on that week until the next one opens. The same applies in
the daily break and on holidays. 24/7 markets such as crypto are unaffected:
their periods only close when the next one starts.

"Session closed" is judged by the chart's own latest candle, not the clock, so
Bar Replay works too: at a replayed Monday 09:16 the previous day is Friday and
the month in progress has no closing price yet.

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
| Daily Open | `Daily Open` | Daily and lower; not on range or tick charts |

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

**Daily Open** is the current trading day's opening price, one level only, with
its own line and text settings. It works like Previous Day Close: its line starts
on the day's first candle and always runs to its label. The price is the open of
the day's first 1-minute candle (for CME futures the 18:00 New York session open;
for crypto 00:00 UTC), read from 1-minute data so it is the same on every chart.
Below 1 minute the chart's own bars are used. Its settings sit after Close Price.

**Close Price** (in the Opens section) picks the price both closes use:

| Close Price | Price | Line starts on |
|---|---|---|
| Settlement (default) | The daily/monthly candle close; for CME futures the settlement, set around 16:00 New York | The last candle of the period that traded through that price (searching up to 3000 bars back), or the closest one if none did |
| Last Trade | The close of the period's last candle | That candle |

The settlement often isn't the last traded price (CME trades on to 17:00), so
anchoring it to the day's last candle left the line floating beside that
candle. On daily and higher charts, and for closes older than the loaded
history, both options use the candle close. For FX the two are the same price.

## Premarket

`Premarket High` and `Premarket Low`: the high and low between 07:00 and 09:00
New York, one instance, read from 1-minute candles so they are exact even on 2h
to 4h charts, whose bars don't line up with 07:00 and 09:00 (below 1 minute the
chart's own bars are used).

- They appear at 07:00 and follow new highs and lows until 09:00, then hold
  until the next day's 07:00 replaces them.
- Each line starts on the candle that set its price (the first one, if the price
  was reached more than once).
- Takes only count after 09:00. **When Taken** works as for the other highs and
  lows; with Stop at take the line ends at the candle containing the first
  1-minute candle that traded through it.
- High and low each have their own line color, style and width and text color,
  size and Bold.
- Shown on 4-hour and lower timeframes; not on daily and higher, range or tick
  charts.
- Every market uses New York time, including crypto and forex.

## Midnight Open

`Midnight Open`: the open of today's 00:00 New York candle, one instance. The
price comes from 1-minute data on every timeframe (the chart's own bars below 1
minute); the line starts on the candle containing 00:00 and always runs to its
label. At the next 00:00 it moves to the new candle. Its own line color, style
and width and text color, size and Bold, in a Midnight Open section at the
bottom. Shown on 4-hour and lower timeframes; not on daily and higher, range or
tick charts.

## How the levels are drawn

- Each line starts at the candle that set the price. If that candle is older than
  the chart's loaded history (for example a monthly high on a 1m chart), the line
  starts at the period's open instead, which is off the left edge of the chart.
- The price always comes from the monthly, weekly or daily candle. TradingView's
  intraday bars can differ from it by a tick (seen on FX:EURUSD: daily low
  1.12149, lowest 1h bar 1.12150). When the whole period is loaded, the line
  still starts at the chart's own high or low candle.
- Untaken lines run to their label, **Label Offset** bars past the current bar.
  The offset is counted in bars, so it holds over weekends and session breaks;
  each such line is drawn in two parts that meet at the current bar.
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
| High Line / Low Line | Highs blue `#2962FF`; lows dark red `#801922`; width 2 (Daily 1); Solid (Weekly Dotted) | Color, style, width; set separately for highs and lows of each type |
| High Text / Low Text | Same colors as the lines, Small, bold | Color, size (Tiny–Large), Bold; set separately for highs and lows of each type |
| When Taken | Keep for every type | Per type: Keep, Stop at take, Remove |
| Monthly Open on/off, Previous | on, 3 | Previous 0-5 |
| Weekly Open on/off, Show Weekly Open on Monthly Chart | on, on | Current week only |
| Open Line | Black, Dashed, width 1 | Color, style, width |
| Open Text | Black, Small, bold | Color, size (Tiny-Large), Bold |
| Monthly Close on/off, Previous | on, 3 | Previous 0-5; line Black Dashed 1, text Black Small bold |
| Previous Day Close on/off | on | Line Black Dashed 1, text Black Small bold |
| Close Price | Settlement | Settlement or Last Trade, for Monthly Close and Previous Day Close |
| Daily Open on/off | on | Line Black Dashed 1, text Black Small bold |
| Midnight Open on/off | on | Line Black Dashed 1, text Black Small bold |
| Premarket High / Low on/off | on | High and low lines and text red `#F23645`; Solid 1, Small, bold; When Taken Keep |

Inputs are hidden from the chart status line.

## Validation

On MNQZ2026 (CME_MINI), 2026-10-02:

- **Midnight Open** (1.16.0, 2026-10-26): the label price matched the 00:00 New
  York 1-minute candle's open on MNQ, MES, MGC, CL, SI, ZB, BTCUSDT.P and EURUSD
  on 15s, 30s, 1, 2, 3, 4, 5, 10, 15 and 30 minutes, 1h and 4h; hidden on daily,
  weekly and monthly. Bar Replay across midnight: on MNQ and MGC 1m the ray moved
  to the new open at 00:00, starting on the 00:00 candle, with one Midnight Open
  throughout; on MNQ 5m and BTC 15m it moved one bar late (see Known limits).
- **Bar Replay** (1.15.4, 2026-10-26, MNQ1!): before, a replayed day, week or
  month counted as finished because Key Levels compared it with the real clock,
  so at Mon 10/27/2025 09:16 Previous Day Close and "October 2025 Closing Price"
  were the current candle's close (25832.75) and the month's, week's and day's
  highs so far showed as completed levels. Expected values were then computed
  from MNQ1!'s daily, weekly and monthly candles for six replay moments (Mon 10/27
  09:16, Wed 10/15 11:00, Wed 10/15 17:30 in the daily break, Sat 10/25, Fri 10/31
  17:30 after October's close, Mon 11/3 10:00). On 1m, 5m, 1h and 4h every one
  matched: Previous Day Close, latest monthly close and high, newest daily
  high/low and Previous Weekly high/low (e.g. Mon 10/27 09:16: Previous Day Close
  25509.25 = Fri 10/24, September 2025 the latest close). Daily chart: matched
  whenever replay showed whole days (Wed 11:00, Wed 17:30, Saturday, Fri 10/31),
  wrong on the first frame of a replay started mid-day (see Known limits). Live
  on 15s, 1m, 5m, 1h, 4h and daily: unchanged and correct.
- **Daily Open and Premarket** (1.15.0, 2026-10-21): expected values computed
  from each market's raw 1-minute candles. On MNQ, MES, MGC, CL, SI, ZB,
  BTCUSDT.P and EURUSD:
  - On 15s, 30s, 1, 2, 3, 4, 5, 10, 15 and 30 minutes, 1h, 2h, 3h and 4h, every
    price matched, and every line started on the bar containing the expected
    candle; on 15s that is the first 15-second bar that set the price. Daily Open
    also matched on daily.
  - Daily Open hidden on weekly and monthly; Premarket hidden on daily, weekly
    and monthly; both hidden on a range chart. Tick charts not tested: they need
    a higher TradingView plan.
  - Stop at take on MNQ, CL and ZB at 1, 5, 60 and 240 minutes: each taken line
    ended on the bar containing the first take after 09:00, label above the high
    and below the low; CL's untaken high kept running. Remove deleted the taken
    levels and kept CL's untaken high.
  - Bar Replay on MNQ 1m: at 06:29 the lines showed 10/20's premarket (29620.5 /
    29478.25, matching its 120 one-minute candles); at 07:29 and 08:58 the range
    built so far; Daily Open was the 18:00 open (29924.5). Same on CL at 07:29.
  - Label spacing with the new labels: no two closer than one label spacing.

- **Fractional prices** (1.14.5, 2026-10-21): the format matched TradingView's own
  price formatter on every possible fraction of ZB, ZN, ZF, ZT and corn (484
  prices). On the chart, all 30 labels on each of ZB, UB, ZN, TN, ZF, ZT, ZC, ZS,
  MNQ, GC and CL showed their level's price exactly as TradingView formats it;
  decimal markets are unchanged.
- **Label spacing on coarse ticks** (1.14.4, 2026-10-21): on ZB, 1.14.3 put labels
  closer than one label spacing on 15m, 5m and 1h (11 pairs, closest 0.83 of a
  label). 1.14.4 has none on ZB 1m, 5m, 15m and 1h, ZN 15m, ZF 15m, MNQ 1m and
  1h, GC 10m and CL 30m, and every column label sits on its line or is joined to
  it by a leader.
- **Straddling weeks** (1.14.2, 2026-10-16): with 12 monthly and 12 weekly levels,
  every visible line started on a bar whose range contained its price on MNQZ2026
  15s, 30s, 1m, 2m, 3m, 4m, 5m, 10m, 15m, 30m, 1h, 4h, daily, weekly and monthly,
  and on EURUSD and AAPL daily, weekly and monthly (21 runs, 0 misses). Before the
  fix the weekly charts had 1 miss each (MNQ April high, EURUSD September low,
  AAPL April high, each inside a week that ran into the next month).
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
- **Colors** (1.14.1, 2026-10-15, MNQZ2026): 1.14.0 could draw old colors after
  a color change. A drawing colored straight from a color setting is drawn from
  a TradingView palette slot, and the chart's copy of those slots kept old
  colors. Changing all 20 colors with an immediate switch to 5m left 1 slot
  stale (Previous Day Close text stayed black); restoring them with an
  immediate switch to 1m left 17 of 20 stale (e.g. Daily Low drawn cyan instead
  of `#801922`). 1.14.1 has 0 palette slots (1.14.0 had 21). All 20 colors were
  then changed with a timeframe switch at the same instant on all 11 timeframes
  from 15s to 1h, alternating two color sets: every line, leader and label had
  its new color (48-58 lines and 24 labels per timeframe, none wrong), and
  screenshots on every timeframe showed them. Restoring the colors with an
  immediate switch to 1m put every drawing back. The saved script matches this
  file by hash.
- **Roll at close** (1.14.0, 2026-10-10, Saturday): on MNQZ2026, EURUSD and AAPL
  (closed) the newest daily levels were Fri 10/09, Previous Day Close was
  Friday's daily close and Previous Weekly High/Low the week just ended, on 1m,
  1h and daily (weekly levels also on weekly and monthly); Weekly Open stayed
  on that week. On BTCUSD (open) nothing moved: Friday was the newest completed
  day and the previous week was 09/28. 20 runs, all values matched the D/W
  candles (AAPL's 332.815 weekly open shows as 332.82 in the label).
- **Full regression** (1.13.2, 2026-10-10, a Saturday): MNQZ2026, EURUSD and
  BTCUSD on 1m, 5m, 10m, 1h, 4h, daily, weekly and monthly at Label Offset 10
  and 31 (59 runs): every label sat exactly offset bars past the last bar,
  every untaken line's two parts met at the last bar, every moved label had its
  leader, and each type showed or hid as per the timeframe table. On MNQ 1h and
  daily, Stop at take, Remove (24 - 6 taken = 18 labels), Show Price off and
  Last Trade behaved as documented; on MNQ monthly, turning off weekly
  highs/lows, Weekly Open on monthly and Weekly Open hid only those levels.
- **Weekly Open** (1.13.0, 2026-10-09): on MNQZ2026, MESZ2026, BTCUSD and AAPL
  at 1m, 10m, 1h, 4h, daily, weekly and monthly, `Weekly Open` matched the current
  weekly candle's open and started on its first candle (on monthly, the month
  containing the week); turning Weekly Open or the monthly option off hid it.
  EURUSD matched everywhere after 1.13.1: on 1m, where no bar exists at the
  Sunday 17:00 open, the line now starts on the first bar after it (17:03)
  instead of Friday's last bar. MNQ 1m and daily lines were unchanged.
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

- Takes of older levels that happened before the chart's loaded history are
  placed at the open of the period that took them, not the exact candle. These
  are off the left edge of the chart.
- Holiday weeks use TradingView's weekly bar open: the July 4th 2026 week opened
  Thursday 7/2 18:00, so (when not the latest week) it is labelled `07/02 Previous Weekly`.
- Label Spacing only sees Key Levels' own labels. It can't move them away from
  Opening Gaps' text at a nearby price; Label Offset is the fix for that.
- Using the visible range makes the script recalculate on every zoom and scroll.
- Bar Replay on a daily chart: when a replay is started at a time of day, its
  first frame shows the current day part-built, and Key Levels treats that day as
  finished (e.g. Previous Day Close is that day's close so far). Pine can't tell
  that candle from a finished one there: the clock is the real time, the daily
  candle closes at the end of the day, and lower-timeframe data on a daily chart
  in replay runs to the end of the day too. Step once (the daily chart steps a
  whole day) and the levels are right; intraday charts are right throughout.
- In Bar Replay on timeframes above 1 minute, Midnight Open (and Daily Open at
  18:00) moves to the new price one bar late: replay delivers the 1-minute data
  for its newest bar with the next step. On 1m it moves on the 00:00 candle.
- Levels tested on MNQ, MES, MYM, MGC, CL, AAPL, BTCUSD, ETHUSD and EURUSD;
  label spacing and prices also on ZB, UB, ZN, TN, ZF, ZT, ZC, ZS and GC.

## Changes

| Version | Change |
|---|---|
| 1.16.0 | Midnight Open: the open of today's 00:00 New York 1-minute candle, one instance, with its own line and text settings; 4h and lower |
| 1.15.4 | Bar Replay: a day, week or month counts as finished only once the chart's latest candle (or latest 1-minute candle, or above 1 hour the latest 1-hour candle) reaches its close, never by the real clock, which made in-progress periods show as completed in replay. 1.15.2 and 1.15.3 were intermediate versions of this fix |
| 1.15.1 | Defaults from the user's chart: Monthly Show Last 3; Monthly lows `#801922`; Weekly Open, Previous Day Close and Daily Open width 1; Premarket High and Low `#F23645`, Solid |
| 1.15.0 | Daily Open (the trading day's first 1-minute open; daily and lower) and Premarket High / Low (07:00-09:00 New York from 1-minute candles, building from 07:00 and held until the next 07:00, with When Taken; 4h and lower). Neither shows on range or tick charts |
| 1.14.5 | Label prices on CBOT's fractional markets show as TradingView does: ZB/UB `106'03`, ZN/TN `105'12'5`, ZF `103'30'7`, ZT `101'27'7`, grains `464'2` (was rounded to one decimal, e.g. 106.2) |
| 1.14.4 | Labels no longer land on each other on coarse-tick markets such as ZB: a label is treated as moved when its slot is off its price by more than 1% of a label height (was half a tick, which on ZB's 1/32 tick is about a whole label) |
| 1.14.3 | Defaults from the user's chart: Monthly Show Last 6; Monthly and Weekly high/low lines, Weekly Open and Previous Day Close width 2 |
| 1.14.2 | Monthly highs/lows on a weekly chart, and weekly highs/lows on a monthly chart, start on a bar that traded the price when a week straddles two months (exact match first, else the latest bar whose range covers it; weekly levels on monthly start on the month after if the week's own month didn't reach the price) |
| 1.14.1 | Changed colors always show on the chart: colors are rebuilt with `color.new(c, color.t(c))` so each drawing carries its own color instead of a palette slot that could keep the old one |
| 1.14.0 | Periods count as completed once their session closes (weekends, the daily break, holidays), not when the next one opens; replaces the weekly-chart month rollover workaround. 24/7 markets unchanged |
| 1.13.2 | Label Offset counted in bars past the current bar; with a time offset, labels collapsed to the next session open whenever the offset landed in a weekend or the daily break |
| 1.13.1 | A time that falls in a gap between bars (weekend, or a minute with no trades) snaps to the next bar, not the one before the gap (daily and lower charts) |
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
