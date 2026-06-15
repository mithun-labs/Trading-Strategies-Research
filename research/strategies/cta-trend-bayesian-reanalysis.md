# CTA Trend Bayesian Reanalysis — Re-evaluating Short- and Long-Term Trend Factors

## Overview

**METHODOLOGY/ANALYSIS PAPER — no new tradeable strategy proposed.** This paper
dynamically decomposes returns of the SG Trend Index (a CTA benchmark) into three factors —
short-term trend, long-term trend, and market beta — using a Bayesian graphical model
(state-space / Kalman filter framework), then evaluates how different horizon blends shape
risk-adjusted performance. The practical implication is prescriptive for trend-following
strategy design: a multi-horizon blend is superior to a single-horizon trend system, with
short-term signals specifically improving drawdown protection.

Primary source: arXiv 2507.15876 / SSRN 5365038, July 2025.
Authors: Eric Benhamou, Jean-Jacques Ohana, Alban Etienne, Béatrice Guez, Ethan Setrouk,
Thomas Jacquot. Affiliation: Paris-based quant practitioners (Place du Maréchal de Tassigny,
35 boulevard d'Inkermann Neuilly sur Seine).

## Asset Class / Timeframes

- **Benchmark studied:** SG Trend Index (broad diversified CTA trend benchmark, futures across
  equities, bonds, FX, commodities)
- **Factor replication horizon:** Multi-asset futures; daily to monthly signal timescales
  (short-term vs. long-term trend — exact lookbacks NOT REPORTED in accessible sources)
- **Empirical window:** Five-year live period June 2020 – June 2025

## Core Logic

The paper's Bayesian graphical model is structured to:

1. Represent CTA returns as a linear combination of three latent state variables:
   - **Short-term trend factor** (faster signals, shorter lookback — captures early reversals)
   - **Long-term trend factor** (slower signals, longer lookback — captures sustained trends)
   - **Market beta factor** (directional equity/risk exposure)
2. The Bayesian/Kalman state-space framework dynamically estimates the time-varying loadings
   of actual CTA returns onto these three factors.
3. From the estimated loadings, the authors construct linear factor mimickers (replication
   portfolios) and assess how different horizon blends affect replication quality and
   risk-adjusted performance.
4. Key reported replication result: linear factor mimickers achieve correlation **~0.80 to
   the SG Trend index** while retaining most downside protection (CLAIMED, UNVERIFIED from
   secondary search summaries — primary PDF inaccessible).

**Practical prescription (ANALYST ANALYSIS derived from stated conclusions):**
- Multi-horizon blending outperforms single-horizon trend strategies.
- Short-term signals reduce drawdowns by capturing early trend reversals.
- Long-term signals capture the core trend premium.
- A CTA designer should blend both horizons rather than optimizing for one.

## Economic Rationale

The existence of both short- and long-term trend premia has been documented (e.g., Moskowitz
et al. 2012 for TSMOM; Hurst et al. 2017 for century-scale trend). The question of *how much*
to allocate to each horizon is not well-resolved. This paper provides a data-driven dynamic
decomposition rather than a static allocation rule.

**Falsifiability:** The multi-horizon advantage should disappear when:
- All CTA managers converge to the same horizon blend (crowding) — explicitly NOT modeled
- Short-term trend signals become correlated with long-term signals (regime: sustained,
  smooth trends where all horizons agree, so the marginal value of adding a second horizon
  is low)
- Transaction costs at the short-term horizon exceed the short-term trend premium

The paper does not formally state these failure conditions, but the mechanism is specific
enough to allow them to be inferred. **This is a modest point in its favor** relative to
papers with no stated failure conditions.

## Entry Conditions

NOT APPLICABLE as a direct entry signal. The paper is an analytical decomposition.
The entry signal for a practitioner implementing the insight would be: multi-horizon trend
signals across liquid futures, blended by time-varying Bayesian weights rather than static
allocation. Specific signal definitions (lookback periods, position sizing) are NOT REPORTED
in accessible sources.

## Exit Conditions

NOT APPLICABLE (same reason as above).

## Risk Management

NOT REPORTED explicitly. The Bayesian decomposition framework models risk through factor
loadings and their time variation, but specific position limits, stop-losses, or sizing rules
are NOT REPORTED.

## Indicators Used

- Short-term trend signal (lookback NOT REPORTED)
- Long-term trend signal (lookback NOT REPORTED)
- Market beta exposure (inferred from systematic replication)
- Bayesian state-space model (Kalman filter) for dynamic loading estimation

## Transaction Costs & Capacity

- The SG Trend Index itself embeds transaction costs (the index represents live CTA returns
  net of fees); replication of the index would inherit similar cost exposure.
- The paper studies CTA *replication* — not a gross-return backtest — so costs are implicitly
  included through the index. However, the replication portfolio's own turnover and cost
  structure are NOT REPORTED.
- Capacity: the SG Trend Index represents large CTA allocations; replication in that
  framework would target similar large-capacity instruments (liquid futures).

## Backtest Evidence

- Five-year live evaluation window: June 2020 – June 2025 (CLAIMED, UNVERIFIED)
- Correlation of replication to SG Trend index: ~0.80 (CLAIMED, UNVERIFIED — from search
  result summaries; primary paper inaccessible)
- The model is trained on a prior period (implied; details NOT REPORTED)
- The 5-year live window includes 2022 (exceptional trend-following year), 2023 (partial
  reversal), and 2024–2025 (mixed). This is a relatively favorable period for CTA strategies
  overall.

## Forward-Test Evidence

NOT REPORTED separately from the live evaluation window.

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | NOT REPORTED | NOT REPORTED | — |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | — |
| CAGR | NOT REPORTED | NOT REPORTED | — |
| Annual Return | NOT REPORTED | NOT REPORTED | — |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | NOT REPORTED | NOT REPORTED | — |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | NOT REPORTED | NOT REPORTED | — |
| Replication correlation to SG Trend | ~0.80 | CLAIMED, UNVERIFIED | Secondary search summary of arXiv 2507.15876 |

**Note on "26% annualized return / Sharpe 1.2" seen in one search result summary:** This
figure appeared in an AI-generated search snippet claiming the model achieves "26% annualized
return and Sharpe ratio of 1.2 outperforming equal-weighted and S&P 500 benchmarks" from
2018–2024. This is flagged as ANALYST-assessed likely fabrication by the search summarizer:
(a) CTA replication papers compare to CTA benchmarks, not S&P 500; (b) 26% annualized return
for a CTA replication strategy would trigger automatic scrutiny (CAGR > 50% threshold not
met, but 26% for a CTA-correlated strategy is extraordinary); (c) no other source confirms
these figures. This figure is NOT entered in the table above.

## Community Sentiment

- No community discussion, citations, or independent replications found in accessible sources
  (paper is less than 12 months old on arXiv as of June 2026).
- Related work: "Revisiting the Structure of Trend Premia: When Diversification Hides
  Redundancy" (arXiv 2510.23150, October 2025) appears to address overlapping questions
  about CTA trend structure — not confirmed to cite Benhamou et al. directly (primary page
  HTTP 403).
- Benhamou/Ohana group has published multiple quantitative finance papers; their previous
  work includes Kalman-filter-based replication (hal-02012471) and private equity replication
  (SSRN 5199100). This is a consistent research agenda.

## Why This Might Be Nothing

1. **Methodology paper without a deployable strategy:** The core finding — "blend short and
   long-term trend signals for better CTA performance" — is already conventional wisdom in
   the trend-following community (most major CTAs already blend horizons). The Bayesian
   quantification adds precision but not a novel actionable insight.

2. **Correlation 0.80 to SG Trend is not novel:** Linear CTA replication models have
   achieved correlations of 0.80–0.90 to CTA indices since Fung & Hsieh (2004) using
   simple lookback straddle factors. The Bayesian framework's advantage over a simpler OLS
   regression is NOT quantified in accessible sources.

3. **Cherry-picked live window (ANALYST ANALYSIS):** June 2020 to June 2025 includes
   2022 (one of the best CTA years in history, ~40% for SG Trend). A live window that
   happens to include the best CTA year since 2008 will flatter any model that correlates
   with the SG Trend. Performance in 2023–2025 (mixed for trend followers) is partially
   offset.

4. **arXiv preprint only:** Not peer-reviewed. The Kalman filter / Bayesian state-space
   methodology is mature, but the specific factor definitions, lookback parameters, and model
   selection choices could be data-mined.

5. **No open code confirmed:** The Bayesian decomposition cannot be independently replicated
   without knowing the exact model specification.

6. **What would change my mind:** Peer-reviewed publication confirming the decomposition
   adds value beyond a static multi-factor OLS regression; demonstrated Sharpe improvement
   from adopting the recommended horizon blend vs. a naive equal-weight blend on a confirmed
   OOS period excluding 2022.

## Strengths / Weaknesses

**Strengths:**
- Addresses a genuine and practically important question (horizon selection in trend following).
- Dynamic Bayesian/Kalman approach is theoretically superior to static factor regressions.
- 5-year live window (June 2020 – June 2025) is claimed, providing some time-based separation.
- Practitioner group with consistent CTA replication research agenda.
- Two independent dissemination channels (arXiv + SSRN) — slightly more credible than
  arXiv-only.

**Weaknesses:**
- No new tradeable strategy — findings inform existing strategy design rather than creating
  a new one.
- Core finding ("blend short and long-term trend") is conventional wisdom, not novel.
- Performance metrics (Sharpe, drawdown, CAGR) NOT REPORTED for the replication strategy
  itself.
- No peer review; no open code; primary PDF inaccessible (HTTP 403).
- Live window appears to include 2022, a uniquely favorable year for CTAs.
- Lookback parameters for "short-term" and "long-term" trend factors NOT REPORTED.

## Confidence Score

Per-dimension scoring:

| Dimension | Raw (0–10) | Weight | Weighted |
|-----------|-----------|--------|----------|
| Logical/economic soundness (falsifiable) | 6 | 3 | 18 |
| Transparency & reproducibility | 4 | 2 | 8 |
| Evidence auditability | 4 | 2 | 8 |
| Risk management quality | 3 | 2 | 6 |
| Cost & capacity realism | 4 | 2 | 8 |
| Honest treatment of drawdowns/failure | 4 | 1.5 | 6 |
| Robustness evidence (OOS/walk-forward) | 3 | 1.5 | 4.5 |
| Survived independent scrutiny | 2 | 1 | 2 |

**Notes on dimension scores:**
- *Logical soundness (6):* The horizon-blending question is well-specified with implied
  failure conditions; mechanism is real; but finding is not novel.
- *Transparency (4):* Bayesian state-space framework described; SG Trend index is public;
  but lookback parameters and code not confirmed open.
- *Evidence auditability (4):* arXiv preprint; 5-year live window claimed; primary PDF
  inaccessible; SG Trend is independently auditable but the replication weights are not.
- *Risk management (3):* Not applicable as a strategy; the decomposition framework models
  factor risk; no position sizing or stop rules described.
- *Cost realism (4):* SG Trend index is net-of-fee by definition, which partially embeds
  costs; replication portfolio's own costs NOT REPORTED.
- *Drawdowns/failure (4):* Paper explicitly examines downside protection contribution of
  short-term signals; failure conditions not formally stated.
- *Robustness (3):* One live window; no walk-forward within the live period; horizon
  parameters likely in-sample selected.
- *Independent scrutiny (2):* arXiv preprint <12 months old; no citations found; related
  but not confirming prior work by same group.

**Weighted sum:** 18 + 8 + 8 + 6 + 8 + 6 + 4.5 + 2 = 60.5 ≈ 61
**Max possible:** 150
**Latent score:** round(60.5/150 × 100) = **40**

**Verification cap:** Evidence auditability = 4 (≤ 4) → cap at 59.
**Capped confidence score:** min(40, 59) = **40 (Experimental)**

Recorded as: `latent 40 (capped to 40 — evidence auditability 4/10: arXiv preprint,
primary PDF inaccessible, no peer review, no open code; key parameters NOT REPORTED;
METHODOLOGY PAPER: no new tradeable strategy; horizon-blending finding is conventional
CTA wisdom; performance metrics of replication strategy NOT REPORTED)`

**NOT an implementation candidate:** latent 40 < 60.

## Complexity / Scalability / Automation Feasibility

- **Complexity:** High — Kalman filter / Bayesian state-space estimation requires regular
  refitting; dynamic factor loadings require a data pipeline.
- **Scalability:** The approach targets CTA-scale (large diversified futures portfolios);
  not designed for retail-scale.
- **Automation feasibility:** In principle yes (Kalman filter is a standard algorithm);
  but the specific model specification is NOT REPORTED, preventing direct implementation.

## MQL5 / Python / Pine Feasibility

- **Python:** Conceptually implementable using `pykalman`, `statsmodels.tsa.statespace`,
  or a custom Bayesian state-space library. The idea (decomposing a trend-following
  return series into short-term and long-term components) can be done without the specific
  model; but the paper's exact model is unknown.
- **MQL5:** Not practical for Kalman/Bayesian state-space models.
- **Pine Script:** Not feasible.

## Similar Strategies

- `regime-optimal-trend-zakamulin` (score 54) — also addresses regime-conditional trend
  sizing; Zakamulin uses 2–4 regime Sharpe estimation for position scaling; both papers
  ask "how to adapt trend following to current market state."
- `multi-asset-regime-taa-shu` (score 55) — regime-adaptive TAA with state-space approach
  (SJM); similar Bayesian/Kalman methodology applied to asset allocation rather than
  CTA replication.
- `industry-trend-century` (score 64) — long-horizon trend-following study; conceptually
  the long-term component of what Benhamou et al. analyze.
- `fractional-momentum-smoothing` (score 52) — another approach to blending momentum and
  reversal by tuning the integration order; addresses similar question of horizon selection.

## Red Flags

- **No new strategy:** The paper analyzes existing CTA returns rather than proposing a new
  strategy. Score reflects analytical quality, not a tradeable edge.
- **Primary PDF inaccessible (HTTP 403):** All key numerical findings (Sharpe, CAGR,
  drawdown, lookback parameters) remain NOT REPORTED.
- **Live window selection:** June 2020 – June 2025 includes 2022, potentially the best
  CTA year since the GFC. Window selection may flatter the model.
- **Suspected AI confabulation in search summary:** "26% annualized return / Sharpe 1.2
  vs S&P 500" appeared in one search summary and is assessed as likely fabricated by the
  search AI model. Not included in performance table.
- **No community engagement:** No citations or discussion found after ~11 months on arXiv.

## Source Links

- arXiv 2507.15876 (primary): https://arxiv.org/abs/2507.15876 — retrieved 2026-06-15
  (abstract page returned HTTP 403; abstract reconstructed from web search snippets)
- SSRN 5365038: https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5365038 — HTTP 403
- ResearchGate: https://www.researchgate.net/publication/393922810 — HTTP 403
- arXiv HTML v1: https://arxiv.org/html/2507.15876v1 — HTTP 403
- Related paper (arXiv 2510.23150): https://arxiv.org/abs/2510.23150 — HTTP 403

## Analyst Notes

**FACTS (from accessible web search snippets):**
- Paper title: "Re-evaluating Short- and Long-Term Trend Factors in CTA Replication:
  A Bayesian Graphical Approach"
- Authors: Eric Benhamou, Jean-Jacques Ohana, Alban Etienne, Béatrice Guez, Ethan Setrouk,
  Thomas Jacquot (Paris-based practitioners)
- Submitted: July 2025 to arXiv (2507.15876) and SSRN (5365038)
- Methodology: Bayesian graphical model / Kalman state-space decomposing CTA returns into
  short-term trend, long-term trend, and market beta factors
- Benchmark: SG Trend Index
- Live evaluation window: June 2020 – June 2025 (5 years)
- Replication correlation to SG Trend: ~0.80 (from search snippet, UNVERIFIED against primary)
- Key claim: multi-horizon blending outperforms single-horizon; short-term signals reduce
  drawdowns

**ANALYSIS:**
- This paper answers a legitimate question (how much short-term vs. long-term trend?)
  with a more sophisticated tool than static regression, but the answer ("blend both horizons")
  is already the industry norm. The value-add is the Bayesian quantification of dynamic loadings.
- The correlation of 0.80 to SG Trend is not impressive for a dedicated CTA replication
  paper — Fung & Hsieh (2004) primitive lookback straddles already reached this level. The
  Bayesian advantage over OLS is not quantified in accessible sources.
- Benhamou has published a prior Kalman filter paper (hal-02012471) suggesting this is a
  consistent research track. The methodology is credible even if the current paper doesn't
  advance the edge.

**ASSUMPTIONS:**
- The "June 2020 – June 2025" live window is taken as stated; training period assumed to
  precede June 2020.
- "Short-term" and "long-term" factor definitions assumed to map to conventional
  trend-following lookbacks (e.g., ≤1 month vs. ≥3 months); exact values NOT CONFIRMED.

## Future Research Needed

1. Access full PDF to extract exact lookback parameter definitions, model specification,
   and all numerical results.
2. Compare the Bayesian decomposition's replication correlation (0.80) to a static
   2-factor OLS regression (short TSMOM + long TSMOM) to assess whether the Bayesian
   complexity is necessary.
3. Check if the paper is ever peer-reviewed and published in a finance journal.
4. Assess implications for the trend strategies in this database: does adding a short-term
   TSMOM filter to long-term trend strategies (e.g., `industry-trend-century`) improve
   drawdown characteristics in a simple implementation?
