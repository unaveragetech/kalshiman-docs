# Lessons from the rework

1. **Paper trades need the same stop speed as live.** The 1.5s fast-stop loop
   only watched live trades; paper rode the 5s tick and booked 17–65¢
   gap-throughs. The paper system was evaluating itself with a handicap.
2. **A stop is a trigger, not a cap — unless you make it one.** Grace-delayed
   exits filled at the crash price. Limit-stop fills (exit at the stop level)
   fixed it.
3. **Recoveries are real.** 9 of 15 stops in the rework recovered and won.
   A 30s grace was too short; 3 minutes captures gold/silver whipsaws while
   the limit-stop still caps genuine breaks.
4. **Entry quality beats stop tuning.** The GBM gate (≥85%) removed the
   mid-price entries that produced the catastrophic wrongs in the first place.
5. **Data first.** The leakage audit caught same-closeTime parallel-strike
   outcomes leaking into prev features (48k rows) and near-close trend candles
   — both fixed before trusting any number.
6. **KV quotas are eaten by sync machinery, not trading.** The pool's per-
   request fan-out + 60s polling exhausted Cloudflare's free tier with zero
   trading traffic. Throttle and cache before scaling nodes.
7. **Cold starts matter.** Entries fired 27s after boot before the first GBM
   score landed; the `gbm-warmup` guard closes that hole.
