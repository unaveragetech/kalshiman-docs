# Architecture (v2)

See [ARCHITECTURE.md](../ARCHITECTURE.md) for the full design. Summary:

```
Data layer
  memory/dataset/master.jsonl       105,888 rounds (entry->trend->resolution)
  memory/dataset/reliability.json   per-series calibration + account overlay
  scripts/audit-leakage.js          PASS (no future information leaks)

Models
  lib/predictor.js                  12-feature logistic (series history)
  lib/trend-model.js                pure-JS GBM (A: entry 93.1%, B: snipe 96.7%,
                                    high-conf 98.4%)
  lib/researcher.js                 live scoring: logistic + GBM entry + snipe

Engine (lib/strategies.js + scripts/watch-headless.js)
  gates -> GBM gate (>=85% / <=15%) -> tiered stops (15c/8c)
  -> 3-min recovery grace -> 1.5s fast stop checks -> close + review

Observability
  scripts/eval-dashboard.js         server-owned eval UI (OOS only)
  scripts/eval-ctl.js               CLI control plane
  scripts/paper-test.js             hour harness -> memory/paper-test-1h.md
```

All state persists in `memory/headless-state.json`; the engine restarts
resume open positions and continue managing them.
