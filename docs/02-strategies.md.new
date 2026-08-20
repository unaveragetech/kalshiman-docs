# Strategies (v2)

## Lanes

| Lane | Series | Behavior |
| --- | --- | --- |
| gold15-late | KXGOLD15M | late near-certain entries, GBM-gated |
| silver15-late | KXSILVER15M | late near-certain entries, GBM-gated |
| universal-snipe | any eligible | 0.5–2.5m to close, GBM-gated where covered |

## Entry pipeline

1. Researcher scores the live session: logistic `modelProbUp`, GBM
   `gbmProbUp` (entry-only), GBM `gbmSnipeProbUp` (trend-aware).
2. Gates: signal, price band, timing, liquidity + EV.
3. **GBM gate (paper)**: entry requires `gbmProbUp >= 0.85` or `<= 0.15` and
   agreement with the gate side. Vetoes: `gbm-low-conf`, `gbm-disagree`,
   `gbm-pending`, `gbm-warmup`.
4. Open paper trade. Qwen advisory logs a second opinion but never blocks.

## Exits

- Bank / bow-out at 99–99.5¢ on winners (or hold to resolution).
- Tiered stop: ≥90¢ entry → 15¢; <90¢ entry → 8¢.
- 3-minute recovery grace before a stop closes; final-minute safety closes at
  ≤60s to close.
- Limit-stop fills: exit marked at the stop level, never a crash price.

## 15-minute priority

The strongest returns come from 15m commodities. Hourly/crypto/weather lanes
are disabled until their models are retrained and pass the same bar.
