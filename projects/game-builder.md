# Game Builder

> Children describe a game, AI refines the concept, and an engine renders it instantly without coding

**Stack:** TypeScript

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

## What makes it interesting

## How it works

<!-- portfolio-entry:game-builder/general/f5e5417 -->
### Turning a child's sentence into a playable game

The hard part is not generating a game — it is generating the *right* game reliably. A child's description is vague, misspelled, and emotionally charged, so a naive prompt-to-code approach produces inconsistent output that breaks silently. The design fix was a deterministic mapping layer that classifies intent first and routes it to a small set of proven game templates (side-scroller, snake, survival swarm), then fills template parameters from the prompt rather than generating freeform logic. This keeps the output space bounded and testable while still feeling generative to the child.

Shareable links were added without a backend by writing game state to Firestore through the public Web API and encoding a short slug into the URL, so a child can hand a link to a parent with no account required.

- Deterministic intent classification keeps generation failure rate near zero
- Stateless sharing via public Firestore — no auth, no server round-trip
- Three distinct game genres covered by composable template architecture

<sub>TypeScript · Firestore · Generative AI · Game Templates</sub>
<!-- /portfolio-entry:game-builder/general/f5e5417 -->

## Recently shipped

## Stack notes
