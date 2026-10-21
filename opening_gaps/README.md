# Opening Gaps

Marks three opening gaps as horizontal lines: the New Week Opening Gap, the New
Day Opening Gap, and the Regular Trading Hours opening gap. Each gap is drawn at
its high and low, with an optional mid line and quarter lines, and all lines
extend past the current bar with a label at the right end.

## The gaps

All times are New York.

| Gap | Prior close | Open | Days |
|---|---|---|---|
| **NWOG** | Friday, last price before 17:00 (the 16:59 close) | Sunday 18:00 | Weekly |
| **NDOG** | Last price before 17:00 (the 16:59 close) | 18:00 the same day | Monday–Thursday |
| **RTH** | The regular session's close (Equity Index: the 16:14 close) | The next regular session's open (Equity Index: 09:30) | Monday–Friday |

A gap where the open equals the prior close is not drawn.

## RTH gap by market

The RTH gap runs between each market's own regular session, set by **Market**
at the bottom of the RTH Gap section. The times are CME's floor hours, New York
time; each close is where that market's daily settlement window ends.

| Market | Open | Close | Settlement window | Auto picks it for |
|---|---|---|---|---|
| Equity Index | 09:30 | 16:15 | 15:59:30–16:00 | anything not listed below |
| Treasuries | 08:20 | 15:00 | 14:59:30–15:00 | ZT, ZF, ZN, TN, ZB, TWE, UB |
| Crude Oil | 09:00 | 14:30 | 14:28–14:30 | CL, MCL, QM |
| Natural Gas | 09:00 | 14:30 | 14:28–14:30 | NG, MNG, QG |
| Gold | 08:20 | 13:30 | 13:29–13:30 | GC, MGC, QO, 1OZ |
| Silver | 08:25 | 13:25 | 13:24–13:25 | SI, SIL, QI, SIC |
| Copper | 08:10 | 13:00 | 12:59–13:00 | HG, MHG, QC |
| Custom | your Custom Hours | | | — |

**Auto** reads the chart's own symbol root, so continuous (`GC1!`) and dated
(`MGCZ2026`) contracts both work, and the micro and E-mini contracts follow
their full-size market. Sources, checked on CME's site on 2026-10-21: the
settlement procedures for each market; the 2014 COMEX notice SER-7040R for the
metals opens; NYMEX's 2006 notice moving the energy open to 09:00. No current CME
page states the Treasury floor open; 08:20 is the interest-rate floor open in
CME's SOFR options guide (7:20 a.m.–2 p.m. CT).

**Close Price** picks what the gap is measured from:

| Close Price | Prior close |
|---|---|
| Auto (default) | Last Trade for Equity Index and Custom, Settlement for every other market |
| Last Trade | The last trade before the close (e.g. 16:14 for Equity Index, 13:29 for Gold) |
| Settlement | CME's daily settlement, which is TradingView's daily close for CME futures |

Auto keeps ES/NQ gaps exactly as before. With Settlement, the prior-close line
starts on the latest candle before the close that traded the settlement price,
since a settlement is an average over its window and can sit off the last
candle. 1OZ and SIC always use Last Trade: they trade around the clock and
their daily bars run 17:00 to 17:00, so the daily close is the 17:00 trade, not
the settlement.

Thinly traded contracts (e.g. QI, QC) sometimes have no trade at the open; the
gap then opens on the first trade after it.

## How the levels are drawn

- **High and Low** each start at the bar where that price was set. For example,
  in a gap down the High line starts at the prior close and the Low line starts
  at the open.
- **Mid and quarter lines** start at the open.
- **Percent levels run from the open toward the prior close**, so `25%` means the
  gap is a quarter filled and `75%` sits next to the prior close. In a gap up,
  25% is near the High; in a gap down, it is near the Low.
- Quarter levels are exact fractions of the gap and can fall between ticks
  (e.g. 29494.125).
- Lines keep extending after a gap fills.
- Past the current bar, the extension, labels and leaders are positioned by bar
  count, not time. Each level is drawn in two parts that meet at the current
  bar: time-based behind it (so a level can start weeks back) and bar-based ahead
  of it. A time-based future position would collapse into the next session open
  during the 17:00 halt and over weekends, squeezing Extend Right to one bar and
  turning leaders vertical.

**Labels** name the gap and carry the date of its open, so several gaps of
different types can sit on the chart without ambiguity:

