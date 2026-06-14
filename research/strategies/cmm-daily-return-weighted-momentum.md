# Characteristic-Managed Momentum (CMM): ML Weighting of Formation-Period Daily Returns

## Overview

Beckmeyer and Wiedemann (JBF 2025) propose that the standard practice of equally weighting all daily
returns in the 12-month momentum formation window discards valuable heterogeneity. Their
Characteristic-Managed Momentum (CMM) strategy uses machine learning to assign sparse, non-uniform
weights to individual days in the formation window, dramatically concentrating the signal in the most
informative days (primarily earnings announcement reaction days and market-wide jump days). The result
is a long-short cross-sectional momentum portfolio with lower volatility, higher Sharpe, and smaller
maximum drawdown than standard 12-1 month momentum — with costs modeled at the bid-ask spread level.

## Asset Class / Timeframes

- **Asset class:** US equities (cross-sectional, large-cap universe implied by JBF publication)
- **Rebalancing:** Monthly (implied by standard momentum practice; not confirmed from accessible sources)
- **Signal horizon:** 12-month formation window (231 trading days) with 1-month skip (standard 12-1)
- **Out-of-sample period:** Four decades, 1983–2022

## Core Logic

1. **Formation window:** For each stock, collect the daily returns over the prior 12 months (minus
   the most recent month), yielding ~231 daily observations.
2. **ML weighting:** A parametric portfolio policy assigns learned non-uniform weights to each of
   those 231 returns. The weights are sparse: on average just 2 of 231 returns receive 30% of the
   total weight; 30 returns receive >50%.
3. **CMM signal:** The weighted average of the 231 daily returns becomes the cross-sectional momentum
   signal for stock selection.
4. **Portfolio construction:** Standard long-short momentum construction — long top decile, short
   bottom decile by CMM signal — dollar-neutral.
5. **Monthly rebalancing:** Portfolio reassembled each month with updated CMM signals.

## Economic Rationale

**Primary mechanism:** Earnings announcement days contain disproportionate fundamental information
about a company's value trajectory. If a stock reacted strongly (positively or negatively) to its
last few earnings releases, that reaction is a more reliable predictor of future momentum than, say,
the return on an ordinary Tuesday in the formation period. Market-wide jump days similarly carry
more systematic information about which stocks are leading versus lagging.

**Falsifiability (stated conditions for edge disappearance):**
- Edge should weaken as post-earnings announcement drift (PEAD) is arbitraged away by faster
  institutions (has been declining since ~2016 per Christensen et al. arXiv 2601.08962).
- Edge should weaken when markets are more information-efficient (e.g., during periods of heavy
  quantitative arbitrage).
- Edge should weaken if ML training period includes regime shifts that invalidate the learned
  weights out-of-sample (structural break in EA-day informativeness).
- Edge should fail for stocks with irregular or very few earnings announcements in the formation
  window (e.g., small companies, non-US equities where EA dates are less standardized).

## Entry Conditions

For each stock at monthly rebalancing:
1. Compute the CMM signal = weighted sum of daily formation-period returns, weights learned from ML.
2. Rank stocks by CMM signal.
3. Enter long positions in top decile stocks.
4. Enter short positions in bottom decile stocks.

Exact weighting function: not disclosed in open sources. The paper uses parametric portfolio policy
where weights are a linear function of stock characteristics (with EA-day indicators and return
magnitude as key characteristics). Specific functional form: NOT REPORTED in accessible sources.

## Exit Conditions

Monthly rebalancing: positions held for one month, then reconstructed from scratch based on updated
CMM signals. No intra-month stop-losses or regime exits described in accessible sources.

## Risk Management

Standard long-short momentum portfolio construction:
- **Dollar-neutrality:** Equal dollar value long and short (implied by standard practice; not
  explicitly confirmed).
- **No explicit stop-losses described** in any accessible source.
- **No regime filter described** — strategy runs continuously regardless of market regime.
- **No explicit position limits** beyond the decile construction.

ASSESSMENT: Risk management is minimal. The strategy relies on diversification across a large stock
universe (typical of academic momentum studies) rather than active risk controls. Standard momentum
portfolios have catastrophic drawdowns during momentum crash episodes (2009: -80%). CMM's improvement
(-27.6% max DD) is structural (diversification through different-day weighting) not explicit risk
management.

## Indicators Used

- Daily stock returns (12-month window, ~231 observations per stock)
- Earnings announcement dates and corresponding daily returns
- Market-wide jump day indicators (days with large cross-sectional return dispersion or
  index-level jumps)
