# Polymarket Bot

**Stack:** Python

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

## What makes it interesting

## How it works

<!-- portfolio-entry:polymarket-bot/commit/f5af653 -->
### Tracking behaviour, not every move

Prediction-market bots generate a stream of events, and naively treating every one as significant produces a system that overreacts to noise and misses the patterns that actually matter. This commit shifts how the bot groups and remembers its own activity: rather than stamping a record on each individual action, it keys its memory on stretches of meaningful behaviour — what the system was *doing*, not just what it did last.

The practical consequence is a bot that learns from patterns rather than moments, and stays coherent over time without accumulating clutter it has to reason around.

- Cohort logic groups behaviour into meaningful patterns, not raw event counts
- Reduces noise so the system responds to what matters, not everything that moves

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/f5af653 -->

<!-- portfolio-entry:polymarket-bot/commit/d68b94c -->
### Memory that only records what it knew

A prediction-market bot builds up a picture of what it has seen and acted on — but only if it actually saw it. This commit enforces a clean boundary: if the AI model was never consulted about a market, that market does not get written into the bot's memory at all. It sounds like bookkeeping, but the consequence is real. A system that remembers things it never properly evaluated will eventually act on those ghost records as if they were informed decisions. Excluding them keeps the bot's internal picture honest.

- Memory entries are gated on whether the model actually evaluated the market
- Prevents ghost records from influencing future decisions

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/d68b94c -->

<!-- portfolio-entry:polymarket-bot/commit/d096618 -->
### Testing a bias the bot had never examined

Prediction markets have a well-documented quirk: long-shots — outcomes the crowd considers unlikely — tend to be systematically overpriced relative to their true odds. This is called the favourite-longshot bias. Measuring it from a specific angle, where capital is pushed into positions by structural forces rather than genuine belief, had not been attempted inside this bot before.

This commit adds that measurement. The result is a more honest picture of where apparent edges come from — and whether a signal is a real inefficiency or just the market's thumb on the scale.

- Distinguishes genuine price inefficiency from structurally forced capital flows
- Tests a market bias angle the system had previously left unexamined

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/d096618 -->

<!-- portfolio-entry:polymarket-bot/commit/c673381 -->
### A bot that quotes the side nobody wants

In a two-sided market, a bot naturally accumulates whichever contract is easy to sell and runs short of the one everyone wants. The naive fix is to pull back from the imbalanced side. This commit does the opposite: when inventory tilts, the bot shades its price on the *missing* leg toward the midpoint — making it more attractive — rather than pushing the surplus leg further away. The result is a position that self-corrects through normal trading flow, without the bot ever stepping back from the market to rebalance manually.

- Inventory imbalance is corrected by attracting trades, not by retreating from them
- Skew logic is inverted: the underweight side gets the better price, automatically

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/c673381 -->

<!-- portfolio-entry:polymarket-bot/commit/a3dc87e -->
### Detectors wired into every digest layer

Prediction markets generate a lot of noise, and the real work is knowing which signals are worth acting on. This commit connects a set of detectors — rules that flag meaningful changes in market conditions — into every daily digest the system produces, so nothing slips through unexamined. The tests and the scoped groundwork for a second layer of analysis mean the detection logic can grow more sophisticated without rewiring the digest pipeline each time.

The practical consequence: the system does not just summarise what happened. It watches for what matters.

- Detectors run across every digest automatically, not on request
- Layered architecture lets detection logic expand without rebuilding the output pipeline

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/a3dc87e -->

<!-- portfolio-entry:polymarket-bot/commit/76ed99a -->
### A pricing bot that assumes it will be deceived

Prediction markets publish questions in plain language, and this bot reads them to set prices — but it does so behind a layer of guards built on the assumption that the text will be ambiguous, misleading, or simply wrong. Rather than trust the question at face value, the system validates its own interpretation before any price is committed. The practical consequence: the bot can price a market it has never seen before without a human in the loop, while the guards prevent a misread question from producing a confidently wrong number.

- Prices markets from natural-language questions, not hand-coded rules
- Guards treat every input as potentially misleading before acting on it

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/76ed99a -->

<!-- portfolio-entry:polymarket-bot/commit/712d597 -->
### Every position carries its own paper trail

When a bot opens a position automatically, a natural question follows: which version of the code made that call, and what settings was it running at the time? Without an answer, debugging a bad trade means guessing. This commit stamps each position with exactly that information — the specific build and configuration active at the moment of entry.

