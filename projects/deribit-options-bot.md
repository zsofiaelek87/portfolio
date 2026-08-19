# Deribit Options Bot

> Trade cryptocurrency options volatility risk premium using defined-risk structures via Telegram control

**Stack:** Python

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

## What makes it interesting

## How it works

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
