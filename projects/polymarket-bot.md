# Polymarket Bot

**Stack:** Python

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

## What makes it interesting

## How it works

<!-- portfolio-entry:polymarket-bot/commit/21598b6 -->
### Knowing what a portfolio cannot lose

Before a prediction-market bot acts on any position, it needs an honest picture of where it is exposed — not just where it expects to profit. This commit adds measurement of two specific books: equity-linked contracts and a short-volatility position (a bet that prices will stay calm). Both carry downside that is easy to ignore when things are going well and painful to discover when they are not.

Encoding that accounting in software means the exposure is visible every time the bot runs, not only when someone thinks to check.

- Downside measured automatically, before any position is acted on
- Covers correlated risk across two structurally different book types

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/21598b6 -->

## Recently shipped

## Stack notes
