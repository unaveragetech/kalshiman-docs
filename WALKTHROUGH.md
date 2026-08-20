# Kalshiman — operating walkthrough

## First five minutes

1. **Start the paper watcher** (hidden):

   ```powershell
   Start-Process node -ArgumentList 'scripts/watch-headless.js' -WorkingDirectory 'C:\Users\b0052\Desktop\kalshiman' -WindowStyle Hidden
   ```

   Expected boot log (`memory/headless.log`):

   ```
   GBM trend models loaded (snipe + entry)
   headless watcher started (pid NNNN) — lock written
   reloaded strategy config from app
   dynamic reasoning layer online (qwen3:1.7b, paper-only advisory)
   ```

2. **Confirm health**:

   ```powershell
   node scripts/eval-ctl.js status
   ```

   Shows current round, GBM call, accuracy, high-confidence stats, P&L.

3. **Open the dashboard**: `node scripts/eval-dashboard.js --rounds 300` →
   http://127.0.0.1:8788 (auto-plays, evaluates only out-of-sample rounds).

## Daily loop

- **Watch** — `node scripts/eval-ctl.js watch` streams each new round.
- **Review** — every close has an `analysis` verdict in the ledger; search for
  "The stop cost us" vs "Correct stop" to audit the stop policy.
- **Measure** — `node scripts/paper-test.js --minutes 60` writes
  `memory/paper-test-1h.md`.
- **Retrain** when the dataset grows (see below).

## Restarting safely

```powershell
Get-CimInstance Win32_Process | Where-Object { $_.Name -eq 'node.exe' -and $_.CommandLine -like '*watch-headless*' } | ForEach-Object { Stop-Process -Id $_.ProcessId -Force }
Remove-Item memory\.headless.lock
Start-Process node -ArgumentList 'scripts/watch-headless.js' -WorkingDirectory 'C:\Users\b0052\Desktop\kalshiman' -WindowStyle Hidden
```

The engine persists its ledger, so open positions resume management after a
restart. Note the `gbm-warmup` guard means entries wait for the first GBM
score (~seconds).

## Retraining

```powershell
node scripts/build-master-dataset.js    # incremental refresh
node scripts/audit-leakage.js           # must print VERDICT: PASS
node scripts/train-continue.js          # 12-feature context models
node scripts/train-trend-model.js       # GBM A + B (~7 min)
python scripts/make-docs-charts.py      # refresh docs charts
```

Restart the watcher after retraining so the researcher loads the new models.

## Troubleshooting

| Symptom | Fix |
| --- | --- |
| Watcher not ticking | state `updatedAt` stale → check process + lock; restart |
| No trades, log full of `[gbm] low-conf` | normal — the gate is refusing weak setups |
| Dashboard blank | refresh; `node scripts/eval-dashboard.js`; check `/status` |
| `strategy sync failed` | expected while pool disabled; see docs/08 |
| Stop filled wider than the tier | impossible post-fix (limit-stop); check trade `exitCents` vs `entryCents - stop` |
| Models stale | `memory/model/*.json` `trainedAt`; retrain |

## Paper vs live

- Paper until: leakage audit passes, harness net-positive over consecutive
  hours, stops fill at the stop level.
- Then enable `liveMode: true` per series with proof, and watch the first live
  hours against the paper ledger.
