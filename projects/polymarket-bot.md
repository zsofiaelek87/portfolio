# Polymarket Bot

**Stack:** Python

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

## What makes it interesting

## How it works

<!-- portfolio-entry:polymarket-bot/commit/d68b94c -->
### Memory that only records what it knew

A prediction-market bot builds up a picture of what it has seen and acted on — but only if it actually saw it. This commit enforces a clean boundary: if the AI model was never consulted about a market, that market does not get written into the bot's memory at all. It sounds like bookkeeping, but the consequence is real. A system that remembers things it never properly evaluated will eventually act on those ghost records as if they were informed decisions. Excluding them keeps the bot's internal picture honest.

- Memory entries are gated on whether the model actually evaluated the market
- Prevents ghost records from influencing future decisions

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/d68b94c -->

## Recently shipped

## Stack notes
