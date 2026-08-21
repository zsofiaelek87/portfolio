# Robinhood Chain Bot

> Paper-trade strategies and track airdrop opportunities on blockchain via Telegram control

**Stack:** Python

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

## What makes it interesting

## How it works

## Recently shipped

<!-- portfolio-entry:robinhood-chain/commit/36d2ec9 -->
### Teaching a trading bot what weekends mean

Price gaps between Friday's close and Monday's open are a normal feature of markets that rest — not a signal that something unusual happened. Before this commit, the bot had no concept of when markets were closed, which meant it could mistake a routine weekend pause for a meaningful price dislocation and treat it as a trading opportunity worth acting on.

The fix is a market-hours check that the bot consults before drawing any conclusion: if the gap appeared while the exchange was simply shut, the gap is filed as expected, not alarming. It is a small piece of calendar awareness that separates a system making genuine decisions from one pattern-matching on noise.

- Bot distinguishes closed-market gaps from real price dislocations
- Prevents false signals from routine weekend price movements

<sub>Python · Telegram</sub>
<!-- /portfolio-entry:robinhood-chain/commit/36d2ec9 -->

## Stack notes
