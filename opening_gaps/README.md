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
| **RTH** | Last price before 16:15 (the 16:14 close) | 09:30 the next day | Monday–Friday |

A gap where the open equals the prior close is not drawn.

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
| Text | Small, Middle, Date on, Bold on | Color, size (Tiny–Large), position (Top, Middle, Bottom), date on/off, bold on/off |
| Mid Line | on, Dotted 1 | Uses the block's line color |
| Quarters | on, Dotted 1 | 25% and 75%; uses the block's line color |
| Label High/Low Only | on (Previous blocks only) | Previous gaps keep their 25/50/75% lines but only High and Low get a label |

Previous gaps default to solid grays, so the current gaps stand out in color:
RTH `#4A4D57` (darkest), NDOG `#6A6D78`, NWOG `#8A8D97` (lightest). Line and
text use the same shade.

Every block has its own **Bold** checkbox on the Text row, on by default. Bold
text reads much better at small sizes, especially the lighter grays, without
making the labels bigger.

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
| **+ Previous Label High/Low Only (default)** | **clean** | **clean** | **clean** |

High/Low Only takes previous gaps from five labels to two: 33 labels instead of
60 for that setup, with labels moving at most about 3 label heights. With every
count at its maximum (45 labels) 1h is still readable, but the upper band is
dense. Max Label Move and Skip Spread When Crowded stay as options, off by
default.

## Holidays

Futures trade on some days with no regular session: Labor Day, Presidents Day and
similar. Globex still prints a 09:30 bar on those mornings and halts at 13:00.

An RTH gap only stands once the day trades past 13:00. If trading halts before
then, the gap is removed at the 18:00 reopen, the gap it pushed out is redrawn,
and the next day's RTH gap runs from the last real session's 16:14 close.
Early-close days run to 13:15, so they still count.

NWOG and NDOG need no special handling: the prior close is simply the last price
before the reopen, so a holiday NDOG runs from the 12:59 close to 18:00.

## Timeframes

| Timeframe | NWOG / NDOG | RTH | RTH source |
|---|---|---|---|
| Divides 15 minutes (1, 3, 5, 15m) | yes | yes | chart's own bars |
| 2m | yes | yes | 1m |
| 5m–14m (e.g. 10m) | yes | yes | 5m |
| 30m and above, intraday | yes | yes | 15m |
| Daily and above | hidden | hidden | — |

All three gaps draw on every intraday timeframe. The RTH boundaries at 09:30 and
16:15 fall inside a bar on sizes that don't divide 15 minutes, so those read the
gap from the largest boundary-safe timeframe at or below the chart's own, through
`request.security`. Those bars have already closed inside the chart bar, so the
gap appears with the chart bar holding 09:30 and nothing is drawn late. Prices
are identical to the chart-native path, since 1m, 3m, 5m and 15m all resolve
09:30 and 16:15 exactly.

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
| RTH | 15m | The 16:00 bar closes at 16:15 on the 16:14 trade; the 09:30 bar opens on the 09:30 trade |

These are the same trades the chart would use, not approximations. The fill
happens once, on the last historical bar. Gaps the chart found itself are matched
by open time and drawn once. Gaps that form after that, including the current
one, still come from the chart's own bars, so nothing appears late. Older gaps
whose start lies before the chart's first bar draw from the left edge, which
makes no difference for a horizontal level.

## Validation

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

- On a futures holiday, the RTH gap is visible from 09:30 until the session halts
  at 13:00 — it cannot be recognised as a holiday before then.
- On stock charts, early-close days end at 13:00 and look like a futures holiday,
  so their RTH gap is dropped and the next day measures from the prior full
  session.
- Tested on MNQ only. Instruments whose bars don't start at 18:00 New York (for
  example crypto on 4h) may place NWOG/NDOG on the wrong bar.
- Spread labels move as you zoom or scroll, since the spacing is measured
  against the visible range.
- With every Previous count at 5 and High/Low Only off, 30m and 1h have more
  labels than the screen can hold. Leave High/Low Only on for those timeframes.
