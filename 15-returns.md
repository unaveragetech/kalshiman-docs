# Returns — what "free gains" actually looks like

## Start with the unit

The rebuilt system, in its best measured hour, took eight trades and won all
eight, averaging 11.12 cents each. That's the unit everything else is built
from: a small, protected win, repeated.

People hear "11 cents" and laugh. They shouldn't. Eleven cents an hour, ten
hours a day, is a wage. And it's a wage that works while you sleep.

## The honest math

All figures below are **paper**, and they assume the measured average holds:

| Trades/hour | Wins/day (16h) | Day | 7 days | 30 days |
| --- | --- | --- | --- | --- |
| 5 | 80 | $8.00 | $56.00 | $240.00 |
| 8 (today's pace) | 128 | $12.80 | $89.60 | $384.00 |
| 12 | 192 | $19.20 | $134.40 | $576.00 |

At the actual measured +11.12¢ average, the middle line is closer to $14 a
day and $426 a month — before any compounding of position size, which is the
whole point of the grain-of-rice idea.

## What the good hour actually looked like

Seven of the eight wins were the same boring trade: buy the side the market
already believes, hold it, bank it. Two stood out:

- Silver, 81 cents, YES. Bought with four minutes left, held to the bell,
  won 23 cents.
- Silver, 86 cents, YES. Same story, 16 cents.

No heroics. No last-second saves. The bot let the market do the work and got
out of its way.

## What changes the number

**Up:** more markets with trained models (there are 16 in the dataset; two are
trading now), a slightly earlier entry window on the setups the model is sure
of, and the network — ten bots sharing what they learn instead of one bot
learning alone.

**Down:** thin hours with few qualifying setups, a wrong call slipping past
the gate, or a regime change that breaks the pattern until the journal catches
it. The stop policy caps the damage while that gets sorted.

## The catch, said plainly

One perfect hour isn't a track record. The path to trusting these numbers is
the same as the path to going live: day after day of net-positive paper hours,
a leakage audit that keeps passing, and stops that keep filling at the stop
level. We're not there yet. But for the first time since we started, the bot
isn't the reason we're not.

## The bigger number

The real prize isn't the cents. It's the network: bots that trade together,
share what they learn, and spread the work so no single machine has to watch
every market every night. Consistent, high-throughput, near-zero-cost trading.
Every quiet day becomes free gains — and the humans get their evenings back.
That's the thing we're building. The cents are just how we know it's working.
