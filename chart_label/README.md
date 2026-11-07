# Chart Label

A small three-line table pinned to the chart, useful for screenshots and trade
journals.

| Line | Shows | Example |
|---|---|---|
| 1 | Ticker | `MNQ1!` |
| 2 | Timeframe | `15m` |
| 3 | Day and date of the last bar, in the symbol's exchange timezone | `FRI 11/06/26` |

Timeframes read `30s`, `5m`, `1h`, `1d`, `1w` or `1Mo`.

## Settings

| Setting | Default | Notes |
|---|---|---|
| Position | Lower Center | Upper or Lower; Left, Center or Right |
| Size | Medium | Small, Medium or Large |
| Background | White | |
| Line 1 (Ticker) | Red, Bold on | Each line has its own color and Bold switch |
| Line 2 (Timeframe) | Black, Bold on | |
| Line 3 (Day / Date) | Black, Bold on | |
| Show Border | on | Black, thickness 2 (1-5) |

## Changes

- 1.5: Bold on by default for all three lines (from the user's chart).
- 1.4: Bold switch for each line.
- 1.3: Ticker, timeframe and day/date on their own lines; trading mode and
  free-text lines removed.
- 1.2: First release (ticker/timeframe, day/date, trading mode, free text).
