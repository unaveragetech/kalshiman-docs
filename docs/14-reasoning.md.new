# The observer layer — why statistics alone failed, why a reasoning model was
# needed, and how the system finds "just right"

Trading is a Goldilocks problem. Too hot — chasing every wiggle, trusting
hunches, stopping on noise — and the noise eats you. Too cold — waiting for a
formula to be perfect, never letting winners breathe, treating every round as
identical — and you either miss everything or get stopped out of wins. The
rework's real discovery was that a trading system needs three layers that
balance each other: **calibrated statistics** (the thermometer), **reasoning
observation** (the trader's eye), and **discipline** (the hand that only acts
when it's just right).

## Why the original system failed

The original brain was a static function. Every round, it read one snapshot of
numbers (implied price, momentum, volume, hour) and output one probability.
That fails for structural reasons:

1. **Markets are processes, not points.** A 90% implied at minute 12 means
   something different than at minute 2. The original model treated every
   moment the same — it had no idea *how the market got here* (the path), only
   where it was.
2. **No noise-vs-signal sense.** A dip on a high-confidence pick is usually
   noise that recovers; a dip that stays is signal. The flat 8¢ stop treated
   both identically, so it "too cold" — it never let winners breathe and
   stopped out on noise (9 of 15 stops recovered and won).
3. **No series context.** Gold at 8pm behaves differently than gold at noon.
   After three NO rounds, the next round behaves differently. The original
   model used history as raw inputs but couldn't reason about what that history
   *meant* for this specific round.
4. **No self-correction.** It never asked "what actually happened?" There was
   no review against resolution, no calibration to its own outcomes, no
   journal. A trader who never reviewed their trades would stay bad forever;
   the original system did exactly that.
5. **No language for judgment.** A formula can't say "this looks like the
   reversal pattern we saw Tuesday." That sentence requires reasoning over
   context — comparing, weighing, remembering — which is not what a logistic
   curve or a fixed gate does.

The result was a mechanical system that reacted to the current number without
understanding the market — too cold, too brittle, and blind to the difference
between noise and opportunity.

## Why a reasoning observer (LLM) was needed

Watch a good trader work: they don't compute, they *observe*. They watch the
book, the candle path, the volume, the series history, the clock, the way the
market is moving — and they ask, "does this setup look like the ones that
win?" That is reasoning over context, not arithmetic.

A language-capable model can hold that conversation. Qwen3-1.7B was added as
exactly that: a dedicated observer watching every market for you. Each round
the researcher builds the same snapshot a trader would see — implied,
bid/ask, trend path, series history, volume, countdown — and the model answers
YES / NO / PASS with a one-line reason. Its call is logged and scored against
the real resolution, like a trader's journal, so we can measure whether the
observer's instincts actually add value over time.

Why it *can* work: language models are trained on how sequences unfold —
trends, momentum, reversals, "the market is doing X, which usually means Y."
That pattern-matching over context is precisely the trader's skill. The LLM
adds judgment on top of probability: it can notice that a 91¢ YES on silver
late in the series has been reversing all evening, something no static feature
vector captures.

## But a reasoning model alone is too hot

Qwen's raw instincts turned out to be noisy and uncalibrated. It said "YES
85%" while the market implied YES at 3% — confident, contradictory, and
untrustworthy as a decision-maker. Left alone it would chase hunches and
overtrade. A pure-LLM system is "too hot": smart-sounding, wrong-calibrated,
expensive, and dangerous.

## The just-right architecture (V2)

The system balances the two failure modes with three layers:

| Layer | Role | Failure if alone |
| --- | --- | --- |
| **Calibrated statistics (GBM)** | The thermometer: a well-calibrated probability (98.4% win rate on high-confidence calls, audited for leakage) | Too cold: mechanical, context-blind, stops on noise |
| **Reasoning observer (Qwen)** | The trader's eye: language reasoning over the snapshot, advisory only, logged and scored | Too hot: noisy, uncalibrated, hunches |
| **Discipline (gates + stops)** | The hand: high-confidence entries only (≥85% / ≤15%), tiered limit-stops, 3-minute grace so winners breathe, fills at the stop level | Without it: no sweet spot, no risk cap |

The flow on every round:

1. The observer (researcher + LLM) watches: here's the snapshot, here's the
   trend, here's the series history.
2. The thermometer (GBM) says: this setup wins ~96% of the time.
3. The hand (gates) acts only when both agree and the timing is right — not
   too hot (chasing weak setups), not too cold (waiting for a perfect world).
4. Every close is reviewed against reality, so all three layers keep getting
   smarter from their own outcomes.

That's what "just right" means in practice: **a calibrated probability and an
observer's judgment, held together by discipline.** The proof is in the
progression — too cold/mechanical was +1¢ an hour; the balanced V2 standard
ran 8W/0L, +88.96¢, $0 drawdown, because it finally understood when to act
and when to let a winner breathe.

![Rework progression](charts/rework-progression.png)

## How we know it works

- The observer's every call is scored against the resolution (its journal
  lives in `memory/llm-score.json` and the trade reviews).
- The thermometer is leakage-audited (PASS) and backtested out-of-sample.
- Every trade carries a review verdict, so "was that stop noise or signal?"
  is answered by data, not vibes.

The system is not "an LLM that trades." It is a trader's team: a statistician
who is rarely wrong, an observer who notices context, and a disciplined hand
that only acts when it's just right.
