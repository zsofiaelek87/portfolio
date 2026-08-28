# Recruit Lead Engine

**Stack:** TypeScript

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

## What makes it interesting

## How it works

## Recently shipped

<!-- portfolio-entry:recruit-lead-engine/commit/b363768 -->
### Safer contact discovery with built-in queue guardrails

Recruiting pipelines fail quietly — the same person gets contacted twice, a bad address makes it into the queue, or the system races ahead faster than a human can review. This commit tightens both ends of that problem: the logic that finds and qualifies contacts, and the safety layer that controls the order and pace at which those contacts enter the outreach queue.

The result is a sourcing engine that moves faster than manual research but refuses to outrun its own guardrails.

- Contact discovery hardened against duplicate and unsafe entries
- Outreach queue now enforces ordering and safety checks before contacts advance

<sub>TypeScript</sub>
<!-- /portfolio-entry:recruit-lead-engine/commit/b363768 -->

## Stack notes
