# Bitcoin MAX/MIN Trend-Following and Mean-Reversion

## Overview

A pair of complementary strategies applied to Bitcoin (BTC) based on whether price is at a
recent n-day high (MAX strategy, trend-following) or a recent n-day low (MIN strategy,
mean-reversion). Originally documented by Padysak & Vojtko (2022, SSRN 4081000); updated
and extended by Beluška & Vojtko (2024, SSRN 4955617) with data through August 2024.
Published and promoted by Quantpedia.

The core finding: when BTC is at a local maximum (n-day high), it tends to continue trending
upward in the following days (trend-following edge). When BTC is at a local minimum (n-day
low), it tends to bounce back (mean-reversion edge). Both strategies substantially reduce
maximum drawdown compared to buy-and-hold Bitcoin.

## Asset Class / Timeframes

- **Asset:** Bitcoin (BTC) — spot or perpetual futures; predominantly tested on spot (BTC/USD)
- **Timeframe:** Daily (signal generated at end of day)
- **Lookback periods tested:** 10, 20, 30, 40, 50 calendar/trading days; shorter lookbacks
  outperform longer ones; 10-day period generally best
- **Market:** Crypto

## Core Logic

### MAX Strategy (Trend-Following)

1. **Signal:** Each day, check if BTC closing price equals the highest close over the last
   n days (n = 10–50; 10 preferred).
2. **Entry:** Go long (buy) BTC when signal is triggered — price is at a new n-day high.
3. **Hold period:** ANALYSIS: "Next few days" language in search summaries suggests a short
   hold of approximately 1–5 days; exact exit conditions NOT CONFIRMED from accessible sources.
4. **Rationale:** At local maxima, demand pressure and momentum-chasing behavior continue
   to push BTC higher in the short term. This is a breakout continuation pattern.

### MIN Strategy (Mean-Reversion)

1. **Signal:** Each day, check if BTC closing price equals the lowest close over the last
   n days (n = 10–50; 10 preferred).
2. **Entry:** Go long (buy) BTC when signal is triggered — price is at a new n-day low.
3. **Hold period:** Same short-term window as MAX (ANALYSIS: 1–5 days; NOT CONFIRMED).
4. **Rationale:** At local minima, panic selling is exhausted; the market has overshot to
   the downside. BTC tends to bounce back from n-day lows on a short-term basis.

### MAX+MIN Combined

Both strategies run simultaneously. The portfolio holds BTC when either signal is active
and is otherwise in cash (or the position is flat). The combination achieves returns "close
to the all-time high watermark" of BTC with substantially lower drawdowns.

**Position sizing:** NOT REPORTED in accessible summaries. ANALYSIS: likely 100% of capital
when signal fires (binary long/flat), based on the research style of Quantpedia working papers.

## Economic Rationale

**MAX (trend-following):** Crypto markets are driven by retail momentum-chasing. When BTC
reaches a new n-day high, it becomes a news event, attracting new buyers. Network effects
and crypto media amplify momentum at local maxima. This is the behavioral mechanism behind
momentum: extrapolation bias and FOMO (Fear of Missing Out) cause continuation.

**MIN (mean-reversion):** At n-day lows, fear and forced liquidations (leveraged long
positions) create overshoot below fundamental value. Once the forced selling is exhausted,
the price bounces. This is the classic panic-selling / capitulation recovery pattern.

**When the edge should fail (falsifiability):**
- MAX edge disappears when: (1) BTC becomes a mature, institutionally-arbitraged market where
  breakout continuation is quickly priced out; (2) breakout patterns become systematically
  front-run as retail algorithmic adoption grows; (3) sustained bear markets where every
  breakout is a bull trap followed by reversal.
- MIN edge disappears when: (1) sustained bear markets with cascading capitulation (each new
  low is followed by further decline rather than bounce); (2) exchange hacks, regulatory bans,
  or structural failures trigger panic selling that does not bounce; (3) crypto winter lasting
  longer than the mean-reversion window.
- **Critical note:** The MIN strategy is conceptually at odds with the Donchian ensemble
  approach (see Similar Strategies). Donchian would SHORT BTC at n-day lows (trend-following);
  the MIN paper goes LONG at n-day lows (mean-reversion). Both cannot be simultaneously correct
  — this is an unresolved empirical question about Bitcoin's short-term dynamics at new lows.

