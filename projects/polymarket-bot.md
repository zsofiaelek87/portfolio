# Polymarket Bot

**Stack:** Python

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

## What makes it interesting

## How it works

<!-- portfolio-entry:polymarket-bot/commit/a3dc87e -->
### Detectors wired into every digest layer

Prediction markets generate a lot of noise, and the real work is knowing which signals are worth acting on. This commit connects a set of detectors — rules that flag meaningful changes in market conditions — into every daily digest the system produces, so nothing slips through unexamined. The tests and the scoped groundwork for a second layer of analysis mean the detection logic can grow more sophisticated without rewiring the digest pipeline each time.

The practical consequence: the system does not just summarise what happened. It watches for what matters.

- Detectors run across every digest automatically, not on request
- Layered architecture lets detection logic expand without rebuilding the output pipeline

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/a3dc87e -->

## Recently shipped

## Stack notes