The consequence is that every decision becomes auditable after the fact. You can look back at any position and reconstruct not just what the bot did, but why it was even allowed to — which is the only honest way to learn from results rather than just observe them.

- Bad trades become debuggable — each position carries its own context
- Configuration drift cannot silently change behaviour between runs

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/712d597 -->

<!-- portfolio-entry:polymarket-bot/commit/6fdb4d4 -->
### Reviving a quiet lane without trusting it

Some categories of market question — call them lanes — go quiet for a while and then return. When that happens, the bot now refuses to carry its old confidence forward: a revived lane is evaluated only on what it has done since coming back, not on whatever reputation it built before going silent.

The consequence is subtle but important. A lane that returned behaving differently cannot coast on a track record it no longer deserves. Rather than add a human review step, the system enforces this automatically — the pace of trust rebuilding is governed by recent evidence alone.

- Revived lanes earn trust from scratch, not from history
- Confidence is paced by post-return evidence, automatically

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/6fdb4d4 -->

<!-- portfolio-entry:polymarket-bot/commit/6ebc3be -->
### Fixing the spread that was skewing every fill

On a prediction market, the "spread" is the gap between the price you can buy at and the price you can sell at — and if a bot triggers its trades at the edges of that gap, the gap itself quietly works against every single position. This commit moves the trigger point to the midprice, the true centre between those two extremes, so the bot is no longer paying a hidden tax on its own activity.

Small as it sounds, a skewed entry compounds across many trades. Fixing it at the trigger level means the correction applies automatically to everything the bot does from that point forward.

- Entry logic now references the midprice, removing systematic spread bias
- One fix propagates across all future trades without per-trade adjustments

<sub>Python · Polymarket</sub>
<!-- /portfolio-entry:polymarket-bot/commit/6ebc3be -->

<!-- portfolio-entry:polymarket-bot/commit/67e32f6 -->
### Grading the reasoning, not just the result

Most automated trading bots are judged by their positions — what they bought, what they sold, what happened next. This commit changes what gets measured: it scores the analyst commentary that preceded each trade, so the system can distinguish a correct bet that was badly reasoned from a correct bet that reflects genuine insight.

That separation matters because a system that got lucky and a system that got it right look identical when you only watch the money. Evaluating the written reasoning creates a signal that survives runs of variance — and makes the next iteration easier to improve deliberately.

- Separates lucky outcomes from sound reasoning in post-trade review
- Scores analyst narrative independently of position performance

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/67e32f6 -->

<!-- portfolio-entry:polymarket-bot/commit/52f26f4 -->
### Clarifying exactly how a prediction market makes money

A prediction market — a platform where people trade on the likelihood of real-world outcomes — offers several distinct ways to profit: taking a position outright, providing liquidity, arbitraging price differences, and so on. Before this change, the bot's internal model lumped some of those together in ways that could lead to muddled strategy decisions. This commit redraws the taxonomy so each income route is defined precisely and sits in the right category.

The practical consequence is that every downstream decision — whether to act, how to size a trade, which mechanism is actually responsible for a return — now starts from a clean, agreed-upon map rather than an inherited approximation.

- Each income route is now its own distinct, reviewable category
- Downstream sizing and strategy logic inherits a cleaner foundation

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/52f26f4 -->

<!-- portfolio-entry:polymarket-bot/commit/21598b6 -->
### Knowing what a portfolio cannot lose

Before a prediction-market bot acts on any position, it needs an honest picture of where it is exposed — not just where it expects to profit. This commit adds measurement of two specific books: equity-linked contracts and a short-volatility position (a bet that prices will stay calm). Both carry downside that is easy to ignore when things are going well and painful to discover when they are not.

Encoding that accounting in software means the exposure is visible every time the bot runs, not only when someone thinks to check.

- Downside measured automatically, before any position is acted on
- Covers correlated risk across two structurally different book types

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/21598b6 -->

## Recently shipped

<!-- portfolio-entry:polymarket-bot/commit/f561544 -->
### Pricing a whole batch of markets at once

Quoting prices one market at a time is the obvious first approach — and the slow one. This commit restructures the pricing lane (the part of the bot responsible for setting buy and sell prices) so it evaluates an entire batch of markets in a single pass rather than looping through them individually. The change is architectural: the lane was already the right place to enforce pricing constraints, and batch input is a natural fit for that boundary.