## Entry Conditions

**MAX:** Long BTC when today's closing price = highest close over previous n days (n=10, preferred)
**MIN:** Long BTC when today's closing price = lowest close over previous n days (n=10, preferred)

Both: enter at close of signal day or open of following day (NOT CONFIRMED which execution timing)

## Exit Conditions

NOT CONFIRMED from accessible search summaries. ANALYSIS: Given the "next few days"
description, likely one of:
- Fixed hold period (e.g., exit after k days)
- Opposite signal (MAX exits when price falls below n-day high condition; MIN exits when
  price stops meeting n-day low condition)
- The combined MAX+MIN may continuously rotate signals so positions are rarely flat

## Risk Management

NOT REPORTED in accessible sources.

ANALYSIS: The implicit risk control in this strategy is its selectivity — it is in BTC only
at n-day extremes and in cash otherwise. However:
- No explicit stop-loss is described
- The MIN strategy is structurally vulnerable to momentum crashes: if BTC makes a new n-day
  low and continues declining (e.g., 2022 when BTC went 60k → 30k → 20k → 16k in a sequence
  of new lows), the strategy would repeatedly buy into declining prices with no stop
- Position sizing of 100% on each signal is binary and unhedged

## Indicators Used

- **n-day rolling maximum price** (MAX signal — n = 10, 20, 30, 40, 50 days; 10 preferred)
- **n-day rolling minimum price** (MIN signal — same lookbacks)
- No additional indicators documented

## Transaction Costs & Capacity

**Transaction costs:** NOT EXPLICITLY MODELED in accessible summaries.

ANALYSIS: For daily Bitcoin trading, costs are low relative to other markets:
- BTC spot bid-ask spread: typically 0.01–0.05% on major exchanges (Coinbase, Binance)
- Futures funding rate: ~0.01% per 8h for perpetual futures (relevant if using perpetuals)
- Exchange commission: typically 0.1% per side (total ~0.2% round-trip)

At a 10-day lookback, MAX signals may fire 5–15 times per month during trending markets
and rarely during ranging markets. At 0.2% round-trip cost, a 10-times-per-month frequency
would cost ~2% per month in costs — significant. If hold period is longer (5+ days per
signal), cost impact is reduced. Without knowing the signal frequency and hold period
precisely, cost impact is uncertain.

**Capacity:** Bitcoin is sufficiently liquid for retail and small institutional use.
The strategy has no capacity constraints at typical retail sizes (up to $10M).

## Backtest Evidence

**Claimed.** Full period: November 2015 – August 2024 (approximately 9 years).
OOS period (stress test): February 2022 – August 2024 (~2.5 years).

The OOS period includes: the 2022 crypto bear market (BTC fell from ~$40k to ~$16k, −60%),
the 2022-2023 market bottom, and the 2023-2024 recovery and new all-time-high cycle.

All primary sources returned HTTP 403. Evidence from search engine summaries only.

The 2024 paper (SSRN 4955617) extends the 2022 paper's data period and explicitly frames
Feb 2022 – Aug 2024 as an OOS stress test — this is methodologically sound (the strategy
was described before this period, and the update evaluates it on new data).

## Forward-Test Evidence

NOT REPORTED.

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio (full period) | NOT REPORTED | NOT REPORTED | — |
| Sharpe Ratio (OOS, MAX strategy) | NOT REPORTED | NOT REPORTED | — |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown (OOS, MAX) | approximately −12% | CLAIMED, UNVERIFIED | Quantpedia search summary (OOS Feb 2022–Aug 2024) |
| Maximum Drawdown (Bitcoin B&H) | −83.65% | CLAIMED, UNVERIFIED | Quantpedia search summary |
| CAGR (full period) | NOT REPORTED | NOT REPORTED | — |
| OOS Return (MAX, Feb 2022–Aug 2024) | >35% | CLAIMED, UNVERIFIED | Quantpedia search summary |
| Annual Volatility (BTC B&H) | 74.35% | CLAIMED, UNVERIFIED | Quantpedia search summary |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | NOT REPORTED (ANALYSIS: likely 1–5 days) | NOT REPORTED | — |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | NOT REPORTED | NOT REPORTED | — |

