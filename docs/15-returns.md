# Examples of returns

This page answers the question every investor asks first: *how much can it
make?* Everything below is **paper** — real money stays off until live mode is
proven. Fees are modelled at the measured 0.63¢/contract.

## The unit of edge

The standard system takes trades at an average of **+11.12¢ per trade**
(measured hour: 8W/0L, +88.96¢). The edge per trade is small; the power is
frequency and win rate:

![Returns scenarios](charts/returns-scenarios.png)

| Trades/hour | Hours/day | Avg/trade | Day | 7 days | 30 days |
| --- | --- | --- | --- | --- | --- |
| 5 | 16 | 10¢ | $8.00 | $56.00 | $240.00 |
| **8 (standard)** | 16 | 10¢ | **$12.80** | **$89.60** | **$384.00** |
| 12 | 16 | 10¢ | $19.20 | $134.40 | $576.00 |

These assume the measured average holds and no scaling costs. At the actual
measured +11.12¢ average, the standard 8 trades/hour line becomes ~$14.20/day
and ~$426/month.

## Why compounding beats linear

The system was designed around the grain-of-rice idea: doubling beats a fixed
allowance. If each cent compounds (reinvested exposure), even a small edge
grows quickly — but Kalshiman also caps risk on every trade, so the curve is a
disciplined compounding, not a lottery ticket:

![Grain of rice](charts/compounding-rice.png)

## A worked hour (the standard hour, 23:32Z→00:32Z)

| # | Market | Side | Entry | Exit | P&L | How |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | KXGOLD15M | NO | 90¢ | 98¢ (bow-out) | +6.74¢ | banked near close |
| 2 | KXSILVER15M | YES | 81¢ | resolution | **+23.00¢** | rode to resolution |
| 3 | KXGOLD15M | NO | 93¢ | 97¢ | +2.74¢ | banked |
| 4 | KXSILVER15M | NO | 93¢ | resolution | +8.00¢ | held |
| 5 | KXGOLD15M | YES | 87¢ | 93¢ | +4.74¢ | banked |
| 6 | KXSILVER15M | YES | 86¢ | resolution | **+16.00¢** | rode to resolution |
| 7 | KXGOLD15M | NO | 93¢ | 99¢ | +4.74¢ | banked |
| 8 | KXSILVER15M | NO | 81¢ | resolution | **+23.00¢** | rode to resolution |

**Total: +88.96¢ · 8W/0L · avg +11.12¢ · $0 drawdown · 0 stops.**

## What drives returns up or down

- **Up:** more high-confidence setups per hour (more GBM-covered 15m series),
  higher avg entry quality (85–90¢ entries that ride to 99¢), and letting
  winners breathe (the recovery grace).
- **Down:** thin-market hours (fewer qualifying setups), a wrong call slipping
  through the gate, or a stop filling wider than the tier (which the limit-
  stop fix prevents).

## The honest caveat

One perfect hour is not a track record. The path to trusting the numbers:
consecutive net-positive harness hours, a passing leakage audit, and
confirmation that stops fill at the stop level — then and only then live.
