# Study App

> Two Hungarian study applications managed from a single GitHub repository and deployed on Firebase

**Stack:** TypeScript

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

<!-- portfolio:overview -->

> Two Firebase-hosted Hungarian study apps for kids, managed from one monorepo

## What it is

Children studying Hungarian reading and primary-school science need practice that fits their exact curriculum and age. This monorepo delivers two separate web apps—one per child—each with age-appropriate content, offline support, and a PIN-protected parent dashboard. A solo builder maintains both from a single GitHub repository without ever duplicating CI/CD work.

## How it works

1. Content is authored once per app using lesson files in each sub-app directory.
2. A push to main triggers path-filtered GitHub Actions workflows that deploy only the changed app.
3. Firebase credentials and the parent-dashboard PIN are injected at build time from GitHub Secrets, keeping all sensitive values out of source.
4. Each app is built and deployed to its own Firebase Hosting project, keeping the two children's data entirely separate.
5. Service workers enable both apps to run fully offline on any device after the first load.
6. A daily mission flow surfaces the next lesson automatically, so children always know where to start.

## What makes it interesting

- Path-filtered CI/CD: changes under orsi-studyapp/** and matyi-studyapp/** each trigger a dedicated deploy workflow, so one child's update never risks the other's deployment.
- Per-app Firebase isolation: each app targets a separate Firebase project with its own credentials, preventing any cross-contamination of data or configuration.
- PIN-gated parent dashboard baked in as a first-class feature, with the PIN injected via GitHub Secrets rather than hardcoded.
- AI-generated curriculum content feeding a structured quiz engine, with lessons written once and propagated everywhere instantly.
- Offline-first architecture ensures the apps remain fully usable on any device regardless of connectivity.

## Stack

TypeScript · Firebase Hosting · GitHub Actions

<!-- /portfolio:overview -->

## What makes it interesting

<!-- portfolio-entry:studyapp/commit/a4f5d6f -->
### Two apps, one codebase, AI-generated content

Building a language-learning app means producing a lot of content — vocabulary sets, quiz questions, audio, mascot interactions — before a single learner touches it. Rather than manage that manually, this first session wired up AI generation directly to Firestore (a cloud document store), so content is created and stored without a copy-paste step in between.

Because both Hungarian study apps share a single codebase and the same data layer, every new feature — gender-aware avatars, speech playback, multiple quiz modes — ships to both products the moment it lands.

- AI-generated study content writes itself to the database without manual transfer
- One commit delivers new features to two separate apps simultaneously

<sub>TypeScript · Firebase · Firestore · GitHub Actions</sub>
<!-- /portfolio-entry:studyapp/commit/a4f5d6f -->

## Recently shipped

<!-- portfolio-entry:studyapp/commit/eb51e98 -->
### A quiz game built for Hungarian third-graders

Környezet Kaland ("Environment Adventure") is a subject-specific quiz game aimed at children studying Hungarian at the third-grade level. Shipping it as the first app in this monorepo — one shared codebase that handles both study products — means every structural decision made here, routing, data storage, build pipeline, carries forward to the second app automatically rather than being rebuilt from scratch.

For a solo builder, that upfront discipline is the leverage: one initial build, two deployable products.

- First complete app shipped inside a shared two-product codebase
- Subject-specific content designed for Hungarian primary-school curriculum

<sub>TypeScript · Firebase · GitHub Actions</sub>
<!-- /portfolio-entry:studyapp/commit/eb51e98 -->

<!-- portfolio-entry:studyapp/commit/bc31ef9 -->
### Richer lesson worlds to keep learners coming back

Keeping a learner engaged past the first few sessions is harder than teaching them anything. This commit deepens the content experience in both Hungarian study apps — expanding the lesson worlds and the hooks that make returning feel worthwhile, rather than obligatory.

Because both apps are built from a single shared codebase, the improvement landed in both products simultaneously. One design decision, two products better for it.

- Engagement improvements deployed to two apps from a single change
- Richer lesson structure built without duplicating engineering effort

<sub>TypeScript · Firebase · GitHub</sub>
<!-- /portfolio-entry:studyapp/commit/bc31ef9 -->

<!-- portfolio-entry:studyapp/commit/8b9a805 -->
### Lessons written once, live everywhere instantly

Generating a lesson by hand and then copying it into a database is exactly the kind of repetitive work that slows down a content pipeline. This commit closes that gap: lessons produced by an automated generation step are written directly into Firestore — a cloud document store — the moment they are ready, with no manual transfer step in between.

Because both Hungarian study apps draw from the same data layer, a lesson generated once is available to both products simultaneously. The content pipeline now runs end to end without a human in the middle.

- Generated lessons reach both apps without any manual copy step
- Single generation pipeline serves two live products at once

<sub>Firebase · Firestore · TypeScript · GitHub Actions</sub>
<!-- /portfolio-entry:studyapp/commit/8b9a805 -->

<!-- portfolio-entry:studyapp/commit/5ef5825 -->
### Both apps work offline, on any device

Installing a web app to your home screen — so it opens like a native app and keeps working when your signal drops — is called a PWA (progressive web app). Adding that capability to two separate products usually means two separate rounds of work. Because both Hungarian study apps share a single codebase, the offline support and the new navigation button shipped to both simultaneously, with no duplication.

The practical result: a learner can open either app on a train with no connection and carry on exactly where they left off.

- Offline-ready: lessons remain accessible without an internet connection
- One codebase change propagates to both apps at once

<sub>TypeScript · Firebase · PWA · GitHub Actions</sub>
<!-- /portfolio-entry:studyapp/commit/5ef5825 -->

<!-- portfolio-entry:studyapp/commit/0e8bea2 -->
### A daily mission flow that guides learners step by step

Language learning apps live or die by whether users return the next day. This commit ships a complete daily mission flow — a structured sequence of tasks that takes a learner from opening the app to a clear sense of accomplishment, without them having to decide what to do next at each step. The full loop is composed from smaller, reusable pieces, which means both Hungarian study apps inherit it from shared code rather than each maintaining their own copy.

- Full daily mission loop delivered to both apps simultaneously from one codebase
- Structured task sequence removes the "what do I do now?" decision for the learner

<sub>TypeScript · Firebase</sub>
<!-- /portfolio-entry:studyapp/commit/0e8bea2 -->
