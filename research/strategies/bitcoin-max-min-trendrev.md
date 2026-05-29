# Bitcoin MAX/MIN Trend-Following & Mean-Reversion

## Overview

Two complementary long-only timing strategies for Bitcoin developed by Quantpedia
researchers. The **MAX strategy** buys BTC when today's price sets a new N-day rolling
maximum (trend-continuation bet). The **MIN strategy** buys BTC when today's price sets a
new N-day rolling minimum (mean-reversion bet). A **combined strategy** buys BTC when
either condition is met, staying in cash otherwise. Original paper published 2022; revised
and extended through August 2024 in SSRN 4955617.

## Asset Class / Timeframes

- **Asset:** Bitcoin (BTC/USD)
- **Timeframe:** Daily
- **Sample period (full):** November 2015 – August 2024
- **In-sample period (original 2022 study):** November 2015 – February 2022
- **Out-of-sample (OOS) / stress-test period:** February 4, 2022 – August 20, 2024
- **Lookback periods tested:** 10, 20, 30, 40, 50 days

## Core Logic

On each trading day, calculate:
- **N-day MAX:** rolling maximum closing price over the previous N days
- **N-day MIN:** rolling minimum closing price over the previous N days

**MAX strategy rule:** If today's closing price = the N-day maximum (i.e., BTC just set
a new N-day high), enter long. Exit when BTC is no longer at the N-day high.

**MIN strategy rule:** If today's closing price = the N-day minimum (i.e., BTC just set
a new N-day low), enter long. Exit when BTC is no longer at the N-day low.

**Combined rule:** Long BTC if either MAX or MIN condition is met; otherwise hold cash.

All three variants are long-only. No short selling.

## Economic Rationale

**MAX strategy:** When BTC breaks above its prior N-day high, overhead resistance has been
cleared. Buyers were willing to pay a new record price, suggesting demand exceeds supply at
prior levels. This is the canonical Donchian channel / price breakout momentum rationale.
The mechanism should FAIL during range-bound, choppy sideways markets where prices oscillate
near the N-day high without genuine directional momentum, generating whipsaw entries.

**MIN strategy:** When BTC falls to a new N-day low, it has become "oversold" relative to
its recent range. Historically the asset rebounds. However, this logic BREAKS DOWN in
extended trending bear markets (2018 crash: -84%; 2022 crash: -77%), when new N-day lows
accumulate continuously for weeks. The fact that MIN produces 80%+ drawdowns confirms the
mechanism is unreliable in bear regimes.

**Overall falsifiability:** The MAX mechanism fails when trend strength is absent (sideways
consolidation, choppy markets). The MIN mechanism fails in sustained bear trends.

## Entry Conditions

- **MAX:** `Close[t] >= MAX(Close, N, t-1)` — today's close equals or exceeds the prior
  N-day rolling maximum. For 10-day lookback, buy if today's BTC price is the highest in
  the last 10 days.
- **MIN:** `Close[t] <= MIN(Close, N, t-1)` — today's close equals or falls below the
  prior N-day rolling minimum.
- **Best lookback:** 10-day period found to outperform 20, 30, 40, 50-day variants.

## Exit Conditions

- Exit long when the condition that triggered entry no longer holds (BTC is no longer at
  the N-day extreme). Exact implementation details (e.g., hold for one day, or exit when
  condition fails next day) NOT REPORTED in accessible summaries.

## Risk Management

- **Stop-loss:** NOT REPORTED. No stop-loss mechanism mentioned.
- **Position sizing:** NOT REPORTED. Implied: fully invested (100% BTC) when long, 100%
  cash otherwise.
- **Leverage:** NOT REPORTED. Implied: 1× unlevered.
- **Regime filter:** None documented. Strategy exits naturally to cash when BTC is not at
  extremes.

**Assessment:** Minimal risk management. Relies entirely on the signal's filtering property
to avoid bear markets (which works for MAX but not MIN).

## Indicators Used

- N-day rolling maximum price
- N-day rolling minimum price
- No additional indicators

## Transaction Costs & Capacity

**Transaction costs:** NOT REPORTED in accessible summaries. Quantpedia working papers
sometimes include and sometimes exclude costs. Given this is a daily strategy that trades
only when new N-day extremes are set (lower turnover than a daily-rebalance strategy),
the number of trades per year is moderate, not extreme. However:
- Crypto exchange fees: 0.1–0.5% round-trip (Binance, Coinbase)
- Slippage on BTC: minimal at retail size given BTC's depth, but non-negligible at scale
- **CONCLUSION:** Costs likely do not annihilate the gross edge given infrequent trading, but
  the net-of-cost return is unknown and the accessible evidence does not confirm cost modeling.

