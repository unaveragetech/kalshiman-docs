# Kalshiman — architecture (V2 standard)

Everything in this file describes the current standard. Older docs that
contradict it are obsolete.

## 1. The idea in one paragraph

Ultra-short Kalshi markets converge hard: a market implied at 90%+ in the
final minutes resolves that way ~96–99% of the time. The system's edge is
knowing *which* setups converge, entering late with high confidence, riding
winners through noise, and capping the rare wrong call at a fixed stop level.
Three layers do this: a calibrated statistical model, a reasoning observer,
and a discipline layer. See [docs/14-reasoning.md](docs/14-reasoning.md).

## 2. Data layer

### The reliability dataset

`scripts/build-master-dataset.js` → `memory/dataset/master.jsonl`
(105,888 rounds, 16 series, ~96 MB). One JSON line per settled round:

```json
{
  "id": "KXGOLD15M|KXGOLD15M-26AUG190015-15",
  "series": "KXGOLD15M",
  "ticker": "KXGOLD15M-26AUG190015-15",
  "closeTime": "2026-08-19T04:15:00Z",
  "resolution": "no",
  "entryPoint": {
    "t": 1787088120, "implied": 27.5, "impliedOpen": 54.0,
    "deltaOpen": -26.5, "delta3": -2922.5, "spread": 1.0,
    "volRate3": 0.27, "oiDelta": 4.7, "volRange5": 24.0, "hour": 4.2
  },
  "previewCandles": [ {"t": 1787087820, "implied": 29.5, "vol": 1000} ],
  "trendPath": [ {"t": 1787088120, "implied": 27.5, "vol": 2000} ],
  "trend": { "slope": -2.0, "endDelta": -16.5, "trendCorrect": true },
  "fills": [ { "side": "yes", "priceCents": 71, "count": 1, "feeCents": 0.63 } ]
}
```

