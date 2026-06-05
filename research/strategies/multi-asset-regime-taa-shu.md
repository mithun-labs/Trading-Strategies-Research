# Multi-Asset TAA via Asset-Specific Regime Forecasts (Shu/Yu/Mulvey)

## Overview

A hybrid, two-stage regime identification-forecasting framework for dynamic multi-asset
portfolio construction. Stage 1 uses the Statistical Jump Model (SJM) to classify each
asset's historical periods into bull/bear regimes. Stage 2 trains a supervised
gradient-boosted decision tree (GBDT) classifier per asset to predict its upcoming regime
using asset-specific return features and cross-asset macro features. Regime-conditional
return and risk forecasts are fed into Markowitz mean-variance optimization (MVO) to
determine optimal portfolio weights. Monthly rebalancing (inferred). Published in
*Annals of Operations Research* (peer-reviewed, 2025).

## Asset Class / Timeframes

- **Primary market:** Multi-Asset — 12 risky assets spanning global equity, bonds, real
  estate (REIT), and commodity indices
- **Geography:** Global (specific country/index tickers NOT REPORTED in accessible sources)
- **Timeframe:** Monthly rebalancing (inferred from publication and related work)
- **Evaluation period:** 1991–2023 (32-year historical sample)

## Core Logic

**Stage 1 — Regime identification (unsupervised):**
The SJM is fit to each asset's return time series independently. For each asset, it
classifies each historical month as bull or bear using a jump-penalized clustering
approach. The jump penalty forces consecutive regime persistence — a key improvement
over HMM (which allows rapid regime switching). Regimes are identified per-asset, not
for the portfolio as a whole.

**Stage 2 — Regime forecasting (supervised):**
A gradient-boosted decision tree (GBDT) classifier is trained for each asset to
predict its regime for the next period. Features include: (a) asset-specific return
features (momentum, volatility), and (b) cross-asset macro features (shared across
assets). The specific feature set is NOT REPORTED in accessible sources (paywalled
Springer publication).

**Stage 3 — Allocation (optimization):**
Asset-specific return and risk forecasts are computed conditional on each asset's
predicted regime (e.g., bull-regime expected return vs. bear-regime expected return
and covariance). These regime-conditional statistics replace the unconditional
estimates in Markowitz MVO. Three portfolio models are tested: minimum-variance,
mean-variance, and naive (equal-weight) diversification. The regime-aware versions
of all three are shown to outperform their static equivalents.

**Key innovation:** Per-asset (not per-portfolio) regime assignment. Traditional
regime-switching approaches assign a single bull/bear label to the entire portfolio
(e.g., "US market is in a bear regime"). This paper assigns independent regime
forecasts to each of the 12 assets, allowing the optimizer to, for example,
overweight US equity (bullish) while simultaneously reducing commodity exposure
(bearish) within the same period.

## Economic Rationale

**Why the edge should exist:** Financial assets exhibit regime-dependent risk/return
dynamics that differ significantly across bull and bear states. By identifying these
regimes per-asset rather than globally, the optimizer can exploit the fact that
different assets are in different regimes simultaneously — generating better
diversification and more accurate forward-looking allocation weights.

**When the edge should disappear:**
- When cross-asset correlations spike (crisis events: 2008, 2020) — all assets enter
  bear simultaneously, eliminating the per-asset differentiation benefit and turning
  the framework into an undiversified drawdown amplifier
- When the GBDT overfit the training regime labels — out-of-sample regime forecast
  accuracy collapses to chance, and the regime-conditional forecasts become noise added
  to MVO estimation error
- When the SJM regime labels are not stable across rolling estimation windows — the
  training target for the GBDT is itself noisy, degrading the classifier
- When markets transition rapidly (short-lived regimes, e.g., COVID 2020 V-shaped
  recovery lasting weeks) — the SJM's persistence assumption breaks down
- When all 12 assets are highly correlated (globalization over the 2010s has increased
  cross-asset correlation) — the per-asset differentiation provides diminishing returns

## Entry Conditions

Per the MVO framework:
1. Compute SJM regime identification on each asset's historical return series up to
   the rebalancing date (in-sample regime fitting)
2. Generate GBDT regime probability forecast for each asset for the next period
3. Compute regime-conditional expected returns and covariance matrix for each asset
4. Solve the MVO problem with regime-conditional inputs → optimal weights
5. Rebalance portfolio to those weights (estimated as monthly)

## Exit Conditions

Portfolio rebalanced monthly. No intramonth stop-losses described. The regime-conditional
MVO weights implicitly shift allocation away from bear-regime assets on each rebalancing.

