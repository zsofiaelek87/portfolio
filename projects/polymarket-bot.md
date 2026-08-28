# Polymarket Bot

**Stack:** Python

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

## What makes it interesting

## How it works

<!-- portfolio-entry:polymarket-bot/commit/f5af653 -->
### Tracking behaviour, not every move

Prediction-market bots generate a stream of events, and naively treating every one as significant produces a system that overreacts to noise and misses the patterns that actually matter. This commit shifts how the bot groups and remembers its own activity: rather than stamping a record on each individual action, it keys its memory on stretches of meaningful behaviour — what the system was *doing*, not just what it did last.

The practical consequence is a bot that learns from patterns rather than moments, and stays coherent over time without accumulating clutter it has to reason around.

- Cohort logic groups behaviour into meaningful patterns, not raw event counts
- Reduces noise so the system responds to what matters, not everything that moves

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/f5af653 -->

<!-- portfolio-entry:polymarket-bot/commit/c673381 -->
### A bot that quotes the side nobody wants

In a two-sided market, a bot naturally accumulates whichever contract is easy to sell and runs short of the one everyone wants. The naive fix is to pull back from the imbalanced side. This commit does the opposite: when inventory tilts, the bot shades its price on the *missing* leg toward the midpoint — making it more attractive — rather than pushing the surplus leg further away. The result is a position that self-corrects through normal trading flow, without the bot ever stepping back from the market to rebalance manually.

- Inventory imbalance is corrected by attracting trades, not by retreating from them
- Skew logic is inverted: the underweight side gets the better price, automatically

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/c673381 -->

<!-- portfolio-entry:polymarket-bot/commit/6ebc3be -->
### Fixing the spread that was skewing every fill

On a prediction market, the "spread" is the gap between the price you can buy at and the price you can sell at — and if a bot triggers its trades at the edges of that gap, the gap itself quietly works against every single position. This commit moves the trigger point to the midprice, the true centre between those two extremes, so the bot is no longer paying a hidden tax on its own activity.

Small as it sounds, a skewed entry compounds across many trades. Fixing it at the trigger level means the correction applies automatically to everything the bot does from that point forward.

- Entry logic now references the midprice, removing systematic spread bias
- One fix propagates across all future trades without per-trade adjustments

<sub>Python · Polymarket</sub>
<!-- /portfolio-entry:polymarket-bot/commit/6ebc3be -->

<!-- portfolio-entry:polymarket-bot/commit/d68b94c -->
### Memory that only records what it knew

A prediction-market bot builds up a picture of what it has seen and acted on — but only if it actually saw it. This commit enforces a clean boundary: if the AI model was never consulted about a market, that market does not get written into the bot's memory at all. It sounds like bookkeeping, but the consequence is real. A system that remembers things it never properly evaluated will eventually act on those ghost records as if they were informed decisions. Excluding them keeps the bot's internal picture honest.

- Memory entries are gated on whether the model actually evaluated the market
- Prevents ghost records from influencing future decisions

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/d68b94c -->

## Recently shipped

## Stack notes
