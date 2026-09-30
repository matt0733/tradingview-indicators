# Sessions

Draws a box over each trading session and marks its high and low with level
lines that extend to the right until price takes them.

## Sessions

Five sessions, each with its own on/off, color, start and end time (15-minute
steps), box label, and high/low tags.

| Session | Default time (New York) | Color |
|---|---|---|
| Asia | 18:00–01:00 | Orange |
| London | 01:00–07:00 | Blue |
| NY AM | 07:00–11:30 | Green |
| Lunch | 11:30–13:30 | Grey |
| NY PM | 13:30–16:00 | Purple |

## Level lines

Each session's high and low are drawn as horizontal lines, labelled with the
session's tag (for example `NY AM High`), and optionally the price and date.

- A level is **taken** when price trades through it. Its line stops at that bar.
- **Remove after taken** (on by default) deletes the line and label once taken.
  **Extend after taken** keeps it for that many more candles first.
- **Labels past line end** puts each untaken label one bar past the right end of
  its line instead of in the position set per session. Once a level is taken,
  its label moves to the middle of the line, above a high and below a low, so it
  does not run over later candles.

## Settings

| Setting | Default | Notes |
|---|---|---|
| Timezone | America/New_York | Used for session times |
| Show Previous Sessions | 20 | 0–20 prior sessions kept on the chart |
| Box Border Style / Width | None, 1 | |
| H-Line Extension (bars) | 5 | How far untaken level lines reach past the current bar |
| Show Session Midpoint | off | Line at (High + Low) / 2 inside each box; Dashed, 1 |
| Show Box Labels | on | Top Left, black, small |
| Show H/L Lines | on | Per session: on/off, label position (Left, Center, Right; default Right), color |
| Extend after taken (Candles) | 0 | |
| Remove after taken | on | |
| Show Price in Label | on | |
| Show Date in Label | on | |
| Line Style / Thickness | Solid, 1 | |
| Label Size | small | |
| Labels past line end | off | |

## Changes

| Version | Change |
|---|---|
| 1.9.0 | Taken levels: labels past line end sit mid-line, above highs and below lows |
| 1.8.0 | Labels past line end option |
| 1.7.1 | Level line labels default to Right |
| 1.7.0 | Optional session midpoint line |
| 1.6.0 | Remove after taken option |
| 1.5.0 | Previous sessions max and default 20; "NY Lunch" renamed "Lunch" |
| 1.4.0 | Preferred default settings |
| 1.3.0 | MM/DD date on level labels; version shown in the title |
