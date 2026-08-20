# The pool — how a fleet of bots works

## Why a fleet at all

One bot can watch a handful of markets every night. Ten bots can watch all of
them — and each one gets to be lazier, because the others cover the rest. The
pool is the nervous system that lets workers share what they find instead of
each learning everything alone.

The compounding story only scales sideways: a single position can't grow
forever before it hits the market's depth, but a fleet can add positions in
new markets all day. That's the real answer to "how do the gains get big."

## The players

- **The main node** — the one that owns the strategy configs and can push
  verified strategies down to the fleet. It's the operator.
- **Workers** — every other instance. They run the same engine, watch their
  slice, and publish what they find.
- **The pool worker** — a small Cloudflare Worker in front of a KV store that
  is the shared inbox: signals, ledgers, strategy versions.

## The credits economy

Information has a price, and the pool prices it in credits so workers keep
their balances healthy:

- Publishing a good signal costs a few credits (escrow).
- Another worker who **verifies** it gets paid for that verification, and the
  **original poster is refunded** when the signal proves actionable — so
  posting good information pays for itself, and posting garbage costs.
- A single piece of information can only be refunded once — no double-dipping.
- Workers cap at **200 credits**; anything above 100 flows into the pool's
  float, which is used to verify incoming information. That keeps every worker
  funded enough to keep exchanging, and it means the pool is always
  self-balancing.

## How a signal travels

1. A worker sees a high-confidence setup (GBM gate passes, the observer
   agrees).
2. It publishes the snapshot — the data history, the trend read, the call —
   and pays the escrow.
3. The pool stores it (KV), and other workers pull the feed on their cycle.
4. A worker that can verify it (same market, independent data) confirms it and
   earns credits; the poster gets refunded.
5. The strategy stays in the pool until the next verified version replaces it.

There are guardrails so the network doesn't drown in noise:

- Any node can publish at most **one post per 10 minutes**.
- Any node can **resolve/verify a post at any time** — verification is the
  counterweight to publishing.
- The main node is the only one that pushes *verified strategies* down;
  workers otherwise share via the communication protocol, so a compromised or
  confused worker can't rewrite the playbook.

## How workers specialize

Workers naturally specialize: a node in a different timezone sees the overnight
markets, one with a preference for commodities only runs the commodity lanes.
They don't have to agree on everything — the pool only needs them to *share*.
Because all workers share their winning information for a flat, small rate,
the whole fleet converges toward the same knowledge over time. That's the
daisy chain: one worker's discovery becomes everyone's, and no single machine
has to watch every market.

## Current status (honest)

- The pool is **disabled right now** — the paper test doesn't need it, and the
  live pool worker hit its daily Cloudflare KV quota.
- Why the quota burned: the pool's `/feed` endpoint did a KV list + a read
  fan-out on every call, and one idle node polling every 60 seconds through
  three overlapping paths used up the free tier's reads and lists in under a
  day. No trading was involved — the sync machinery alone did it.
- Fixed client-side: polling cut to 5 minutes, heartbeat to 15, duplicate sync
  removed. A worker-side TTL cache is staged in `network/worker.js` but has
  not been deployed, because the live pool lives on the tokenbroker account
  and this machine's login can't reach it.
- Re-enabling requires deploying the cached worker from the account that owns
  the pool, then flipping `network.enabled` back on.

## The roadmap

1. Deploy the cache fix to the live pool worker.
2. Re-enable the network and run the main node + one test worker side by side
   on paper, verifying signals flow both ways.
3. Let workers specialize (timezone, market preference) and watch the
   verification economy keep balances healthy.
4. Only then — when the fleet is sharing verified, profitable information —
   scale positions within each market's liquidity, not beyond it.
