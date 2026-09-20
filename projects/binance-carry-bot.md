# Binance Carry Bot

**Stack:** Python

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

<!-- portfolio:overview -->

> Automated carry-trade executor that self-evaluates its own performance history

## What it is

Carry trading on perpetual futures requires constant monitoring of funding rates and disciplined position sizing — work that is easy to automate badly. This bot manages the full cycle of a carry trade and, crucially, reads its own historical results to inform the next decision. It is built for a solo trader who wants execution and self-correction running without manual intervention.

## How it works

1. Monitors funding rates and spot-futures spreads to identify carry opportunities.
2. Opens and sizes positions according to rules derived from past performance.
3. Logs each trade and its outcome to a persistent track record.
4. Re-reads that track record to evaluate whether the current trading arm has earned continued operation.
5. Promotes or demotes the active strategy based on its own measured results.

## What makes it interesting

- Strategy self-evaluation: the bot reads its own trade log and uses the results to gate future activity, rather than running open-loop.
- Earned promotion model: a trading arm must demonstrate performance before being allowed to scale up, reducing runaway losses from a misconfigured strategy.
- Autonomous decision loop: promotion and demotion logic runs without manual review, making the feedback cycle part of the execution engine rather than an offline process.

## Stack

Python

<!-- /portfolio:overview -->

## Recently shipped

<!-- portfolio-entry:binance-carry-bot/commit/944f5ff -->
### A trading arm that earns its own promotion

Before a new strategy is allowed near real positions, it spends time in a shadow mode — watching the market, logging what it would have done, and building a record. When that record is strong enough, the system promotes the arm automatically. When it is not, the system undoes the promotion itself. Nobody sets a calendar reminder to check; the feedback loop is closed entirely inside the bot.

The practical consequence is that every strategy earns its place through evidence rather than assumption, and the cost of being wrong about a new idea is a few forecasts, not a live position gone the wrong way.

- New strategies run in forecast-only mode before touching live positions
- Automatic promotion and automatic rollback, both driven by evidence
- No human intervention needed to approve or reject a new trading arm

<sub>Python · Binance API · Telegram</sub>
<!-- /portfolio-entry:binance-carry-bot/commit/944f5ff -->

<!-- portfolio-entry:binance-carry-bot/commit/f06eac7 -->
### A bot that reads its own track record and decides what to do next

Once a day, the bot reviews its own trading history — every position it opened, every one it declined, and the reasoning behind each call — and produces a plain verdict on what that evidence suggests. This is the kind of reflective audit a disciplined human trader would do over coffee on a Sunday morning, except it runs automatically and without anyone sitting down to do it.

The value is in closing a loop that most automated systems leave open: the bot is not just acting on the market, it is accountable to its own past behavior, and the output is actionable rather than decorative.

- Daily self-review pass surfaces patterns a human would otherwise miss
- Conclusions drawn from the bot's own logged evidence, not external signals

<sub>Python · Telegram</sub>
<!-- /portfolio-entry:binance-carry-bot/commit/f06eac7 -->
