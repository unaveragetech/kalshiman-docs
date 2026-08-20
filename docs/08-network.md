# Network (signal pool)

## Current status

- Pool sync is **disabled** during the paper test (`network.enabled: false`).
- Client polling was cut from 60s to **5 minutes**; heartbeat from 5 to 15
  minutes; the duplicate strategies-sync interval in the watcher was removed.
- A worker-side TTL cache is staged in `network/worker.js` (collapses
  feed/agents/strats/config reads into ~1 KV op per window) but has not been
  deployed: the live pool lives on the **tokenbroker account subdomain**, and
  this machine's wrangler login cannot reach that account's KV.

## Why the KV quota burned

The pool's `/feed` endpoint does a KV list + per-agent read fan-out on every
call, and `/strategies` does ~14 reads per call (index + one per strategy).
One idle node polling every 60s through three overlapping paths burned the
free tier's 100k reads and 1k lists per day — without any trading traffic.

## Re-enabling safely

1. Deploy the cached `network/worker.js` from the account that owns
   `kalshiman-signal-pool.tokenbroker.workers.dev`.
2. Set `network.enabled: true` (polling already throttled to 5/15 min).
3. Verify `/strategies` returns 200 and daily KV usage stays under quota.
