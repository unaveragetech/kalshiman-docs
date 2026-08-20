# Shared signals (V2) — schema reference

## Researcher prediction (per market)

```json
{
  "ticker": "KXGOLD15M-26AUG190215-15",
  "series": "KXGOLD15M",
  "title": "Gold price up in next 15 mins?",
  "countdownMin": 8.7,
  "modelDir": "up",
  "modelProbUp": 73.8,
  "modelConfidence": 47.6,
  "gbmProbUp": 73.8,
  "gbmSnipeProbUp": 79.0,
  "marketUp": 71.7,
  "marketDir": "up",
  "research": "confirm",
  "reasons": ["model up 73.8% (implied 71.7c)", "market 71.7%"],
  "at": "2026-08-19T..."
}
```

Field meanings:

| Field | Meaning |
| --- | --- |
| `modelProbUp` | 12-feature logistic probability (series history) |
| `gbmProbUp` | GBM entry-only probability — the gate's input |
| `gbmSnipeProbUp` | GBM with trend1/trend2 (guarded to close-3m..close-1m) |
| `marketUp` | book-implied YES % |
| `research` | confirm / misconfirm / neutral vs market |

## GBM gate vetoes

| Reason | Meaning |
| --- | --- |
| `gbm-low-conf` | probability between 15% and 85% |
| `gbm-disagree` | high-confidence model call opposite the gate side |
| `gbm-pending` | round still building candles; score not ready |
| `gbm-warmup` | watcher just started; no score yet |

## Trade record (ledger)

```json
{
  "id": "st-...",
  "strategyId": "universal-snipe",
  "kind": "paper",
  "ticker": "KXGOLD15M-...",
  "seriesTicker": "KXGOLD15M",
  "side": "yes",
  "entryCents": 89,
  "exitCents": 81,
  "pnlCents": -6.74,
  "won": false,
  "status": "stopped",
  "openedAt": "2026-08-19T...Z",
  "closedAt": "2026-08-19T...Z",
  "closedReason": "stop",
  "stopLossCents": 8,
  "holdToResolution": false,
  "gates": [ {"id": "signal", "pass": true, "detail": "..."} ],
  "analysis": {
    "result": "yes",
    "resWin": true,
    "heldPnl": 11,
    "diff": 14.7,
    "verdict": "The stop cost us: this side recovered and WON at resolution...",
    "reviewedAt": "2026-08-19T..."
  }
}
```

Closed reasons: `stop`, `bank`, `bow-out`, `target`, `api`, `price`
(held to resolution), `timeout`, `flip`.

## Eval dashboard record

`/state` rows add per-model calls, correctness, reasoning drivers, sim trade,
and the candle path:

```json
{
  "i": 0, "series": "KXGOLD15M", "ticker": "...", "split": "test",
  "probs": {"market": 92.9, "logistic": 68.7, "gbmA": 91.0, "gbm": 90.8},
  "calls": {"market": "yes", "logistic": "yes", "gbm": "yes"},
  "correct": {"market": true, "logistic": true, "gbm": true},
  "reasoning": {"gbm": [{"name": "implied", "value": 98.5, "delta": -36.01}]},
  "sim": {"side": "yes", "ask": 90.1, "pnlCents": 9.27, "won": true}
}
```
