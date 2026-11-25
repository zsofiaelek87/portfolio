# Dev Team

**Stack:** TypeScript

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

<!-- portfolio:overview -->

> Routes rough ideas through a gated PM workflow to reviewed GitHub PRs — semi-automatically

## What it is

Turning a vague idea into a merged pull request normally requires a developer to context-switch through requirements gathering, planning, and execution. This orchestrator handles that pipeline end to end, with a PM Gatekeeper that structures the incoming idea before any code is written. It is built for a solo operator who wants to stay in control of merges while automating the tedious steps in between.

## How it works

1. An operator submits a rough idea or nontechnical report through the Next.js dashboard.
2. The PM Gatekeeper separates evidence from unknowns, asks only answerable clarification questions, and produces a testable structured specification.
3. The plan passes through a requirements gate and a plan gate before any execution begins.
4. A Node.js worker service running on an Ubuntu VM picks up the approved task and executes it inside a hardened sandbox that holds no orchestrator secrets.
5. The worker creates a branch and opens a pull request on GitHub via a GitHub App integration.
6. The operator reviews the PR; automatic merge is available but off by default via the AUTO_MERGE_PRS flag.

## What makes it interesting

- Executor sandbox is explicitly isolated from orchestrator secrets (D1), so a runaway task cannot exfiltrate credentials.
- Hard per-task ceilings on wall-clock time, turns, and tokens prevent runaway cost; dollar figures are display-only estimates rather than enforced limits (D3).
- Hybrid subscription-first model routing uses a fresh Codex session for planning and a bounded model ladder for execution, with paid API as an explicit capacity-only fallback — not the default path.
- Spec Assist separates the intake concern from the execution concern: the Gatekeeper converts a nontechnical report into a structured spec so each fresh model context starts with verified, testable requirements.
- A day-planner integration protects building time, keeping the operator's calendar aware of when the system is running tasks.

## Stack

TypeScript · Next.js · Node.js · Firebase Firestore · Firebase Authentication · GitHub Apps · Anthropic · pnpm · systemd · Ubuntu

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
