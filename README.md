# XAUT/USDT Perpetual — Live Dashboard

Dashboard live real-time untuk XAUT/USDT Perpetual (Tether Gold, Binance Futures), dibina 100% static (HTML + JS tulen) — tiada backend, tiada build step. Terus boleh host di **GitHub Pages**.

## Ciri-ciri

| Ciri | Cara ia dikira |
|---|---|
| Chart candlestick live | WebSocket `kline` stream, Binance Futures |
| 24h High / Low / Volume | WebSocket `ticker` stream |
| Support & Resistance (kuat/lemah) | Pivot swing high/low + clustering. "Kuat" = ≥3 sentuhan, "Lemah" = <3 sentuhan |
| Entry Buy/Sell | Reaksi candle bullish/bearish di level S/R |
| Re-entry | Level yang sama disentuh & confirm buat kali kedua |
| Breakout | Candle *close* menembusi level S/R > 0.15×ATR berbanding candle sebelumnya — ditanda ungu (`BO`) di chart & diberi keutamaan berbanding entry/re-entry biasa |
| TP / SL | Reversal: TP = level S/R bertentangan, SL = ATR(14)×0.6 di luar level. Breakout: TP = ATR×3.2 ke arah breakout, SL = kembali ke belakang level yang pecah |
| Big trader pending order (whale wall) | Order book depth (`depth20`) — level dengan qty > mean + 1.5×std dev ditandakan sebagai "wall" |

Layout responsif — pada skrin sempit (mobile), chart akan duduk di atas (bukan tersepit tepi) dan panel sidebar disusun di bawahnya, boleh scroll.

## ⚠️ Kalau status tunjuk "Terputus" / data "--"

Punca paling biasa: fail dibuka terus dari storan tempatan (`content://...` / `file://...`) bukan melalui `http(s)://`. Sesetengah browser mobile sekat `fetch`/`WebSocket` dari fail lokal atas sebab keselamatan. Penyelesaian:
1. Jalankan `python3 -m http.server` dan buka `http://localhost:8080`, **atau**
2. Deploy terus ke GitHub Pages (langkah di bawah) — sekali live di `https://...`, ia akan connect terus.

Jika masih terputus selepas itu, kemungkinan `fapi.binance.com` / `fstream.binance.com` disekat oleh rangkaian/ISP anda — cuba guna VPN.

## ⚠️ Disclaimer penting

Semua entry/re-entry/TP/SL/S&R di sini dijana oleh **algoritma heuristik ringkas** (pivot + ATR), **bukan** sistem prediktif yang terbukti profit dan **bukan nasihat kewangan**. Ia satu alat bantu visual sahaja. Sentiasa buat analisis dan pengesahan sendiri sebelum membuat sebarang keputusan trading. Leverage/perpetual futures berisiko tinggi — anda boleh kehilangan lebih daripada modal asal.

## Jalankan secara tempatan

Tiada build step diperlukan — cuma buka fail terus, atau serve secara statik:

```bash
# guna Python
python3 -m http.server 8080
# atau guna Node
npx serve .
```

Kemudian buka `http://localhost:8080`.

> Nota: browser perlu boleh akses `fapi.binance.com` dan `fstream.binance.com`. Jika Binance disekat di lokasi/rangkaian anda, guna VPN, atau tukar `REST_BASE`/`WS_BASE` dalam `index.html` kepada exchange lain yang senaraikan XAUT perpetual (contoh Bybit) — struktur kod sama, cuma format mesej WebSocket perlu disesuaikan.

## Deploy ke GitHub Pages

```bash
git init
git add .
git commit -m "Initial commit: XAUT/USDT live dashboard"
git branch -M main
git remote add origin https://github.com/<username>/<repo>.git
git push -u origin main
```

Kemudian di GitHub:
1. Buka repo → **Settings** → **Pages**
2. Bawah **Build and deployment**, pilih **Deploy from a branch**
3. Branch: `main`, folder: `/ (root)` → **Save**
4. Tunggu ~1 minit, dashboard akan live di `https://<username>.github.io/<repo>/`

## Struktur fail

```
xau-dashboard/
├── index.html   ← semua logik (chart, WebSocket, S/R engine, signal engine) dalam satu fail
└── README.md
```

## Nak ubah suai

Semua parameter tuning ada di bahagian atas `<script>` dalam `index.html`:

```js
const SWING_LOOKBACK = 3;        // ketat/longgar pengesanan swing point
const SR_TOLERANCE_PCT = 0.0012; // toleransi cluster S/R (0.12%)
const MAX_SR_LEVELS = 6;         // berapa banyak level S/R dipaparkan
const ATR_PERIOD = 14;           // period ATR untuk SL sizing
```

Tukar `SYMBOL` (baris atas) jika nak pasangan lain yang ada di Binance Futures (contoh `btcusdt`, `xauusdt`).
