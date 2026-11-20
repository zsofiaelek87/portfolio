# Short Videos

**Stack:** TypeScript

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

<!-- portfolio:overview -->

> An automated short-video production and posting engine for solo creators

## What it is

Keeping a short-video channel running consistently is a scheduling, scripting, and distribution problem that compounds quickly for one person. This system automates the full pipeline from content selection through narration and posting, with logic that tracks what has already been said and chooses what to publish next. It is built for a single operator who wants the channel to run without daily intervention.

## How it works

1. A planner selects the next topic using a reach-first posting strategy built into its scheduling logic.
2. A queue layer checks what has already been covered so the same ground is never repeated across episodes.
3. A narration engine scripts the piece and flags claims it cannot verify with explicit uncertainty markers.
4. A production engine assembles the final video autonomously without manual rendering steps.
5. Audience retention signals feed back into future content selection to weight what viewers finish watching.
6. The finished video is posted according to the planner's schedule, with all state persisted so the system survives being switched off and restarted.

## What makes it interesting

- Persistent state across restarts: the system writes enough state to disk that a hard stop does not lose queue position, posting history, or in-progress production work.
- Idempotent queue memory: the queue tracks its own output history and will not re-surface content it has already dispatched, preventing accidental repeats.
- Calibrated narration uncertainty: the scripting layer distinguishes what it knows from what it is inferring and surfaces that distinction in the delivered audio rather than papering over it.
- Verifiable shuffle fairness: the content shuffle algorithm produces a proof of its own ordering so the distribution can be audited rather than trusted blindly.
- Retention-weighted selection: audience completion data loops back into the planner, so topics are ranked by demonstrated keep-watching behaviour rather than assumed appeal.

## Stack

TypeScript

<!-- /portfolio:overview -->

## Recently shipped

<!-- portfolio-entry:shortvideos/commit/00e93b8 -->
### Queue logic that remembers what it already said

An automated channel that posts frequently enough to build an audience will eventually repeat itself — the same angle, the same framing, the same fact — unless the scheduling layer actively refuses to. This commit teaches the pipeline to recognise when a fact-based Short (a brief vertical video built around a single claim) has already run, block it from queuing again, and cleanly retire any companion content — related clips intended to post alongside it — that no longer has a reason to exist.

The result is that the channel can run indefinitely without a producer auditing the backlog for duplicates by hand.

- Duplicate detection built into the queue, not bolted on after the fact
- Companion content retired automatically when its anchor is blocked

<sub>TypeScript</sub>
<!-- /portfolio-entry:shortvideos/commit/00e93b8 -->

<!-- portfolio-entry:shortvideos/commit/9bc2583 -->
### Videos that learn from what audiences keep

After a short video is published, the system watches what happens to it — specifically, how long viewers actually stay — and feeds that signal back into the decisions the pipeline makes next time. This is post-publication retention learning: the production engine does not treat each video as a finished, isolated object but as evidence about what is worth doing again.

For a solo creator with no analytics team, it means the system grows slightly more useful with every upload, calibrating future production choices against real audience behavior rather than guesswork.

- Audience watch-time feeds back into future production decisions automatically
- No analyst needed — the pipeline reads its own results and adjusts

<sub>TypeScript</sub>
<!-- /portfolio-entry:shortvideos/commit/9bc2583 -->

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
