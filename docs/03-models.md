# Models

## 12-feature logistic (per series)

- Features: implied, impliedOpen, deltaOpen, delta3, spread, volRate3,
  oiDelta, volRange5, hour, prev1-3 (series history).
- Walk-forward accuracy: gold 82%, silver 87%, WTI 90% (current).
- Train: `scripts/train-continue.js` (context-preserving; never clobbers the
  12-feature models with 9-feature configs).

## GBM trend model (the decision layer)

- Pure JS, no ML dependencies (`lib/trend-model.js`).
- A — entry-only (18 features): **93.1%** out-of-sample.
- B — snipe (20 features, + first 2 min of trend): **96.7%**.
- B high-confidence (≥90% / ≤10%): **98.4%** win rate, 4,140 OOS trades,
  +$188 simulated.
- Train: `scripts/train-trend-model.js`; artifacts in
  `memory/dataset/trend-model.json` + `trend-model-a.json`.

![Model accuracy](charts/model-accuracy.png)

![Calibration](charts/calibration.png)

## Live scoring

`lib/researcher.js` pulls each live session's candles, rebuilds the exact
training feature vector (with the trend-window guard), and publishes
`gbmProbUp` / `gbmSnipeProbUp` per market.

## Integrity

`scripts/audit-leakage.js` — PASS: no future candles, no same-closeTime prev
leaks, chronological split, no suspicious train/test correlations.

## Qwen advisor

`lib/trader-llm.js` (Qwen3-1.7B via Ollama) — advisory only; logged and
scored, never blocks entries.
