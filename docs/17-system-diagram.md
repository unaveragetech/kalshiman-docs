# System diagram — the full flow, granular

This is the complete V2 system, from data to live loop to review. GitHub
renders the Mermaid diagram natively; the PDF includes the charts but not the
interactive diagram.

```mermaid
flowchart TD
  subgraph DATA["DATA LAYER — offline"]
    A1["Kalshi REST API"] --> A2["build-master-dataset.js"]
    A2 --> A3[("master.jsonl — 105,888 rounds<br/>entryPoint → previewCandles → trendPath → resolution")]
    A2 --> A4[("fills.json — 644 real account fills")]
    A3 --> A5["audit-leakage.js"]
    A5 --> A6[("leakage-audit.json — PASS")]
    A3 --> A7["train-continue.js"]
    A7 --> A8[("memory/model/*.json<br/>12-feature logistic per series")]
    A3 --> A9["train-trend-model.js"]
    A9 --> A10[("trend-model.json (GBM B snipe)<br/>trend-model-a.json (GBM A entry)")]
  end

  subgraph LIVE["LIVE LOOP — scripts/watch-headless.js"]
    B1["tick (5s): discoverSessions<br/>gold15-late · silver15-late · universal-snipe"] --> B2["researcher.tick — pull 1m candles"]
    B2 --> B3["score session<br/>logistic modelProbUp · GBM gbmProbUp · GBM gbmSnipeProbUp"]
    B3 --> B4{"signal gate<br/>(learned model + market implied)"}
    B4 -->|pass| B5{"price band 80–98¢"}
    B5 -->|pass| B6{"timing gate"}
    B6 -->|pass| B7{"EV + liquidity gate"}
    B7 -->|pass| B8{"GBM gate<br/>≥85% or ≤15% AND agreement"}
    B8 -->|"gbm-low-conf · gbm-disagree<br/>gbm-pending · gbm-warmup"| B9["veto — re-check next tick"]
    B8 -->|"high-confidence agree"| B10["open paper trade (ledger)"]
    B10 --> B11["manageTrades (tick) + fastStopCheck (1.5s)"]
    B11 --> B12{"winner ≥ bank 99–99.5¢?"}
    B12 -->|yes| B13["close: bank / bow-out"]
    B12 -->|no| B14{"loss ≥ tiered stop?<br/>(entry ≥90¢ → 15¢ · entry <90¢ → 8¢)"}
    B14 -->|no| B15["keep position"]
    B14 -->|yes| B16{"recovery grace 180s<br/>price recovered?"}
    B16 -->|recovered| B15
    B16 -->|still breached| B17["close at stop LEVEL — limit-stop fill"]
    B11 --> B18{"≤60s to close AND breached?"}
    B18 -->|yes| B17
    B17 --> B19["review vs actual resolution<br/>analysis{result, resWin, heldPnl, verdict}"]
    B19 --> B20[("headless-state.json<br/>ledger + reviews")]
  end

  subgraph OBS["REASONING OBSERVER — advisory"]
    C1["lib/trader-llm.js — Qwen3-1.7B via Ollama"]
    C2["reads snapshot: implied · book · trend path · series history"]
    C1 --> C2 --> C3["YES / NO / PASS + one-line reason"]
    C3 --> C4[("llm-advisor.log + llm-score.json")]
  end

  subgraph OBSV["OBSERVABILITY"]
    D1["eval-dashboard.js :8788 — server-owned state, OOS-only"] --> D2["/status · /ctl · /state · /selftest · /leakaudit"]
    D3["eval-ctl.js CLI"] --> D1
    D4["paper-test.js hour harness"] --> D5[("paper-test-1h.md + .json")]
  end

  B2 -.-> C2
  B19 -.->|"review verdicts feed retraining"| A7
  B19 -.->|"review verdicts feed retraining"| A9
  B20 -.-> D1
```

## Reading the diagram

- **Data layer** — built once, audited for leakage (PASS), then used to train
  both the per-series logistic models and the GBM trend models. Reviews from
  live closes feed back into retraining.
- **Live loop** — every 5s the watcher discovers active markets; the researcher
  scores each with logistic + GBM; gates filter; the GBM gate is the final
  paper entry authority; open trades are managed every tick AND every 1.5s by
  the fast-stop loop; exits are bank/bow-out on winners or tiered limit-stops
  with a 180s recovery grace.
- **Reasoning observer** — Qwen reads the same snapshot a trader would and
  gives a scored second opinion; it never decides.
- **Observability** — the dashboard (server-owned state, out-of-sample only),
  the CLI control plane, and the hour harness all read the same ledger.