```
NWOG High 09/13   NWOG 75% 09/13   NWOG 50% 09/13   NWOG 25% 09/13   NWOG Low 09/13
```

Every level of a gap shows the open's date, including the one taken from the
prior close — the NWOG High above is Friday 09/11's close, labelled 09/13.
Dates can be turned off per block with each block's **Date** checkbox.

## Settings

**General:**

| Setting | Default | Notes |
|---|---|---|
| Extend Right (bars) | 20 | How far past the current bar lines and labels reach |
| Spread Overlapping Labels | on | Spaces out labels that would overlap; see below |
| Label Spacing (% of view) | 2 | Roughly one label height, as a share of the visible price range |
| Max Label Move (label heights) | 0 (no limit) | A label that would move further stays on its line |
| Skip Spread When Crowded | off | No spreading when more labels are in view than fit |

Each gap type (NWOG, NDOG, RTH Gap) has its own section with two blocks:

| Block | Default | What it controls |
|---|---|---|
| **Current** | on | The newest gap of that type |
| **Previous** + Show Last | off, 2 (max 5) | Up to 5 gaps before the current one |

Each block has its own full styling:

| Row | Default (Current) | Notes |
|---|---|---|
| Line | NWOG `#673AB7` Solid 2 · NDOG `#2962FF` Solid 2 · RTH `#FF9800` Solid 1 | Color, style, width for High/Low |
| Text | Small, Middle, Date on, Bold on | Color, size (Tiny–Large), position (Top, Middle, Bottom), date on/off, bold for the High/Low labels |
| Mid Line | on, Dotted 1, Bold off, Label on (Current) / off (Previous) | Uses the block's line color; Bold and Label apply to the 50% label |
| Quarters | on, Dotted 1, Bold off, Label on (Current) / off (Previous) | 25% and 75%; uses the block's line color; Bold and Label apply to their labels |

Previous gaps default to solid grays, so the current gaps stand out in color:
RTH `#4A4D57` (darkest), NDOG `#6A6D78`, NWOG `#8A8D97` (lightest). Line and
text use the same shade.

Each label type has its own **Bold** switch in every block: High/Low on the
Text row (default on), the 50% label on the Mid Line row and the 25%/75% labels
on the Quarters row (both default off). That's 18 switches in total. By
default High and Low stand out in bold while the percent levels stay plain.
Bold text reads much better at small sizes, especially the lighter grays,
without making the labels bigger.

The **Label** checkbox on each Mid Line and Quarters row shows or hides that
row's text. Off draws the line at its exact price with no label, which also
keeps it out of the label spreading. By default current gaps label every level
and previous gaps label only High and Low, keeping charts with several previous
gaps readable. A row's Bold switch only matters while its Label is on.

The two checkboxes are independent, and each one switches off everything under
it: lines, labels, mid line, quarters and dates. With Current off and Previous on,
the older gaps draw and the newest is hidden. Up to 6 gaps of each type can show
at once.

Text position is relative to the line: Top sits above it, Middle level with it,
Bottom below it. Inputs are hidden from the chart status line.

## Overlapping labels

Gap levels often sit within a few points of each other. RTH High uses the 16:14
close and NDOG Low the 16:59 close of the same day, so the two regularly land
almost on top of each other. When labels are closer than about one label height
(**Label Spacing**, as a share of the visible price range), they're spaced out
evenly around their prices, highest at the top. Each moved label gets a short
leader in its own line's color, style and width, and its line stops just short
so the leader can join it. Labels with room stay on their lines. This is the same
method as Key Levels.

Because "too close" is measured against the visible range, the layout is
recalculated whenever you zoom or scroll, so labels shift as the view changes.
Turning **Spread Overlapping Labels** off puts every label back level with its
line.

What works and what doesn't, tested on MNQ across 15s to 1h with NWOG Previous 5
and NDOG/RTH Previous 2:

| Setup | 15s–5m | 10m–15m | 30m–1h |
|---|---|---|---|
| Spread alone (all percent labels) | clean | busy, readable | a column of labels running off the screen |
| + Max Label Move 3 | clean | a few overlapping clumps | many overlapping clumps |
| + Skip Spread When Crowded | clean | same as spread alone | no spreading, overlaps everywhere |
| **+ Previous gaps label High/Low only (default)** | **clean** | **clean** | **clean** |

