# Account

## Paper

The engine trades a paper ledger in `memory/headless-state.json`. Cumulative
paper results (including the old-system era): 180 settled, 113W/67L (63%),
+$720.21.

## Real

- Balance: **$23.09** (event-contract lane).
- Open event positions: 0. Open margin positions: 0.
- The watcher is paper-only (`liveMode: false`) — no real orders have been
  placed by the v2 system.

## Going live

Live mode requires explicit enablement, positive walk-forward models or a
proven paper ledger per series, and a passing leakage audit. Until then all
reported P&L is paper.
