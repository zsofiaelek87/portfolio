# Recruit Lead Engine

**Stack:** TypeScript

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

## What makes it interesting

## How it works

## Recently shipped

<!-- portfolio-entry:recruit-lead-engine/commit/efc2195 -->
### Review queue keeps moving when contacts reply

Recruitment outreach tends to stall at the moment it should accelerate: a prospect replies, and instead of the pipeline advancing, it sits waiting for someone to notice and nudge it forward. This commit removes that waiting. A reply now triggers the next review step on its own, so the queue moves at the pace of actual responses rather than the pace of whoever happens to be watching the inbox.

Small in scope, meaningful in practice: the system earns its keep precisely during the hours no one is logged in.

- Reply events unblock downstream review steps without manual intervention
- Queue stays live even when contact responses arrive asynchronously

<sub>TypeScript · Telegram</sub>
<!-- /portfolio-entry:recruit-lead-engine/commit/efc2195 -->

<!-- portfolio-entry:recruit-lead-engine/commit/e47eb1e -->
### The engine that never sends the same email twice

Automated outreach is only as trustworthy as its worst failure mode, and the worst one is a live system that fires the same message repeatedly because nothing was watching for it. This commit closes that gap by adding a duplicate guard — a persistent check that sits in front of the sending path and refuses to let the same contact receive the same message more than once, regardless of how many times the daemon restarts or how quickly it runs.

- Duplicate guard runs before every send, not as a cleanup step after
- Protection survives daemon restarts and repeated execution cycles

<sub>TypeScript</sub>
<!-- /portfolio-entry:recruit-lead-engine/commit/e47eb1e -->

<!-- portfolio-entry:recruit-lead-engine/commit/ce64a20 -->
### Outreach that reads as a person wrote it

Automated recruiting messages have a reputation for sounding like they were written by a machine trying to pass as polite. This commit sharpens the prompt — the instruction set that shapes how the system drafts outreach — so the language it produces carries the kind of professional credibility that makes a candidate actually write back.

It is a small lever with a direct consequence: the gap between a message that gets ignored and one that gets a reply is almost entirely in how it sounds, and that is now tuned deliberately rather than left to chance.

- Prompt refined to produce credible, human-sounding outreach automatically
- Quality improvement happens before any human reviewer touches the draft

<sub>TypeScript · LLM prompting</sub>
<!-- /portfolio-entry:recruit-lead-engine/commit/ce64a20 -->

<!-- portfolio-entry:recruit-lead-engine/commit/b363768 -->
### Safer contact discovery with built-in queue guardrails

Recruiting pipelines fail quietly — the same person gets contacted twice, a bad address makes it into the queue, or the system races ahead faster than a human can review. This commit tightens both ends of that problem: the logic that finds and qualifies contacts, and the safety layer that controls the order and pace at which those contacts enter the outreach queue.

The result is a sourcing engine that moves faster than manual research but refuses to outrun its own guardrails.

- Contact discovery hardened against duplicate and unsafe entries
- Outreach queue now enforces ordering and safety checks before contacts advance

<sub>TypeScript</sub>
<!-- /portfolio-entry:recruit-lead-engine/commit/b363768 -->

<!-- portfolio-entry:recruit-lead-engine/commit/a891bcc -->
### One message captures the whole board

Keeping track of where every recruiting lead stands usually means checking several places and assembling a picture in your head. This commit condenses it: once a day, a single digest goes out covering every contact currently in the pipeline — their stage, their status, anything that needs attention — so the person running outreach starts each morning already oriented.

The underlying value is about switching costs. Decisions that previously required opening a dashboard and cross-referencing states now happen from a single read.

- Full pipeline state delivered in one daily message
- No dashboard login required to stay oriented

<sub>TypeScript</sub>
<!-- /portfolio-entry:recruit-lead-engine/commit/a891bcc -->

<!-- portfolio-entry:recruit-lead-engine/commit/9952e78 -->
### Outreach that knows when to stay quiet

Sending a follow-up message to someone the same day they received an introduction is exactly the kind of mistake a tired human makes and a well-designed system should not. This commit adds a same-evening quiet window — once a company card has been delivered, the engine holds any nudge until the next day at the earliest.

It is a small rule with a real consequence: the system now governs its own pace, so the outreach it sends feels considered rather than automated.

- Enforces a cooldown automatically — no manual scheduling required
- Prevents the double-touch that signals a bot, not a person

<sub>TypeScript</sub>
<!-- /portfolio-entry:recruit-lead-engine/commit/9952e78 -->

<!-- portfolio-entry:recruit-lead-engine/commit/9874d39 -->
### Follow-ups that know when to stop

Before queuing a follow-up message, the engine now checks whether the original job posting is still live. If the role has been taken down, the contact is quietly retired — no message sent, no human needed to notice. It is the kind of check that sounds obvious once described, and that almost never happens in practice because it requires the system to look outward at the world before acting, rather than simply executing what is already in the queue.

The result is outreach that holds itself accountable to current reality rather than the snapshot it started with.

- Verifies the job is still open before any follow-up is queued
- System self-governs — no manual pruning of stale leads required

<sub>TypeScript</sub>
<!-- /portfolio-entry:recruit-lead-engine/commit/9874d39 -->

<!-- portfolio-entry:recruit-lead-engine/commit/8f9fe0e -->
### A quiet rule that protects every recipient

Automated outreach can go wrong in two quiet ways: messaging someone whose address has been flagged as off-limits, or chasing a lead so late that the follow-up arrives seven weeks after anyone would expect it. This commit teaches the engine to check both conditions before anything is sent — a suppressed address gets no further contact, and a contact that has aged past a hard deadline is simply left alone.

