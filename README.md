# TradingView Indicators

Pine Script v6 indicators for TradingView, built for intraday index futures
(MNQ/NQ) and ICT-style trading. All times are New York unless noted.

| Indicator | Version | What it does |
|---|---|---|
| [SMT Divergence](smt_divergence/) | 2.0.0 | Marks SMT divergence between the chart and one or two comparison markets (ES, YM by default) |
| [Key Levels](key_levels/) | 1.7.0 | Previous monthly, weekly and daily highs and lows, plus monthly and weekly opens and monthly closes, labelled by period |
| [Opening Gaps](opening_gaps/) | 1.1.0 | New Week, New Day and RTH opening gaps with mid and quarter levels |
| [Sessions](sessions/) | 1.10.0 | Asia, London, NY AM, Lunch and NY PM session boxes with high/low level lines |
| [Macros](macros/) | 1.2.0 | Boxes over the hourly :50–:10 macro windows with a line at each macro's open |
| [Chart Label](chart_label/) | 1.2 | A small table showing ticker, timeframe, date and trading mode, for screenshots |

Each folder has a README covering its settings and behaviour.

## Installing an indicator

1. Open the indicator's `.pine` file and copy its contents.
2. In TradingView, open the **Pine Editor** and choose **Open → New indicator**.
3. Select everything in the editor, paste, and **Save**.
4. Click **Add to chart**.

To update to a newer version, paste the new file over the saved script and save
again. Settings on a chart are kept by position, so if a new version adds or
reorders inputs, remove the indicator from the chart and add it again.

## Data

SMT Divergence reads other markets (ES and YM by default). If you don't have
real-time data for a comparison market, TradingView delays it, and live SMTs
against that market will be late or missing.

## License

[CC BY-NC 4.0](LICENSE): free to use, share and adapt for non-commercial
purposes, with credit to matt0733. Commercial use needs permission.