## Risk Management

- MVO covariance matrix provides implicit portfolio volatility control
- Regime-conditional inputs reduce allocation to bear-regime assets relative to
  unconditional MVO
- No explicit per-asset stop-losses, max-loss limits, or leverage caps described in
  accessible sources
- No explicit leverage stated (long-only assumed, consistent with related work)

## Indicators Used

- **SJM regime labels:** Derived from each asset's own return time series (bull/bear)
- **GBDT features:** Asset-specific return features + cross-asset macro features
  (specific variables NOT REPORTED)
- **MVO inputs:** Regime-conditional expected returns + regime-conditional covariance

## Transaction Costs & Capacity

- **Costs modeled?** NOT CONFIRMED — paywalled. The related JAM 2024 paper ("Downside
  Risk Reduction using Regime-Switching Signals") explicitly models transaction costs
  and trading delays for similar SJM-based strategies. Whether the AOR 2025 paper
  replicates this rigor is unknown without the full text.
- **Monthly rebalancing:** At monthly frequency with 12 diversified assets, turnover
  is moderate. Typical MVO rebalancing generates 10–30% annual turnover at monthly
  frequency. For institutional-grade products (ETFs, index funds), transaction costs
  would be 5–20bps round-trip per position — unlikely to be the primary performance
  driver given multi-asset diversification.
- **Capacity:** Multi-asset indices have large capacity; this is not a capacity-
  constrained strategy.
- **Net-of-cost edge:** If the ~1–2% annualized return improvement holds after
  moderate costs, the net benefit is likely positive — but unconfirmed.

## Backtest Evidence

Peer-reviewed (Annals of Operations Research, 2025). 32-year evaluation period
(1991–2023) across 12 risky assets. Outperforms HMM-based strategies and buy-and-hold
across min-var, mean-var, and naive-diversified portfolio variants. Specific metrics
paywalled.

## Forward-Test Evidence

NOT REPORTED. Evaluation ends 2023. No live or forward-test evidence in accessible sources.

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | NOT REPORTED | NOT REPORTED | Paywalled (AOR 2025) |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | Paywalled (AOR 2025) |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | Paywalled (AOR 2025) |
| Profit Factor | NOT REPORTED | NOT REPORTED | NOT REPORTED |
| Win Rate | NOT REPORTED | NOT REPORTED | NOT REPORTED |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | NOT REPORTED |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | Paywalled (AOR 2025) |
| CAGR | NOT REPORTED | NOT REPORTED | Paywalled (AOR 2025) |
| Annual Return | ~1–2% improvement vs. static MVO | CLAIMED, UNVERIFIED | Web search summary (not sourced from full paper) |
| Number of Trades | NOT REPORTED | NOT REPORTED | NOT REPORTED |
| Average Trade Duration | Monthly (inferred) | NOT REPORTED | Inferred from context |
| Recovery Factor | NOT REPORTED | NOT REPORTED | NOT REPORTED |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | NOT REPORTED |
| Exposure Percentage | ~100% (long-only assumed) | NOT REPORTED | NOT REPORTED |

**Note on "~1–2% improvement":** This was extracted from a web search summary described
as consistent outperformance; the specific source document was not directly retrieved.
Do NOT treat as a precise figure — it is an approximation from secondary descriptions.

## Community Sentiment

No independent community replication or critical forum discussion found (searches across
Quantpedia, r/algotrading, Nuclear Phynance, MQL5 Community returned no direct hits).
The work is cited in subsequent Mulvey group papers (arXiv 2410.14841, SSRN 4960484)
as foundational. The peer-review process in AOR implies academic vetting, but no public
independent scrutiny found. No criticism noted — but absence of criticism may simply
reflect limited public awareness of the paper.

## Why This Might Be Nothing

**1. Layered in-sample estimation bias (most likely concern):**
The GBDT is trained on SJM regime labels derived from the SAME asset return history.
If the SJM is fit on the full sample first and then regime labels are used to train
the GBDT in a rolling manner, the procedure is sound. But if the SJM labels overlap
with the GBDT training window (or if regime-conditional return statistics are computed
from the same window), then the input forecasts are contaminated by in-sample
information. The paper likely avoids this through careful expanding-window CV — but
we cannot confirm without reading the full paywalled text.

**2. GBDT regime forecasting accuracy unknown:**
A GBDT trained on relatively few regime transitions (a 32-year monthly dataset generates
~384 observations; bull/bear regime transitions might occur 10–20 times) is highly prone
to overfitting. The number of features (asset-specific + cross-asset macro) may exceed
the effective number of training events. The out-of-sample regime forecasting accuracy
— the critical assumption for the entire framework to work — is not reported in
accessible sources.

**3. MVO estimation error compounds the regime forecast error:**
Markowitz MVO is notoriously sensitive to input estimation errors. If the
regime-conditional forecasts are noisy or biased, MVO amplifies those errors into
extreme weights. The paper tests three portfolio models, but whether shrinkage or
regularization is applied to the MVO inputs is NOT REPORTED.

**4. All specific performance metrics are paywalled:**
We cannot independently verify any Sharpe, CAGR, or drawdown figure. The
"~1–2% annualized return improvement" is from secondary description only.

**5. Regime persistence assumption:**
The SJM imposes a jump penalty to force persistent regimes. If markets have become
less regime-persistent since the 2010s (due to faster information diffusion, algorithmic
trading, and central bank intervention), the core SJM assumption breaks down. The
evaluation ends 2023 — no 2024–2026 out-of-sample evidence exists.

**6. Survivorship in asset universe:**
The 12 specific assets and how they were selected are NOT REPORTED. If assets were
selected post-hoc knowing their performance characteristics, the sample is
survivorship-biased.

**The single piece of evidence that would change my mind:** An independent
replication using the full arXiv preprint methodology, showing walk-forward OOS
Sharpe ≥ 0.7 on a post-2023 period, with transaction costs explicitly modeled.

## Strengths / Weaknesses

**Strengths:**
- Per-asset regime forecasting is a genuine methodological advance over single-regime
  (portfolio-level) approaches
- Peer-reviewed publication in Annals of Operations Research (rigorous venue)
- Part of a coherent research program (related JAM 2024 + JPM 2025 papers) with
  consistent empirical findings across multiple studies
- SJM component is open-source (jumpmodels Python package)
- 32-year evaluation period covers multiple market cycles including 1991 recession,
  dot-com bust, GFC 2008, COVID 2020
- Multi-asset diversification (equity + bonds + REIT + commodity) reduces single-market
  risk relative to single-asset strategies

**Weaknesses:**
- Full performance metrics are paywalled — no specific Sharpe, CAGR, or drawdown
  retrievable without institutional library access
- GBDT forecasting pipeline code is not open-source
- GBDT trained on limited regime transition events (small effective sample)
- Costs not confirmed as explicitly modeled
- No independent replication found
- Evaluation ends 2023 — no evidence from post-publication period

## Confidence Score

| Dimension | Weight | Score | Weighted |
|-----------|--------|-------|----------|
| Logical / economic soundness (falsifiable) | 3 | 7 | 21.0 |
| Transparency & reproducibility | 2 | 5 | 10.0 |
| Evidence auditability | 2 | 7 | 14.0 |
| Risk management quality | 2 | 5 | 10.0 |
| Cost & capacity realism | 2 | 4 | 8.0 |
| Honest treatment of drawdowns / failure | 1.5 | 5 | 7.5 |
| Robustness evidence (OOS / walk-forward / multi-market) | 1.5 | 5 | 7.5 |
| Survived independent scrutiny | 1 | 5 | 5.0 |
| **Total** | **15** | | **83.0** |

`latent_score = round(83 / 150 × 100) = 55`

**Verification cap:** Evidence auditability = 7 → cap at 74 (range 5–7)
`confidence = min(55, 74) = 55`

**latent 55 (capped to 55 pending full metric verification: specific Sharpe/CAGR/drawdown
paywalled; GBDT overfitting risk unresolved)**

**Band:** Experimental (40–59)

**Note:** Evidence auditability scored 7 (not 8) because the peer-reviewed paper exists
but the full pipeline code is not open, and specific results tables cannot be accessed
without institutional library access. If the full AOR paper is accessed and confirms
cost-adjusted OOS performance with walk-forward, and the GBDT component code is released,
the evidence auditability could rise to 8+ and the cap would lift.

## Complexity / Scalability / Automation Feasibility

**Complexity:** HIGH. Two-stage ML pipeline (SJM + GBDT per asset) + rolling MVO.
Requires: time-series regime identification, ML classification, covariance estimation,
MVO optimization. More complex than the single-regime SJM approach (JAM 2024).

**Scalability:** Good for institutional use. Monthly rebalancing with 12 assets is
operationally manageable. The jumpmodels library handles the SJM component.

**Automation:** Feasible with Python (jumpmodels + XGBoost/LightGBM + scipy/cvxpy).
However, no reference implementation for the full GBDT+MVO pipeline exists.

## MQL5 / Python / Pine Feasibility

- **Python:** Feasible. jumpmodels handles SJM; scikit-learn/XGBoost handles GBDT;
  scipy.optimize or cvxpy handles MVO. The main challenge is the proprietary feature
  set for the GBDT.
- **MQL5:** NOT FEASIBLE. MQL5 lacks the ML and optimization libraries required.
- **Pine Script:** NOT FEASIBLE. Monthly multi-asset portfolio optimization is not
  implementable in Pine.

## Similar Strategies

- `dynamic-factor-allocation-regime-shu-mulvey` (same research group, JPM 2025; factor
  indices + SJM + Black-Litterman; score 61 Worth Researching) — same SJM regime-
  identification technology but applied to factor rotation rather than multi-asset TAA
- `regime-switching-jump-model-equity` (JAM 2024; single-asset SJM for equity timing;
  score 68 Worth Researching) — antecedent paper; single-asset, single-regime approach
  that this paper extends to per-asset multi-asset regime forecasting
- `hmm-rl-regime-taa` (arXiv 2605.27848; SPY/TLT/GLD rotation with HMM; score 44) —
  similar multi-asset regime-switching TAA concept but uses HMM (which SJM outperforms
  per this paper) and RL policy rather than MVO
- `gold-cross-asset-regimes` (cross-asset momentum on GLD+IEF; score 51) — simpler
  two-signal regime filtering for gold allocation

## Red Flags

- All specific performance metrics paywalled — unusual for a paper that also has an
  arXiv preprint; the preprint was inaccessible (403) during this research session
- GBDT trained on regime labels from the same return data (sequential estimation
  dependency) — potential for subtle in-sample bias if not handled carefully
- No open code for the GBDT + MVO pipeline
- Cost treatment not confirmable from accessible sources

## Source Links

| URL | Retrieved | Notes |
|-----|-----------|-------|
| https://arxiv.org/abs/2406.09578 | 2026-06-05 | arXiv abstract (403 on full HTML/PDF) |
| https://link.springer.com/article/10.1007/s10479-024-06266-0 | 2026-06-05 | AOR 2025 publication (403) |
| https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4864358 | 2026-06-05 | SSRN mirror (403) |
| https://ideas.repec.org/p/arx/papers/2406.09578.html | 2026-06-05 | IDEAS/REPEC (403) |
| https://github.com/Yizhan-Oliver-Shu/jump-models | 2026-06-05 | jumpmodels open-source code |
| https://arxiv.org/html/2402.05272v3 | 2026-06-05 | Related "Downside Risk" JAM 2024 paper (403) |

## Analyst Notes

**FACTS (from sources):**
- Paper published in Annals of Operations Research, vol. 346(1), pp. 285-318, March 2025
- Authors: Yizhan Shu, Chenyu Yu, John M. Mulvey (Princeton ORFE)
- arXiv identifier: 2406.09578; SSRN: 4864358
- 12 risky assets: global equity, bond, REIT, commodity indices
- Evaluation period: 1991–2023
- Methodology: SJM regime identification → GBDT regime forecasting → MVO allocation
- Three portfolio constructions compared: min-var, mean-var, naive
- jumpmodels Python package is open-source (SJM component only)
- Outperforms HMM-based strategies and buy-and-hold (stated in search summaries)

**ANALYSIS (my reasoning):**
- This is the multi-asset TAA analog of the factor rotation paper (JPM 2025); the
  research program is internally consistent and each paper extends the previous one
- The per-asset regime forecasting innovation is methodologically sound and addresses
  a real limitation of single-regime approaches
- The GBDT layer introduces complexity that may or may not add value over using SJM
  regime labels directly — the marginal benefit of forecasting vs. identifying regimes
  is not separable from accessible sources
- A ~1–2% annualized improvement over a 32-year period is consistent with what similar
  regime-based TAA papers report; this order of magnitude is plausible

**ASSUMPTIONS (flagged):**
- Monthly rebalancing assumed based on related work (not explicitly confirmed)
- Long-only allocation assumed (consistent with related papers)
- ~1–2% annualized return improvement treated as order-of-magnitude indicator only

## Future Research Needed

1. Access the full AOR 2025 paper to retrieve specific performance metrics (Sharpe,
   CAGR, max drawdown) and confirm cost treatment and OOS methodology
2. Examine whether the GBDT forecasting accuracy is reported — this is the key
   intermediate validation step that is missing from accessible summaries
3. Check if any independent researchers have implemented the full pipeline from
   the arXiv preprint and published replication results
4. Assess whether performance holds in the 2024–2026 out-of-sample period
5. Determine the specific 12 assets and their tickers for implementation scoping