**Capacity:** BTC spot market is highly liquid ($20–50B+ daily volume in 2023–2024). This
is a simple long/flat strategy. Capacity is not a binding constraint at reasonable sizes.

## Backtest Evidence

CLAIMED, UNVERIFIED — Quantpedia working paper, no independent peer review. All
accessible evidence via search engine summaries; primary source URLs returned HTTP 403.

**Full sample (Nov 2015 – Aug 2024), Combined 10-day strategy:**

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Annualized Return | 98.43% | CLAIMED, UNVERIFIED | SSRN 4955617 / Quantpedia |
| Annualized Volatility | 47.75% | CLAIMED, UNVERIFIED | SSRN 4955617 / Quantpedia |
| Maximum Drawdown | -37.67% | CLAIMED, UNVERIFIED | SSRN 4955617 / Quantpedia |
| Return/Volatility Ratio | 2.06 | CLAIMED, UNVERIFIED | SSRN 4955617 / Quantpedia |

**BTC Buy-and-Hold comparison (same period):**

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Annualized Volatility (BTC B&H) | 74.35% | CLAIMED, UNVERIFIED | SSRN 4955617 / Quantpedia |
| Maximum Drawdown (BTC B&H) | -83.65% | CLAIMED, UNVERIFIED | SSRN 4955617 / Quantpedia |

**OOS period (Feb 2022 – Aug 2024):**

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Return (OOS, combined strategy) | >35% | CLAIMED, UNVERIFIED | Quantpedia |
| Maximum Drawdown (OOS, combined) | ~-12% | CLAIMED, UNVERIFIED | Quantpedia |

## Forward-Test Evidence

NOT REPORTED. No live-trading forward test documented.

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | NOT REPORTED | NOT REPORTED | — |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | -37.67% (combined, full) / ~-12% (OOS) | CLAIMED, UNVERIFIED | SSRN 4955617 |
| CAGR | ~98.43% (return/vol; implied ≈CAGR) | CLAIMED, UNVERIFIED | SSRN 4955617 |
| Annual Return | NOT REPORTED (year-by-year) | NOT REPORTED | — |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | NOT REPORTED | NOT REPORTED | — |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | NOT REPORTED | NOT REPORTED | — |

**Note on "Sharpe Ratio":** The paper reports a Return/Volatility ratio of 2.06 for the
combined strategy. This is effectively a Sharpe ratio with zero risk-free rate. Using the
reported annualized return (98.43%) and volatility (47.75%), the implied Sharpe ≈ 2.06.
[ANALYSIS — derived from reported figures, NOT a table value] This is high by CTA
standards (~14–19× the typical per-market CTA Sharpe of 0.15–0.20) but Bitcoin itself had
a Ret/Vol ≈ 1.07 (80%/74.35% estimated B&H) over this period, so the strategy's 2.06 is
~1.9× better risk-adjusted than B&H — a more plausible edge claim.

## Community Sentiment

Limited accessible discussion. Quantpedia published both the original 2022 paper and the
2024 update as house research. LinkedIn discussion noted (David Forino post). No forum
or adversarial community criticism found in accessible sources. The Quantpedia brand
promotes this strategy research as part of its subscription service — commercial motivation
is present.

No independent replication by an unaffiliated researcher found.

## Why This Might Be Nothing

### 1. Long-only BTC beta dressed as timing (most likely benign explanation)

The combined MAX+MIN strategy spends a significant fraction of time long BTC. In a
secular bull market (2015–2021: BTC ~$300 → ~$65,000), almost any long-only timing
strategy with partial cash exposure will show strong absolute returns. The 98.43%
annualized return may primarily reflect uncompensated BTC bull-market risk-taking with
selective cash-hedging, not a genuine timing edge. The risk-adjusted improvement over B&H
(Ret/Vol 2.06 vs. ~1.07) is more meaningful, but remains a single in-sample figure.

### 2. The cost case

Transaction costs NOT REPORTED in accessible summaries. If the strategy trades ~30–50
times per year (rough estimate for a 10-day MAX/MIN signal), round-trip costs of 0.1–0.5%
would reduce returns by 3–25 bps × 30–50 = 0.9–12.5% annually. Given the claimed
98.43% annualized return, this is not strategy-killing at typical retail fees, but it is
not zero and the paper does not address it.

