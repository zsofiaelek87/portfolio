# Robinhood Chain Bot

> Paper-trade strategies and track airdrop opportunities on blockchain via Telegram control

**Stack:** Python

<!-- Entries below are drafted from private repositories by the portfolio agent
     and published only after review. Source code is not public. -->

## What makes it interesting

## How it works

<!-- portfolio-entry:robinhood-chain/commit/6cd0a82 -->
### History that survives a cold restart

Blockchain networks keep a running log of every event that ever touched a liquidity pool — deposits, withdrawals, fee accruals — but those logs are not deleted or compressed over time the way a typical database might be. This commit takes advantage of that permanence: rather than trusting that the bot was running when history happened, it reaches back and reconstructs the full record from raw on-chain events. The result is a position history that is complete even if the bot was switched off for weeks, with no gaps and nothing dependent on uptime.

- Reconstructs full LP history from on-chain event logs, not local state
- Survives restarts and downtime without losing historical context

<sub>Python · Web3 · Telegram · Blockchain</sub>
<!-- /portfolio-entry:robinhood-chain/commit/6cd0a82 -->

<!-- portfolio-entry:robinhood-chain/commit/377ebee -->
### When the headline number quietly lies

Liquidity pools — shared pots of money that automated markets draw from — advertise a yield figure, but that number routinely omits the silent cost known as impermanent loss: the gap between what you deposited and what you could withdraw, caused by the pool's assets drifting in price relative to each other. This commit measures that gap directly for a specific pool and builds a clear-eyed accounting of what the position actually earns once the hidden drag is counted.

The point is not to expose a scandal but to make the system's decisions honest: a bot acting on the advertised number will behave very differently from one that knows what the position truly costs to hold.

- Separates advertised yield from realised return after accounting for price drift
- Makes the hidden cost visible before the bot acts on it

<sub>Python · Telegram · Blockchain</sub>
<!-- /portfolio-entry:robinhood-chain/commit/377ebee -->

## Recently shipped

<!-- portfolio-entry:robinhood-chain/commit/e4f0f79 -->
### The metric that actually drives the decision

Paper-trading a blockchain strategy produces a lot of numbers, but not all of them are the ones that matter. This commit adds the concentration multiple — a single figure that expresses how heavily a position is weighted relative to the rest of the portfolio — to the bot's Telegram reports. Before this, the system could tell you what a trade looked like; now it tells you what the trade *means* for overall exposure.

The distinction matters because a position that looks modest in isolation can still be the thing that sinks the portfolio if it goes wrong. Surfacing that number in the report means the person reading it sees the real lever, not a proxy for it.

- Delivers the concentration figure that governs risk, not just raw position size
- Reported directly in Telegram, no dashboard login required

<sub>Python · Telegram Bot API · Blockchain</sub>
<!-- /portfolio-entry:robinhood-chain/commit/e4f0f79 -->

<!-- portfolio-entry:robinhood-chain/commit/d90bdcf -->
### Measuring liquidity pools instead of estimating them

Decentralised liquidity pools — shared pots of cryptocurrency that anyone can trade against — are usually described in vague terms like "deep" or "thin." This commit replaces that guesswork with direct measurement: the bot now reads the actual size of the pools it cares about, so every downstream decision about whether to enter a position is grounded in real data rather than assumption.

The consequence is the kind of thing that sounds obvious once you say it aloud: a paper-trading system that argues from evidence is more trustworthy than one that argues from intuition, even when the stakes are still simulated.

- Replaces pool-size assumptions with live, direct measurement
- Grounds simulated trade decisions in observed data, not estimates

<sub>Python · Telegram · Web3 · DeFi</sub>
<!-- /portfolio-entry:robinhood-chain/commit/d90bdcf -->

<!-- portfolio-entry:robinhood-chain/commit/d10166a -->
### Fee tiers answered by measuring actual flow

Choosing a fee tier on a decentralised exchange — the percentage taken on each swap — sounds like a settings question, but the right answer depends on how much trading volume actually moves through a given pool at any moment. Instead of guessing or using a fixed default, the bot now measures that flow directly and uses it to answer the question. The result is a decision grounded in live market behaviour rather than a standing assumption that may have been true last week and wrong today.

- Fee-tier selection driven by measured volume, not defaults
- Decision logic updates on live data, not historical assumptions

<sub>Python · Telegram</sub>
<!-- /portfolio-entry:robinhood-chain/commit/d10166a -->

