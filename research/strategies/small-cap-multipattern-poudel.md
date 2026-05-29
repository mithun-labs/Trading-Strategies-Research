# Small-Cap Multi-Pattern Trading Suite (Poudel 2025)

## Overview

This entry covers SSRN 5921742 (December 2025) by Sandip Poudel, titled "Small-Cap Stock
Trading Strategies for Retail Traders: A Data-Driven, Statistically Validated, and Reproducible
Study of Strategy Design, Risk Management, and Technical-Pattern-Based Signals." The paper
develops and validates six distinct strategy families targeting US small-cap equities
($300M–$2B market cap), with the best-performing composite achieving approximately 14% CAGR,
Sharpe 0.7–0.9 OOS, and max drawdown below 17%. These are credible (non-extreme) claims
supported by walk-forward methodology and bias-control measures.

**Author note:** Poudel writes for a retail day-trader audience (Medium marketing article with
retail-marketing framing) and his quantitative credentials are not independently verified.
This does not invalidate the methodology but adds author-credibility uncertainty. The paper
appears to be a comprehensive dissertation rather than an academic journal submission.

**Classification: EXPERIMENTAL — promising methodology, requires independent replication**

---

## Asset Class / Timeframes

- **Market:** US Small-Cap Equities ($300M–$2B market capitalization)
- **Timeframe:** Intraday (implied by gap-and-go, ORB, and mean-reversion descriptions);
  exact timeframes NOT REPORTED for each strategy family
- **Exchanges:** US equity markets (Alpaca, Interactive Brokers, Tradier compatible)
- **Liquidity filter:** Minimum $500K average daily volume
- **Backtest period:** NOT REPORTED in accessible sources

---

## Core Logic — Six Strategy Families

### Family 1: Volatility-Scaled Momentum with Liquidity Filters
Entry on directional momentum signals scaled by realized volatility, gated by minimum
liquidity conditions. High-vol environments get smaller position sizes; low-vol get larger.
Exact momentum signal construction NOT REPORTED.

### Family 2: Gap-and-Go Opening Range Breakout Variants
Stocks that gap up (or down) at market open on catalysts (news, earnings, sector moves)
and then continue breaking out of the opening range. Entry on ORB breakout confirmation
above the first N minutes' range. Multiple variant parameters tested with walk-forward
selection. Exact ORB window and breakout threshold NOT REPORTED.

### Family 3: Mean Reversion After Extreme Intraday Moves
Buys extreme intraday dips (and shorts extreme spikes) in small-cap stocks, betting on
price snapback. Entry on extreme intraday deviation from anchor (VWAP or prior close implied
from description). Risk: small-cap stocks in free-fall may not revert. Exact deviation threshold
and reversal confirmation NOT REPORTED.

### Family 4: Breakout-Retest Pattern Recognition
Price breaks above a prior resistance level, retests the breakout level from above, and then
resumes upward. Entry on successful retest confirmation. Exact pattern definition and confirmation
criteria NOT REPORTED.

### Family 5: Event-Driven Volatility with Timestamp Discipline
Trades volatility expansion following earnings announcements, FDA decisions, or other scheduled
catalysts. Uses "timestamp discipline" to prevent look-ahead bias in event-time data (events
are tagged with the exact release time, not just the date). Entry and exit rules NOT REPORTED.

### Family 6: Market Regime-Filtered Composite
Combines signals from the five families above, filtered by a market regime classifier (bull/bear/
sideways detection). Each regime enables/disables specific strategy families. Composite achieves
the best reported OOS performance (~14% CAGR, Sharpe 0.7–0.9, max DD <17%).

---

## Economic Rationale

Small-cap equities exhibit genuine structural inefficiencies:

- **Low analyst coverage:** Information is slow to be priced in — retail-driven price discovery
  creates momentum and mean-reversion opportunities not present in large-caps
- **Retail-dominated trading:** Retail participants follow technical patterns more rigidly than
  institutional algos, making pattern-based entries more reliable
- **Catalyst-driven volatility:** News and earnings impact is disproportionate in small-caps
  due to limited institutional market-making depth
- **Liquidity constraints:** Bid-ask spreads and depth are features of this market, not bugs —
  they create predictable short-term mispricing episodes
