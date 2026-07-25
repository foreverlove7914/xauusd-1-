# XAUT Signal Suite (TradingView Pine Script)

Indicator for **XAUT/USDT perpetual contract** (Tether Gold, tradable on Binance, Bitget, Gate, MEXC, BingX, etc.) built for TradingView's Pine Script v5.

## Features

- **Buy / Sell signals** — based on fast/slow moving average crossover (EMA or SMA, both configurable)
- **Reentry Buy / Reentry Sell** — flags continuation signals on the same trend, separate from the first entry
- **ATR-based Take Profit / Stop Loss** — auto-adjusts to current volatility instead of fixed price offsets
- **Support & Resistance detection** — pivot-based, auto-classified as:
  - **Strong** (solid line) — touched ≥3 times (configurable)
  - **Weak** (dotted, faded line) — touched only 1–2 times
- Built-in `alertcondition()` for all four signal types, so you can set TradingView alerts (app/email/webhook)

## Installation

1. Open TradingView, load the `XAUTUSDT.P` chart (or `XAUTUSDT` depending on exchange)
2. Open **Pine Editor** (bottom panel) → **Open** → paste the contents of `xaut_signal_suite.pine`
3. Click **Add to Chart**
4. (Optional) Click the alert (bell) icon → choose one of the four alert conditions to get notified live

## Inputs

| Group | Input | Default | Description |
|---|---|---|---|
| MA Crossover | Fast MA length | 9 | Fast moving average period |
| MA Crossover | Slow MA length | 21 | Slow moving average period |
| MA Crossover | MA type | EMA | SMA or EMA |
| ATR SL/TP | ATR length | 14 | Lookback for ATR |
| ATR SL/TP | Stop loss multiplier | 1.5 | SL distance = ATR × multiplier |
| ATR SL/TP | Take profit multiplier | 3.0 | TP distance = ATR × multiplier |
| Support/Resistance | Pivot left/right bars | 10 | Sensitivity of pivot detection |
| Support/Resistance | Touch tolerance (%) | 0.3 | How close a touch must be to count as the same level |
| Support/Resistance | Min touches = Strong | 3 | Threshold to mark a level "strong" |

Adjust these directly in TradingView's input panel — no code editing needed. Recommended to re-tune per timeframe (1H/4H/1D) and backtest before live use.

## Disclaimer

This script is a technical-analysis tool, not financial advice. Moving-average crossover systems lag price and can produce false signals in sideways/choppy markets. Perpetual contracts carry high leverage risk — always backtest, use proper position sizing, and never risk more than you can afford to lose.

## License

MIT
