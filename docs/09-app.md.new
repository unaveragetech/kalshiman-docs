# App, dashboard and tooling

## Eval dashboard

`node scripts/eval-dashboard.js --rounds 300` → http://127.0.0.1:8788

- Server owns the evaluation state; the browser and CLI stay in sync.
- Out-of-sample rounds only (`OOS ✓` badge; the model never trained on them).
- Per round: market vs logistic vs GBM calls, correctness color-coding, top
  reasoning drivers, candle path chart.
- Charts: rolling accuracy, cumulative P&L, probability histogram,
  calibration, per-series accuracy.
- Endpoints: `/status`, `/ctl`, `/state`, `/selftest`, `/leakaudit`, `/meta`.

## CLI

`node scripts/eval-ctl.js status|play|pause|step|restart|speed|goto|round|selftest|watch`

## Paper-test harness

`node scripts/paper-test.js --minutes 60` samples every 60s and writes
`memory/paper-test-1h.md` + `.json` (trades, W/L, net, avg, accuracy, errors,
growth, max drawdown).

## Trade reviews

Every close is reviewed against the actual resolution and carries an
`analysis` verdict (`resWin`, `heldPnl`, "The stop cost us…" / "Correct
stop…"), so policy changes are always data-driven.