Sources: cached sessions, archived 1-minute candles, and the account's full
`/portfolio/fills` history (644 fills). `memory/dataset/reliability.json`
holds per-series calibration, trend accuracy (74.4% at close-3min over 85k
rounds), and the account overlay (611 matched trades, 61% win, −$11.83 net,
$14.56 fees — the account's real history before the rework).

### Leakage audit

`scripts/audit-leakage.js` — **PASS**:

- Entry/preview candle timestamps ≤ decision time (0 violations).
- Trend features only from the first 2 minutes after decision (near-close
  candles excluded by the guard; 56 such rows nulled).
- Prev-outcome features only from strictly earlier rounds (48,109 same-
  closeTime parallel-strike outcomes excluded).
- Chronological train/test split (oldest 80% / newest 20%, cutoff
  2026-08-10T16:15Z).
- Feature correlation train-vs-test: no leakage signature.

## 3. Models

### 12-feature logistic (per series)

`lib/predictor.js` — logistic regression with z-scored features:
`implied, impliedOpen, deltaOpen, delta3, spread, volRate3, oiDelta,
volRange5, hour, prev1, prev2, prev3`. Trained by `scripts/train-continue.js`
(walk-forward: gold 82%, silver 87%, WTI 90%). Used as research/context, not
the entry decision.

### GBM trend model (the decision layer)

`lib/trend-model.js` — pure-JS gradient-boosted decision trees:

- 200 trees, depth 3, lr 0.08, subsample 0.6, histogram splits (32 bins),
  early stopping on a validation slice.
- A — entry-only, 18 features: **93.1%** OOS.
- B — snipe, 20 features (adds trend1/trend2): **96.7%** OOS.
- B high-confidence (≥90% / ≤10%): **98.4%** win rate, 4,140 OOS trades,
  +$188.24 simulated.
- Serialized to `memory/dataset/trend-model.json` + `trend-model-a.json`.

### Live scoring

`lib/researcher.js` pulls each live session's 1-minute candles and exposes:

```json
{
  "ticker": "KXGOLD15M-...", "series": "KXGOLD15M",
  "modelProbUp": 68.7, "gbmProbUp": 73.8, "gbmSnipeProbUp": 79.0,
  "marketUp": 71.7, "research": "confirm"
}
```

### Qwen observer (advisory only)

`lib/trader-llm.js` — Qwen3-1.7B via Ollama. Reads the snapshot, replies
`{"side":"YES|NO|PASS","confidence":0-100,"reason":"..."}`, is logged and
scored, and **never blocks** an entry.

> Full granular system diagram (Mermaid, renders on GitHub):
> [docs/17-system-diagram.md](docs/17-system-diagram.md).

## 4. Engine pipeline

`lib/strategies.js` + `scripts/watch-headless.js` (5s tick + 1.5s fast stops):

```
discover sessions (enabled lanes)
  -> researcher prediction (logistic + GBM entry + GBM snipe)
  -> gates: signal / price / timing / liquidity+EV
  -> GBM gate (paper): require >=85% or <=15% AND agreement
       vetoes: gbm-low-conf | gbm-disagree | gbm-pending | gbm-warmup
  -> open paper trade
  -> manageTrades (every tick + 1.5s fast stop loop):
       bank/bow-out at 99-99.5 on winners
       tiered stop: entry >=90c -> 15c; entry <90c -> 8c
       3-minute recovery grace (stopBreachAt) before a stop closes
       limit-stop fill: exit marked at the stop LEVEL
  -> close + review: analysis{result,resWin,heldPnl,verdict}
```

### Enabled lanes (paper)

| Lane | Series | Timing | Notes |
| --- | --- | --- | --- |
| gold15-late | KXGOLD15M | late | GBM-gated |
| silver15-late | KXSILVER15M | late | GBM-gated |
| universal-snipe | any eligible | 0.5–2.5m | GBM-gated where covered |

`liveMode: false`. Disabled: hourly/daily/crypto lanes, perps, weather,
btc-dip, both-sides arbs.

## 5. Observability

### Eval dashboard + control plane

`scripts/eval-dashboard.js` (http://127.0.0.1:8788). Server owns evaluation
state; browser + CLI stay in sync; **out-of-sample rounds only**.

| Endpoint | Purpose |
| --- | --- |
| `/status` | current round + cumulative stats |
| `/ctl?action=play|pause|step|restart|speed=<ms>|goto=<n>` | control |
| `/state?from&to` | raw records |
| `/selftest?n=` | stability check |
| `/leakaudit` | quick leak check |
| `/meta` | dataset/model info |

### CLI

`node scripts/eval-ctl.js status|play|pause|step|restart|speed|goto|round|selftest|watch`

### Paper-test harness

`scripts/paper-test.js --minutes N` → `memory/paper-test-1h.md` / `.json`
(trades, W/L, net, avg, accuracy, errors, growth, max drawdown).

## 6. Network pool

Disabled for the paper test. The live pool's KV quota was exhausted by
per-request read fan-out + 60s polling (see [docs/08-network.md](docs/08-network.md)).
Client fixed (5-min polls, dedupe); worker cache staged, undeployed.

## 7. Reference — files and scripts

```
lib/predictor.js           logistic models (12 features incl. series history)
lib/trend-model.js         pure-JS GBM (train + predict + serialize)
lib/researcher.js          live scoring: logistic + GBM entry + GBM snipe
lib/strategies.js          engine: gates, GBM gate, tiered stops, grace, reviews
lib/trader-llm.js          Qwen3-1.7B advisory (never blocks)
lib/kalshi-api.js          Kalshi REST client
scripts/watch-headless.js  paper watcher (5s tick + 1.5s fast stops)
scripts/eval-dashboard.js  server-owned eval UI + control plane
scripts/eval-ctl.js        CLI controller
scripts/paper-test.js      1-hour paper harness
scripts/build-master-dataset.js  dataset builder
scripts/audit-leakage.js   leakage audit (PASS)
scripts/train-continue.js  12-feature context retrain
scripts/train-trend-model.js GBM retrain
scripts/make-docs-charts.py    chart generator
scripts/build-docs-pdf.py      PDF builder
memory/dataset/           master.jsonl, models, reports, audit
memory/headless-state.json    live engine state
```

## 8. Troubleshooting quick map

| Symptom | Look at |
| --- | --- |
| No trades | `headless.log` for `[gbm] low-conf` vetoes; researcher predictions for `gbmProbUp` |
| Stops wider than expected | trade record `stopLossCents`, entry vs tier (≥90¢ → 15¢) |
| Dashboard blank | refresh; `/status`; `node scripts/eval-dashboard.js` |
| Pool errors | `network.enabled` (false), KV quota (docs/08) |
| Models stale | rerun train scripts; verify `trainedAt` in `memory/model/*.json` |
