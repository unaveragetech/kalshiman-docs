# Projection — where the system goes next

The current standard is stable and profitable at the paper level. The known
levers, in order of expected value:

1. **Throughput** — the final hour averaged ~8 trades/hr at +11¢. The 15m
   gold/silver book produces a limited number of ≥85% GBM setups per hour.
   Options, each small and reversible:
   - Add more 15m series with trained models (crypto lanes are disabled but
     the GBM covers them) once their leakage-clean models are retrained.
   - Allow a slightly earlier entry window on the near-certain lanes (the
     trend model's entry-only call is 93% accurate at close-3min).
2. **$1/hr target** — at +11¢/trade the target needs ~9-10 trades/hr. The
   final hour did 8; adding a second qualified series set should clear it.
3. **Stop policy is data-driven** — the review pipeline scores every close.
   If "stop cost us" verdicts stay rare, keep 15¢/8¢ + 3-min grace. If they
   return, re-tune the grace or tier boundary from the review data, not vibes.
4. **Live promotion** — only after consecutive net-positive harness hours,
   a passing leakage audit, and stop fills confirmed at the stop level.

## Guardrails that never change

- Paper until proven. No martingale. No revenge trading. No extreme leverage.
- Stops cap every event trade; the grace only delays, it never removes the cap.
