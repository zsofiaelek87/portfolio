# Polymarket Bot

**Stack:** Python

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

<!-- portfolio:overview -->

> An automated market-maker for Polymarket prediction markets, built in Python

## What it is

Prediction markets reward accurate pricing, but they also attract informed traders who will take the other side of any mispriced quote. This bot acts as an automated market-maker on Polymarket, quoting both sides of binary markets while managing the real risk of being picked off by better-informed participants. It is a solo engineering project focused on robust pricing logic, position accountability, and honest self-evaluation.

## How it works

<!-- portfolio-entry:polymarket-bot/commit/f4aeeff -->
### Measuring arbitrage by time, not just profit

Spotting a price gap on a prediction market is only half the problem — if the gap closes before the trade settles, the edge was never real. This commit reframes how the bot measures opportunity: instead of asking only "how large is the discrepancy?", it now asks "how long does it survive?" Speed becomes a first-class signal alongside margin, so the system stops chasing edges that look good on paper but vanish before they can be captured.

The consequence is a tighter filter. The bot learns to distinguish a durable inefficiency from a fleeting one, and only acts when the evidence supports both dimensions.

- Opportunity size measured by lifespan, not just price difference
- Filters out edges that close before a trade can settle

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/f4aeeff -->

<!-- portfolio-entry:polymarket-bot/commit/8a224e5 -->
### When the reward structure is the risk

Prediction market maker rewards — the bonuses paid to participants who keep prices liquid — are not evenly distributed. A small number of outlier events account for a disproportionate share of the total, which means any strategy that ignores that shape will look profitable in normal conditions and get quietly destroyed when it matters most.

This commit recalibrates the bot's internal model to treat the distribution honestly: skewed, outlier-dominated, and close to dangerous if misread. The system now reasons about expected reward the way a statistician would rather than the way a spreadsheet average suggests it should.

- Reward model corrected for outlier-dominated distributions, not naive averages
- Prevents a strategy that looks safe on paper from failing at the worst moment

<sub>Python · Prediction Markets</sub>
<!-- /portfolio-entry:polymarket-bot/commit/8a224e5 -->

<!-- portfolio-entry:polymarket-bot/commit/10d1d27 -->
### Widening the view before placing a bet

Arbitrage on prediction markets — finding the same question priced differently across venues and profiting from the gap — only works if the bot can see enough of the market at once. This commit expanded the search from a narrow slice to twelve distinct ladders across nearly six hundred individual price levels. A ladder here is one layer of the order book, the ranked list of what buyers and sellers are currently willing to accept. Looking at more of them means the bot catches opportunities that were previously invisible simply because it was not looking in the right place.

- Search coverage expanded from a narrow slice to 12 full order-book ladders
- 594 price levels now evaluated per scan, versus a fraction of that before

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/10d1d27 -->

<!-- portfolio-entry:polymarket-bot/commit/14ab796 -->
### A constraint that nobody knew was drifting

Automated trading systems accumulate rules over time, and some of those rules quietly stop meaning what anyone thought they meant. This commit adds an observation layer that watches for a constraint behaving unexpectedly — not crashing, not throwing an error, just silently producing outcomes nobody intended — and surfaces it before the bot acts on a mistaken assumption.

In an unattended market-maker, a rule that drifts without complaint is more dangerous than one that fails loudly. Catching the quiet kind is harder, and worth more.

- Detects constraint drift before it reaches a live trading decision
- Surfaces unintended behaviour without requiring a crash or error first

<sub>Python · Polymarket</sub>
<!-- /portfolio-entry:polymarket-bot/commit/14ab796 -->

<!-- portfolio-entry:polymarket-bot/commit/ebcc262 -->
### Counting observations, not just rows

Prediction markets generate data that can lie about itself: the same underlying event can appear in multiple rows, making a naive tally overconfident about how much the bot actually knows. This commit changes the unit of measurement from database rows to independent observations — distinct, non-overlapping signals — so the bot's confidence in any given price reflects how many genuinely separate data points support it, not how many times the same information was recorded.

The consequence is quieter than it sounds: a system that knows the difference between one observation seen ten times and ten genuinely separate observations makes meaningfully different decisions at the margin.

- Confidence scores now reflect independent signal count, not raw data volume
- Prevents repeated observations from inflating the bot's certainty about a price

<sub>Python · Polymarket</sub>
<!-- /portfolio-entry:polymarket-bot/commit/ebcc262 -->

1. The bot scans available markets and selects lanes worth quoting based on volatility and activity signals.
2. It computes bid and ask prices, embedding assumptions about adversarial flow and spread requirements into every quote.
3. Quotes are posted and continuously revised as market conditions change, with spread corrections applied when fills are skewing.
4. Every position is tagged with its own paper trail so the source of each fill can be reviewed independently.
5. Behavioural detectors run inside every digest layer to flag manipulation, mispricing, or unexpected patterns.
6. Performance is graded on the quality of reasoning behind each prediction, not just whether the outcome was correct.

