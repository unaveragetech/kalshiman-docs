# Kalshiman

A small trading bot that watches the markets all night so you don't have to.

That's the easy half to explain. The hard half — teaching it *when to keep its
hands still* — is the part that cost us weeks, and it's the part this readme
is really about.

---

## The idea, in plain words

Every 15 minutes, Kalshi runs a market on gold, silver, or crypto: **"up or
down in the next 15 minutes?"** And here's the thing nobody tells you — most of
the time, the market already knows the answer before the round is over. A side
priced at 92 cents settles that way about 97% of the time. It's not a secret.
The secret is having the patience to wait for that 92-cent moment, the nerve to
buy it, and the self-control to let it finish instead of flinching at the first
dip.

The first version of Kalshiman had none of that. It traded constantly. It got
stopped out on noise, lost 50 cents in gaps it never saw coming, and never once
asked itself "what actually happened?" after a trade. It reviewed nothing,
learned nothing, and quietly proved that a bot can lose money faster than a
human — if the human is being sloppy.

We rebuilt it. The new one barely trades at all. And it makes more in an hour
than the old one made in a day.

## The story, in three acts

**Act one — the leak.** The old bot won 79% of its trades and still lost money,
because the losses were five times bigger than the wins. Gold alone cost us
$80. A stop was supposed to cap a loss at 8 cents; in practice paper trades
gaped 65 cents past it. We fixed the trading before we understood the
psychology, and the psychology is what mattered.

**Act two — the rebuild.** We stopped guessing and started measuring. We pulled
105,000 finished rounds and laid them out the way a trader sees them: what the
market looked like before the entry, how it actually moved afterward, and where
it landed. Then we taught a model to read that. The model is picky — it turns
down most setups — and that pickiness is the entire point.

**Act three — the boring, beautiful hour.** The rebuilt system ran one hour on
paper: 8 trades, 8 wins, zero losses, zero drawdown. Average take: 11 cents.
That's the whole goal, repeated a hundred times a day. Not one big score —
hundreds of small, boring, protected wins.

## How it works, like a person would explain it

Think of a good trader's morning. They don't crunch formulas all day; they
*watch* — the book, the candles, the way the market is moving — and they ask
one question: *does this look like the setups that win?* Kalshiman is three
people in one:

- **A statistician** who is rarely wrong. It's read 105,000 finished rounds
  and knows, to the percentage point, how often a 92-cent gold side actually
  settles. On the setups it's most sure of, it's right 98.4% of the time.
- **A quiet observer** who reads the room. It gets the same snapshot a trader
  would see and gives a one-line opinion: "the market's been reversing late
  tonight — careful." It's always listened to, never obeyed blindly, and every
  opinion it gives is written down and scored later.
- **A disciplined hand** that only acts when both of the above agree, then
  does the hard part — holding through the scary dip, because that's usually
  just noise, and capping the rare real break at a fixed stop, so a bad call
  costs 15 cents, not the whole position.

That's it. That's the whole system. Nothing fancier than three people doing
their jobs and keeping notes.

## The numbers, kept honest

Everything here is **paper** — the real account is untouched, and it stays that
way until the paper record holds up over many days.

The best measured hour so far: **8 trades, 8 wins, +88.96¢, average 11.12¢ per
trade, zero drawdown, zero stops fired.**

The rework's arc, hour by hour:

| What changed | Trades | Result | Avg/trade |
| --- | --- | --- | --- |
| The old system | 5 | +1¢, one loss | +0.2¢ |
| Paper stops that actually work | 7 | +45¢, one loss | +6.4¢ |
| Stops that respect the entry price | 5 | +48.5¢, zero losses | +9.7¢ |
| Stops that wait out noise | 8 | **+88.96¢, zero losses** | **+11.12¢** |

You can watch every one of those trades — the model's reasoning, the entry,
the exit, the post-mortem — on the live dashboard. Nothing is hidden.

## The bigger idea

Here's what we're actually building toward: **what if your bot worked with
other bots?**

One bot has to watch every market, every night, alone. Ten bots, each watching
a slice and sharing what they learn, cover more ground with less work each —
and a worker that finds a good setup posts it to the pool so the others can
see it too. The whole network compounds: consistent, high-throughput,
near-zero-cost trading. Every quiet day becomes free gains, and the humans get
their evenings back.

That's the end state. Kalshiman is the first bot in that fleet, and it just
learned to stop losing money.

## Where to go from here

- [docs/00-thesis.md](docs/00-thesis.md) — the thinking in full
- [docs/13-versions.md](docs/13-versions.md) — the journey, version by version
- [docs/14-reasoning.md](docs/14-reasoning.md) — why three layers, not one
- [docs/15-returns.md](docs/15-returns.md) — what "free gains" actually looks
  like in a month
- [docs/06-performance.md](docs/06-performance.md) — the receipts
- [ARCHITECTURE.md](ARCHITECTURE.md) — how it's built, for the curious
- [docs/kalshiman-full-breakdown.pdf](docs/kalshiman-full-breakdown.pdf) — the
  full write-up, designed to be read front to back

---

*Nothing here is financial advice. Kalshi isn't available in all states. The
real money stays off until the paper record earns it.*
