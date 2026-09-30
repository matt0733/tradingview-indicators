# Macros

Marks the ICT hourly "macro" windows: the 20 minutes from :50 past each hour to
:10 past the next. Each window gets a shaded box, plus a line at the price of the
window's first bar labelled **M.O.** (macro open).

## Windows

24 windows, each with its own on/off, grouped by session. All on by default.

| Group | Windows (New York) |
|---|---|
| Asia | 16:50–17:10 through 00:50–01:10 (9) |
| London | 01:50–02:10 through 05:50–06:10 (5) |
| New York AM | 06:50–07:10 through 10:50–11:10 (5) |
| New York PM | 11:50–12:10 through 15:50–16:10 (5) |

## Box placement

| Mode | Where the box sits |
|---|---|
| **Envelope** (default) | Wraps the window's full high-to-low range |
| Below | Under the candles, Box Offset ticks below the low |
| Above | Over the candles, Box Offset ticks above the high |
| Auto | Below when price is in the lower half of the day's range, above when it's in the upper half |

In Below, Above and Auto modes, a box slides further away if a later bar in the
window would run into it. Box Offset and Box Height are ignored in Envelope mode.

## Settings

| Setting | Default | Notes |
|---|---|---|
| Timezone | America/New_York | |
| Show Previous Days | 3 | 0–20 prior days of macros kept on the chart |
| Box Placement | Envelope | See above |
| Box Offset (ticks from candles) | 40 | |
| Box Height (ticks) | 40 | |
| Box Label Position | Top Left | Where the window's title sits in the box |
| Box Color | Orange, 80% transparent | |
| Box Text | Black, 8 px, bold | |
| M.O. Ray | Black, width 1, Solid | |
| M.O. Label | "M.O.", black, 12 px | |

## Known limits

TradingView caps each drawing type at 500 per script. With all 24 windows on and
20 days back, the oldest macros may be dropped automatically.

## Changes

| Version | Change |
|---|---|
| 1.2.0 | Show Previous Days extended to 20 |
| 1.1.0 | Envelope mode, box label position, text sizes in pixels |
| 1.0 | Initial release |