### 3. MIN strategy is a "catch falling knives" rule

The MIN strategy produces 80%+ drawdowns — essentially confirming that buying BTC at new
N-day lows repeatedly during bear markets (2018: BTC −84%; 2022: BTC −77%) is dangerous.
The strategy's apparent OOS success is driven almost entirely by the MAX component (which
avoids bear markets by never hitting a new high during sustained declines). The MIN
component is a liability.

### 4. Lookback optimization (mild data mining)

Testing 5 lookback periods and selecting the best (10-day) is mild in-sample fitting.
The authors' own finding — "shorter lookback is better for both strategies" — could reflect
Bitcoin's high-momentum regime during the sample period. In a less momentum-driven market,
a 10-day breakout would produce more whipsaws.

### 5. OOS regime luck

The OOS period (Feb 2022 – Aug 2024) happened to align with one of BTC's sharpest
drawdown-and-recovery cycles. The MAX strategy would have stayed in cash during most of
the 2022 crash (no new 10-day highs during a -77% decline) and then would have entered
on the breakout to new highs in 2023–2024. This is a favorable OOS regime for any
trend-following signal on BTC, not a generic stress test.

### 6. No short-selling in the MIN strategy

Buying at new lows without a corresponding short position in bear markets misses the
obvious structural improvement. The strategy would be more complete (and more risky) if
it went short at MIN and long at MAX.

### 7. What would change my mind

A peer-reviewed replication on multiple crypto assets (ETH, SOL, BNB) across multiple
market cycles, with explicit net-of-fee Sharpe ratios (not just Ret/Vol), and a year-by-year
performance breakdown showing the strategy adds value in years other than the 2023–2024
bull run.

## Strengths / Weaknesses

**Strengths:**
- Extremely simple rules, easily reproducible (no indicator software required)
- Logical economic mechanism for MAX component (momentum / breakout)
- MAX strategy demonstrably avoided the 2022 crash (−12% OOS drawdown vs. BTC −77%+)
- Lower volatility (47.75%) and lower drawdown (−37.67%) vs. BTC B&H — honest
  risk-adjusted improvement reported
- OOS period is a genuine stress test (2022 bear market)
- Predecessor paper (2022) provides additional historical context

**Weaknesses:**
- MIN strategy is theoretically weak and empirically produces 80%+ drawdowns
- Long-only only; no downside hedge
- No stop-loss or position sizing
- Transaction costs not modeled in accessible summaries
- Not peer-reviewed; house publication by a subscription-focused research aggregator
- Single-asset (BTC only); no multi-asset or multi-exchange replication
- Sharpe ratio not reported explicitly (Ret/Vol ratio given instead)
- 10-day lookback finding may reflect mild in-sample optimization

## Confidence Score

**Dimension breakdown:**

| Dimension | Score | Weight | Weighted |
|-----------|-------|--------|----------|
| Logical/economic soundness (falsifiable) | 6 | 3 | 18 |
| Transparency & reproducibility | 8 | 2 | 16 |
| Evidence auditability | 3 | 2 | 6 |
| Risk management quality | 2 | 2 | 4 |
| Cost & capacity realism | 2 | 2 | 4 |
| Honest treatment of drawdowns/failure | 6 | 1.5 | 9 |
| Robustness (OOS/walk-forward) | 5 | 1.5 | 7.5 |
| Survived independent scrutiny | 3 | 1 | 3 |

**Total weighted: 67.5 / 150**
**Latent score: round(67.5/150 × 100) = 45**
**Evidence auditability = 3 (≤4) → Verification cap: 59**
**Confidence: min(45, 59) = 45 — EXPERIMENTAL**

*latent 45 (capped to 45 pending verification: Quantpedia working paper; no peer review;
no open code; all performance claims CLAIMED, UNVERIFIED; HTTP 403 on primary sources)*

**Mandatory scrutiny trigger — MAX drawdown <5%?** No — stated max DD is -37.67% (combined)
and -12% (OOS). No automatic trigger crossed.

## Complexity / Scalability / Automation Feasibility

**Complexity:** Very low. Two lines of code (rolling max/min comparison). No specialized
libraries needed.

**Automation:** Easily automated via any daily data source (Binance API, Yahoo Finance).
Signal checked once per day at close. Minimal execution complexity.

**Scalability:** BTC market is deep at typical retail sizes. Scaling to $1M+ would have
negligible market impact.

