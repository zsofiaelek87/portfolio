# Dev Team

**Stack:** TypeScript

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

<!-- portfolio:overview -->

> Routes rough ideas through a PM gatekeeper to reviewed GitHub PRs with configurable auto-merge

## What it is

Turning a vague idea into a mergeable pull request normally requires context-switching across planning, spec writing, and code review. This orchestrator handles that pipeline semi-autonomously: a PM Gatekeeper refines the idea into a testable specification before any code runs, and a sandboxed VM worker produces a branch and PR for operator review. It is built for a solo operator who wants to stay in control of merges without doing the scaffolding work by hand.

## How it works

1. An operator submits a rough idea or nontechnical report through the Next.js dashboard.
2. The PM Gatekeeper separates evidence from unknowns, asks only answerable clarification questions, and produces a structured specification.
3. The specification passes a requirements gate and a plan gate before any execution begins.
4. A Node.js worker service running on an Ubuntu VM picks up the approved task from Firestore.
5. The worker executes inside a hardened sandbox that holds no orchestrator secrets, then pushes a branch and opens a pull request.
6. The operator reviews the PR; with AUTO_MERGE_PRS enabled, low-risk PRs can be merged automatically.

## What makes it interesting

- Hardened executor sandbox (D1): orchestrator secrets are never passed inside the execution environment, limiting blast radius if generated code misbehaves.
- Hard per-task ceilings on wall-clock time, turns, and tokens (D3) enforce resource bounds independently of cost estimates, which are display-only.
- Hybrid subscription-first model routing: a fresh session handles planning while bounded sessions step down a model ladder for execution, with paid API as an explicit capacity-only fallback.
- Spec Assist separates evidence from unknowns at intake so every executor context starts from a testable structured specification rather than a raw prompt.
- Semi-autonomous by design (D4): PR merge is always an operator decision, making automation opt-in rather than the default failure mode.

## Stack

TypeScript · Next.js · Node.js · Firebase · Firestore · Firebase Authentication · GitHub Apps · Anthropic · pnpm · systemd

<!-- /portfolio:overview -->

## Recently shipped

<!-- portfolio-entry:dev-team/commit/b9004b1 -->
### A day planner that protects building time

Backlog items waiting on a single person have a habit of piling up invisibly — present on the list, absent from the day. This commit gives the day planner a scheduling pass that looks at everything blocked on her and books the highest-priority items into three real calendar slots, rather than leaving the selection to willpower or memory.

Three slots is a deliberate constraint, not a default. It keeps the day from becoming a triage exercise and leaves room for the work that does not arrive pre-labelled.

- Automatically selects and schedules work that is waiting on one person
- Hard cap of three slots per day prevents overcommitment by design

<sub>TypeScript · GitHub</sub>
<!-- /portfolio-entry:dev-team/commit/b9004b1 -->
