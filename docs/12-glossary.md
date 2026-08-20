# Glossary

- **GBM** — gradient-boosted decision trees; the trend model (A entry-only,
  B snipe) that gates entries.
- **OOS / out-of-sample** — rounds the model never trained on (test split).
- **Entry snapshot** — market features at close−3min (what the system saw).
- **Trend path** — the candle path from decision to close (the suspected
  trend the model learns to read).
- **Limit-stop** — a stop whose paper fill is marked at the stop level, not a
  crash price.
- **Recovery grace** — the 3-minute window a breach must persist before a stop
  closes; recoveries keep the position.
- **Tiered stop** — 15¢ for entries ≥90¢, 8¢ below.
- **Review / analysis verdict** — every close scored against the real
  resolution (`resWin`, `heldPnl`).
- **Fast stop loop** — 1.5s polling of open positions (paper and live).
- **Advisory (Qwen)** — a logged second opinion that never blocks entries.
- **CleanStats / ledger** — the engine's settled-trade accounting.
