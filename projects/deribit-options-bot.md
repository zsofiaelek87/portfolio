# Deribit Options Bot

> Trade cryptocurrency options volatility risk premium using defined-risk structures via Telegram control

**Stack:** Python

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

## What makes it interesting

## How it works

<!-- portfolio-entry:deribit-options-bot/commit/b927ca4 -->
### Five years of data before risking a trade

An iron condor is a defined-risk options structure — a bet, with hard limits on what can be lost, that a market will stay inside a range. Before this commit, the bot decided whether to place one by looking at ninety days of historical volatility data. That window is long enough to feel thorough and short enough to be dangerously misleading: a calm recent quarter can sit inside a much more turbulent long-run picture.

The gating logic now requires five years of volatility history to confirm the trade thesis before anything is sent to the exchange. The practical consequence is that the bot will refuse to act when recent calm is an outlier rather than the norm — which is precisely when the structure would look attractive and actually be dangerous.

- Refuses to trade when short-term calm contradicts the five-year picture
- Hard-coded caution: the system declines, it does not ask the operator

<sub>Python · Telegram</sub>
<!-- /portfolio-entry:deribit-options-bot/commit/b927ca4 -->

## Recently shipped

<!-- portfolio-entry:deribit-options-bot/commit/cb37e74 -->
### Measuring volatility premium instead of assuming it

Most automated trading systems are built on an assumption: that the gap between expected and realised volatility — the so-called variance risk premium, the persistent tendency of options to overprice calm — is simply there to be harvested. This commit removes that assumption and replaces it with live measurement. Before the bot decides anything, it now calculates whether the premium actually exists in the current market rather than inferring it from historical habit.

The practical consequence is that the system can decline to act when conditions do not support the thesis, rather than pressing forward on stale logic.

- Premise verified at runtime, not baked in at build time
- Bot abstains when measured conditions do not support the trade

<sub>Python · Telegram · Deribit API</sub>
<!-- /portfolio-entry:deribit-options-bot/commit/cb37e74 -->

## Stack notes