## What makes it interesting

<!-- portfolio-entry:polymarket-bot/commit/ba43f61 -->
### One edge, sized by reality not by code

Most automated trading systems limit themselves in code — a hard ceiling written into the logic. This bot takes a different approach: it finds one well-measured edge and lets real-world constraints do the limiting. Capital availability and the capacity of the venues it trades on act as the natural governor, so the system never overreaches its own evidence.

The consequence is a bot that stays honest about what it actually knows. It does not scale a signal beyond what the market will bear, and it cannot be tricked by its own ambition into a position the underlying edge does not support.

- Position sizing governed by capital and venue capacity, not arbitrary code limits
- Single measured edge: depth over breadth, signal over noise

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/ba43f61 -->

- Adversarial pricing model: the bot prices quotes under the explicit assumption it will be deceived by informed traders.
- Memory architecture that records only what the bot knew at decision time, preventing look-ahead contamination during backtesting.
- Spread calibration loop that identified and corrected a systematic skew that was distorting every fill.
- Per-position audit trail allowing precise post-mortems when a strategy loses, down to the individual decision.
- Behavioural detectors embedded at every digest layer rather than applied as a post-processing step.

## Stack

Python

<!-- /portfolio:overview -->

## Recently shipped

<!-- portfolio-entry:polymarket-bot/commit/46d09a1 -->
### When volatility itself becomes the signal

Prediction markets publish prices. Buried inside those prices is implied volatility — a market's collective guess about how uncertain an outcome is, expressed as a number rather than a feeling. This commit corrects a misclassification: an event the system had been treating as a straightforward price move was actually a volatility event, and the gates — the conditions the bot checks before acting — were reading the wrong signal entirely.

The fix matters because a system trading on the wrong input is not just inefficient; it is confident in the wrong direction. Correcting the event type means the bot's decisions are now grounded in what the market is actually expressing.

- Distinguishes price-driven events from uncertainty-driven events before acting
- Entry gates now read the signal the market is actually publishing

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/46d09a1 -->

<!-- portfolio-entry:polymarket-bot/commit/4dda268 -->
### Predicting the past is cheating

A regime model — a layer that classifies whether the market is currently trending, choppy, or shifting — is only honest if it makes its call using information that existed at the time, not information from afterward. The first version of this bot's regime classifier had a subtle flaw: it was sorting historical periods using data that had not yet arrived, which made backtests look cleaner than any live deployment ever could.

This commit makes the split causal — meaning each moment in the historical record is labeled using only what the bot could have known then. The consequence is unglamorous but important: the strategy now trains on a truthful picture of the past, so its confidence in live conditions is earned rather than borrowed from the future.

- Backtests now reflect only information the bot could have had at the time
- Causal labeling prevents historical performance from flattering live results

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/4dda268 -->

<!-- portfolio-entry:polymarket-bot/commit/faef95c -->
### When the strategy loses, find exactly where

Losing on a trade has three distinct explanations: the signal was wrong, the entry was bad, or the exit was mishandled. Blaming the wrong one leads to the wrong fix. This commit establishes exactly where in the sequence value was leaking — not at pair completion, which was working correctly, but earlier, at the moment the position was acquired.

A system that can tell those failure modes apart does not patch the exit when the problem lives in the fill. That precision is what makes the diagnosis worth having.

- Separates fill-time loss from exit-time loss before any fix is applied
- Prevents misdiagnosis from producing a confident but incorrect correction

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/faef95c -->

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

<!-- portfolio-entry:polymarket-bot/commit/e7c5267 -->
### A circuit breaker that learned to step aside

Prediction-market bots often use a circuit breaker — an automatic stop that halts activity when conditions look dangerous — but a breaker that fires too eagerly can block trades it should allow. This commit draws a precise line: the breaker still guards against genuine risk, but it now recognises a specific class of position (one that profits when market uncertainty rises, rather than when a particular outcome wins) and holds its lane open rather than shutting it down.

The result is a bot that protects itself without protecting itself into inaction.

- Circuit breaker now distinguishes protective halts from unnecessary interference
- First long-volatility position held without manual override

<sub>Python · Prediction Markets</sub>
<!-- /portfolio-entry:polymarket-bot/commit/e7c5267 -->

<!-- portfolio-entry:polymarket-bot/commit/7a56a8f -->
### A daily briefing the bot writes about itself

Once a day, the system reads back through its own evidence — the signals it has gathered, the positions it holds — and produces a plain recommendation about what to do next. That recommendation is allowed to propose a direction, but it cannot touch the numerical thresholds that control how aggressively the bot acts. Proposing and adjusting are kept as separate powers on purpose.

The consequence is a reviewable opinion that cannot quietly rewrite its own risk settings on the way out.

- Self-review cycle reads evidence and surfaces a next action automatically
- Proposing a move and changing risk limits are deliberately separate operations

<sub>Python</sub>
<!-- /portfolio-entry:polymarket-bot/commit/7a56a8f -->

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
