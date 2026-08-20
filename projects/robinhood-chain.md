# Robinhood Chain Bot

> Paper-trade strategies and track airdrop opportunities on blockchain via Telegram control

**Stack:** Python

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

## What makes it interesting

## How it works

<!-- portfolio-entry:robinhood-chain/commit/377ebee -->
### When the headline number quietly lies

Liquidity pools — shared pots of money that automated markets draw from — advertise a yield figure, but that number routinely omits the silent cost known as impermanent loss: the gap between what you deposited and what you could withdraw, caused by the pool's assets drifting in price relative to each other. This commit measures that gap directly for a specific pool and builds a clear-eyed accounting of what the position actually earns once the hidden drag is counted.

The point is not to expose a scandal but to make the system's decisions honest: a bot acting on the advertised number will behave very differently from one that knows what the position truly costs to hold.

- Separates advertised yield from realised return after accounting for price drift
- Makes the hidden cost visible before the bot acts on it

<sub>Python · Telegram · Blockchain</sub>
<!-- /portfolio-entry:robinhood-chain/commit/377ebee -->

## Recently shipped

## Stack notes
