# Matt Cloud

A trend cloud built from three moving averages, drawn on the price chart.

| Line | Calculation | Default |
|---|---|---|
| Short (`MC Short`) | EMA of the source | 4 |
| Lead (`MC Lead`) | EMA of the source | 20 |
| K (`MC K`) | SMA of the source | 20 |

## How to read it

- **Main cloud** (`MC Cloud`), between Short and Lead: bullish (teal) while
  Short is above Lead, bearish (red) otherwise.
- **K-Lead cloud** (`MC K Lead Cloud`), between K and Lead: uses the same
  bullish/bearish state as the main cloud, drawn more solid.
- **Line colors**: each line is green while it is rising and red while it is
  falling. K counts as rising when it is flat.

## Settings

| Setting | Default | Notes |
|---|---|---|
| Short EMA length | 4 | |
| Lead EMA length | 20 | |
| K SMA length | 20 | |
| Source | close | |
| Line up / Line down | Green / red, 50% transparent | Line colors by slope |
| Cloud bull / Cloud bear | Teal / red, 80% transparent | Main cloud |
| K-Lead cloud bull / bear | Teal / red, 50% transparent | K-Lead cloud |

## Alerts

| Alert | Fires when | Message |
|---|---|---|
| Cloud flip bull | Short crosses above Lead | Matt Cloud turned bullish |
| Cloud flip bear | Short crosses below Lead | Matt Cloud turned bearish |

Create them from TradingView's alert dialog by choosing Matt Cloud as the
condition.

## Changes

- 1.0.1: Plot, cloud and variable names use the MC prefix instead of BT;
  alert messages read "Matt Cloud turned bullish/bearish".
- 1.0.0: First release. Renamed from BT Cloud Clone; short title Matt Cloud.