- Large individual return indicators (days where a stock's return exceeded a threshold)
- The ML training uses these to learn the sparse weighting function

## Transaction Costs & Capacity

- **Bid-ask spreads explicitly modeled.** Net Sharpe after bid-ask: 0.77 (vs gross 1.47).
- **Commission and market impact NOT confirmed modeled** in accessible sources. Given that the
  net Sharpe drops from 1.47 to 0.77 from bid-ask alone — a reduction of ~0.70 — the strategy
  carries very high implied turnover. Additional costs (commissions, price impact from rebalancing
  a large long-short equity portfolio) would further degrade performance.
- **Capacity:** Long-short equity strategy. Standard momentum is known to have significant capacity
  constraints in small-cap stocks (where much of the momentum premium resides). CMM's relationship
  to market cap is NOT REPORTED from accessible sources.
- **Verdict:** The bid-ask-only transaction cost modeling is insufficient for a real-world
  evaluation. Realistic total costs (bid-ask + commissions + market impact) could reduce Sharpe
  well below 0.77, potentially to below 0.5. This is the single biggest red flag.

## Backtest Evidence

Published in Journal of Banking & Finance, Volume 181, 2025 (CLAIMED, peer-reviewed journal, so
methodology is refereed; specific results are CLAIMED, UNVERIFIED until code is independently run).
Full sample: implied to include training period + 1983-2022 OOS. Total sample period start date
NOT CONFIRMED from accessible sources (standard for momentum research: 1963 or 1973 NYSE/AMEX/NASDAQ).

## Forward-Test Evidence

NOT REPORTED. The OOS period (1983-2022) does not include 2023-2026 performance.

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | 1.47 (gross); 0.77 (net after bid-ask) | CLAIMED, UNVERIFIED | Web search summaries of JBF 2025 paper |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | −27.6% | CLAIMED, UNVERIFIED | Web search summaries of JBF 2025 paper |
| CAGR | NOT REPORTED | NOT REPORTED | — |
| Annual Return | 18.5% (CMM) vs 13.2% (standard momentum) | CLAIMED, UNVERIFIED | Web search summaries |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | ~1 month (implied by monthly rebalancing) | ANALYST ANALYSIS | Standard practice |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | ~100% long, ~100% short (dollar-neutral long-short) | ANALYST ANALYSIS | Standard practice |
| Annualized Volatility | 12.6% (CMM) vs 26.9% (standard momentum) | CLAIMED, UNVERIFIED | Web search summaries |

**Note on annual return figures:** Two different search summaries produced inconsistent annual return
figures (18.5% vs 11.5%). The 18.5% figure appears in more specific summary (with the matching
13.2% for standard momentum and consistent Sharpe math: 18.5/12.6 ≈ 1.47). The 11.5% figure
appears to be a misquote. Both figures are CLAIMED, UNVERIFIED from the full paper.

## Community Sentiment

- Lars Kestner (systematic trading professional, author of "Quantitative Trading Strategies")
  mentioned the paper approvingly on X/Twitter, suggesting it attracted attention from the
  systematic trading community.
- JBF peer review implies the methodology passed expert scrutiny.
- **No independent replication studies found** as of 2026-06-14.
- **No critical forum discussion found** (ForexFactory, r/algotrading) — the strategy is an
  academic paper, not a retail-oriented system.

## Why This Might Be Nothing

### 1. Extreme in-sample weight concentration is a red flag for overfitting

The defining feature of CMM — that just 2 of 231 days receive 30% of the total weight — is alarming.
An ML algorithm searching over 231 features to find the best linear predictor will gravitate toward
the most extreme past returns. If earnings announcement days happen to be unusually informative in
the training period, the ML will over-concentrate weight there. When the relationship between
EA-day reactions and subsequent momentum weakens (e.g., post-2016 PEAD decay), the ultra-sparse
weights would hurt more than help. The four-decade OOS claim needs to be interpreted carefully:
if the ML is re-estimated rolling each month using only past data (expanding window), the OOS
results are meaningful. If the weights were estimated on the full 1963-2022 sample and then
"backtested" OOS, this is in-sample fitting with OOS labeling.

### 2. Bid-ask-only cost model is insufficient — likely fatal for a realistic evaluation

The Sharpe ratio drops from 1.47 to 0.77 from bid-ask alone, suggesting the strategy rebalances
very frequently or trades illiquid stocks. Every additional cost layer (exchange commissions
typically 0.03-0.10% per trade, prime broker fees, short borrow costs for the short book,
market impact from moving a large portfolio) would further degrade performance. A gross Sharpe
of 1.47 with bid-ask-only costs of 0.70 Sharpe points implies the gross-to-net ratio is already
~50%, which is very high. Real-world total costs could easily push the net Sharpe to 0.3-0.4
or below — barely better than buy-and-hold.

### 3. Post-earnings announcement drift (PEAD) decay makes the mechanism questionable

The mechanism depends on EA-day reactions being predictive of subsequent momentum. PEAD — the
post-earnings drift phenomenon — has been documented as declining in magnitude since the 1990s
and becoming near-zero after ~2015-2016 as markets became more efficient and algorithms began
trading earnings releases faster (Christensen et al., arXiv 2601.08962). If PEAD is the core
signal, its structural decay means the CMM weights learned in 1963-2000 may not generalize to
2020-2026 at all. The paper's OOS period ends in 2022 — it doesn't include the high-efficiency
post-COVID period.

### 4. Multiple testing over 231 features

The ML algorithm is essentially searching for the best linear combination of 231 daily features
(each day's return) plus characteristics. With 231 features for each stock and a large training
sample, the risk of data snooping is real even with out-of-sample validation, especially if the
OOS split is a one-time split rather than a proper rolling out-of-sample.

### 5. Standard momentum's catastrophic DD (-80%) makes comparison misleading

The paper positions CMM's -27.6% max DD as superior to standard momentum's -80.3%. But this
comparison is cherry-picked from the 2009 momentum crash. A fairer comparison would be against
other momentum improvements (intermediate horizon momentum, residual momentum, industry-adjusted
momentum) that also avoid the 2009 crash, many of which don't require ML weighting. The correct
comparison is whether CMM outperforms simpler alternatives — which the paper reportedly does, but
this claim is based solely on accessible search summaries of the JBF paper.

### 6. What single piece of evidence would change this assessment

An independent rolling-window replication using only past data for ML estimation at each step,
with full cost modeling (bid-ask + commissions + short borrow + market impact), demonstrating
net Sharpe > 0.5 in the 2022-2026 period not included in the original sample — AND showing that
the performance holds for large-cap stocks where institutional execution costs are lower.

## Strengths / Weaknesses

**Strengths:**
- Peer-reviewed in Journal of Banking & Finance (credentialed journal; methodology passed referee scrutiny)
- Published under CC BY 4.0 license — full paper is openly accessible in principle
- Four decades of OOS testing claimed (1983-2022 if properly conducted)
- Dramatically reduces maximum drawdown (-27.6% vs -80.3%) — eliminates the main practical
  objection to standard momentum
- Bid-ask costs explicitly modeled
- Clear economic mechanism (EA-day reactions are more informative) that is independently supported
  by the PEAD literature
- Sparse weighting (30 of 231 days dominate) makes the signal interpretable — not a black box

**Weaknesses:**
- No open code available for independent replication
- Bid-ask-only cost model is insufficient given the implied very high turnover
- PEAD decay post-2016 challenges the primary mechanism going forward
- ML over-concentration risk (2 days get 30% of weight = extreme sparsity)
- OOS methodology (rolling vs. one-time split) NOT CONFIRMED from accessible sources
- No risk management beyond portfolio construction
- No 2022-2026 OOS evidence
- Conflicting return figures in search summaries (18.5% vs 11.5%) — exact number unclear

## Confidence Score

### Per-Dimension Breakdown

| Dimension | Wt | Score | Reasoning |
|---|---|---|---|
| Logical/economic soundness (falsifiable) | 3 | 7 | Clear mechanism: EA days carry more signal. Falsifiable: edge should disappear as PEAD decays and EA processing becomes instantaneous. Conditions named. |
| Transparency & reproducibility | 2 | 5 | JBF paper is CC BY 4.0 and fully readable. Parametric portfolio policy methodology described. No open code available — exact ML weights and implementation not reproducible without effort. |
| Evidence auditability | 2 | 6 | JBF peer-reviewed, open-access. Four-decade OOS claimed. But no open code; full paper fetch blocked; exact OOS methodology not confirmed from accessible sources. Below "8" threshold. |
| Risk management quality | 2 | 3 | Standard long-short construction only. No explicit stops, regime filter, or position limits described. Reliance on diversification across large stock universe. |
| Cost & capacity realism | 2 | 5 | Bid-ask explicitly modeled (net Sharpe 0.77). But commissions, market impact, and short-borrow costs NOT confirmed modeled. Implied turnover is very high (0.70 Sharpe reduction from bid-ask alone). |
| Honest treatment of drawdowns | 1.5 | 7 | Max drawdown explicitly stated (−27.6%) and compared to standard momentum (−80.3%). Honest characterization — the improvement is structural, not manufactured by ignoring drawdowns. |
| Robustness evidence (OOS/walk-forward) | 1.5 | 6 | Four decades of OOS claimed (1983-2022). "Significantly more robust in recent decades." OOS train/test methodology not confirmed from accessible sources. |
| Survived independent scrutiny | 1 | 3 | Passed JBF peer review. Professional (Lars Kestner) attention. No independent replication or critical community debate found. |

**Weighted sum:** (3×7) + (2×5) + (2×6) + (2×3) + (2×5) + (1.5×7) + (1.5×6) + (1×3)
= 21 + 10 + 12 + 6 + 10 + 10.5 + 9 + 3 = **81.5**

**latent_score** = round(81.5 / 150 × 100) = **54**

**Evidence auditability = 6** → cap = 74

**capped confidence = min(54, 74) = 54** (Experimental band: 40–59)

Recorded as: **latent 54 (capped to 54 — evidence auditability 6/10; JBF peer-reviewed but no open
code; OOS methodology not confirmed; bid-ask-only cost model insufficient for high-turnover strategy;
PEAD decay mechanism risk; no post-2022 data)**

## Complexity / Scalability / Automation Feasibility

- **Complexity:** High. Requires: (a) daily return data for large US equity universe; (b) earnings
  announcement dates accurately linked to daily returns; (c) ML parametric portfolio policy
  implementation (optimization at each rebalancing); (d) cross-sectional portfolio construction.
- **Scalability:** Uncertain. Momentum strategies generally have significant capacity constraints in
  small-cap stocks. The ML weight learning requires historical data infrastructure (CRSP or equivalent).
- **Automation:** In principle, the monthly rebalancing schedule is automation-friendly. But the ML
  re-estimation at each step adds computational cost.

## MQL5 / Python / Pine Feasibility

- **MQL5:** Not suitable. This is a cross-sectional equity strategy requiring CRSP-equivalent
  data and a universe of hundreds of stocks simultaneously. MQL5 is single-instrument focused.
- **Python:** Feasible with data infrastructure. Would require:
  - CRSP or Compustat daily returns data
  - Earnings announcement dates (Compustat IBES or similar)
  - scikit-learn or custom optimization for parametric portfolio policy
  - Monthly rebalancing engine
- **Pine Script (TradingView):** Not suitable. Cross-sectional multi-stock ML strategy.

## Similar Strategies

- `fractional-momentum-smoothing` (score 52): Also modifies momentum signal by reweighting returns,
  but uses fractional differencing of the cumulative return path. Different approach to the same
  problem. CMM has stronger evidence (JBF peer-reviewed vs. SFI working papers).
- `factor-timing-momentum-regime-tai` (score 34): Also applies ML/regime to momentum-adjacent signals.
  Much weaker evidence.
- `network-momentum-trend-following` (score 52): Applies network-based lead-lag weighting across
  assets. Related concept (not all signals equal) but cross-asset rather than intra-formation-period.
- `attention-factors-statistical-arbitrage` (score 66): Also uses ML attention mechanisms in equity
  cross-sectional strategies. Higher evidence quality (ICAIF 2025 paper with 24-year OOS).

## Red Flags

- **Extreme weight concentration (2 of 231 days get 30% weight):** Data mining or genuine signal?
- **Bid-ask-only cost model for high-turnover strategy:** Net Sharpe likely overstated.
- **PEAD decay post-2016:** Core mechanism may be weakening precisely as ML optimization peaks.
- **No open code:** Independent replication impossible without significant investment.
- **OOS methodology unconfirmed:** Critical unknown whether weights are re-estimated rolling or
  learned once on historical data.
- **Conflicting return figures in secondary sources:** 18.5% vs 11.5% — inconsistency in summaries.

## Source Links

- SSRN 5702162 — Wiedemann/Beckmeyer (2025): https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5702162 (HTTP 403; abstract not retrieved directly)
- ScienceDirect — JBF Vol 181, DOI 10.1016/j.jbankfin.2025.107565: https://www.sciencedirect.com/science/article/pii/S0378426625001852 (HTTP 403; confirmed published via IDEAS/RePEC)
- IDEAS/RePEC record: https://ideas.repec.org/a/eee/jbfina/v181y2025ics0378426625001852.html (HTTP 403)
- Finance Centre Münster: https://www.wiwi.uni-muenster.de/fcm/en/research/publications/172086841 (HTTP 403)
- Heinerbeckmeyer.com/research (HTTP 403)
- GitHub: heinerbeckmeyer — 2 public repos; no CMM code: https://github.com/heinerbeckmeyer
- Lars Kestner (@LarsKestner) X/Twitter mention: https://x.com/LarsKestner/status/1976256332006392109

All sources retrieved 2026-06-14 via web search summaries (primary pages HTTP 403).

## Analyst Notes

**FACTS (from search summaries of the JBF paper):**
- Authors: Timo Wiedemann, Heiner Beckmeyer (University of Münster, Finance Centre Münster)
- Published: Journal of Banking & Finance, Vol. 181, 2025, DOI 10.1016/j.jbankfin.2025.107565
- License: CC BY 4.0 (open access; full text in principle accessible)
- SSRN: 5702162, first submitted October 7, 2025
- Strategy name: Characteristic-Managed Momentum (CMM)
- Formation window: 231 daily returns (12-1 month standard)
- Weight distribution: 2 of 231 days get 30% of total weight; 30 days get >50% weight
- Net Sharpe (after bid-ask): 0.77
- Gross Sharpe (before bid-ask): 1.47 (computed from stated 18.5% return / 12.6% vol)
- Annualized return: 18.5% (CMM) vs 13.2% (standard momentum) — CONFLICTING with 11.5% elsewhere
- Annualized standard deviation: 12.6% (CMM) vs 26.9% (standard momentum)
- Maximum drawdown: −27.6% (CMM) vs −80.3% (standard momentum, 2009)
- OOS period claimed: "four decades, 1983–2022"
- Most informative days: earnings announcement reactions, market-wide jumps, large individual returns
- CMM "subsumes" standard momentum (implies standard momentum adds no alpha conditional on CMM)
- Does not suffer from momentum crashes

**ANALYSIS:**
- Implied gross Sharpe from return/vol: 18.5/12.6 ≈ 1.47 — CONFIRMS the stated 1.47.
- Cost reduction analysis: net Sharpe 0.77 vs gross 1.47 means bid-ask alone reduces Sharpe by ~0.70.
  For a monthly-rebalanced strategy, this implies extremely high daily turnover or concentration in
  illiquid stocks. A 0.70 Sharpe reduction from bid-ask alone on a monthly-rebalanced portfolio is
  abnormally high (typical long-short equity strategies lose 0.1-0.3 from bid-ask). This strongly
  suggests the CMM weights over-concentrate on high-turnover or illiquid stocks, or the ML implicitly
  favors stocks with larger spreads.
- Calmar ratio (if available): 18.5% / 27.6% ≈ 0.67 — decent but not exceptional for a long-short
  equity strategy; this is ANALYST ANALYSIS, not reported by the source.
- The "2 of 231 days get 30% weight" finding is independently consistent with PEAD literature:
  EA days are 2-4 times per year per stock in the formation window, and PEAD says these days predict
  subsequent drift. But PEAD has been decaying since ~2016.

**ASSUMPTIONS:**
- Monthly rebalancing is ASSUMED (standard for 12-1 momentum; not confirmed from accessible sources).
- Dollar-neutral long-short construction is ASSUMED (standard academic practice; not confirmed).
- Large US equity universe (NYSE/AMEX/NASDAQ standard) is ASSUMED; international applicability unknown.

## Future Research Needed

1. **Access the full CC BY 4.0 paper** (DOI 10.1016/j.jbankfin.2025.107565) to confirm:
   - Exact OOS methodology (rolling window re-estimation vs. one-time in-sample/OOS split)
   - Full cost model (did they include commissions and market impact?)
   - Specific stock universe (market cap range, exchange filters)
   - Sub-period performance (did it work post-2010?)
   - Factor model alpha (Fama-French 5 factor alphas)
2. **Verify OOS period (2022-2026):** The paper ends in 2022. Check whether the strategy maintained
   performance in the 2023-2026 period independently.
3. **Check if Beckmeyer has released CMM code** after publication — his GitHub currently has 2 other
   repos with open code.
4. **Compare to intermediate horizon momentum** (6-month, Grundy/Martin): whether CMM's gains over
   standard 12-1 momentum survive comparison to the simpler alternative of using 6-month lookback
   which already avoids the Jan 2009 momentum crash.
5. **Check whether PEAD decay affects CMM OOS performance** specifically in the 2016-2022 sub-period.