Labelling only High/Low on previous gaps takes them from five labels to two: 33 labels instead of
60 for that setup, with labels moving at most about 3 label heights. With every
count at its maximum (45 labels) 1h is still readable, but the upper band is
dense. Max Label Move and Skip Spread When Crowded stay as options, off by
default.

## Holidays

Futures trade on some days with no regular session: Labor Day, Presidents Day and
similar. Globex still prints bars at the open on those mornings, and halts early
(13:00 for equities, 14:30 for metals and energy).

CME books a holiday's trading under the next trading date, so a day whose open
belongs to a later trading date is a holiday: no RTH gap is drawn for it and its
trades are ignored. The next day's RTH gap runs from the last real session's
close. Early-close days (e.g. the day after Thanksgiving) are their own trading
date, so they still count. This is known at the open, so a holiday gap never
appears.

NWOG and NDOG need no special handling: the prior close is simply the last price
before the reopen, so a holiday NDOG runs from the last trade before the early
halt (e.g. 12:59 for equities) to 18:00.

## Timeframes

| Timeframe (Equity Index) | NWOG / NDOG | RTH | RTH source |
|---|---|---|---|
| Divides 15 minutes (15s, 30s, 1, 3, 5, 15m) | yes | yes | chart's own bars |
| 2m | yes | yes | 1m |
| 5m–14m (e.g. 10m) | yes | yes | 5m |
| 30m and above, intraday | yes | yes | 15m |
| Daily and above | hidden | hidden | — |

All three gaps draw on every intraday timeframe. The RTH open and close fall
inside a bar on sizes that don't land on both, so those read the gap from the
largest timeframe at or below the chart's own that does, through
`request.security`. Those bars have already closed inside the chart bar, so the
gap appears with the chart bar holding the open and nothing is drawn late.
Prices are identical to the chart-native path. Which sizes land on both depends
on the market: 15 minutes for 09:30/16:15, 30 for Crude's 09:00/14:30, 10 for
Gold's 08:20/13:30 and Treasuries' 08:20/15:00, 5 for Silver's 08:25/13:25.

Lines start on the chart bar that holds their time: on 1h, the RTH open's line
starts on the 09:00 candle that contains 09:30.

NWOG and NDOG always come from the chart's own bars, since the 17:00 halt ends
the last bar early whatever the bar size.

## Gaps older than the chart's history

A chart only loads a limited number of bars. On MNQ 1m that was about 25,800 bars,
reaching back only to Sun 09/20, enough for 3 NWOGs out of 6. Short charts have
even less: 15s reached about a day.

A `request.security` context runs over its own, longer history, so the indicator
scans for gaps there and fills in any the chart's own bars could not reach:

| Gap | Backfill source | Why that timeframe |
|---|---|---|
| NWOG, NDOG | 60m | The 16:00 bar closes at the 17:00 halt on the 16:59 trade; the 18:00 bar opens on the 18:00 trade |
| RTH | up to 30m, landing on the open and close (15m for Equity Index) | The 16:00 bar closes at 16:15 on the 16:14 trade; the 09:30 bar opens on the 09:30 trade |

These are the same trades the chart would use, not approximations. The fill
happens once, on the last historical bar. Gaps the chart found itself are matched
by open time and drawn once. Gaps that form after that, including the current
one, still come from the chart's own bars, so nothing appears late. Older gaps
whose start lies before the chart's first bar draw from the left edge, which
makes no difference for a horizontal level.

## Validation

**v1.9.1, 2026-10-21.** Fixes labels drawn on top of each other on ZB. A label
counted as "not moved" when its spread slot was within half a tick of its price,
and stayed on its price while its neighbours were spaced around it. On ZB (tick
1/32) half a tick was about one label height, so "NWOG 25% 10/18" and "NDOG 50%
10/20" both sat at 105.703125. "Moved" is now judged against 1% of a label
height. Tested on defaults and with every Previous at 5, on ZB 1m, 5m, 15m and
1h, ZN 15m, MNQ 1m and 1h, GC 10m and CL 30m: no two labels closer than one label
spacing, and every moved label has exactly one leader from its line.

**v1.9.0, 2026-10-21, tested on a separate copy of the script.** RTH gap per
market, Close Price, and a new holiday rule. Every check reads the drawn objects
and compares them with values computed separately from each market's own 5m
bars and daily closes, with history loaded back to late August.
- **ES/NQ unchanged:** on MES and MNQ at 1m, 3m, 10m and 1h, all 63 shared
  labels (RTH, NWOG, NDOG) had the same prices as 1.8.3. Start points were
  identical on 1m, 3m and 10m. On 1h the RTH lines now start on the candle
  holding 09:30 (High/Low lines not touching their price: 3 → 0).
