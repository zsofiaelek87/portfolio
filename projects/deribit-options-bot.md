# Deribit Options Bot

**Stack:** Python

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

<!-- portfolio:overview -->

> Automated options trading bot for Deribit with self-written daily reviews

## What it is

Discretionary options trading on crypto derivatives requires reviewing large amounts of historical data and tracking live volatility conditions consistently — work that is easy to skip and hard to systematize. This bot handles the research and monitoring loop for a solo trader on Deribit, surfacing the information needed to make a position decision or refusing to proceed when the data isn't there. It is built for a single operator who wants discipline enforced by code.

## How it works

1. Pulls up to five years of historical options data from Deribit before any trade is considered.
2. Checks whether a requested study has sufficient data and returns a clear refusal if it does not.
3. Measures realized versus implied volatility to quantify the volatility premium rather than assuming one exists.
4. Evaluates current market conditions against the historical record to support or reject a trade idea.
5. Writes a structured daily review of its own activity, positions, and market observations.
6. Executes or skips orders based on the outcome of that review cycle.

## What makes it interesting

- Five years of historical data are loaded and validated before any trade is risked, making recency bias structurally harder.
- The bot authors its own daily review, creating a written audit trail of decisions without manual journaling.
- Data requests return an explicit refusal when a study lacks sufficient history, rather than silently degrading to a smaller sample.
- Volatility premium is measured from realized versus implied volatility rather than assumed, grounding each trade in current evidence.

## Stack

Python

<!-- /portfolio:overview -->

## Recently shipped

<!-- portfolio-entry:deribit-options-bot/commit/b927ca4 -->
### Five years of data before risking a trade

An iron condor is a defined-risk options structure — a bet, with hard limits on what can be lost, that a market will stay inside a range. Before this commit, the bot decided whether to place one by looking at ninety days of historical volatility data. That window is long enough to feel thorough and short enough to be dangerously misleading: a calm recent quarter can sit inside a much more turbulent long-run picture.

The gating logic now requires five years of volatility history to confirm the trade thesis before anything is sent to the exchange. The practical consequence is that the bot will refuse to act when recent calm is an outlier rather than the norm — which is precisely when the structure would look attractive and actually be dangerous.

- Refuses to trade when short-term calm contradicts the five-year picture
- Hard-coded caution: the system declines, it does not ask the operator

<sub>Python · Telegram</sub>
<!-- /portfolio-entry:deribit-options-bot/commit/b927ca4 -->

<!-- portfolio-entry:deribit-options-bot/commit/ae3c324 -->
### A daily review the bot writes for itself

Once a day, the bot reads its own trading history — the positions it opened, what happened to them, and what the outcomes suggest — and produces a structured verdict on what to do next. Rather than asking a human to sift through logs and draw conclusions, the system turns its own evidence into a recommendation.

The practical consequence is a tighter feedback loop: the bot is not just acting on market conditions but actively learning from its own track record, on a schedule, without anyone having to prompt it.

- Self-review runs daily from the bot's own position history
- Output is a concrete recommendation, not a raw log dump

<sub>Python · Telegram</sub>
<!-- /portfolio-entry:deribit-options-bot/commit/ae3c324 -->

<!-- portfolio-entry:deribit-options-bot/commit/01e5d44 -->
### Data a study needs, or a clear refusal

Options volatility research lives or dies on having the right historical price index — the baseline record of where an asset traded over time. Before this commit, a missing index meant the long-gamma study (a strategy that profits from large price swings) would either run on incomplete data or fail in a way that was hard to diagnose. Now it fetches exactly what it needs upfront, and if that data is unavailable, it stops and says so clearly rather than continuing silently with a gap.

A system that refuses to proceed on bad inputs is more trustworthy than one that proceeds and quietly gets it wrong.

- Study-critical data fetched explicitly, not assumed to be present
- Missing inputs produce a clear failure, not silent bad output

<sub>Python · Telegram</sub>
<!-- /portfolio-entry:deribit-options-bot/commit/01e5d44 -->

<!-- portfolio-entry:deribit-options-bot/commit/cb37e74 -->
### Measuring volatility premium instead of assuming it

Most automated trading systems are built on an assumption: that the gap between expected and realised volatility — the so-called variance risk premium, the persistent tendency of options to overprice calm — is simply there to be harvested. This commit removes that assumption and replaces it with live measurement. Before the bot decides anything, it now calculates whether the premium actually exists in the current market rather than inferring it from historical habit.

The practical consequence is that the system can decline to act when conditions do not support the thesis, rather than pressing forward on stale logic.

- Premise verified at runtime, not baked in at build time
- Bot abstains when measured conditions do not support the trade

<sub>Python · Telegram · Deribit API</sub>
<!-- /portfolio-entry:deribit-options-bot/commit/cb37e74 -->
