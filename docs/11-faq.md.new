# FAQ

**Is this making real money?**
No — the system is paper-only. The final standard hour made +88.96¢ paper
(8W/0L, avg +11.12¢/trade). Real mode stays off until proven.

**Why did we stop losing money after the rework?**
Three fixes: fast stops on paper (no more 50¢ gap-throughs), the GBM entry
gate (no more weak mid-price entries), and the 3-minute recovery grace +
limit-stop fills (no more "stopped then recovered" losses).

**Why do we sometimes hold through a stop breach?**
The 3-minute grace exists because gold/silver dips that recover are noise, not
signal. A breach that persists 3 minutes (or reaches the final minute) still
closes at the stop level — the cap is never removed, only delayed.

**What does the GBM gate do?**
It blocks paper entries unless the trend model is ≥85% (or ≤15%) and agrees
with the side. Vetoes are logged as `gbm-low-conf`, `gbm-disagree`,
`gbm-pending`, `gbm-warmup`.

**What is the dashboard evaluating?**
Only out-of-sample rounds (test split after 2026-08-10) — the model never
trained on them — so the displayed accuracy is honest.

**When can we go live?**
After consecutive net-positive harness hours, a passing leakage audit, and
confirmation that stops fill at the stop level. Then `liveMode: true` per
series with proof.

**Why is the pool disabled?**
Its Cloudflare KV quota was exhausted by sync fan-out, not trading. Client
polling is throttled and a worker cache is staged; re-enable after deploying
the cache from the account that owns the live pool.
