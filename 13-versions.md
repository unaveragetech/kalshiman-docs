# Versions — the journey, told straight

## V0: the bot that couldn't keep its hands still

The first Kalshiman was a machine with one skill: it found markets where the
favorite was obvious, bought the favorite, and got out when the trade went
against it. On paper that sounds fine. In practice it was like a dog that
chases every car — technically fast, emotionally doomed.

It won 79% of its trades and lost money. Here's the ledger, unsparingly:

- Its wins averaged 12 cents. Its losses averaged 48. To break even at those
  numbers it needed a 134% win rate. It was winning at 79% and losing anyway.
- Its stop was supposed to cap losses at 8 cents. Paper trades — the ones we
  used to test it — gaped through by 17 to 65 cents, because the stop only
  checked every five seconds and the market didn't wait for it.
- Nine of its fifteen stopped trades recovered and won at the bell. The stop
  was the leak wearing a helmet.
- Gold alone cost $80: 110 trades, a 75% win rate, and an average loss of
  $5.46. It won constantly and lost more on the losses than it made on the
  wins.

The worst part wasn't the money. It was that the bot never asked why. No
post-mortems, no journals, no retraining from mistakes. It was a trader who
never reviewed their trades — guaranteed to stay bad forever.

## V1: the rebuild — data before opinions

We stopped and did the unglamorous work. We pulled every finished round we
could find — 105,000 of them — and laid them out the way a trader sees them:
what the market looked like before the entry, how it moved after, where it
landed. We audited the data for leaks (a model that reads the answer before it
answers isn't a model, it's a cheat sheet) and fixed the leaks we found.

Then we trained a gradient-boosted model on all of it. Out-of-sample — on
rounds it had never seen — it hit 93% using only what you'd know before
entering, and 96.7% when it got to see the first two minutes of the trend.
On its highest-confidence calls: 98.4%.

We also built the honest evaluation loop: a dashboard that plays every round
back, shows the model's reasoning, and scores it against reality. For the
first time, we could watch the bot think.

## V2: the boring, beautiful hour

The final piece was behavior, not intelligence. The model was smart; the bot
was still jittery. Three changes:

- **Stops that fill at the stop level.** If a trade is supposed to cost 15
  cents, it costs 15 cents — not the 50-cent panic price the market printed
  while the bot was between checks.
- **Stops that wait.** A dip has three minutes to prove it's real. If the
  market comes back — and it usually does — the winner stays.
- **Stops that respect the entry.** Pay 90 cents or more, and you get 15
  cents of room, because at those prices the dip is almost always noise. Pay
  less, and the stop stays tight at 8.

Then we let it run one hour on paper and watched it do something the old bot
had never done: nothing stupid. Eight trades, eight wins, +88.96 cents, zero
drawdown, zero stops, zero errors.

## The comparison that matters

| | V0 | V1 | V2 |
| --- | --- | --- | --- |
| What it saw | the current price | + candle history | + learned resolution dynamics |
| What it trusted | the market's mood | + a model | + a model with a confidence bar |
| Stop | flat 8¢, instant | flat 8¢, slow on paper | 15¢/8¢, fills at the stop |
| Waiting | none | 30 seconds | 3 minutes |
| After a loss | nothing | a dashboard | a written post-mortem |
| Hour | +1¢ | +45¢ | **+88.96¢** |
| Losses that hour | 1–2 | 1 | **0** |

The bot didn't get smarter by adding parameters. It got smarter by learning
from its own outcomes, refusing weak setups, and finally understanding the
difference between noise and signal. That's the whole story. It's not magic —
it's a trader who started keeping a journal.
