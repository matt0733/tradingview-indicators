# Core Levels

A simplified set of key levels for live trading and Bar Replay. It shows only the
levels that apply now. Each level is replaced when the next one forms, so the chart
stays uncluttered. Everything is worked out from the chart's own bars as they
arrive, so nothing looks ahead in Bar Replay. The monthly, weekly and daily prices
come from the higher-timeframe candle of the bar before.

## Levels

| Level | Label | Definition | Shown on |
|---|---|---|---|
| Previous Monthly High / Low | `Previous Monthly High` | Last completed month | Monthly and lower |
| Previous Weekly High / Low | `Previous Weekly High` | Last completed week | Weekly and lower |
| Previous Day High / Low | `Previous Day High` | Last completed trading day (18:00-17:00 for CME futures) | Daily and lower |
| Previous Day Close | `Previous Day Close` | That day's settlement (default) or last trade | Daily and lower |
| Asia High / Low | `Asia High` | Latest completed Asia session, 18:00-01:00 | 1 hour and lower |
| London High / Low | `London High` | Latest completed London session, 01:00-07:00 | 1 hour and lower |
| Midnight Open | `Midnight Open` | Open of today's 00:00 candle | 1 hour and lower |
| Premarket High / Low | `Premarket High` | 07:00-09:00 high and low | 1 hour and lower |
| RTH ORG | `RTH High 11/06`, `RTH 50% 11/06` | Regular session close to the next regular open, with 50%, 25% and 75% lines | 1 hour and lower |

All times are New York. With **Show Price in Label** on, the price follows in
brackets, e.g. `Previous Day High (28668.75)`.

### When each level changes

- **Previous month, week and day:** replaced when the next period opens. For CME
  futures the day turns over at 18:00, the week at Sunday 18:00, and the month
  with its first trading session.
- **Previous Day Close:** replaced at the same time as the previous day's high
  and low.
- **Asia and London:** appear when the session ends, at 01:00 and 07:00, and
  stay until the next one ends. The session times can be changed.
- **Midnight Open:** set on the 00:00 candle and replaced at the next midnight.
- **Premarket:** appears at 07:00 and follows new highs and lows until 09:00,
  then holds until the next day's 07:00. It can only be taken after 09:00.
- **RTH ORG:** appears on the candle holding the regular open and stays until
  the next session opens.

### Where lines start

Each line starts on the candle that set its price. For a monthly or weekly level
set before the chart's first bar, the line starts at the first bar. A settlement
close (Previous Day Close, RTH ORG under Settlement) starts on the latest candle
before the close that traded that price, since the settlement often sits off the
last candle.

### Taken levels

A level is **taken** when price trades through it (a wick touch counts). Each
high/low group has a **When Taken** setting:

| When Taken | What happens |
|---|---|
| Keep (default) | The line runs on to its label |
| Stop at take | The line ends at the candle that took it; the label moves to the middle of the line, above it for highs and below it for lows |
| Remove | The line and label are deleted |

Previous Day Close, Midnight Open and the RTH ORG have no takes; their lines
always run to their labels.

## RTH ORG

The gap between the regular session's close and the next regular open, Monday to
Friday. For Equity Index that is the 16:14 last trade to the 09:30 open. The High
and Low are the two prices, whichever is higher; the 50%, 25% and 75% lines are
measured from the open toward the prior close, so 25% means the gap is a quarter
filled. If the close and open are the same price, no gap is drawn that day.

| Market | Session (New York) | Close Price under Auto |
|---|---|---|
| Equity Index (default) | 09:30-16:15 | Last Trade |
| Treasuries (ZT, ZF, ZN, TN, ZB, TWE, UB) | 08:20-15:00 | Settlement |
| Crude Oil (CL, MCL, QM) | 09:00-14:30 | Settlement |
| Natural Gas (NG, MNG, QG) | 09:00-14:30 | Settlement |
| Gold (GC, MGC, QO, 1OZ) | 08:20-13:30 | Settlement |
| Silver (SI, SIL, QI, SIC) | 08:25-13:25 | Settlement |
| Copper (HG, MHG, QC) | 08:10-13:00 | Settlement |
| Custom | Custom Hours | Last Trade |

**Market** set to Auto picks the row from the chart's symbol. 1OZ and SIC always
use Last Trade, because they trade around the clock. Futures holidays are
skipped, so the next gap runs from the last real session's close.

Where the chart's bars land exactly on the open and close (1, 3, 5 and 15m for
Equity Index), the gap is read from the chart. On other timeframes up to 1 hour
(e.g. 30m and 1h) the prices come from the largest lower timeframe that does, and
give the same values.

## Labels

Labels sit in one column past the current bar. Labels closer together than
**Label Spacing** are spread out evenly around their prices, each joined to its
line by a short leader. The RTH ORG labels can sit above, on or below their lines
(Top, Middle, Bottom). The 50% and quarter labels can be turned off, leaving the
line only.

## Settings

| Section | Settings (default) |
|---|---|
| General | Label Offset (10 bars), Show Price in Label (on), Spread Overlapping Labels (on), Label Spacing (2% of view), Max Label Move (0 = no limit), Skip Spread When Crowded (off) |
| Previous Month | On; High and Low each with line color, style, width and text color, size, Bold (blue / maroon, Solid, 2, Small, Bold); When Taken (Keep) |
| Previous Week | Same as Previous Month (Dotted, 2) |
| Previous Day | Same (Solid, 2); Previous Day Close (black, Dashed, 1); Close Price (Settlement or Last Trade) |
| Asia | On, session (18:00-01:00), High and Low styling (orange, Solid, 1), When Taken (Keep) |
| London | On, session (01:00-07:00), High and Low styling (blue, Solid, 1), When Taken (Keep) |
| Midnight Open | On; line and text (black, Dashed, 1) |
| Premarket | On; High and Low styling (red, Solid, 1); When Taken (Keep) |
| RTH ORG | On; line (red, Solid, 2); text (Small, Middle, Date on, Bold); Mid Line (Dotted, 2, Bold, label on); Quarters (Dotted, 2, Bold, labels on); Market (Auto); Custom Hours (09:30-16:15); Close Price (Auto) |

Default colors follow Key Levels + Gaps and Sessions. Premarket and RTH ORG share
the same red by default.

## Limits

- **Weekends and the daily break, live:** a day, week or month counts as
  finished only when the next one opens. Over a weekend the previous day is
  still Thursday until Sunday's 18:00 open. Bar Replay is not affected.
- **Session levels need intraday bars:** Asia, London, Midnight Open,
  Premarket and the RTH ORG show on 1-hour and lower charts only, not on
  range or tick charts.

## Changes

| Version | Change |
|---|---|
| 1.1.1 | Defaults: Previous Day High / Low width 2; RTH ORG width 2, Date on, Mid Line and Quarters width 2 and Bold |
| 1.1.0 | RTH ORG; a settlement Previous Day Close starts on a candle that traded it |
| 1.0.2 | Renamed from Backtesting to Core Levels |
| 1.0.1 | Previous month, week and day highs and lows now draw (they were missing in 1.0.0) |
| 1.0.0 | Prototype |