Neither check requires a human to remember the rule. The system enforces its own boundaries, which means the outreach that does go out carries none of those avoidable mistakes.

- Suppressed addresses are permanently excluded from follow-up, automatically
- Stale leads past a time threshold are dropped without human review

<sub>TypeScript</sub>
<!-- /portfolio-entry:recruit-lead-engine/commit/8f9fe0e -->

<!-- portfolio-entry:recruit-lead-engine/commit/7dc80f2 -->
### Outreach that knows when to stop

Credit cards expire, and when one does, the engine now treats that contact's journey as complete rather than looping back to retry or stall indefinitely. It also stops watching that contact's data stream — the live feed of updates the system had been maintaining in case something changed. Both decisions together mean no wasted processing and no phantom follow-ups chasing a dead end.

Small as it sounds, this is the kind of edge case that quietly corrupts outreach quality over time: without an explicit rule, the system just keeps looking at a contact it can no longer help.

- Expired payment details automatically close the contact's flow
- Live data stream is released the moment the contact is retired

<sub>TypeScript</sub>
<!-- /portfolio-entry:recruit-lead-engine/commit/7dc80f2 -->

<!-- portfolio-entry:recruit-lead-engine/commit/68e8a5b -->
### Outreach that knows when to follow up

Recruiting outreach lives or dies on timing and phrasing — send the wrong message at the wrong moment and a candidate goes cold. This commit tightens the call-to-action wording and adjusts the follow-up cadence, the sequence of touchpoints the system sends automatically after an initial contact. The result is a pipeline that reaches candidates at the right intervals without anyone watching the clock or deciding when to nudge.

- Automated follow-up sequence tuned for conversion without manual intervention
- CTA copy refined to prompt action at each stage of outreach

<sub>TypeScript</sub>
<!-- /portfolio-entry:recruit-lead-engine/commit/68e8a5b -->

<!-- portfolio-entry:recruit-lead-engine/commit/633211f -->
### Outreach drafts that refuse to go out wrong

Before a recruiting message leaves the system, it now passes through a tighter set of quality checks — guardrails that catch phrasing problems, tone drift, or structural issues that would quietly reduce a candidate's chance of responding. The check happens automatically, inside the draft stage, before any human reviews it. That means the version a reviewer sees is already clean, and the version a candidate receives has passed a second filter they never have to think about.

- Quality checks run before human review, not after
- Catches draft problems structurally, not by chance

<sub>TypeScript</sub>
<!-- /portfolio-entry:recruit-lead-engine/commit/633211f -->

<!-- portfolio-entry:recruit-lead-engine/commit/5aa4287 -->
### Outreach emails that read like a human wrote them

Automated recruitment outreach has a tell: the subject line fills up with internal tracking codes — job reference numbers, system identifiers, raw database labels — that were never meant for a candidate's inbox. This commit closes that gap. The engine now strips those codes before any message leaves the system, so every subject line reads as intended rather than exposing the machinery behind it.

It is a small guard with an outsized consequence: the first thing a candidate sees stays professional without requiring a human to proofread every send.

- Prevents internal job codes from leaking into outbound subject lines
- Output stays clean automatically, no manual review required

<sub>TypeScript</sub>
<!-- /portfolio-entry:recruit-lead-engine/commit/5aa4287 -->

<!-- portfolio-entry:recruit-lead-engine/commit/55348db -->
### A status check that actually answers the question

Recruitment automation tends to fail quietly — a pipeline stalls, nothing goes out, and nobody notices until someone asks why the numbers look thin. This commit adds a `/status` endpoint (a single URL you can hit to ask "is it running?") that gives a plain yes or no, and pairs it with an escape route for contacts that have reached a dead end in the flow: instead of sitting there indefinitely, they get a path forward.

Small infrastructure, real consequence — the system can now be checked without opening a dashboard, and no lead gets permanently stuck.

- Single endpoint answers operational health without opening any dashboard
- Dead-end contacts get an exit path rather than sitting silently in limbo

<sub>TypeScript</sub>
<!-- /portfolio-entry:recruit-lead-engine/commit/55348db -->

<!-- portfolio-entry:recruit-lead-engine/commit/4f61759 -->
### A stalled batch no longer stalls the pipeline

Recruitment outreach runs in batches — groups of companies contacted together and waited on together. When one batch goes quiet, the naive design lets that silence freeze everything downstream: no new leads move until someone notices and manually unsticks it. This commit removes that single point of failure. The engine now detects an unanswered batch and routes around it automatically, so the funnel keeps moving without a human having to intervene.

For a solo operator, that distinction matters enormously: the system recovers from a common, predictable problem on its own rather than waiting to be rescued.

- Unanswered batches are detected and bypassed without manual intervention
- Funnel progress is decoupled from the slowest unresponsive thread

<sub>TypeScript</sub>
<!-- /portfolio-entry:recruit-lead-engine/commit/4f61759 -->

<!-- portfolio-entry:recruit-lead-engine/commit/2f37430 -->
### Outreach that reads the room before writing

Personalised recruiting outreach usually means someone opening a profile, pulling out a few relevant details, and drafting a message that feels specific. This commit automates that research step: before any message is composed, the system gathers facts about the recipient and builds the copy around what it actually found. The result is outreach that opens on something real rather than a generic greeting — because the system is explicitly prohibited from writing one.

- Copy is generated from researched facts, not templates
- "Hi there" and equivalent openers are structurally forbidden

<sub>TypeScript</sub>
<!-- /portfolio-entry:recruit-lead-engine/commit/2f37430 -->

## Stack notes
