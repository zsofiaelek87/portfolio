# Hyperliquid Equity Bot

> Paper-trade tokenized-equity perpetual contracts on Hyperliquid protocol

**Stack:** Python

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

## What makes it interesting

## How it works

<!-- portfolio-entry:hyperliquid-equity-bot/commit/b00b75c -->
### Sizing up before the signal is certain

Most rule-based trading systems wait for a clean verdict before committing capital — which sounds prudent until you notice that prices move during the deliberation. This commit changes that posture: as evidence accumulates in favour of a trade, the bot begins building its position incrementally rather than waiting for a final threshold to flip. By the time conviction arrives, the entry is already underway.

The practical consequence is that the system treats certainty as a spectrum rather than a switch — a more honest model of how good decisions actually form under uncertainty.

- Position size grows in proportion to evidence, not all at once on a binary trigger
- Separates the decision to act from the decision about how much to act

<sub>Python · Hyperliquid</sub>
<!-- /portfolio-entry:hyperliquid-equity-bot/commit/b00b75c -->

<!-- portfolio-entry:hyperliquid-equity-bot/commit/8ad8b41 -->
### A bot that knows when to stop exploring

Paper-trading — running a strategy with simulated money to test it before risking real capital — generates a quiet temptation: keep trying new signals indefinitely, because nothing is technically "lost." This commit refuses that logic. It enforces a rolling exploration budget, meaning the system tracks how much of its testing capacity has been spent on unproven ideas over any given window, and stops opening new experimental positions once that allowance is used up.

The practical consequence is that curiosity has a cost even in simulation. A system that can explore without limit will eventually mistake noise for signal; one that rations exploration is forced to be selective about what it actually tests.

- Exploration is capped per rolling window, not just in total
- Prevents unlimited signal-fishing from corrupting paper-trade results

<sub>Python · Hyperliquid</sub>
<!-- /portfolio-entry:hyperliquid-equity-bot/commit/8ad8b41 -->

## Recently shipped

<!-- portfolio-entry:hyperliquid-equity-bot/commit/7bd4670 -->
### Honest silence instead of false confidence

Automated trading systems have a habit of projecting certainty on days when nothing meaningful is happening — filling quiet periods with noise rather than admitting the market simply has nothing to say. This commit does two things at once: it groups the logic that decides whether to act into a single, auditable switch (so the reasoning is in one place and reviewable), and it teaches the system to label genuinely uneventful days accurately rather than manufacturing a rationale.

The practical consequence is a bot that knows the difference between "no signal" and "bad signal" — and says so in its own output rather than hiding that distinction from whoever reads the log later.

- Quiet days are labelled as quiet rather than explained away
- Decision logic consolidated into one auditable control point

<sub>Python · Hyperliquid</sub>
<!-- /portfolio-entry:hyperliquid-equity-bot/commit/7bd4670 -->

<!-- portfolio-entry:hyperliquid-equity-bot/commit/790576a -->
### Critical signals promoted into the daily digest

A daily summary is only useful if it surfaces the things that actually require attention. Before this change, the bot's detector — the layer that watches position data for warning signs — could flag a problem without that flag making it into the report a human reads. Now, anything the detector marks as critical rises automatically into the digest, so the person receiving it sees the full picture without having to cross-reference a separate log.

The consequence is simple: a finding that matters cannot quietly stay buried while the summary lands clean.

- Critical alerts now flow directly into the human-readable digest
- No manual cross-referencing needed to catch a flagged condition

<sub>Python · Telegram</sub>
<!-- /portfolio-entry:hyperliquid-equity-bot/commit/790576a -->

<!-- portfolio-entry:hyperliquid-equity-bot/commit/6ef0fd7 -->
### Resting orders instead of chasing the price

Crossing the spread means buying at the ask or selling at the bid — paying a small tax on every single trade just to get filled immediately. This commit switches the bot to resting orders instead: it places its bid or offer and waits for the market to come to it, which costs nothing in spread. Alongside that, position size increased fourfold, making the combination meaningful: better entry economics at genuinely larger scale.

Both changes arrived in a single commit, which keeps the risk model coherent — you do not want a bigger bet with the old, more expensive fill logic still running underneath it.

- Passive order placement eliminates the per-trade spread cost entirely
- 4× position sizing applied consistently after the fill logic was fixed first

<sub>Python · Hyperliquid</sub>
<!-- /portfolio-entry:hyperliquid-equity-bot/commit/6ef0fd7 -->

<!-- portfolio-entry:hyperliquid-equity-bot/commit/33af1a4 -->
### A bot that knows when not to trust itself

Forecasting market direction is only half the problem — the other half is knowing whether the forecast is any good before acting on it. This commit adds a forecast-skill check to the bot's order-book detectors: a layer that evaluates its own predictive accuracy and can withhold a signal when the evidence does not support confidence. Rather than trading on every pattern it spots, the system now distinguishes between a genuine edge and noise that merely looks like one.

The practical consequence is a paper-trading engine that models intellectual honesty as well as strategy — useful both for refining signals and for trusting the output you do get.

- Detectors now self-evaluate: a signal only fires when skill is confirmed
- Order-book analysis paired with built-in forecast validation

<sub>Python · Hyperliquid</sub>
<!-- /portfolio-entry:hyperliquid-equity-bot/commit/33af1a4 -->

<!-- portfolio-entry:hyperliquid-equity-bot/commit/2eb4883 -->
### Stepping back when the news is coming

Certain stocks have known events on the calendar — earnings announcements, regulatory decisions, index rebalances — where price moves stop being driven by the usual signals and start being driven by a single piece of news no model can read in advance. This commit teaches the bot to recognise those moments and simply stand aside: if a name has a scheduled catalyst, no position is opened, no paper trade is logged, nothing happens until the window passes.

That kind of restraint is harder to build than it sounds. Knowing when *not* to act is the control a purely reactive system lacks.

- Detects scheduled catalysts and pauses trading automatically, without human intervention
- Refuses to open a position when the price signal is likely to be noise

<sub>Python</sub>
<!-- /portfolio-entry:hyperliquid-equity-bot/commit/2eb4883 -->

## Stack notes
