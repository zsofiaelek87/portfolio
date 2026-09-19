# Binance Carry Bot

> Harvest cryptocurrency funding-rate premiums through delta-neutral positions via Telegram control

**Stack:** Python

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

## What makes it interesting

## How it works

<!-- portfolio-entry:binance-carry-bot/commit/944f5ff -->
### A trading arm that earns its own promotion

Before a new strategy is allowed near real positions, it spends time in a shadow mode — watching the market, logging what it would have done, and building a record. When that record is strong enough, the system promotes the arm automatically. When it is not, the system undoes the promotion itself. Nobody sets a calendar reminder to check; the feedback loop is closed entirely inside the bot.

The practical consequence is that every strategy earns its place through evidence rather than assumption, and the cost of being wrong about a new idea is a few forecasts, not a live position gone the wrong way.

- New strategies run in forecast-only mode before touching live positions
- Automatic promotion and automatic rollback, both driven by evidence
- No human intervention needed to approve or reject a new trading arm

<sub>Python · Binance API · Telegram</sub>
<!-- /portfolio-entry:binance-carry-bot/commit/944f5ff -->

## Recently shipped

## Stack notes
