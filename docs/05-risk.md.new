# Risk

## Stop policy (the standard)

- **Tiered**: entry ≥90¢ → 15¢ stop; entry <90¢ → 8¢ stop.
- **Recovery grace**: a breach must persist 3 minutes (or be within 60s of
  close) before closing. Transient dips that recover keep the position.
- **Limit-stop fills**: paper exits are marked at the stop level — a crash
  price the market gapped through is never booked as the fill.
- **Fast checks**: every open position is checked every 1.5s, paper and live.

![Stop review](charts/stop-review.png)

## Why this shape

Review data from the rework (15 stops):

- 9 recovered and won at resolution — the old flat stop cost us 188¢.
- 6 were genuine wrongs — the stop saved 344¢ (worst: silver YES@91, would
  have been −89¢).
- Net, stops saved 155¢; the grace + limit-stop keep the savings while
  eliminating most of the costs.

## Hard rules

- No martingale. No revenge trading. No extreme leverage. No both-sides.
- Paper-first: live mode stays off until proof (positive walk-forward or a
  proven net-positive paper ledger) and a passing leakage audit.
- Every close is reviewed against the actual resolution (`analysis` verdict).
