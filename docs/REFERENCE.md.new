# Kalshiman — reference

The single source for "how does X work / where is Y". Links go deeper.

## Commands

| Command | Purpose |
| --- | --- |
| `node scripts/watch-headless.js` | paper watcher |
| `node scripts/eval-dashboard.js --rounds N` | eval UI at :8788 |
| `node scripts/eval-ctl.js status` | CLI status |
| `node scripts/eval-ctl.js play|pause|step|restart|speed <ms>|goto <n>|round <n>|selftest [n]|watch` | control |
| `node scripts/paper-test.js --minutes 60` | hour harness |
| `node scripts/build-master-dataset.js` | rebuild dataset |
| `node scripts/audit-leakage.js` | leakage audit (PASS) |
| `node scripts/train-continue.js` | 12-feature context models |
| `node scripts/train-trend-model.js` | GBM A + B |
| `python scripts/make-docs-charts.py` | docs charts |
| `python scripts/build-docs-pdf.py` | PDF |

## Memory files

| File | Contents |
| --- | --- |
| `memory/headless-state.json` | live engine state (ledger, predictions, stats) |
| `memory/strategies.json` | strategy/lane config |
| `memory/config.json` | API creds, network, fees |
| `memory/dataset/master.jsonl` | reliability dataset |
| `memory/dataset/trend-model.json` / `trend-model-a.json` | GBM B / A |
| `memory/dataset/reliability.json` | dataset gauges |
| `memory/dataset/trend-model-report.json` | GBM OOS report |
| `memory/dataset/leakage-audit.json` | audit result |
| `memory/model/*.json` | 12-feature logistic models |
| `memory/paper-test-1h.md` / `.json` | last harness report |
| `memory/llm-advisor.log` / `llm-score.json` | Qwen advisory + score |
| `memory/headless.log` | watcher log |
| `memory/overnight-log.md` | session history |

## Metrics definitions

- **Net P&L** — sum of `pnlCents` over closed trades (fees included).
- **Avg/trade** — net P&L ÷ closed trades.
- **Prediction accuracy** — engine `predictionStats.modelRate`.
- **Max drawdown** — peak-to-trough of cumulative P&L in the window.
- **High-confidence** — GBM ≥90% or ≤10%.
- **OOS** — rounds after the test cutoff (2026-08-10T16:15Z) the model never
  trained on.

## Engine config reference (current values)

| Setting | Value |
| --- | --- |
| liveMode | false |
| GBM entry bar | ≥85% or ≤15% + agreement |
| Tiered stops | ≥90¢ entry → 15¢; <90¢ → 8¢ |
| Recovery grace | 180s |
| Final-minute close | breach at ≤60s to close closes immediately |
| Fast stop loop | 1.5s (paper + live) |
| Fee model | 0.63¢/contract measured |
| Amount per trade | $1 paper (amountUsd 3 config, capped) |
| Universal timing | 0.5–2.5m to close |
| Qwen model | qwen3:1.7b via Ollama |

## Data schema

See [01-architecture](01-architecture.md) for the master.jsonl schema and
[shared-signals](../shared-signals.md) for the live record schemas.

## Charts

See [16-charts](16-charts.md) for the chart index and regeneration.

## Returns

See [15-returns](15-returns.md) for scenario math and the worked standard
hour.

## Questions this should answer

- How does the system decide? → GBM gate (02-strategies, 03-models)
- Why did the old system fail? → 13-versions, 14-reasoning
- Why an LLM observer? → 14-reasoning
- How much can it make? → 15-returns
- Is it safe? → 05-risk
- Can I reproduce it? → this file + WALKTHROUGH
