# XAUT/USDT Perpetual — Live Signal Terminal

Dashboard real-time untuk **XAUT/USDT Perpetual Contract** (Binance Futures) dengan analisa multi-timeframe (M15, M30, H1, H4, Daily) — entry, re-entry, take profit, stop loss, dan pengesanan sideway, dipaparkan dalam chart gaya TradingView.

**Live demo:** buka `index.html` terus di browser, atau host guna GitHub Pages (lihat bawah).

## Ciri-ciri

- Harga live + perubahan 24 jam dari Binance Futures public API (tiada API key diperlukan).
- Chart candlestick interaktif (lightweight-charts, library rasmi TradingView) dengan overlay EMA9/21/50 — boleh tukar timeframe M15/M30/H1/H4/Daily.
- Kad signal untuk **setiap** 5 timeframe serentak: Trend (Bullish/Bearish/Sideways), Entry, Re-entry (zon pullback EMA21), TP1, TP2, Stop Loss, RSI(14), ADX(14).
- Meter "Confluence" — tunjuk berapa banyak timeframe sepakat bullish/bearish/sideways untuk bias keseluruhan.
- Semasa pasaran sideway (ADX < 20), dashboard tukar ke mod range: papar Resistance/Support 20-candle sebagai ganti entry breakout.
- Auto-refresh setiap 15 saat. Semua pengiraan berlaku dalam browser (client-side) — tiada data dihantar ke mana-mana server.

## Logik Signal (ringkas)

| Syarat | Trend | Signal |
|---|---|---|
| ADX(14) < 20 | SIDEWAYS | WAIT — trade range antara Support/Resistance |
| EMA9 > EMA21 > EMA50, harga > EMA21, ADX ≥ 20 | BULLISH | BUY |
| EMA9 < EMA21 < EMA50, harga < EMA21, ADX ≥ 20 | BEARISH | SELL |
| Lain-lain | CAMPUR | WAIT |

- **Entry** = harga market semasa.
- **Re-entry** = zon EMA21 (untuk masuk semula selepas pullback/retrace).
- **TP1 / TP2** = Entry ± (2 × ATR14) / ± (3.5 × ATR14).
- **Stop Loss** = Entry ∓ (1.5 × ATR14).

## Cara guna di GitHub

1. Buat repo baru di GitHub (contoh: `xaut-signal-terminal`).
2. Upload/push `index.html` dan `README.md` ini ke repo tersebut.
3. Untuk host secara live (percuma): **Settings → Pages → Source: main branch → Save**. Dashboard akan live di `https://<username>.github.io/<repo-name>/`.

Push guna command line:
```bash
git init
git add index.html README.md
git commit -m "Add XAUT/USDT live signal dashboard"
git branch -M main
git remote add origin https://github.com/<username>/<repo-name>.git
git push -u origin main
```

## Penting — Disclaimer

Dashboard ini adalah **alat bantu teknikal automatik**, bukan nasihat kewangan atau jaminan keuntungan. Semua signal dijana daripada formula EMA/RSI/ATR/ADX standard dan **tidak** mengambil kira berita, fundamental, atau likuiditi pasaran semasa. Sentiasa sahkan dengan money management sendiri sebelum masuk sebarang posisi sebenar.

## Sumber Data

- Binance Futures public API: `https://fapi.binance.com/fapi/v1/klines` dan `/fapi/v1/ticker/24hr`, simbol `XAUTUSDT`.
- Chart: [lightweight-charts](https://github.com/tradingview/lightweight-charts) (open-source, oleh TradingView).
