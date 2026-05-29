# VIX Constant Maturity Futures ML Walk-Forward Strategy

## Overview

A machine learning strategy that predicts next-day returns of VIX constant-maturity futures
(VIX CMFs) using term structure features, then constructs a trading portfolio via Constrained
Mean-Variance Optimization (C-MVO). Published in PLOS One (peer-reviewed, open-access) in
April 2024. Authors are Wang S, Li K, Liu Y, Chen Y, and Tang X.

The central contribution is demonstrating that two specific VIX CMF term structure features —
μt (mean level of the term structure) and Δroll (change in roll yield) — have reliable
predictive power for next-day VIX futures returns over an 11-year walk-forward period.

## Asset Class / Timeframes

- **Asset:** VIX Constant Maturity Futures (CBOE; synthetic interpolated constructs from
  front/back month VIX futures)
- **Timeframe:** Daily (next-day return prediction)
- **Market type:** US volatility derivatives

VIX futures are traded on the CBOE. "Constant maturity" futures are synthetic time series
created by interpolating between adjacent monthly VIX contracts to maintain a fixed time
to maturity (e.g., 30-day, 60-day CMF). They are not directly tradeable instruments but
serve as the study's signal and approximated position vehicle.

## Core Logic

1. **Feature construction:** Derive two key term structure features from the VIX CMF curve:
   - **μt** — the mean level of the VIX CMF curve across maturities (smoothed VIX level,
     capturing overall market fear premium)
   - **Δroll** — the change in roll yield (roll yield = the rate at which futures converge
     toward spot VIX at expiration; positive in backwardation, negative in contango)
   Three incremental feature sets are tested: baseline technical features → baseline +
   μt/Δroll → baseline + μt/Δroll + "statistically derived features."

2. **ML prediction:** Seven machine learning models predict next-day VIX CMF returns using
   the feature sets. Models achieving prediction IR > 0.02 are considered informative.

3. **Portfolio construction:** A Constrained Mean-Variance Optimization (C-MVO) uses the
   ML predictions to form a portfolio across VIX CMF maturities. A long-short benchmark
   strategy (go long/short based on ML sign prediction) is also tested.

4. **Walk-forward execution:** Expanding-window training: model retrained as new data
   arrives; backtesting strictly adheres to walk-forward protocol (no look-ahead).

## Economic Rationale

VIX futures in contango (approximately 85% of calendar days since 2004) produce a
systematic negative roll yield for long holders — i.e., a structural risk premium accrues
to sellers of VIX futures, paid by investors using VIX products as portfolio hedges.

The term structure features capture:
- **Level (μt):** High VIX levels signal elevated realized vol risk; low levels suggest
  stable regimes with persistent roll yield.
- **Slope (Δroll):** Increasing contango signals growing carry opportunity; increasing
  backwardation signals an emerging volatility spike — the most dangerous environment
  for short VIX positions.

The mechanism should fail (or reverse) when: (1) a genuine volatility spike occurs that
causes VIX to massively exceed futures prices (as in Feb 2018 or Mar 2020), overwhelming
any carry income; (2) structural changes reduce the vol risk premium (e.g., CBOE market
structure changes); (3) the strategy becomes crowded and market makers reprice the carry.

This partially satisfies the falsifiability test — the authors would presumably discuss
spike-risk failure modes, but those sections are inaccessible.

## Entry Conditions

NOT REPORTED in accessible search summaries — specific position entry thresholds,
ML signal sign rules, and C-MVO constraint parameters not accessible due to HTTP 403
blockade on all primary sources.

ANALYSIS: Likely involves going long/short VIX CMFs when the ML model predicts
positive/negative next-day returns above a threshold. C-MVO adds portfolio-level sizing.

## Exit Conditions

NOT REPORTED in accessible search summaries. Likely daily rebalancing based on ML
predictions (next-day return horizon implies daily exit/rebalancing).

## Risk Management

NOT REPORTED in accessible search summaries.

ANALYSIS: The C-MVO constraint limits maximum position sizing by incorporating the
predicted return covariance matrix. However, mean-variance optimization is known to
systematically underestimate the left tail of VIX futures returns — VIX spike events
(Feb 2018: VIX from 14 to 37 in one day; Mar 2020: VIX to 82) produce losses that no
MVO constraint can fully bound. No explicit stop-loss or maximum-loss criteria are
documented in accessible summaries.

## Indicators Used

- **μt:** Mean level of VIX constant maturity futures term structure
- **Δroll:** Change in roll yield (contango/backwardation shift indicator)
- Three feature sets: baseline technical + term structure + statistically derived features
- Seven machine learning models (exact algorithms NOT REPORTED)

## Transaction Costs & Capacity