**Note on OOS MAX drawdown:** The claim that MAX strategy achieved >35% return with
approximately −12% max drawdown during Feb 2022 – Aug 2024 is significant. The BTC
buy-and-hold likely achieved comparable or slightly lower returns over the same period
(BTC went from ~$40k to ~$16k in 2022, then recovered to ~$65k by Aug 2024 — net positive),
but with a massive intermediate drawdown of ~60%. If the MAX strategy avoided the 2022
crash and captured the 2023-2024 recovery, its risk-adjusted performance is genuinely
impressive. However, without knowing the entry/exit mechanics exactly, I cannot independently
verify this claim.

**No automatic scrutiny triggers apply:** OOS max DD −12% is above the −5% trigger threshold
for "multi-year period without explicit position limits" (OOS is 2.5 years, not multi-year+,
and the strategy is inherently non-positioned much of the time). The >35% return over 2.5
years does not reach the CAGR >50% trigger.

## Community Sentiment

Quantpedia has been tracking this strategy family (MAX/MIN on Bitcoin) since 2022. It has
been featured in multiple Quantpedia publications and a YouTube video. No substantial
community criticism or replication found on forums (ForexFactory, r/algotrading, MQL5).

The strategy is primarily known within Quantpedia's audience and has not been subjected
to academic peer review or broad community scrutiny.

## Why This Might Be Nothing

