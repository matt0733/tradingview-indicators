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

## Settings

| Setting | Default | Notes |
|---|---|---|
| Extend Right (bars) | 20 | How far past the current bar lines and labels reach |
| *Gap* ▸ on/off | on | One checkbox per gap type |
| Show Last | 1 | 1–10 most recent gaps of that type |
| Line | NWOG `#673AB7` Solid 2 · NDOG `#2962FF` Solid 2 · RTH `#FF9800` Solid 1 | Color, style, width for High/Low |
| Text | Small, Middle | Color, size (Tiny–Large), position (Top, Middle, Bottom) |
| Mid Line | on, Dotted 1 | Uses the gap's line color |
| Quarters | on, Dotted 1 | 25% and 75%; uses the gap's line color |

Text position is relative to the line: Top sits above it, Middle level with it,
Bottom below it. Inputs are hidden from the chart status line.

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

| Timeframe | NWOG / NDOG | RTH |
|---|---|---|
| 1s–15m that divide 15 minutes (1, 3, 5, 15) | yes | yes |
| Other intraday (2, 10, 30m, 1h, 4h) | yes | hidden |
| Daily and above | hidden | hidden |

The RTH gap is limited to timeframes that divide evenly into 15 minutes because
any other bar size has a bar spanning 16:15, whose close is not the 16:14 close.
NWOG and NDOG work on every intraday timeframe because the 17:00 halt ends the
last bar early regardless of bar size.

## Validation

On MNQZ2026 (CME_MINI), 2026-09-15:

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
- Labels for small gaps overlap, since all five levels sit within a few points.