The practical result is that the bot can reprice a large set of positions quickly enough to stay current when conditions shift across many markets at once.

- Batch pricing replaces one-at-a-time loops without touching the constraint layer
- Pricing rules and throughput improvements share the same enforced path through the system

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/f561544 -->

<!-- portfolio-entry:polymarket-bot/commit/d376f50 -->
### Routing AI calls only to models that accept them

Not every AI model accepts every kind of input — some handle text only, others accept richer formats — and calling the wrong one wastes time and fails silently. This commit adds routing logic that inspects each request and sends it only to a model capable of handling it, skipping the rest. It is a small gate with a compounding payoff: the bot stops burning time on calls that were never going to work, and new model types can be added later without rewriting the dispatch logic around them.

- Calls are matched to capable models before dispatch, not after failure
- Routing is structural, so adding new model types does not require rewriting call logic

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/d376f50 -->

<!-- portfolio-entry:polymarket-bot/commit/bc0e689 -->
### Daily digest that explains, not just totals

A trading bot that sends you a daily summary is useful. One that tells you *why* yesterday looked the way it did — not just a running total, but a plain account of what actually happened in the period — is the thing you actually read.

This commit shifts the digest from a scoreboard into a brief: yesterday gets its own explanation rather than disappearing into a cumulative figure. The person receiving it no longer has to reconstruct the story themselves.

- Yesterday's activity explained in plain language, not buried in totals
- Digest becomes a readable account, not just a number that moved

<sub>Python · Telegram</sub>
<!-- /portfolio-entry:polymarket-bot/commit/bc0e689 -->

<!-- portfolio-entry:polymarket-bot/commit/a1dbf45 -->
### P&L broken down by cohort, not just total

A single profit-and-loss figure tells you whether the bot is working. A figure broken out by cohort — grouping positions by when they were opened, or by strategy type — tells you *which part* is working and whether different groups should even be measured together. This commit adds that breakdown to the daily digest, then audits whether the cohorts are genuinely comparable or whether averaging across them would quietly distort the picture.

The practical consequence is that a good aggregate number can no longer hide a struggling segment, and a bad one can no longer obscure a pocket that is actually performing.

- Daily digest now shows per-cohort P&L, not just a running total
- Audit step checks whether cohorts are comparable before pooling their results

<sub>Python · Telegram</sub>
<!-- /portfolio-entry:polymarket-bot/commit/a1dbf45 -->

<!-- portfolio-entry:polymarket-bot/commit/1f10798 -->
### Detectors that investigate without being asked

Most monitoring tools wait for a question. This commit turns that around: a layer of detectors now runs its own investigation continuously, scanning prediction-market conditions for changes that meet criteria worth acting on — without anyone deciding when to look.

The practical consequence is that the system catches meaningful shifts between check-ins rather than only at them. The detection logic is also structured to grow more sophisticated over time without requiring the surrounding pipeline to be rebuilt each time a new signal is added.

- Proactive detection: flags market changes before a human thinks to check
- Designed so new detection rules slot in without rewiring the pipeline

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/1f10798 -->

<!-- portfolio-entry:polymarket-bot/commit/1c5db56 -->
### Smarter trust for lanes that go quiet and return

A prediction-market bot earns trust in a lane — a category of question it has learned to price — over time. When a lane goes dark and then comes back, the naive move is to treat it as trusted again immediately. This commit rejects that: a revived lane is now judged only on what it has done since its return, not on its history before going silent. The practical effect is that a lane that came back behaving differently cannot coast on a reputation it no longer deserves.

- Reputation resets on revival — past performance before a silence does not carry forward
- Trust is earned incrementally, not assumed from prior history

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/1c5db56 -->

<!-- portfolio-entry:polymarket-bot/commit/1b172ac -->
### Judging a market by who actually traded it

Before the bot forms an opinion on a prediction-market position, it now looks at the trading history of that specific lane — the individual contract being considered — and asks whether the people who traded it knew what they were doing. A lane with a credible build (a track record of informed, well-timed trades) gets treated differently from one populated by noise.

The practical consequence: the bot's confidence in a signal is shaped by the quality of participants who produced it, not just the price itself. That is a distinction a human analyst would make instinctively; getting a machine to make it consistently is the harder part.

- Signal quality filtered by the credibility of prior traders, not price alone
- Per-lane assessment runs automatically before any position is considered

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/1b172ac -->

## Stack notes