TRANSACTION COSTS: NOT REPORTED in accessible summaries. This is a significant gap.

ANALYSIS: Daily trading in VIX futures incurs:
- Exchange commissions (~$1–2 per contract)
- Bid-ask spread: typically $0.05–0.15 per VIX point × 1,000 (contract multiplier) =
  $50–150 per contract per round trip under normal conditions, wider during spikes
- Roll costs when VIX futures contracts expire (monthly rollover from front to next month)

For a daily rebalancing strategy on VIX CMFs, these costs compound significantly. An IR
of 0.623 on a gross basis could deteriorate substantially net of realistic execution costs.

CAPACITY: VIX futures open interest is orders of magnitude smaller than equity index
futures. This strategy is effectively limited to institutional accounts with modest AUM
(rough estimate: tens of millions of dollars, not hundreds of millions). Retail participation
via VIX ETPs (SVXY, UVXY) is possible but introduces tracking error relative to the CMF
signals studied.

## Backtest Evidence

**Claimed.** Walk-forward expanding-window over 11-year backtest period. No exact date
range in accessible summaries (ANALYSIS: likely 2012–2023 or similar, given VIX futures
data availability and the paper's 2024 publication date). All primary source URLs returned
HTTP 403. Accessible evidence is from search engine summaries only.

## Forward-Test Evidence

NOT REPORTED.

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | NOT REPORTED | NOT REPORTED | All sources 403 |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | All sources 403 |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | All sources 403 |
| Profit Factor | NOT REPORTED | NOT REPORTED | All sources 403 |
| Win Rate | NOT REPORTED | NOT REPORTED | All sources 403 |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | All sources 403 |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | All sources 403 |
| CAGR | NOT REPORTED | NOT REPORTED | All sources 403 |
| Annual Return | NOT REPORTED | NOT REPORTED | All sources 403 |
| Number of Trades | NOT REPORTED | NOT REPORTED | All sources 403 |
| Average Trade Duration | 1 day (daily rebalancing, ANALYSIS) | NOT REPORTED | — |
| Recovery Factor | NOT REPORTED | NOT REPORTED | All sources 403 |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | All sources 403 |
| Exposure Percentage | ~100% (daily holding, ANALYSIS) | NOT REPORTED | — |
| C-MVO Information Ratio | 0.623 | CLAIMED, UNVERIFIED | PLOS One search summary |
| Long-Short Baseline IR | 0.404 | CLAIMED, UNVERIFIED | PLOS One search summary |
| Avg Prediction IR (4 of 7 models) | > 0.02 (avg: 0.037) | CLAIMED, UNVERIFIED | PLOS One search summary |

**Note on IR vs. Sharpe:** Information Ratio = (Strategy Return − Benchmark Return) / Tracking Error.
If the benchmark is zero (cash), IR ≈ Sharpe. However, if the C-MVO strategy uses an
internal VIX futures benchmark, the absolute Sharpe is unknown. ANALYSIS: An IR of 0.623
on a VIX futures strategy is plausible if the benchmark is zero (which would make it
equivalent to Sharpe 0.623 — above average for a single-market strategy, but not extreme).
Without knowing the benchmark, this figure cannot be interpreted precisely.

**No automatic scrutiny triggers apply** — extreme Sharpe/drawdown thresholds cannot be
evaluated since Sharpe and max drawdown are NOT REPORTED.

## Community Sentiment

HarbourFront Quant published a review ("Trading VIX Futures Using Machine Learning
Techniques") on their Substack. The URL is accessible in search results but the content
is HTTP 403 blocked. No independent replication found. No criticism threads identified
on ForexFactory, r/algotrading, or academic commentary forums.

PLOS One peer review implies the methodology was vetted, but PLOS One does not evaluate
novelty or significance — it evaluates whether the methodology is correctly described and
the conclusions follow from the results.

## Why This Might Be Nothing

**1. Volmageddon failure mode — the most likely reason this is dangerous, not profitable.**
VIX futures strategies that harvest roll yield have a severe left tail. On February 5, 2018,
VIX spiked from 14.5 to 37.3 in a single trading session — a 157% move in one day. Short
VIX products including XIV (the $1.7B inverse VIX ETN) were terminated with a 96% loss.
The ML model can learn to *reduce* position during elevated VIX, but it cannot learn to
predict unprecedented spike events from term structure features alone, because such spikes
often begin from low-VIX, high-contango environments that look *identical* to low-risk
carry-harvesting conditions. Similarly, March 2020 (VIX to 82) represents another scenario
where term structure features had no predictive power for the magnitude of the move.

**2. PLOS One peer review is light.** The journal accepts papers on methodological correctness,
not on financial economics significance or novelty. The review process does not require
reviewers to assess whether the predictive IR is economically meaningful after transaction
costs, or whether the strategy survives known stress events. This is not a critique of the
authors — but it means the publication alone does not confer the quality of, say, a Journal
of Finance or Review of Financial Studies paper.

**3. The IR of 0.623 is pre-cost.** Daily VIX futures trading incurs meaningful bid-ask
spread costs and monthly roll costs. A gross IR of 0.623 could be substantially negative
after realistic costs on a high-frequency-for-volatility strategy. The accessible summaries
do not confirm whether costs are modeled.

**4. Seven models, three feature sets = 21 variants.** Reporting the average IR across
four informative models is less cherry-picked than reporting the best single model, but the
best-of-21 IR is certainly higher. Statistical significance of the reported IR across 21
trials is not discussed in accessible summaries.

**5. VIX CMF are synthetic constructs.** The constant-maturity futures do not directly
trade. Replicating the strategy in practice requires trading the front and back VIX futures
contracts and interpolating — adding additional execution complexity and transaction costs
beyond the CMF-level analysis.

**6. Single piece of evidence that would change this assessment:** A Sharpe ratio after
realistic transaction costs, with explicit discussion of Volmageddon and March 2020 behavior,
from a peer-reviewed source. Currently neither is confirmed.

## Strengths / Weaknesses

**Strengths:**
- Peer-reviewed in PLOS One (open access), April 2024
- Strict 11-year walk-forward expanding window — methodologically sound
- Three feature sets tested, not just one — provides some robustness evidence
- Economic rationale for term structure features is well-grounded in volatility risk premium literature
- C-MVO adds genuine portfolio optimization layer beyond simple long/short

**Weaknesses:**
- Critical metrics (Sharpe, max drawdown, CAGR) NOT REPORTED in accessible sources
- Daily VIX futures trading costs unknown
- Seven ML models, three feature sets = 21 variants tested; statistical correction not discussed
- Volmageddon and COVID-crash behavior not documented in accessible summaries
- VIX CMF are synthetic — trading requires VIX futures, adding execution complexity
- Capacity limited to modest institutional AUM
- Only one market tested (VIX futures) — no cross-market validation possible by design

## Confidence Score

| Dimension | Weight | Score (0–10) | Product | Notes |
|-----------|--------|--------------|---------|-------|
| Logical/economic soundness (falsifiable) | 3 | 6 | 18 | Mechanism sound (roll yield premium); failure conditions partially specified; ML opacity reduces score |
| Transparency & reproducibility | 2 | 4 | 8 | Feature names given; ML models not named; no code; rules not fully reproducible |
| Evidence auditability | 2 | 5 | 10 | PLOS One peer-reviewed (light tier); open access but 403 blocked; no code release |
| Risk management quality | 2 | 4 | 8 | C-MVO position sizing; no explicit stop-loss; Volmageddon-class tail risk unaddressed |
| Cost & capacity realism | 2 | 2 | 4 | Daily VIX futures trading; costs NOT REPORTED; capacity issues not discussed |
| Honest treatment of drawdowns / failure | 1.5 | 3 | 4.5 | IR reported but Sharpe/max DD absent; spike-risk failure not documented in accessible sources |
| Robustness (OOS / walk-forward / multi-market) | 1.5 | 7 | 10.5 | 11-year walk-forward is strong; three feature sets; single market by design |
| Survived independent scrutiny | 1 | 4 | 4 | PLOS One peer review + HarbourFront practitioner review (blocked); no independent replication |

**Weighted sum:** 18 + 8 + 10 + 8 + 4 + 4.5 + 10.5 + 4 = 67
**Max possible:** 15 × 10 = 150
**Latent score:** round(67/150 × 100) = **45**
**Evidence auditability = 5 → cap at 74**
**Capped confidence: min(45, 74) = 45**

`latent 45 (capped to 45 pending verification: PLOS One peer review but max DD, Sharpe, and costs NOT REPORTED; Volmageddon tail risk unquantified)`

**Band: 45 — EXPERIMENTAL**

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Medium-high — requires ML model training pipeline, daily VIX CMF feature engineering, C-MVO portfolio solver
- **Scalability:** Low — VIX futures capacity constrains strategy to modest AUM
- **Automation:** Feasible in Python (scikit-learn + cvxpy for C-MVO + CBOE VIX futures data), but VIX futures data access requires specialized provider (Quandl, CBOE DataShop, or a broker API)

## MQL5 / Python / Pine Feasibility

- **Python:** Feasible — scikit-learn, cvxpy, pandas; data from CBOE DataShop or a futures broker API
- **Pine Script:** NOT FEASIBLE — VIX futures constant-maturity data is not available natively on TradingView; Pine does not support VIX CMF construction
- **MQL5:** Feasible in principle but impractical — VIX futures are US exchange products, not Forex/CFDs; MetaTrader's data feed does not carry CBOE VIX futures at the required granularity

## Similar Strategies

- `regime-switching-jump-model-equity` — also a regime/feature-based approach, but targets equity index timing (binary 0/1 allocation) rather than VIX futures directly
- The "VIX 200-day MA crossover" (from QuantifiedStrategies, web search) is a simple rule-based version of the same idea: use VIX level to time equity exposure, producing dramatically lower drawdown without major return sacrifice

## Red Flags

- **Volmageddon tail risk** — unconstrained short VIX strategies can lose 90%+ in a single day; unclear how this is handled
- **Costs unknown** — daily trading on VIX futures; no cost model confirmed in accessible sources
- **21 model variants** — moderate multiple-testing risk
- **VIX CMF are synthetic** — not directly tradeable; adds execution complexity
- **IR metric only** — Sharpe and drawdown absent; IR without benchmark context is ambiguous

## Source Links

| Source | Retrieved | Notes |
|--------|-----------|-------|
| [PLOS One 10.1371/journal.pone.0302289](https://journals.plos.org/plosone/article?id=10.1371%2Fjournal.pone.0302289) | 2026-05-29 | Primary source; HTTP 403 blocked |
| [PMC11029606](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC11029606/) | 2026-05-29 | Open-access full text; HTTP 403 blocked |
| [Semantic Scholar](https://www.semanticscholar.org/paper/VIX-constant-maturity-futures-trading-strategy:-A-Wang-Li/81ad52e22c78317be235b57603efe52f3fd19ec5) | 2026-05-29 | HTTP 403 blocked |
| [HarbourFront Quant review](https://harbourfrontquant.substack.com/p/trading-vix-futures-using-machine) | 2026-05-29 | Practitioner review; HTTP 403 blocked |
| [Quantpedia: Exploiting VIX Term Structure](https://quantpedia.com/strategies/exploiting-term-structure-of-vix-futures) | 2026-05-29 | Related prior work; search summary only |

All primary sources returned HTTP 403. All evidence in this analysis is from search engine
summaries only.

## Analyst Notes

**FACTS (from sources):**
- Paper title: "VIX constant maturity futures trading strategy: A walk-forward machine learning study"
- Authors: Wang S, Li K, Liu Y, Chen Y, Tang X
- Publication: PLOS One 19(4): e0302289, April 19, 2024; PMC11029606
- 7 ML models; 3 feature sets; 11-year walk-forward expanding window
- Key features: μt and Δroll from VIX CMF term structure
- C-MVO IR: 0.623 (CLAIMED, UNVERIFIED from search snippet)
- Long-short baseline IR: 0.404 (CLAIMED, UNVERIFIED)
- 4 of 7 models achieve prediction IR > 0.02; avg prediction IR = 0.037 (CLAIMED, UNVERIFIED)

**ANALYSIS (my reasoning):**
- IR of 0.623 is approximately equivalent to Sharpe 0.623 if the benchmark is zero — above-average for a single-market strategy, but not extreme
- Daily VIX futures trading implies significant turnover costs that would compress net IR materially
- The 11-year walk-forward period (likely 2012–2023) covers Feb 2018 Volmageddon and Mar 2020 COVID spike — if the strategy survived these in the backtest, that is meaningful, but we cannot confirm this without primary source access
- The C-MVO framework is a genuine improvement over naive long/short sign prediction (IR 0.623 vs. 0.404)

**ASSUMPTIONS (flagged):**
- ASSUMED: Backtest covers approximately 2012–2023 based on paper publication date and 11-year window
- ASSUMED: "Constant maturity futures" trading is approximated by rolling front+back VIX futures contracts
- ASSUMED: IR ≈ Sharpe if benchmark is zero/cash; the actual benchmark is not confirmed

## Future Research Needed

1. Access the full PLOS One paper to obtain: exact ML model list, exact feature formulas,
   Sharpe ratio, max drawdown, transaction cost assumptions, and stress test behavior
   (Feb 2018, Mar 2020)
2. Check whether the strategy held short positions during Feb 2018 and Mar 2020 and what
   the P&L was during those events
3. Determine if HarbourFront Quant's practitioner review confirms or challenges the IR claims
4. Compare to the simpler VIX 200-day MA crossover approach (which achieved max DD reduction
   from -55% to -22% with comparable returns at 1× leverage) as a lower-complexity benchmark
5. Assess whether implementation via SVXY/UVXY ETPs (rather than VIX futures directly)
   introduces unacceptable tracking error
