# Kalshiman documentation

The current standard is the **V2 new system**: a reliability dataset, a
pure-JS gradient-boosted trend model, a strict high-confidence entry gate,
tiered limit-stops with a recovery grace, fast stop checks, and evaluation on
every close. Anything older that contradicts this set is obsolete.

## Index

| Doc | What it covers |
| --- | --- |
| [00-thesis](00-thesis.md) | Why the system works (the edge) |
| [01-architecture](01-architecture.md) | End-to-end design |
| [02-strategies](02-strategies.md) | Lanes, gates, entries |
| [03-models](03-models.md) | Logistic + GBM models, training, leakage audit |
| [04-markets](04-markets.md) | Markets we trade and resolution timing |
| [05-risk](05-risk.md) | Stops, grace, drawdown controls |
| [06-performance](06-performance.md) | Measured results through the rework |
| [07-account](07-account.md) | Paper/live accounts and balance |
| [08-network](08-network.md) | Signal pool: current status and KV fixes |
| [09-app](09-app.md) | Dashboard, CLI, harness |
| [10-lessons](10-lessons.md) | What the rework taught us |
| [11-faq](11-faq.md) | Questions and answers |
| [12-glossary](12-glossary.md) | Terms |
| [13-versions](13-versions.md) | System versions and intelligence over time |
| [14-reasoning](14-reasoning.md) | Why statistics alone failed, why a reasoning observer, and the just-right balance |
| [15-returns](15-returns.md) | Examples of returns and scenarios |
| [16-charts](16-charts.md) | Chart index and regeneration |
| [REFERENCE](REFERENCE.md) | Commands, files, metrics, config — the everything reference |
| [17-system-diagram](17-system-diagram.md) | Granular Mermaid diagram of the full system |
| [18-pool](18-pool.md) | The worker fleet: credits, verification, signals, and why a fleet scales |
| [19-were-not-done](19-were-not-done.md) | The engine as it stands: honest backtest, lane cut, hatch tuning |

## Headline result (current standard, full-history faithful replay)

**608 trades, 77.3% win rate, +$6.29 net, +1.03¢/trade, $1.96 max drawdown** - after the lane cut (hourly gold/silver frozen) and the 15c hatch tune. See [19-were-not-done](19-were-not-done.md).

![Rework progression](charts/rework-progression.png)

## Quick commands

```powershell
node scripts/watch-headless.js               # paper watcher
node scripts/eval-dashboard.js --rounds 300  # dashboard at :8788
node scripts/eval-ctl.js status              # CLI status
node scripts/paper-test.js --minutes 60      # hour harness
node scripts/audit-leakage.js                # leakage audit (PASS)
```