- **Every market:** 38 contracts, continuous and dated: all five Treasuries,
  CL/MCL/QM, NG/MNG/QG, GC/MGC/QO/1OZ, SI/SIL/QI/SIC, HG/MHG/QC, ES/NQ/YM/MES/MNQ/
  MYM/M2K. 220 of the 6 most recent gaps matched; the misses were QI and QC
  (fixed below, then 34 of 34).
- **Timeframes:** MNQ, MES, MGC, CL, SI, HG, ZB and NG on 15s, 30s, 1, 2, 3, 4,
  10, 15, 30 minutes, 1h and 4h: every RTH level identical to 5m, and every High/
  Low line starts on a candle that traded its price.
- **Labor Day (2026-09-07), replayed** on GC, CL, NG, SI, HG, ZB, MNQ and ES: no
  gap on the holiday morning, and Tuesday's gap ran from Friday. Friday's
  settlements matched the daily closes exactly (e.g. GC 4476.6, CL 91.48, ZB
  108.6875).
- **Settings:** Close Price Last Trade on GC, Settlement on MNQ, Market Equity
  Index on GC, Treasuries on MNQ, Custom 10:00-15:00 on CL with Last Trade and
  with Settlement: 36 of 36 gaps correct.
- **Other symbols** (Auto = Equity Index, Last Trade): SPY, QQQ, SPX, AAPL,
  EURUSD, USDJPY, DXY, BTCUSDT.P, XAUUSD: 54 of 54 correct.
- **Found and fixed during testing:** weekend trades on 1OZ/SIC were counted
  (Monday's gap ran from Sunday 13:15); 1OZ/SIC's daily close is not the
  settlement (now Last Trade); lines started one candle late wherever a time fell
  inside a bar (also in 1.8.3); QI/QC had no gap on days without a trade at the
  open.

**v1.8.3, 2026-10-15, MNQZ2026.** Fixes changed colors not showing on the chart.
- **Cause:** a drawing colored straight from a color setting is drawn from a
  TradingView palette slot, and the chart's copy of those slots could keep the
  old colors. After resetting to defaults with an immediate timeframe switch,
  1.8.2's settings and TradingView's computed palette both held the defaults,
  but 11 of 12 slots still drew the previous colors.
- **Fix:** every line and text color is rebuilt at runtime with
  `color.new(c, color.t(c))`. That gives the same color and transparency, but
  each drawing now carries its own color instead of a slot number. On the chart,
  1.8.3 has 0 palette slots (1.8.2 had 13).
- **Tests:**
  - All 12 colors were changed with Previous on, and the timeframe was switched
    at the same instant.
  - On all 11 timeframes from 15s to 1h, every line and label then showed its
    new color, with none wrong or missing.
  - Screenshots on every timeframe showed all 12 colors.
  - Resetting to defaults with an instant switch to 1m put every drawing back on
    its default color.
- **1.8.2**, which built the settings every bar instead of once, did not fix
  this. It is kept, since it does no harm.

**v1.8.1, 2026-10-15, MNQZ2026.** Tested during the 17:00 halt (last bar 16:59),
where 1.8.0 collapsed everything to the first future bar slot: labels at 301 and
leaders 301 → 301, so the leaders were vertical. In 1.8.1, on all 11 timeframes
from 15s to 1h with Previous on at 2:
- Every label sits exactly 20 bars past the last bar.
- Every leader spans the last 3 of those bars (e.g. 317 → 320).
- Every line reaches the current bar and continues to its label.
- All 45 levels are at their correct prices.
- Screenshots on every timeframe show angled leaders and full-length lines.

**v1.8.0, 2026-10-15, MNQZ2026.** Label High/Low Only (previous blocks) was
replaced by a Label checkbox on every Mid Line and Quarters row.
- Pure defaults: all 11 timeframes from 15s to 1h draw 15 lines and 15 labels.
- Previous on at 2: all 11 timeframes draw all 45 lines at their exact prices.
  Current gaps label 25/50/75%, and previous gaps draw those lines with no
  text. Checked by screenshot too.
- On 1h, each of the 12 Label switches flipped on its own changed only its
  own labels and never removed a line.

**v1.7.0, 2026-10-15, MNQZ2026.**
- On defaults, all 11 timeframes from 15s to 1h draw every High/Low label bold
  and every 25/50/75% label plain.
- Prices, leaders and spacing are correct, and each timeframe was checked by
  screenshot too.
- On 1h, with Previous on and Label High/Low Only off, each of the 18 Bold
  switches was flipped on its own; each changed only its own labels and left
  the other 17 groups unchanged.

**v1.6.0, 2026-10-15, MNQZ2026.** On defaults, with every Previous count at 5:
- 15s, 30s, 1, 2, 3, 4, 5, 10, 15 and 30 minutes and 1h each draw all 45 labels
  in bold.
- Every label is at its raw-1m-verified price, every moved label has its own
  leader, and nothing touches.
- Screenshots checked on every timeframe; on 30s and 10m the gray labels were
  compared zoomed in with and without bold.
- Turning off one block's Bold on 1h made only that block's labels plain
  (NWOG Previous: 10 labels; RTH Current: 5).

**v1.5.0, 2026-10-15, MNQZ2026.** On defaults, 15s, 30s, 1, 2, 3, 4, 5, 10, 15
and 30 minutes and 1h each show 15 labels at the raw-1m-verified prices, with
every moved label joined to its own line by exactly one leader and no labels
touching. Checked by reading the drawn objects and by screenshot on each
timeframe. Each Previous block's Label High/Low Only was switched off on its own
on 1h: only that type's previous gaps gained percent labels (NWOG 33 → 48 labels,
NDOG and RTH 33 → 39). Turning spreading off puts all labels back on their
lines.

