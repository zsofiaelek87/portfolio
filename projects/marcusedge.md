# Marcus Edge

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

## What makes it interesting

<!-- portfolio-entry:marcusedge/commit/388544d -->
### Forensics methodology for spotting pump-and-dump schemes

Launch-and-dump is a pattern where a project generates artificial excitement, attracts buyers, and then collapses — usually before most people realize what happened. Marcus Edge publishes a structured methodology for reconstructing that sequence after the fact: what signals to look for, in what order, and how to distinguish a genuine failure from a deliberate exit. Making that reasoning explicit and replicable is the work; anyone can have an opinion, but a documented method can be taught, challenged, and improved.

- Structured forensic framework, not just a checklist of red flags
- Designed to be replicable — judgment made explicit and teachable
<!-- /portfolio-entry:marcusedge/commit/388544d -->

## How it works

<!-- portfolio-entry:marcusedge/commit/db8642f -->
### Editorial opinions that must show their work

Before any editorial judgment leaves the system, it has to cite between one and four pieces of evidence — specific sources it can actually point to. That constraint is baked into the data structure itself, not left as a guideline someone might follow or forget. A claim with no citation attached is structurally invalid; the system won't let it through.

The practical consequence is that every output is a argued position, not an assertion. Readers can check the reasoning; auditors can trace it; and the system cannot produce confident-sounding conclusions that float free of the material that should support them.

- Citation count enforced at the schema level, not as a style rule
- Between one and four sources required — not optional, not unbounded

<sub>JSON Schema · TypeScript</sub>
<!-- /portfolio-entry:marcusedge/commit/db8642f -->

## Recently shipped

<!-- portfolio-entry:marcusedge/commit/cc8705c -->
### Automated scanner that reconstructs a deliberate market exit

Spotting a launch-and-dump — where artificial excitement is engineered, buyers are drawn in, and the project is quietly abandoned before the damage is visible — normally requires hours of manual pattern-matching across timeline data. Marcus Edge now ships a forensic scanner that runs that sequence automatically: identifying the characteristic signals, ordering them chronologically, and separating a genuine collapse from a calculated one.

The value of encoding that logic in software rather than leaving it as expert intuition is that the reasoning becomes auditable. Someone can inspect why a pattern was flagged, disagree with a step, and propose a correction — none of which is possible when the analysis lives only in a person's head.

- Detects the launch-and-dump pattern programmatically, not by hand
- Makes the classification logic explicit and open to challenge

<sub>Python</sub>
<!-- /portfolio-entry:marcusedge/commit/cc8705c -->

<!-- portfolio-entry:marcusedge/commit/ac95267 -->
### A daily screener that does the scanning you would not

Every morning, a skilled trader could sit down and work through the market looking for stocks that match a specific set of conditions — a time-consuming, repeatable job that looks exactly like something a machine should own. ELIT v5 is the fifth iteration of that screener, built into Marcus Edge and run on a schedule so the output is waiting before the first human decision of the day.

The v5 label matters here: each version reflects a refined set of criteria, not just a rerun of the same logic with fresher data.

- Runs on a schedule and delivers results before the trading day begins
- Fifth-generation screening logic reflects accumulated refinement over prior versions

<sub>Python · Scheduled Jobs · Market Data APIs</sub>
<!-- /portfolio-entry:marcusedge/commit/ac95267 -->

<!-- portfolio-entry:marcusedge/commit/76749aa -->
### Book journal and bot dashboards in one view

Tracking what a system has read, decided, and acted on — across both a curated book journal and multiple automated trading bots — usually means flipping between disconnected screens. This commit pulls both into a single unified dashboard, so the state of the whole system is legible at a glance rather than assembled from fragments.

For a solo operator running bots around the clock, visibility is the control surface: knowing what each bot has done, and what the research pipeline has consumed, is what makes confident intervention possible.

- Bot activity and research reading tracked in one unified interface
- Removes the need to cross-reference separate tools to understand system state

<sub>Dashboard UI · Bot orchestration</sub>
<!-- /portfolio-entry:marcusedge/commit/76749aa -->

<!-- portfolio-entry:marcusedge/commit/2cc8000 -->
### Planning documents turned into tracked system logic

Planning a complex automated system on paper is one thing; getting those plans to live inside the system itself — versioned, visible, and checkable — is another. This commit formalises ELIT v5 and its addendum as first-class artifacts within Marcus Edge: a paper tracker records what was planned and when, a regime gate enforces that the system cannot operate outside its intended rules, and dedicated dashboard tabs surface the state of the underlying bots at a glance.

The v4.3 snapshot preserved alongside them means there is a clean before-and-after record — not just of what changed, but of what the system looked like the moment the new plan took over.

- Regime gate prevents the system running outside its defined operating rules
- Paper tracker keeps design intent and live state in the same place
- Versioned snapshot locks the prior configuration at the point of handover

<sub>Marcus Edge · dashboard tooling · automated trading · version control</sub>
<!-- /portfolio-entry:marcusedge/commit/2cc8000 -->

<!-- portfolio-entry:marcusedge/commit/058174f -->
### Research dashboard with finer move controls

A research dashboard is only as useful as the control it hands back to the person running it. This commit tightens both sides of that equation: the display of research output and the controls for stepping through extended moves — sequences of positions that unfold over time — are now more precise and easier to navigate. Small improvements to how a system surfaces information tend to have an outsized effect on how much anyone actually trusts and uses it.

- Extended-move navigation made more precise and controllable
- Research output display improved for clearer review

<sub>Python · Deribit</sub>
<!-- /portfolio-entry:marcusedge/commit/058174f -->

<!-- portfolio-entry:marcusedge/commit/030c88a -->
### Daily market scan runs itself, unattended

Every day, without being asked, Marcus Edge wakes up on its own dedicated runner — a machine set aside purely for this job — inspects the relevant markets on Deribit, and finishes before the next scheduled run begins. The concurrency constraint (only one scan at a time, enforced by the workflow itself) means two overlapping runs can never collide and corrupt each other's results. The scan completes cleanly whether anyone is watching or not.

- Scheduled daily without manual triggering or human oversight
- Concurrency controls prevent overlapping runs from corrupting results

<sub>Deribit · GitHub Actions</sub>
<!-- /portfolio-entry:marcusedge/commit/030c88a -->

## Stack notes
