# Short Videos

**Stack:** TypeScript

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

## What makes it interesting

<!-- portfolio-entry:shortvideos/general/fdc6bbf -->
### A video production engine that runs itself

Turning a topic into a finished, platform-ready short video normally means writing a script, sourcing footage, recording or cloning a voice, timing captions, and then manually uploading to each channel. This engine does all of that in sequence — planning, scripting, rendering, reviewing, and publishing — with a human approving the result rather than performing the steps.

It covers a wide range of formats (vertical reels, infographics, stickman explainers, before-and-after reveals, long-form YouTube pieces) and handles the mechanics that are easy to get wrong: it will not publish a video that fails a visual quality check, will not post the same slot twice, and retries only the failures it can safely retry.

- Approval gate sits between generation and publish — nothing goes live unreviewed
- Voice, captions, and b-roll are assembled automatically from approved assets
- YouTube OAuth, scheduling, thumbnails, and analytics snapshots all handled in-system

<sub>TypeScript · ElevenLabs · YouTube Data API · Manim · Google Cloud</sub>
<!-- /portfolio-entry:shortvideos/general/fdc6bbf -->

## How it works

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

## Recently shipped

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

## Stack notes
