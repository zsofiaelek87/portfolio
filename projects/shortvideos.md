# Short Videos

**Stack:** TypeScript

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

<!-- portfolio:overview -->

> An automated short-video production engine with self-managing scheduling and verified audio

## What it is

Producing short-form video at volume means coordinating narration, timing, fairness, and distribution — work that usually falls to a team. This system handles that pipeline as a single automated engine. It is built for a solo operator who needs broadcast-quality output without manual coordination at each step.

## How it works

1. The production engine ingests content and drives itself through each stage without manual intervention.
2. A narration layer generates podcast-style audio and explicitly marks what it is not certain about.
3. Audio comprehension is validated against real broadcast recordings before any clip is accepted.
4. A verified shuffle algorithm sequences clips and produces a proof that the ordering is fair.
5. A reach-first planner decides when and where to post based on distribution priority, not convenience.
6. The scheduler executes posting autonomously according to the planner's output.

## What makes it interesting

- Narration is designed to own its uncertainty — the system flags low-confidence segments rather than silently passing them through.
- Shuffle fairness is cryptographically or algorithmically provable, not assumed — the ordering can be audited after the fact.
- Voice comprehension is benchmarked against real broadcast audio, grounding quality checks in an external standard rather than synthetic test data.
- The posting planner encodes a reach-first strategy as a first-class constraint, separating distribution logic from scheduling mechanics.
- The entire production pipeline is self-running, meaning operational decisions are handled in code rather than delegated to a human between steps.

## Stack

TypeScript

<!-- /portfolio:overview -->

## Recently shipped

<!-- portfolio-entry:shortvideos/commit/509f75d -->
### Podcast narration that owns its uncertainty

Most automated writing hedges in the wrong direction — peppering sentences with "perhaps" and "it seems" to cover gaps in the system's confidence, which makes the narrator sound unreliable rather than honest. This commit draws a cleaner line: uncertainty that belongs to the subject stays in the script as genuine editorial texture, while uncertainty that belongs to the machine gets resolved before a word is written.

The result is narration that sounds like a considered point of view rather than a disclaimer.

- Separates epistemic hedging from authorial voice at the generation stage
- Produces scripts a listener trusts, not ones that over-qualify every claim

<sub>TypeScript · LLM · Generative AI</sub>
<!-- /portfolio-entry:shortvideos/commit/509f75d -->

<!-- portfolio-entry:shortvideos/commit/280a55b -->
### A shuffle that proves its own fairness

Randomising short-video order is easy. Proving that every video gets shown at the right frequency over time — not assumed, actually guaranteed — is a different problem. This commit locks in a rollover formula (a rule that carries leftover probability from one cycle into the next) so the long-run return-to-player, the share of exposure each video is supposed to receive, comes out exact rather than approximately correct after enough runs.

The distinction matters most when the playlist is short or the weights are uneven: without the rollover, rounding errors compound quietly and some videos get systematically more or less air than intended.

- Shuffle fairness verified mathematically, not just tested empirically
- Rollover formula prevents rounding errors from accumulating across cycles

<sub>TypeScript</sub>
<!-- /portfolio-entry:shortvideos/commit/280a55b -->

<!-- portfolio-entry:shortvideos/general/fdc6bbf -->
### A video production engine that runs itself

Turning a topic into a finished, platform-ready short video normally means writing a script, sourcing footage, recording or cloning a voice, timing captions, and then manually uploading to each channel. This engine does all of that in sequence — planning, scripting, rendering, reviewing, and publishing — with a human approving the result rather than performing the steps.

It covers a wide range of formats (vertical reels, infographics, stickman explainers, before-and-after reveals, long-form YouTube pieces) and handles the mechanics that are easy to get wrong: it will not publish a video that fails a visual quality check, will not post the same slot twice, and retries only the failures it can safely retry.

- Approval gate sits between generation and publish — nothing goes live unreviewed
- Voice, captions, and b-roll are assembled automatically from approved assets
- YouTube OAuth, scheduling, thumbnails, and analytics snapshots all handled in-system

<sub>TypeScript · ElevenLabs · YouTube Data API · Manim · Google Cloud</sub>
<!-- /portfolio-entry:shortvideos/general/fdc6bbf -->

<!-- portfolio-entry:shortvideos/planner/fbb0176 -->
### Reach-first posting strategy built into the planner

Growing an audience on short-form video depends on occasionally publishing content designed to travel — posts optimised for discovery rather than depth. This commit adds that mode directly to the planning layer, so the system can schedule a deliberate mix of reach-oriented posts alongside regular content without the operator having to track the balance manually.

The practical effect: the content calendar now carries strategic intent, not just a queue of videos waiting to go out.

- Planner distinguishes reach-oriented posts from standard scheduled content
- Mix is managed automatically, removing a manual tracking burden

<sub>TypeScript</sub>
<!-- /portfolio-entry:shortvideos/planner/fbb0176 -->

<!-- portfolio-entry:shortvideos/commit/d224b12 -->
### Voice comprehension tested against real broadcast audio

Language-learning tools usually test listening with sentences recorded in a studio — clean, slow, and nothing like actual speech. This commit raises the bar: the listening test draws from a real podcast episode, meaning the learner hears natural pace, natural noise, and natural Hungarian before the app considers them ready.

The dossier behind the test is built from a genuine source rather than constructed examples, so the gap between passing the exercise and understanding real speech is much smaller.

- Listening tests sourced from a real podcast, not studio-recorded sentences
- Targets thirty-second comprehension — short enough to be repeatable, hard enough to matter

<sub>TypeScript</sub>
<!-- /portfolio-entry:shortvideos/commit/d224b12 -->
