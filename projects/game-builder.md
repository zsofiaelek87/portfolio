# Game Builder

**Stack:** TypeScript

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

<!-- portfolio:overview -->

> Mobile PWA that turns a child's game idea into a playable game with AI assistance

## What it is

Children struggle to make games without knowing how to code. Game Builder lets a child describe a game in plain words, refines that description with an AI Prompt Coach, and renders a playable game instantly in the browser — no programming required.

## How it works

1. A child types or speaks a description of a game they want to play.
2. An AI Prompt Coach (powered by Google Gemini or local fallback logic) improves and clarifies the idea.
3. The refined prompt is handed to a Phaser 3 game engine that constructs a playable game on the fly.
4. The result renders immediately in the browser as a mobile-first PWA — no install needed.
5. Every push to main runs TypeScript checks, a Vite build, and an automated deploy to Firebase Hosting via GitHub Actions.

## What makes it interesting

- AI prompt refinement runs fully offline via local fallback logic when no Gemini API key is present, keeping the app functional without a network dependency.
- The pipeline goes from a single natural-language sentence to a running Phaser 3 game — bridging generative AI output directly to a real-time game engine.
- Mobile-first PWA architecture means the game creation and play loop works on a phone browser with no app store install.
- CI/CD pipeline enforces TypeScript strict checks and linting before any build reaches production, gating deploy quality automatically.

## Stack

TypeScript · React 18 · Vite · Tailwind CSS · Phaser 3 · Firebase Hosting · Google Gemini API · GitHub Actions

<!-- /portfolio:overview -->

## Recently shipped

<!-- portfolio-entry:game-builder/general/f5e5417 -->
### Turning a child's sentence into a playable game

The hard part is not generating a game — it is generating the *right* game reliably. A child's description is vague, misspelled, and emotionally charged, so a naive prompt-to-code approach produces inconsistent output that breaks silently. The design fix was a deterministic mapping layer that classifies intent first and routes it to a small set of proven game templates (side-scroller, snake, survival swarm), then fills template parameters from the prompt rather than generating freeform logic. This keeps the output space bounded and testable while still feeling generative to the child.

Shareable links were added without a backend by writing game state to Firestore through the public Web API and encoding a short slug into the URL, so a child can hand a link to a parent with no account required.

- Deterministic intent classification keeps generation failure rate near zero
- Stateless sharing via public Firestore — no auth, no server round-trip
- Three distinct game genres covered by composable template architecture

<sub>TypeScript · Firestore · Generative AI · Game Templates</sub>
<!-- /portfolio-entry:game-builder/general/f5e5417 -->
