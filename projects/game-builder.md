# Game Builder

**Stack:** TypeScript

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

<!-- portfolio:overview -->

> Mobile PWA that turns a child's description into a playable game, no coding required

## What it is

Children often want to make games but hit a wall the moment any code appears. Game Builder lets a child type a plain-English description of a game, refines that idea through an AI Prompt Coach, and renders a playable result on-screen in seconds. It is aimed at kids using phones or browsers, with no installation and no programming knowledge needed.

## How it works

1. A child types a sentence describing the game they want to make.
2. The AI Prompt Coach rewrites or improves the description to make it more specific and game-ready.
3. The refined prompt is interpreted by local logic, with an optional call to Google Gemini for richer output.
4. Phaser 3 receives the structured game definition and renders an interactive game in the browser.
5. The finished build is served as a PWA from Firebase Hosting, so it works on any phone or desktop without an app store.

## What makes it interesting

- Graceful API fallback: the app runs entirely on local AI logic when no Gemini key is present, so the experience never breaks for users without a key.
- Prompt coaching layer sits between raw child input and the game engine, turning vague sentences into structured, renderable game definitions.
- CI/CD pipeline on every push to main: TypeScript checks, linting, build, and Firebase deploy all run automatically via GitHub Actions.
- PWA architecture means the game creation tool itself requires no installation and works on the same mobile devices children already have.

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
