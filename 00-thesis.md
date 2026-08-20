# The thesis — why this works

## The 92-cent question

Walk up to a Kalshi market for gold fifteen minutes before it closes. The
price is 92 cents for "up." Ask a stranger what happens, and they'll shrug.
Ask the market itself, and it's already told you: the buyers have decided.
Almost all of the time — 97% of the time, on our measured record — that side
is right.

So where's the catch? The catch is that 97% isn't 100%. The other 3% of the
time, the 92-cent side collapses, and if you paid 92 cents for it, a loss
costs you 92 cents — almost eleven times what a win pays. That's the whole
game in one paragraph:

> Win small, often. Lose small, rarely. Never let the two get out of balance.

The first version of Kalshiman got the first half right and the second half
catastrophically wrong. It won 79% of the time and still lost money, because
its losses averaged five times its wins and its stops didn't actually stop
anything. It was a coin that landed heads often — and paid out like a lottery
ticket when it landed tails.

## The two ways a system dies

We learned that trading fails in exactly two directions, and the fixes are
opposite.

**Too hot.** You trust every hunch, chase every wiggle, and let a confident
model talk you into trades it has no business taking. You get stopped out on
noise. You bleed.

**Too cold.** You wait for certainty that never comes, or you let a formula
stop you out of winners because it can't tell a dip from a disaster. You miss
everything, or you win on paper and lose in practice because the stop fired
before the market came back.

The old bot was too cold in a specific way: its stop treated every dip like an
emergency. Nine of its fifteen stopped trades recovered and won at the
bell. The stop wasn't protecting anything — it was donating wins.

## The fix is a balance, not a formula

The rebuilt system doesn't trust any single answer. It runs three layers
together, and each one exists to catch the other two's failure mode:

1. **A statistician.** The model has read 105,000 finished rounds. It knows
   what 92 cents usually means, in this market, at this hour, after this
   series of outcomes. It's calibrated — when it says 98%, it's right 98.4%.
2. **An observer.** A small language model reads each round the way a person
   would — the book, the path, the history — and gives a one-line opinion.
   It's often wrong on its own, which is exactly why it's never in charge.
3. **A disciplined hand.** Entries only happen when the statistician is
   confident and the observer agrees. Once in, the hand does the hard part:
   it lets a dip breathe for three minutes before it even considers leaving,
   and if it does leave, it leaves at the stop level — never at whatever
   panicked price the market printed while it wasn't looking.

That's not a cleverer formula. It's a team.

## Why we believe it's real

Because we stopped believing our own story and started keeping receipts.
Every trade now gets a post-mortem against what actually happened: did the
side win? What would we have made if we'd held? Did the stop cost us or save
us? The answers go into the training data, the model gets retrained, and the
rules get adjusted from evidence, not vibes.

That's the part most trading bots never build — and it's the part that turned
a losing bot into one that ran a perfect hour.

## The grain of rice

People ask why we care about 11 cents. Here's the honest answer: because it
compounds. A trader who reliably keeps 10 cents out of every round, ten times
an hour, sixteen hours a day, isn't earning a wage — they're building a
machine that earns while everyone sleeps. The doubling grain-of-rice curve
only works if you can keep the wins and cap the losses, every single round,
forever. That's the discipline we built. The cents are just the proof.