**1. The MIN strategy may be catching a falling knife in bear markets.** The MIN strategy
buys when BTC hits an n-day low. In 2022, BTC dropped from $60k to $16k in a sequence of
cascading new lows — each new low would have triggered a new long position, accumulating
losses as the decline continued. The OOS claim (combined MIN+MAX achieves "high returns
close to all-time high watermark") suggests the combined strategy performed well, but the
MIN component during 2022 may have significantly detracted from performance, with the MAX
component (cash when not at n-day highs during bear market) providing the protection.

**2. Survivorship/sample bias in Bitcoin.** The entire backtest period (Nov 2015 – Aug 2024)
is dominated by one of history's greatest bull markets in any asset class. Bitcoin rose from
~$400 to ~$65,000 over this period (+16,000%). Any long-BTC strategy that avoids the very
worst drawdowns will look impressive on paper. The "lower drawdown than B&H" bar is set
against B&H Bitcoin's −83.65% drawdown — beating that threshold is not difficult. The real
test is whether the strategy adds value over a passive risk-adjusted Bitcoin allocation (e.g.,
10% BTC / 90% cash would also dramatically reduce drawdown).

**3. Transaction costs not modeled.** Daily lookback signals may generate frequent entries
in volatile markets. Without explicit cost modeling, the net performance is unknown.

**4. Exit condition ambiguity.** The key rules that determine the actual P&L distribution
(how long to hold after an entry, when to stop-loss) are NOT CONFIRMED in accessible sources.
The "next few days" duration and the exact hold/exit logic are essential for any replication.
Without them, this strategy cannot be reproduced.

**5. The MAX strategy's edge may be the same as Donchian breakout.** The `catching-crypto-
trends-donchian-ensemble` (score 64) already in the database uses n-day high/low breakouts
on BTC with multiple lookback periods. If the MAX strategy is economically identical to a
long-only Donchian breakout, the 2024 paper is a restatement of a known concept rather than
a novel finding. The MIN (mean-reversion at new lows) is genuinely distinct, but its edge
is more contestable.

**6. What would change this assessment:** Full strategy specification (exact exit conditions,
position sizing, cost model) plus Sharpe ratio for the full 2015–2024 period. If the
Sharpe is above 1.0 after realistic costs, this is a genuinely interesting approach.

## Strengths / Weaknesses

**Strengths:**
- OOS stress test (Feb 2022–Aug 2024) covers the most severe recent crypto bear market
- Simple, transparent signal (n-day high/low) — easily replicable
- Multiple lookback periods tested — robustness evidence for the "shorter is better" finding
- Dramatic drawdown reduction vs. B&H Bitcoin (−12% OOS vs. −83% B&H full period)
- Quantpedia's institutional reputation for reproducible research adds credibility to the
  methodology even without independent peer review

**Weaknesses:**
- SSRN working paper only — no journal peer review
- Exit conditions NOT CONFIRMED from accessible sources — strategy cannot be fully replicated
- Transaction costs not modeled
- MIN strategy's mean-reversion rationale is weak in sustained bear markets
- All metrics are CLAIMED, UNVERIFIED from search summaries; full paper inaccessible (HTTP 403)
- Only Bitcoin tested; not diversified across altcoins or asset classes

## Confidence Score

| Dimension | Weight | Score (0–10) | Product | Notes |
|-----------|--------|--------------|---------|-------|
| Logical/economic soundness (falsifiable) | 3 | 6 | 18 | MAX mechanism sound (BTC momentum at new highs); MIN weaker (falling knife risk); failure conditions specified; "shorter is better" adds robustness |
| Transparency & reproducibility | 2 | 5 | 10 | Entry signal fully described; exit conditions and sizing NOT CONFIRMED from accessible sources |
| Evidence auditability | 2 | 3 | 6 | Quantpedia working paper (SSRN); no peer review; no open code; all primary sources 403; OOS framing is methodologically sound |
| Risk management quality | 2 | 4 | 8 | Implicit cash-when-flat protection; no explicit stop-loss; MIN strategy structurally vulnerable in sustained bear markets |
| Cost & capacity realism | 2 | 4 | 8 | BTC is liquid; costs not modeled but manageable for low-frequency signals; capacity not a concern |
| Honest treatment of drawdowns / failure | 1.5 | 6 | 9 | OOS max DD ~−12% reported; B&H comparison stated; 2022 bear market covered in OOS; honest about drawdown reduction claim |
| Robustness (OOS / walk-forward / multi-market) | 1.5 | 4 | 6 | One genuine OOS period covering stress (2022–2024); 5 lookback periods tested; BTC-only |
| Survived independent scrutiny | 1 | 3 | 3 | Quantpedia reputation; no independent academic replication; no community forum discussion |

**Weighted sum:** 18 + 10 + 6 + 8 + 8 + 9 + 6 + 3 = 68
**Max possible:** 15 × 10 = 150
**Latent score:** round(68/150 × 100) = **45**
**Evidence auditability = 3 → cap at 59**
**Capped confidence: min(45, 59) = 45**

`latent 45 (capped to 45 pending verification: SSRN working paper only; exit conditions NOT CONFIRMED; transaction costs not modeled; full metrics unavailable due to HTTP 403 blockade)`

**Band: 45 — EXPERIMENTAL**

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Very low — n-day rolling max/min is a single-line operation in Python/pandas
- **Scalability:** Excellent — BTC spot/futures can accommodate large retail and small
  institutional AUM; strategy logic does not depend on order size
- **Automation:** Highly feasible — daily lookback signal, straightforward execution via
  Coinbase, Binance, or any BTC-enabled brokerage API

## MQL5 / Python / Pine Feasibility

- **Python:** Trivially feasible — `pandas.DataFrame.rolling(n).max()` / `.min()`; 
  < 30 lines of logic
- **Pine Script:** Trivially feasible — `ta.highest(close, n)` / `ta.lowest(close, n)`;
  implementable in TradingView for BTC/USD charts
- **MQL5:** Feasible — BTC/USD is available as a CFD on MT4/MT5; `iHigh` / `iLow` period-
  based indicators; straightforward implementation

## Similar Strategies

- `catching-crypto-trends-donchian-ensemble` (score 64) — Uses Donchian channels (n-day
  high/low) on BTC + altcoins with 9 lookbacks simultaneously, with BOTH long (breakout
  above n-day high) and short (breakdown below n-day low) positions. The MAX strategy is
  similar to the Donchian long signal. The KEY DISTINCTION: Donchian goes SHORT at n-day
  lows (trend-following) while this paper's MIN strategy goes LONG at n-day lows
  (mean-reversion). Both cannot simultaneously be right — unresolved empirical question.
- `adaptivetrend-crypto` (score 41) — Different mechanism (6-hour MACD-based trend on
  150+ Binance perpetual pairs); no n-day high/low signal

## Red Flags

- **Exit conditions unknown** — critical missing piece; strategy cannot be fully replicated
- **MIN strategy: falling knife risk** — buying new n-day lows in sustained bear markets
- **Transaction costs not modeled** — especially relevant if signal frequency is high
- **SSRN working paper only** — no peer review; Quantpedia-internal research
- **All primary sources 403** — full metrics unavailable; evidence limited to search summaries
- **Single asset (BTC) tested** — no generalizability beyond Bitcoin

## Source Links

| Source | Retrieved | Notes |
|--------|-----------|-------|
| [SSRN 4955617](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4955617) | 2026-05-29 | Primary (2024 update); HTTP 403 blocked |
| [Quantpedia: Revisiting trend-following and mean-reversion in Bitcoin](https://quantpedia.com/revisiting-trend-following-and-mean-reversion-strategies-in-bitcoin/) | 2026-05-29 | Key search summary source; HTTP 403 blocked |
| [Quantpedia: Original trend-following and mean-reversion in Bitcoin](https://quantpedia.com/trend-following-and-mean-reversion-in-bitcoin/) | 2026-05-29 | Original 2022 paper overview; HTTP 403 blocked |
| [SSRN 4081000](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4081000) | 2026-05-29 | Predecessor paper (Padysak & Vojtko 2022); HTTP 403 blocked |
| [Quantpedia Medium](https://quantpedia.medium.com/revisiting-trend-following-and-mean-reversion-strategies-in-bitcoin-c21e190ca69e) | 2026-05-29 | Practitioner writeup; HTTP 403 blocked |

All primary sources returned HTTP 403. All evidence from search engine summaries only.

## Analyst Notes

**FACTS (from search summaries):**
- 2024 paper: "Revisiting Trend-following and Mean-Reversion Strategies in Bitcoin"
- Authors: Soňa Beluška, Radovan Vojtko (Quantpedia); SSRN 4955617 (Sep 12, 2024)
- Predecessor: Padysak & Vojtko (2022), SSRN 4081000
- Full period: November 2015 – August 2024
- OOS stress test: February 2022 – August 2024
- Lookbacks: 10, 20, 30, 40, 50 days; shorter is better; 10 preferred
- MAX strategy OOS: >35% return, approximately −12% max drawdown (CLAIMED, UNVERIFIED)
- BTC B&H: volatility 74.35%, max drawdown −83.65% (CLAIMED, UNVERIFIED)
- "When BTC is at local maxima, it tends to continue trending upwards" — supporting MAX
- "BTC tends to mean-revert and bounce back" at local minima — supporting MIN
- Both strategies lower volatility/drawdown than B&H Bitcoin (CLAIMED, UNVERIFIED)
- MAX strategy "remains alive and well" in OOS (CLAIMED, UNVERIFIED)

**ANALYSIS (my reasoning):**
- The OOS MAX claim (>35% return, ~−12% DD over Feb 2022–Aug 2024) is internally plausible:
  BTC net return over this period was approximately +60% (from ~$40k to ~$65k) but with a
  −60% intermediate drawdown; a strategy that sat out the bear phase and entered on
  breakout-continuation could plausibly achieve +35% with −12% DD
- The MIN strategy's OOS behavior during 2022 is the critical unknown — if it repeatedly
  bought new lows during the bear, it would have dragged combined performance significantly
- The tension between MAX (momentum) and MIN (mean reversion) strategies at the same lookback
  suggests these are being tested independently, not as simultaneous positions

**ASSUMPTIONS (flagged):**
- ASSUMED: Positions are 100% long/flat (no partial sizing or leverage)
- ASSUMED: Exit is a fixed short holding period (1–5 days) based on "next few days" language
- ASSUMED: The OOS >35% MAX return covers the full Feb 2022–Aug 2024 window, not just the recovery
- ASSUMED: Transaction costs were NOT modeled in the SSRN paper (consistent with Quantpedia style)

## Future Research Needed

1. Access SSRN 4955617 to obtain: exact hold period, exact exit conditions, position sizing,
   transaction cost assumptions, Sharpe ratio, full-period CAGR, and MIN strategy standalone
   OOS performance (was it positive or negative during 2022?)
2. Implement 10-day MAX strategy in Python with explicit cost modeling and compare to
   `catching-crypto-trends-donchian-ensemble` on the same BTC data
3. Test whether MIN strategy (buy at n-day lows) is actually profitable net of costs across
   the 2022 bear market, or whether the combined MAX+MIN performance is primarily driven by
   the MAX component
4. Extend to altcoins to test generalizability beyond BTC
5. Compare to a simple 10-day Donchian long-only strategy (close when price falls below
   n-day high) to establish whether the MIN component adds or detracts value
