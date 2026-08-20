# Markets

## Active trading set

| Series | Type | Status |
| --- | --- | --- |
| KXGOLD15M | gold up/down, 15m | active lane (gold15-late) |
| KXSILVER15M | silver up/down, 15m | active lane (silver15-late) |
| universal catalog | sports/weather/macro/digital | universal-snipe, 0.5–2.5m |

The dataset covers 16 series including crypto 15m (BTC/ETH/SOL/XRP/DOGE/ZEC/
BNB/HYPE/NEAR), hourly gold/silver, and daily digitals — models are trained on
them and can be re-enabled lane-by-lane after the same GBM gate standard.

## Resolution timing

15m rounds close every 15 minutes. The watcher scans continuously and the
1.5s fast-stop loop plus the 5s engine tick keep positions managed to
resolution. Universal entries are restricted to markets resolving within
2.5 minutes so capital is always near the money.

## Overnight behavior

15m commodity/crypto series pause briefly overnight; hourly gold/silver and
macro markets (e.g. inflation prints) still provide resolutions, which the
universal lane can catch.
