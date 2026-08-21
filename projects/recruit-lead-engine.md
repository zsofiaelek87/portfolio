# Recruit Lead Engine

**Stack:** TypeScript

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

## What makes it interesting

## How it works

## Recently shipped

<!-- portfolio-entry:recruit-lead-engine/commit/e47eb1e -->
### The engine that never sends the same email twice

Automated outreach is only as trustworthy as its worst failure mode, and the worst one is a live system that fires the same message repeatedly because nothing was watching for it. This commit closes that gap by adding a duplicate guard — a persistent check that sits in front of the sending path and refuses to let the same contact receive the same message more than once, regardless of how many times the daemon restarts or how quickly it runs.

- Duplicate guard runs before every send, not as a cleanup step after
- Protection survives daemon restarts and repeated execution cycles

<sub>TypeScript</sub>
<!-- /portfolio-entry:recruit-lead-engine/commit/e47eb1e -->

## Stack notes
