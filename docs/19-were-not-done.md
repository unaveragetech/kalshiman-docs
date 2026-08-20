# 19 - We're not done: the engine as it stands

Written after the honest backtest. The engine that shipped this week is not
the engine that started the week.

## The honest backtest

`scripts/backtest-live.js` is not a simulation of the rules - it *is* the
rules. It imports the live engine's actual decision functions
(`evaluateNearCertain`, `evaluateSureThing`, the gap-risk and crypto-NO
guards), mirrors the GBM gate and the exit loop line for line, uses the
watcher's real config (enabled lanes, 1.5-8m snipe window, 0.63c fee, normal
risk governor), and replays every round minute-by-minute against real Kalshi
candle books - scoring the same models from only the candles that existed at
each scan minute. No lookahead.

## Three honest states (full 105,888-round replay)

| State | Trades | Win rate | Net |
| --- | --- | --- | --- |
| As configured (hourly leak) | 2,823 | 73.6% | -$76.14 |
| Lane cut (no hourly, no silver 15m) | 608 | 78.1% | +$3.52 |
| + hatch tuned to 15c (shipped) | 608 | 77.3% | +$6.29 |

## What the numbers said

- **The leak was hourly gold/silver.** The engine's own freeze list already
  said they were structurally unprofitable, but the universal scanner ignored
  it. 1,650 of 2,823 trades were hourly lanes, and they lost $75 of the $76.
- **The lane cut flipped the sign.** Freeze the hourly markets, drop silver
  15m, and the same history goes from -$76 to +$3.52.
- **The hatch had the wrong width.** Sweeping the final-minute emergency quit
  from 25c down to 5c showed a knee at 15c: +$3.52 to +$6.29, with lower
  drawdown. Below ~12c the hatch starts eating recoveries.

## The hatch receipt

At 15c, 37 final-minute tails were caught. Held to resolution they would have
lost $18.94. The hatch cut that to $6.02 - it saved $12.92. The hatch was
never the leak; the lanes were.

## Shipped now (paper only)

- Hourly gold/silver frozen in the engine (`SERIES_FREEZE` now enforced in
  the universal scan) and excluded in the saved strategy config.
- Silver 15m excluded from the universal lane.
- Emergency hatch = 15c (was 25c).
- Watcher restarted and scanning on paper. Real account untouched.

## What's next

Crypto 15m does not fire under the current GBM agreement gate; it needs its
own tuning pass through this same replay before it touches paper. Then:
day after day of net-positive paper hours, the leakage audit, and the sizing
ladder - $1, $5, $20, $50 as the record earns it.