- **Regime sensitivity:** Small-caps are cyclically sensitive; regime filtering appropriately
  conditions strategy selection on macro environment

The economic rationale for each family is sound and grounded in small-cap market structure.

---

## Entry Conditions

NOT FULLY REPORTED for individual families. General structure:
- Family 2 (Gap-and-Go): gap at open + ORB breakout confirmation
- Family 3 (Mean Reversion): extreme intraday deviation from anchor price
- Family 4 (Breakout-Retest): confirmed retest of prior resistance/support
- Family 5 (Event-Driven): scheduled catalyst + volatility expansion signal

All entries gated by: minimum $500K daily average volume; market regime classification

---

## Exit Conditions

NOT REPORTED in accessible sources. Implied: ATR-based or time-based stops typical for
intraday small-cap strategies. The paper describes "risk management" components but exact
stop/target levels are not recoverable from search summaries.

---

## Risk Management

- **Liquidity filter:** Minimum $500K average daily volume — prevents entry in extremely thin
  stocks where execution slippage would destroy edge
- **Transaction costs modeled:** $0.01–$0.05 spreads + $0–$1.00/share commissions (captures
  both ECN and per-share brokerage models)
- **Survivorship bias controlled:** Explicitly includes delisted and bankrupt stocks in
  backtests — this is a significant methodological strength
- **Regime filtering:** Composite strategy disables families that don't perform in current regime
- **Ethics framework:** Addresses pump-and-dump risk — suggests the strategies avoid or flag
  potentially manipulated stocks (important for small-caps)
- **Stress testing:** Cross-regime validation mentioned (parameters NOT REPORTED)

---

## Indicators Used

NOT FULLY REPORTED. Implied from strategy descriptions:
- Volume (average daily, intraday patterns)
- OHLCV-based opening range
- Intraday price deviation from anchor (VWAP implied)
- Catalyst/event timestamp data
- Market regime classifier (construction NOT REPORTED)
- ATR (implied for stop placement)

---

## Backtest Evidence

- **Type:** Walk-forward analysis with nested cross-validation
- **Bias control:** Survivorship bias (includes delisted stocks), look-ahead bias (point-in-time
  data), multiple-hypothesis testing corrections
- **Stress testing:** Cross-regime validation
- **Transaction costs:** Realistic ($0.01–$0.05 spread, $0–$1.00/share)
- **Period:** NOT REPORTED
- **Auditable?** Partially — paper describes Python code with broker API integration; full
  code not publicly accessible; all direct fetches returned HTTP 403

---

## Forward-Test Evidence

NOT REPORTED. No live trading record mentioned.

---

## Reported Metrics

