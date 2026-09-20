# Game Builder

**Stack:** TypeScript

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

<!-- portfolio:overview -->

> Mobile game remixer for kids: describe a game idea, play it in seconds

## What it is

Children often have vivid game ideas but no way to act on them without coding skills. Game Builder lets a child type or speak a plain-language description of a game, refines that idea through an AI Prompt Coach, and renders a playable result instantly in the browser. It is designed for young users on phones and requires no installation.

## How it works

1. A child enters a freeform description of the game they want to make.
2. An AI Prompt Coach — backed by Google Gemini or local fallback logic — sharpens the description into a structured game specification.
3. The specification is handed to a Phaser 3 game engine running entirely in the browser.
4. Phaser renders a playable game in real time without any page reload.
5. The finished game runs as a mobile-first PWA, so it works on any phone browser without an app store.

## What makes it interesting

- Graceful AI fallback: the app ships with local prompt-improvement logic so it remains fully functional when no Gemini API key is present.
- The core engineering challenge — translating a child's single sentence into a runnable Phaser 3 game — is the explicit focus of the published write-up, signalling deliberate work on the prompt-to-engine contract.
- CI/CD pipeline runs TypeScript checks, linting, and a production build on every push to main before deploying to Firebase Hosting via GitHub Actions.
- PWA architecture means the game creator and the created games load on any phone browser with no native install step.

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
