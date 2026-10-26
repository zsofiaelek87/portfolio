# Dev Team

**Stack:** TypeScript

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

<!-- portfolio:overview -->

> Routes raw ideas through a PM gatekeeper to reviewed GitHub PRs with configurable auto-merge

## What it is

Turning a vague idea into a merged pull request normally requires a developer to hold the full context of requirements, planning, and execution in their head at once. This orchestrator breaks that into a gated pipeline: a PM Gatekeeper refines rough input into a testable specification before any code runs, and a sandboxed worker handles the rest. It is built for a solo operator who wants meaningful human oversight without approving every micro-decision.

## How it works

1. An operator submits a rough idea or nontechnical report through the Next.js dashboard.
2. The PM Gatekeeper's Spec Assist pass separates evidence from unknowns and produces a structured, testable specification.
3. A strong Codex session plans the work; the plan must clear a gate before execution begins.
4. Bounded Codex sessions execute the plan through a model ladder, with paid API as an explicit capacity-only fallback.
5. The worker runs inside a hardened executor sandbox on an Ubuntu VM, isolated from orchestrator secrets.
6. Completed work is pushed to a branch and opened as a GitHub PR, with merge requiring operator approval unless AUTO_MERGE_PRS is enabled.

## What makes it interesting

- Hardened executor sandbox (D1) keeps orchestrator secrets out of the execution environment by design.
- Hard per-task ceilings on wall-clock time, turns, and tokens prevent runaway jobs; cost figures are display-only estimates, never control values.
- Hybrid subscription-first model routing uses a model ladder so paid API calls are an explicit last resort, not the default path.
- The PM Gatekeeper asks only answerable clarification questions, ensuring every Codex context starts from a testable specification rather than ambiguous prose.
- Semi-autonomous by policy (D4): PR merge is always operator-gated, with auto-merge opt-in rather than opt-out.

## Stack

TypeScript · Next.js · Node.js · Firebase · Firestore · Firebase Authentication · GitHub Apps · Anthropic · pnpm · Ubuntu

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