All metrics CLAIMED, UNVERIFIED (SSRN preprint; no independent replication; all fetches 403'd):

| Metric | Value | Tag | Plausibility |
|--------|-------|-----|-------------|
| OOS Sharpe ratio (composite) | 0.7–0.9 | CLAIMED, UNVERIFIED | Plausible for small-cap intraday strategies |
| CAGR (best composite) | ~14% | CLAIMED, UNVERIFIED | Plausible |
| Max drawdown | <17% | CLAIMED, UNVERIFIED | Plausible |
| Liquidity filter | $500K+ avg daily volume | CLAIMED, UNVERIFIED | Plausible |
| Individual family Sharpe range | NOT REPORTED | NOT REPORTED | — |

The reported metrics are in a **plausible range** — Sharpe 0.7–0.9 is achievable for well-designed
intraday small-cap strategies and does not trigger the same implausibility concerns as the gold
paper (Sharpe 2.88). CAGR 14% and max drawdown <17% are consistent with each other and with
moderate institutional retail strategy performance.

---

## Community Sentiment

**No community discussion found** on Reddit r/algotrading, ForexFactory, MQL5, or any
practitioner forum.

The paper was published December 2025 and has very limited external visibility. The author
published a Medium article ("Why I Wrote My Small-Cap Trading Strategy Thesis And How Day
Traders Can Profit Long-Term") with a retail-marketing tone, which does not inspire institutional
confidence but is not in itself a disqualifying red flag.

---

## Strengths

1. **Credible performance metrics:** Sharpe 0.7–0.9 OOS is not implausible — within the range
   of what well-designed intraday small-cap strategies achieve
2. **Bias control:** Survivorship bias explicitly addressed (includes delisted stocks);
   look-ahead bias controlled; multiple-hypothesis testing corrections applied — above average
   rigor for a retail-authored paper
3. **Realistic transaction costs:** $0.01–$0.05 spread + commission modeling prevents the
   "zero-cost" backtest error common in retail research
4. **Diversification:** Six strategy families with regime filtering reduces regime dependency
5. **Practical implementation:** Broker API integration (Alpaca, IB, Tradier) described
6. **Sound economic rationale:** Small-cap structural inefficiencies are genuine and
   well-documented in academic literature (e.g., small-firm effect, limited analyst coverage)
7. **Ethics framework:** Pump-and-dump risk addressed — unusual and positive signal

## Weaknesses

1. **Author credentials unclear:** Writes for retail day traders, no institutional quantitative
   background evident. Methodology may be correct, but execution-level expertise is uncertain.
2. **Retail marketing framing:** Medium article tone suggests commercial motivation
3. **December 2025 publication:** Too recent for independent replication
4. **No live trading record**
5. **Exact entry/exit rules not accessible:** Full signal specifications not recoverable from
   available summaries
6. **$500K volume filter is low:** Many small-caps at $500K daily volume are extremely illiquid
   in practice — institutional traders consider $5M+ a minimum. Retail slippage at $500K volume
   could be substantial.
7. **Six strategy families = parameter proliferation:** Even with walk-forward validation,
   the regime classifier + 6 families × multiple parameters creates overfitting risk
8. **No peer review in a recognized quant finance journal**
9. **Backtest period not specified:** Cannot assess regime coverage

---

## Confidence Score

| Dimension | Score (0–10) | Weight | Weighted |
|-----------|-------------|--------|----------|
| Logical / economic soundness | 6 | ×3 | 18.0 |
| Transparency & reproducibility | 6 | ×2 | 12.0 |
| Evidence quality | 5 | ×2 | 10.0 |
| Risk management quality | 6 | ×2 | 12.0 |
| Honest treatment of drawdowns / failure | 6 | ×1.5 | 9.0 |
| Robustness evidence (OOS, walk-forward) | 5 | ×1.5 | 7.5 |
| Simplicity / low overfitting risk | 4 | ×1 | 4.0 |
| Survived independent scrutiny | 2 | ×1 | 2.0 |
| **Total** | | | **74.5** |

**Score = (74.5 / 140) × 100 = 53** — EXPERIMENTAL

**Dimension rationale:**
- *Logical soundness 6/10:* Six pattern families have genuine economic rationale grounded in
  small-cap market structure. Not penalized as heavily as gold paper because these claims are
  modest and the mechanisms are well-understood.
- *Transparency 6/10:* Python code described with broker integration. Walk-forward + nested CV
  explained. But full code not public; entry/exit rules not recoverable.
- *Evidence quality 5/10:* OOS Sharpe 0.7–0.9 is plausible. Survivorship bias and look-ahead
  bias controlled. Multiple-testing corrections applied. But no live trading, no independent
  replication, backtesting period not specified.
- *Risk management 6/10:* Liquidity filter, realistic costs, regime filtering, ethics framework.
  $500K volume filter is low but present.
- *Honest drawdowns 6/10:* Reports max drawdown <17% explicitly. No hiding of regime failures
  (stress testing mentioned). But no live results to verify.
- *Robustness 5/10:* Walk-forward with nested CV is strong. Regime stress testing helps. But
  one author, no peer review, no independent replication.
- *Simplicity 4/10:* Six strategy families + regime classifier = high parameter count.
  Nested CV helps but doesn't eliminate all overfitting risk.
- *Scrutiny 2/10:* Zero community discussion found. No independent replication. Author's
  HarbourFront-style practitioner review absent.

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** High — six strategy families + regime classifier + nested CV parameter
  selection requires significant infrastructure
- **Scalability:** Limited to small-cap universe; $500K volume filter already restricts
  to thin stocks where capacity is constrained
- **Automation:** Feasible with described broker APIs (Alpaca for retail, IB for semi-pro)

## MQL5 / Python / Pine Feasibility

- **Python:** YES — explicitly described; vectorized + event-driven Python code in paper
- **Pine Script:** Partial — individual patterns feasible; regime-filtered composite is complex
- **MQL5:** Partial — event-driven volatility strategy would need external news/catalyst feed

---

## Similar Strategies

- `orb-stocks-in-play` — score 78 (High Potential) — also US equities intraday, ORB on
  news-driven stocks, but single strategy rather than multi-family suite; better evidence quality
- `intraday-momentum-spy` — score 50 (Experimental) — also intraday US equities but large-cap
  ETF; different market dynamics (SPY vs small-cap)

---

## Red Flags

- **Author retail framing:** Marketing tone ("How Day Traders Can Profit Long-Term") suggests
  possible commercial motivation
- **$500K volume filter:** May be too low for practical retail execution without significant
  slippage
- **Six strategy families:** High parameter count risk despite nested CV
- **December 2025:** Too recent for any independent validation
- **No live trading record**
- **Backtest period not disclosed:** Cannot assess out-of-sample regime coverage
- **"Ethics framework addressing pump-and-dump risk":** Suggests the strategies may sometimes
  touch stocks at risk of manipulation — regulatory and execution risk

---

## Source Links

| URL | Retrieved |
|-----|-----------|
| https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5921742 | 2026-05-29 (HTTP 403) |
| https://papers.ssrn.com/sol3/Delivery.cfm/5921742.pdf | 2026-05-29 (HTTP 403) |
| https://medium.com/@poudelsandip/why-i-wrote-my-small-cap-trading-strategy-thesis | 2026-05-29 (HTTP 403) |

All primary source fetches failed (HTTP 403). All data gathered via search engine summaries.

---

## Analyst Notes

**FACTS (from search engine summaries of SSRN 5921742):**
- Paper by Sandip Poudel, SSRN December 2025
- Market: US small-cap equities ($300M–$2B), minimum $500K average daily volume
- Six strategy families: volatility-scaled momentum, gap-and-go ORB, mean reversion,
  breakout-retest, event-driven volatility, regime-filtered composite
- OOS Sharpe 0.7–0.9; composite CAGR ~14%; max drawdown <17%
- Walk-forward with nested cross-validation; survivorship bias controlled; multiple-testing corrections
- Python code with Alpaca/IB/Tradier broker API integration
- Transaction costs: $0.01–$0.05 spread + $0–$1.00/share commission

**ANALYSIS:**
This paper is more credible than the gold futures paper due to: (a) moderate, non-extreme
performance claims; (b) explicit bias-control measures; (c) realistic cost modeling; (d) sound
economic rationale per strategy family.

The most promising of the six families for further research is the **gap-and-go ORB** (Family 2),
which is closely related to the `orb-stocks-in-play` strategy already in the database (score 78).
The small-cap variant may offer higher edge due to lower analyst coverage and greater retail
participation, though execution costs and slippage are higher.

The **mean reversion after extreme intraday moves** (Family 3) is the highest-risk family:
small-cap stocks in genuine downtrends (dilutive offerings, fraud revelations) do NOT mean-revert
— they continue lower. Risk management for this family must clearly distinguish between
liquidity-driven dips and fundamental collapses.

**ASSUMPTIONS:**
- The 6-family composite does not suffer from in-sample regime optimization beyond what
  nested CV addresses (not independently verifiable without full code/data access)
- $500K volume filter adequately filters out the most illiquid stocks where execution
  costs would destroy the edge (not independently verified for each family)
- The walk-forward periods span multiple market regime types (2008-style crash, COVID crash,
  bull runs) — backtest period not disclosed, so this is unverified

---

## Future Research Needed

1. Access the full paper to extract specific entry/exit rules for each family
2. Independent Python replication of at least Family 2 (gap-and-go ORB) and Family 3
   (mean reversion) using a public data source (Polygon.io, Alpaca)
3. Check if the $300M–$2B market cap definition is consistent across the walk-forward periods
   (survivorship bias risk if cap range is defined at backtest start)
4. Benchmark Family 2 against the `orb-stocks-in-play` strategy — both target ORB on active
   stocks; understanding what Poudel adds above Zarattini/Aziz would be valuable
5. Understand the regime classifier — poorly designed regime classifiers frequently add spurious
   out-of-sample performance loss
6. Look for forward-test data from the author (December 2025 publication → 6 months of live
   data could be available by mid-2026)