**v1.4.0, 2026-10-15, MNQZ2026.** With Previous at 5 for all three types, the 5m
chart, where every gap comes from the chart's own bars, gave the reference set:
6 NWOG, 6 NDOG and 6 RTH (90 labels). Every label on 15s, 30s, 1, 2, 3, 4, 10, 15
and 30 minutes and 1h matched it exactly: same gaps, same prices, no duplicates,
nothing missing. Before backfill, 1m showed 3 NWOGs and 15s showed 0 NWOG, 2 NDOG
and 3 RTH. A standalone probe had already confirmed the six 60m-sourced NWOGs
against the 1m-derived values to the tick.

**v1.0.0, 2026-09-15, MNQZ2026:**

- **Levels** — every High, Low, mid and quarter level for all three gaps matched
  the raw bars on 1m, and was identical on 3m, 5m and 15m. NWOG and NDOG were
  also identical on 2m, 1h and 4h.
- **Show Last 10** — exactly 10 gaps of each type (150 labels); each gap from
  09/10 onward checked against the bars, and no NDOG was created on a Friday.
- **Settings** — toggles, line style and width, text size and position, and
  Extend Right (0 and 30) all verified by reading back the drawn objects.
- **Line anchors** — High/Low start at the bar that set each price, mid and
  quarters at the open.
- **Bar Replay** — stepping bar by bar, the RTH gap appeared at the 09:30 bar and
  NDOG at 18:00, each replacing the previous one.
- **Holiday** — replaying Labor Day 2026-09-07, the holiday RTH gap appeared at
  09:30, was removed at 18:00 with Friday's gap restored, and Tuesday's gap ran
  from Friday's close (29836.75 → 29944).

## Known limits

- Instruments whose bars don't start at 18:00 New York (for example crypto on 4h)
  may place NWOG/NDOG on the wrong bar. 1OZ and SIC trade around the clock with
  no 17:00 halt or weekend close, so their NWOG/NDOG don't mean anything.
- With Settlement on a 15s chart, a settlement price last traded more than 5,000
  bars back (a long holiday weekend) can't be searched for, and its line starts
  on the last-trade candle instead.
- The Treasury open (08:20) comes from CME's interest-rate floor hours, not a
  Treasury-specific page.
- Very thin contracts (QI, QC) have holes in their bars, so a line can start on
  the last candle that traded before its time.
- 1OZ on 15s shows only the 4 most recent RTH gaps (5m shows 6).
- Spread labels move as you zoom or scroll, since the spacing is measured
  against the visible range.
- With every Previous count at 5 and the previous gaps' Mid Line and Quarters
  labels turned on, 30m and 1h have more labels than the screen can hold. Leave
  them off (the default) for those timeframes.
