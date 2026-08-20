# Returns — the honest compounding story

## The unit of edge is a percent, not a cent

The most important thing to understand about this system: when it wins, it
wins about **11% of whatever it deployed**, not a flat 11 cents. You buy the
favorite at 90 cents, it settles at 100, you made 10 cents on 90 cents
invested — that's +11.1%. The cents in our hourly reports are the *per-contract*
unit; the dollars you actually make scale with position size.

| Position | One win (+11%) | One stop (−8¢ on a 90¢ entry) |
| --- | --- | --- |
| $1 | +$0.11 | −$0.09 |
| $50 | +$5.56 | −$4.44 |
| $150 | +$16.67 | −$13.33 |
| $500 | +$55.56 | −$44.44 |

That's why "entering and winning with $150 instead of $1" changes the game:
the percentage is the same, the dollars are not. Compounding is the engine;
this table is why it works.

## The compounding math, done honestly

If you reinvest everything and never lose, a win at 90¢ is ×1.111 per trade.
Reality is a coin with a small bad side, so the honest per-trade multiplier is:

**W × 1.111 + L × (what a loss costs)**

| Win rate | Loss shape | Per-trade growth | Trades to $200 (from $1) | Trades to $500 |
| --- | --- | --- | --- | --- |
| 98% | 8¢ stop | **+10.7%** | 53 | 62 |
| 96% | 8¢ stop | +9.6% | 59 | 69 |
| 96% | full loss (final-minute hold wrong) | +6.7% | 83 | 97 |

At 64 trades a day, the +10.7% line reaches $200 in **under a day** of perfect
compounding. Which tells you the truth about the whole thing:

> The math is not the bottleneck. Liquidity and win rate are.

## Why it does not go to infinity

If +10.7% compounded 1,280 times in a month, $1 becomes 10⁵⁶ dollars. It
doesn't, and the reasons are the real subject of this document:

1. **Win rate is not 100%.** Our best era was 20 of 21 trades. And the newest
   rule — holding GBM-confirmed trades to resolution in the final two minutes —
   means the rare wrong call can cost the whole position, which drags the
   multiplier from +10.7% to +6.7% if the tail shows up at 4%.
2. **Liquidity is a wall.** A 15-minute gold round has maybe $50–100k of
   total volume. A $500 position is fine; a $50,000 one is not — it moves the
   book, eats the spread, and destroys the edge that made it possible.
3. **Fees are flat, so small accounts pay a higher percentage.** The 0.63¢
   round-trip is 0.7% on a 90¢ contract; on a $1 position that's the same
   percentage, but it's a bigger share of the compounding early on.

## The realistic shape of the curve

The honest way to read the scenario table from the earlier docs:

| Trades/hour | Day (16h, 10¢/trade) | 30 days |
| --- | --- | --- |
| 5 | $8.00 | $240 |
| 8 | $12.80 | $384 |
| 12 | $19.20 | $576 |

Those are **flat per-trade** numbers — they assume you keep betting $1 and
banking the cents. The compounding version is steeper but bounded: the moment
your position starts eating the market's depth, the per-trade percentage
falls. Somewhere between "banking every cent" and "all-in every round" is the
real curve, and finding it is a sizing problem we have not solved yet.

## The fleet is the scaling answer

This is why the pool matters. You cannot make one position much bigger
without hitting the liquidity wall — but you can run **more positions, in
more markets, on more machines**, all sharing what they learn. Ten workers
trading $20 positions across different rounds and markets compound the same
way one worker trading $200 does not. The network is the compounding story,
scaled sideways instead of up. See [18-pool](18-pool.md).

## The honest bottom line

The direction of the compounding idea is right: near-100% wins at +11% per
trade is explosive, and position size is the multiplier. But "$200 in month
one, $500 in month two" is not compounding — compounding that fast would be
$40,000 in month two, and liquidity and the loss tail are what stop it long
before either. The correct sentence for the docs is: *the edge is ~11% per
trade on position, compounding is real and fast, and the two things that cap
it are the rare full loss and the size of the market itself.*
