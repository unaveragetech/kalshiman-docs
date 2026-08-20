# Charts — index and regeneration

All charts live in `docs/charts/` and are generated with **plotnine**
(ggplot for Python) from measured data:
`.venv\Scripts\python.exe scripts/make-docs-charts.py` (venv created via
`python -m venv .venv && .venv\Scripts\pip install plotnine`).

| Chart | Shows |
| --- | --- |
| [rework-progression.png](charts/rework-progression.png) | Hourly paper P&L and avg/trade across V0 → V2 |
| [equity-rework.png](charts/equity-rework.png) | Cumulative paper P&L across the rework hours |
| [version-comparison.png](charts/version-comparison.png) | V0 vs V1 vs V2: avg/trade, hourly P&L, losses/hour |
| [model-accuracy.png](charts/model-accuracy.png) | GBM A / B / high-confidence out-of-sample accuracy |
| [calibration.png](charts/calibration.png) | Predicted vs actual resolution rate by decile (test set) |
| [stop-review.png](charts/stop-review.png) | The 15-stop review: recovered vs genuinely wrong |
| [returns-scenarios.png](charts/returns-scenarios.png) | 10¢/trade returns across throughput levels |
| [compounding-rice.png](charts/compounding-rice.png) | Doubling vs linear over 30 days |

To refresh after retraining or a new measured hour, run:

```powershell
python scripts/make-docs-charts.py
```

The PDF (`docs/kalshiman-full-breakdown.pdf`) is rebuilt by
`python scripts/build-docs-pdf.py` and embeds the current live state plus
these charts.