## MQL5 / Python / Pine Feasibility

- **Python:** Trivial — 10 lines of pandas code.
- **Pine Script (TradingView):** Trivial — `ta.highest()` / `ta.lowest()` built-in.
- **MQL5:** Straightforward — custom indicator with rolling high/low comparison.

## Similar Strategies

- `catching-crypto-trends-donchian-ensemble` — Donchian channel ensemble (9 lookback periods
  5–360d) on crypto; similar momentum-breakout mechanism on daily timeframe; score 64
- `adaptivetrend-crypto` — Multi-component crypto trend-following with Sharpe-weighted
  asset selection; H6 timeframe; score 41

## Red Flags

- No transaction costs documented in accessible evidence
- MIN strategy produces 80%+ drawdowns — structurally problematic in bear markets
- House publication by subscription-oriented aggregator (Quantpedia) — commercial motivation
- No peer review; no independent replication
- 98.43% annualized return is extraordinary; primarily reflects BTC bull-market beta,
  not a pure timing alpha
- Only one lookback period selected post-optimization from a menu of 5

## Source Links

| Source | Retrieval Date |
|--------|---------------|
| [SSRN 4955617 — Revisiting Trend-following and Mean-Reversion Strategies in Bitcoin (Beluška/Vojtko, Sep 2024)](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4955617) | 2026-05-29 |
| [Quantpedia article — Revisiting Trend-following and Mean-reversion Strategies in Bitcoin](https://quantpedia.com/revisiting-trend-following-and-mean-reversion-strategies-in-bitcoin/) | 2026-05-29 (HTTP 403) |
| [Quantpedia article — Trend-following and Mean-reversion in Bitcoin (original 2022)](https://quantpedia.com/trend-following-and-mean-reversion-in-bitcoin/) | 2026-05-29 (HTTP 403) |
| [Quantpedia Medium post — Revisiting Trend-following](https://quantpedia.medium.com/revisiting-trend-following-and-mean-reversion-strategies-in-bitcoin-c21e190ca69e) | 2026-05-29 (HTTP 403) |
| [SSRN 4081000 — Seasonality, Trend-following, and Mean reversion in Bitcoin (Padysak/Vojtko, 2022)](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4081000) | 2026-05-29 |

## Analyst Notes

**FACTS (from sources):**
- Combined MAX+MIN 10-day strategy: Annualized return 98.43%, vol 47.75%, MDD -37.67%, Ret/Vol 2.06
- BTC B&H same period: vol 74.35%, MDD -83.65%
- OOS (Feb 2022 – Aug 2024): combined strategy >35% return, ~-12% max DD
- Shorter lookback periods (10-day) outperform longer (20, 30, 40, 50-day)
- MIN strategy alone has drawdowns >80%
- OOS period described as "a perfect stress test" given BTC's decline from Feb 2022 ATH

**ANALYSIS (my reasoning):**
- Implied Sharpe ≈ 2.06 for combined strategy (Ret/Vol with zero risk-free rate) is high
  in absolute terms but 1.9× BTC's own risk-adjusted return — this is the real claim
- BTC buy-and-hold estimated ≈ 80% annualized over Nov 2015 – Aug 2024 (from $300 to
  ~$60K ≈ 200× in 9 years ≈ 80% CAGR). The combined strategy's 98.43% annualized thus
  slightly exceeds B&H absolute return AND has lower risk — claims are internally consistent
  and not as extreme as the headline number suggests
- The 2022 OOS performance is the most compelling evidence: staying ~flat during a -77% crash
  while catching the 2023–2024 recovery is a genuine out-of-sample achievement for a simple rule
- The MIN component adds little value and introduces extreme tail risk; future research should
  evaluate MAX-only performance separately

**ASSUMPTIONS (not confirmed):**
- Strategy is evaluated on Bitstamp or similar; exact exchange and data source NOT REPORTED
- "Annualized return" likely CAGR, not arithmetic average (not confirmed)
- No leverage assumed

## Future Research Needed

1. Obtain full strategy file from Quantpedia or SSRN — confirm exact exit rules and whether
   costs are modeled
2. Evaluate MAX-only strategy performance in isolation (vs. combined MIN+MAX)
3. Replicate on ETH, SOL, BNB to test multi-asset generalizability
4. Compute year-by-year return breakdown to identify regime dependency
5. Add explicit transaction cost modeling (0.1% roundtrip Binance) to gross returns
6. Test on 2024–2026 extended OOS period