<!-- portfolio-entry:robinhood-chain/commit/ba47a85 -->
### Dashboard snapshots published on a controlled schedule

Tracking paper trades and airdrop opportunities across a blockchain generates a steady stream of state — positions, signals, balances — that is only useful if someone can actually see it at the right moment. This commit adds a reserved snapshot publisher: at a defined interval, the system captures the current dashboard view and routes it out automatically, so a Telegram-based operator sees a consistent picture of where things stand without having to ask for it.

The word "reserved" matters here: snapshots are prepared ahead of time and held until the scheduled moment, rather than generated on demand. That keeps the output predictable and the delivery orderly, even when underlying market conditions are changing quickly.

- Snapshots prepared in advance and held until the scheduled release window
- Operator receives a consistent dashboard view through Telegram without manual requests

<sub>Python · Telegram Bot API · Blockchain · Task Scheduling</sub>
<!-- /portfolio-entry:robinhood-chain/commit/ba47a85 -->

<!-- portfolio-entry:robinhood-chain/commit/6b9b959 -->
### Measuring what a liquidity position actually earns

Providing liquidity to a decentralised pool — lending your assets to a shared trading pot in exchange for a cut of its fees — sounds profitable until you try to measure it precisely. This commit wires in the yield calculation that answers the real question: what does parking capital here actually pay, expressed as a rate, not a vague accumulation of fees?

Alongside that, a column that had been frozen in the Telegram reports (locked against display while the logic behind it was still being worked out) is now live. The bot's output now gives a complete, readable picture of pool performance rather than a set of figures with a deliberate gap in the middle.

- Yield rate surfaced directly in Telegram — no spreadsheet required
- Previously hidden depth column restored once its underlying logic was settled

<sub>Python · Telegram Bot API · Web3 · DeFi</sub>
<!-- /portfolio-entry:robinhood-chain/commit/6b9b959 -->

<!-- portfolio-entry:robinhood-chain/commit/36d2ec9 -->
### Teaching a trading bot what weekends mean

Price gaps between Friday's close and Monday's open are a normal feature of markets that rest — not a signal that something unusual happened. Before this commit, the bot had no concept of when markets were closed, which meant it could mistake a routine weekend pause for a meaningful price dislocation and treat it as a trading opportunity worth acting on.

The fix is a market-hours check that the bot consults before drawing any conclusion: if the gap appeared while the exchange was simply shut, the gap is filed as expected, not alarming. It is a small piece of calendar awareness that separates a system making genuine decisions from one pattern-matching on noise.

- Bot distinguishes closed-market gaps from real price dislocations
- Prevents false signals from routine weekend price movements

<sub>Python · Telegram</sub>
<!-- /portfolio-entry:robinhood-chain/commit/36d2ec9 -->

<!-- portfolio-entry:robinhood-chain/commit/1f74a89 -->
### Pre-registration logic added for liquidity positions

Airdrop farming often depends on being early — registering a position in a liquidity pool (a shared pot of assets that earns fees) before a reward window closes. This commit adds a pre-registration step so the bot can queue that action automatically rather than waiting for a human to notice the timing.

It also closes out the arbitrage question for this strategy: the system now has a settled answer for whether simultaneous price-difference trades are in scope, so future decisions build on a documented choice rather than revisiting open ground.

- Bot registers liquidity positions automatically before reward windows close
- Arbitrage scope formally resolved, removing an ongoing design ambiguity

<sub>Python · Telegram · Web3 · Blockchain</sub>
<!-- /portfolio-entry:robinhood-chain/commit/1f74a89 -->

<!-- portfolio-entry:robinhood-chain/commit/06286b2 -->
### Spotting price gaps that actually mean something

A cross-venue gap monitor now watches for price differences across multiple trading venues simultaneously and only raises a flag when the spread is real — not an artifact of thin order books or a venue with temporarily absent buyers. The liquidity gate (a check that confirms enough active trading volume exists to act on the gap) sits in front of any decision, so the bot will not treat an illiquid, half-empty market as an opportunity worth pursuing.

The practical consequence: paper-trading signals — practice trades logged without real money — are now filtered by whether the market could actually absorb them, which is the difference between backtesting against reality and backtesting against a fantasy.

- Monitors price gaps across venues before treating any spread as actionable
- Liquidity gate blocks signals when market depth is too thin to act on

<sub>Python · Telegram Bot API · Blockchain data feeds</sub>
<!-- /portfolio-entry:robinhood-chain/commit/06286b2 -->

## Stack notes
