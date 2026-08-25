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

<!-- portfolio-entry:polymarket-bot/commit/1f10798 -->
### Detectors that investigate without being asked

Most monitoring tools wait for a question. This commit turns that around: a layer of detectors now runs its own investigation continuously, scanning prediction-market conditions for changes that meet criteria worth acting on — without anyone deciding when to look.

The practical consequence is that the system catches meaningful shifts between check-ins rather than only at them. The detection logic is also structured to grow more sophisticated over time without requiring the surrounding pipeline to be rebuilt each time a new signal is added.

- Proactive detection: flags market changes before a human thinks to check
- Designed so new detection rules slot in without rewiring the pipeline

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/1f10798 -->

## Stack notes
