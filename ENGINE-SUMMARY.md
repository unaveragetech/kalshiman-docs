# Engine summary — current standard (V2)

Live status for the paper watcher. All real money remains off.

## Active lanes

| Lane | Series | Timing | Notes |
| --- | --- | --- | --- |
| gold15-late | KXGOLD15M | late near-certain | GBM-gated |
| silver15-late | KXSILVER15M | late near-certain | GBM-gated |
| universal-snipe | any eligible | 0.5–2.5m to close | GBM-gated where covered |

`liveMode: false`. Disabled: hourly/daily/crypto lanes, perps, weather,
btc-dip, both-sides arbs.

## Entry gate (paper)

1. Gates: signal (learned model + market implied), price band, timing,
   liquidity + EV.
2. GBM gate: entries on model-covered series require `gbmProbUp >= 0.85` or
   `<= 0.15` AND agreement. Veto reasons: `gbm-low-conf`, `gbm-disagree`,
   `gbm-pending`, `gbm-warmup`.
3. Qwen advisor: advisory only, never blocks.

## Risk controls

- Tiered stops: entry ≥90¢ → **15¢**; entry <90¢ → **8¢**.
- Recovery grace: breach must persist **3 minutes** (or be within 60s of
  close) before closing.
- Limit-stop fills: paper exits marked at the stop level, never a crash price.
- Fast stop loop: 1.5s, paper AND live.
- No martingale, no revenge, no leverage, no both-sides.

## Latest performance

| Window | Trades | W/L | Net | Avg | Max DD |
| --- | --- | --- | --- | --- | --- |
| Standard hour (23:32Z→00:32Z) | 8 | 8W/0L | +88.96¢ | +11.12¢ | $0 |
| Follow-up (00:32Z→01:05Z) | 1 | 1W/0L | +2.74¢ | +2.74¢ | $0 |

Live prediction accuracy: 89.5% model / 99% market (200 resolved).

## Account

- Real balance: **$23.09** · 0 open event positions · 0 margin positions.
- Paper ledger (cumulative, incl. old-system era): 256 settled, 150W/106L
  (59%), paper +$720.21; live −$7.64 (historical, pre-rework).

## Watch it live

```
node scripts/eval-ctl.js status
node scripts/eval-ctl.js watch
```
