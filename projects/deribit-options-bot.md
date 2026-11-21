# Deribit Options Bot

**Stack:** Python

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

<!-- portfolio:overview -->

> Automated options analysis and daily review for Deribit traders

## What it is

Discretionary options trading on Deribit requires consistent pre-trade research that is easy to skip or rush. This bot enforces that discipline by grading setups, pulling five years of historical context, and writing its own daily review before any position is considered. It is built for a solo trader who wants a repeatable process without a team.

## How it works

1. The bot fetches current options data from Deribit and organizes it by instrument lane.
2. Each lane is graded independently against defined criteria before it is considered tradeable.
3. Five years of historical data are loaded to give each setup long-run context.
4. Volatility premium is measured from the data rather than assumed from convention.
5. If a study lacks the data it needs, the bot refuses clearly rather than proceeding on incomplete input.
6. At the end of each day the bot composes a structured review of what it saw and decided.

## What makes it interesting

- Per-lane grading isolates each instrument so a weak setup in one contract cannot contaminate the analysis of another.
- Hard data-sufficiency checks cause the bot to issue a clear refusal rather than silently degrade to incomplete analysis.
- Volatility premium is derived by measurement against the historical record, removing the assumption that premium is always present.
- Five years of historical depth are loaded before any trade is considered, enforcing a long-run view as a system constraint.
- The self-written daily review creates an auditable log of the bot's own reasoning, not just its outputs.

## Stack

Python

<!-- /portfolio:overview -->

## Recently shipped

<!-- portfolio-entry:deribit-options-bot/commit/2f2821f -->
### Per-lane grading for the options analyst

An options analyst — the part of the bot that proposes trades by reading market conditions — now gets its questions evaluated category by category rather than as a single pass-or-fail judgment. Think of it as a scorecard with rows: each type of question the analyst asks is assessed on its own terms, so a weak question in one lane cannot hide behind strong answers in another.

The practical consequence is sharper accountability. When a trade idea is rejected, the system knows exactly which dimension failed, making it possible to improve the analyst's reasoning without guessing which part to fix.

- Each question category judged independently, not averaged away
- Failures are localized — the system knows which lane underperformed

<sub>Python · Telegram</sub>
<!-- /portfolio-entry:deribit-options-bot/commit/2f2821f -->

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
