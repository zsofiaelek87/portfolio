# Game Builder

**Stack:** TypeScript

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

<!-- portfolio:overview -->

> Mobile studio where kids describe a game and play it instantly — no code needed

## What it is

Children struggle to turn game ideas into something playable without learning to code. Game Builder lets a child type a sentence describing a game, refines that idea through an AI Prompt Coach, and renders a working game on the spot. It runs in the browser on any phone, with no installation required.

## How it works

1. Child types a free-form description of a game they want to play.
2. An AI Prompt Coach (powered by Google Gemini or local fallback logic) rewrites and sharpens the idea into a structured game spec.
3. The spec is handed to a Phaser 3 game engine that assembles and renders the game in the browser.
4. The finished game runs immediately inside the same PWA — no page change, no download.
5. Every push to main runs TypeScript checks, builds the app, and deploys to Firebase Hosting via GitHub Actions.

## What makes it interesting

<!-- portfolio-entry:game-builder/general/69b1567 -->
### A child describes a game, AI builds it immediately

A child types something like "a space shooter where the enemies get faster every wave" and, within seconds, a playable game appears — no code written, no tools installed, no adult required. Under the hood, a prompt coach (a conversational layer that asks clarifying questions before anything is generated) refines the idea, and a deterministic mapping — meaning the same description always produces the same game, reliably — selects and configures one of several ready-made game templates. The finished game gets its own shareable link anyone can open in a browser.

Six distinct game types are supported so far, from side-scrolling runners to a Vampire Survivors-style survival mode, each with its own procedurally generated levels, enemy waves, or power-ups. If the AI service is unreachable, the system falls back to local logic rather than failing silently.

- Same description always produces the same game — no randomness, no surprises
- Prompt coach refines vague ideas before the engine commits to a design
- Finished games get a public link, no account required to play

<sub>TypeScript · React · Phaser · Gemini AI · Firestore · Vite</sub>
<!-- /portfolio-entry:game-builder/general/69b1567 -->

- Prompt Coach has a local fallback: the app is fully functional without a Gemini API key, so it degrades gracefully rather than breaking.
- End-to-end pipeline converts a child's natural-language sentence into a live Phaser 3 game session — the write-up singles this out as the core engineering challenge.
- PWA architecture means the game studio installs and runs on a phone like a native app with no App Store dependency.
- CI/CD pipeline enforces TypeScript type-checking and linting before any build reaches Firebase Hosting, keeping a solo codebase stable.

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
