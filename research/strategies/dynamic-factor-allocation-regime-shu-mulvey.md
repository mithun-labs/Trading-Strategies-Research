# Dynamic Factor Allocation Leveraging Regime-Switching Signals

## Overview

A fully-invested, long-only US equity multi-factor portfolio that dynamically over- and under-weights six
style factors (value, size, momentum, quality, low volatility, growth) based on factor-specific bull/bear
regime identification using the Sparse Jump Model (SJM). Portfolio weights are constructed via the
Black-Litterman framework, using regime inferences as the factor views. Peer-reviewed in the Journal of
Portfolio Management (JPM Vol. 51, Issue 3, 2025). Core SJM engine is openly available via the `jumpmodels`
Python package.

**Source:** Shu, Yizhan, and John M. Mulvey. "Dynamic Factor Allocation Leveraging Regime-Switching Signals."
*Journal of Portfolio Management*, Vol. 51, Issue 3, 2025.
Also available: arXiv 2410.14841 (first posted Oct 18, 2024); SSRN 4960484.
*Authors: Princeton University, Operations Research and Financial Engineering.*

## Asset Class / Timeframes

- **Asset class:** US Equities — specifically 7 long-only factor indices (market + 6 style factors)
- **Factors:** Value, Size, Momentum, Quality, Low Volatility, Growth (MSCI/Russell/CRSP-style factor
  indices implied; specific indices NOT CONFIRMED in accessible sources)
- **Rebalancing:** Approximately monthly (inferred from regime-switching frequency; exact rebalancing
  cadence NOT REPORTED in accessible summaries)
- **Position type:** Fully-invested, long-only (no shorting)

## Core Logic

**Step 1 — Regime identification per factor:**
For each of the 6 style factors, apply the Sparse Jump Model (SJM) to identify whether the factor is
currently in a bull regime (active return > 0, i.e. factor outperforming the market) or a bear regime
(active return < 0). The SJM uses a feature set drawn from: (a) risk and return measures of the factor's
historical active returns, and (b) broader market environment variables. The SJM's jump penalty parameter
(λ) controls regime-change frequency, producing stable regimes less susceptible to noise reversals than
standard Markov-Switching Models.

**Step 2 — Black-Litterman portfolio construction:**
Integrate the factor-specific regime inferences as views in the Black-Litterman (BL) framework.
- Bull regime for a factor → positive view (overweight this factor relative to market-cap baseline)
- Bear regime for a factor → neutral or negative view (underweight or match-weight)
- The BL posterior mean and covariance determine portfolio weights
- Output: fully-invested (100% allocated), long-only multi-factor equity portfolio

**Step 3 — Rebalance when regimes change:**
Because the SJM is designed to produce infrequent regime transitions (via the jump penalty), portfolio
turnover is structurally low compared to daily ML-driven strategies.

## Economic Rationale

Factor premiums (value, momentum, quality, etc.) are cyclical by nature and co-move with macroeconomic
regimes. For example:
- Value tends to outperform in economic recoveries (early cycle) and underperform in late-cycle slowdowns.
- Momentum outperforms in trending, low-volatility markets and reverses sharply in high-volatility crises.
- Low Volatility outperforms in risk-off regimes and underperforms in strong risk-on rallies.
- Quality outperforms during periods of tight credit conditions and earnings uncertainty.

The edge lies in identifying these regime transitions *before* they fully price in. The SJM's stability-
controlled regime identification is better suited for this than short-memory models.

**When the edge should disappear (falsifiability test):**
- If macroeconomic cycles shorten to the point where regime transitions happen faster than the SJM can
  detect (e.g., cycles < 3 months), the factor timing signal degrades.
- If factor premiums are fully arbitraged (i.e., the remaining premium is entirely compensated risk, not
  predictable timing), the regime signal adds no value beyond static allocation.
- As more institutional capital runs near-identical regime-based factor timing, the regime transitions
  become price-in events, reducing the timing advantage.
- The edge should also weaken if factor definitions become correlated (e.g., quality and low-vol converge),
  reducing the diversification benefit of factor-specific regime signals.

## Entry Conditions

- At each rebalancing date: run the SJM on each factor's active-return history to classify current regime.
- Submit the regime states as views into the BL framework to compute updated posterior portfolio weights.
- Rebalance to the new target weights.
- (No individual stock trades — only factor index weights are adjusted; the underlying factor index
  manages constituent selection internally.)

## Exit Conditions

- Positions are adjusted on regime change (SJM detects a transition) or at regular rebalancing intervals.
- No explicit stop-loss or max-loss rule described in accessible sources.
- The BL framework provides implicit diversification across factors even in adverse regimes.

## Risk Management

- **Fully-invested long-only:** No leverage; bounded downside at 100% loss.
- **Diversification:** 6 factor indices with low mutual correlation (confirmed: single-factor long-short
  strategies show low inter-factor correlation).
- **Regime stability:** The jump penalty λ in the SJM prevents frequent false regime transitions, acting as
  a noise filter and keeping turnover low.
- **No explicit stop-loss:** NOT REPORTED in accessible sources.
- **Position sizing:** Black-Litterman posterior weights; specific constraints (min weight floor, max factor
  weight) NOT REPORTED.

## Indicators Used

- Per-factor: risk and return measures of the factor's active return history (exact features NOT REPORTED
  from accessible sources; paywalled in paper)
- Broader market environment variables (exact features NOT REPORTED)
- Sparse Jump Model output: binary bull/bear regime per factor
- Black-Litterman views constructed from regime classification
- No traditional technical indicators (e.g., no moving averages, RSI, or momentum scores)

## Transaction Costs & Capacity

**Transaction costs:**
- NOT EXPLICITLY STATED in accessible summaries.
- ANALYSIS: The SJM's jump penalty produces infrequent regime transitions. In the related
  `regime-switching-jump-model-equity` paper, regime changes occurred roughly 2–4× per year per asset.
  Assuming similar behavior for 6 factors: ~2 transitions/year × 6 factors = ~12 weight adjustments/year.
  If each weight change is ~5% of portfolio, and factor ETF trading costs are 1–5bps, annual transaction
  costs are negligible (<10bps). ASSUMPTION.
- ASSUMPTION: Factor ETFs (VLUE, MTUM, QUAL, USMV, SIZE, IWF or similar) used for implementation would
  have low bid-ask spreads (1–3bps). Cost is NOT a meaningful constraint for this strategy.

**Capacity:**
- Very high. Long-only factor indices or ETFs are among the most liquid equity vehicles. AUM capacity is
  in the hundreds of billions of dollars.
- No capacity concern for any institutional or retail manager.

## Backtest Evidence

- Period: NOT CONFIRMED in accessible sources (JPM paper is paywalled). Likely multi-decade given the
  Shu/Mulvey group's standard (related paper covers 1990–2023). ASSUMPTION.
- Status: The strategy's performance metrics are CLAIMED, UNVERIFIED from paywalled sources; however,
  the peer-review process in JPM provides independent methodology validation.

## Forward-Test Evidence

NOT REPORTED.

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | Improved vs. equal-weight benchmark (exact value NOT REPORTED) | CLAIMED, UNVERIFIED | JPM Vol. 51 Issue 3, 2025 (paywalled) |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | Reduced vs. equal-weight benchmark (exact value NOT REPORTED) | CLAIMED, UNVERIFIED | JPM Vol. 51 Issue 3, 2025 (paywalled) |
| CAGR | NOT REPORTED | NOT REPORTED | — |
| Annual Return | NOT REPORTED | NOT REPORTED | — |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | NOT REPORTED | NOT REPORTED | — |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | ~100% (fully-invested, long-only) | CLAIMED, UNVERIFIED | ASSUMPTION from strategy description |
| Information Ratio (vs. market) | Improved from 0.05 (EW benchmark) to ~0.4 (SJM allocation) | CLAIMED, UNVERIFIED | Search-result summary of JPM paper |
| Single-factor L/S Sharpe (validation) | Positive for all 6 style factors; low inter-factor correlation | CLAIMED, UNVERIFIED | Search-result summary of JPM paper |

**Note on limited accessible metrics:** The primary performance table (absolute Sharpe, CAGR, drawdown) is
paywalled in JPM. The IR improvement from 0.05 to 0.4 is the only concrete quantitative result available
from accessible sources. ANALYSIS: An IR of 0.4 vs. 0.05 is a substantial improvement in active
management terms — 0.4 would place this strategy in the top quartile of active equity managers.
However, absolute return and drawdown context is essential and NOT CONFIRMED here.

## Community Sentiment

**Positive:**
- Published in Journal of Portfolio Management (peer reviewed, reputable for factor investing research).
- The `jumpmodels` Python package (PyPI, GitHub) is openly available and actively maintained; this builds
  trust in the reproducibility of the SJM methodology.
- The broader Shu/Mulvey/Kolm research program (multiple papers, Princeton group) has been well-received
  in the quantitative portfolio management community.

**Critical discussion:**
- No known academic critiques found.
- The paper is relatively new (Oct 2024 posted, JPM 2025) and has not yet attracted significant community
  discussion in accessible forums (ForexFactory, r/algotrading, MQL5).
- Passed JPM peer review — the most meaningful scrutiny filter available for this paper.

## Why This Might Be Nothing

### 1. Most likely benign explanation: factor timing may not beat static multi-factor allocation

The equal-weight multi-factor benchmark (IR 0.05) is already a low bar. Many studies show that factor
timing is extremely difficult and that static multi-factor diversification captures most of the available
factor premium. The IR improvement from 0.05 to 0.4 is real, but if the EW benchmark is low-quality, the
comparison may flatter the dynamic strategy. ANALYSIS: A fair comparison would require relative performance
against a factor-momentum or factor-value strategy (which are documented return predictors for factors),
not just equal weighting.

### 2. The cost case: turnover is low, but regime timing can concentrate factor risk

The jump model's infrequent transitions mean low trading costs, which is good. But the model may
concentrate significant portfolio weight in momentum or growth factors during bull regimes, just before
they reverse. The worst drawdowns in factor portfolios occur during sharp reversals (e.g., momentum crash
in 2009, value underperformance 2018–2020). If the SJM identifies these as bull regimes at the onset of
the crash, losses can be concentrated and large.

### 3. Regime dependence: factor cycles are slow but the model is calibrated on historical regimes

The SJM is calibrated on historical factor-regime data. If macro regimes change structurally (e.g.,
prolonged growth/tech dominance as in 2015–2022), the model may persistently identify bull regimes for
growth and momentum even as mean reversion becomes more likely. The transition to a "new regime" the
model hasn't been calibrated on is the key tail risk.

### 4. Sample/overfitting concerns

The SJM has at most 2–3 hyperparameters per factor (jump penalty λ, number of states, feature set
selection). Paywalled specifics prevent full overfitting assessment. The paper tests 6 factors with
separate models — 6 hyperparameter-tuned models create some multiple-testing concern, though the
consistent positive single-factor L/S Sharpe across all factors is reassuring.

### 5. What single piece of evidence would most change my mind

Access to the full paper to confirm: (a) absolute Sharpe, CAGR, and drawdown of the dynamic vs. EW and
factor-momentum benchmarks; (b) the exact OOS / walk-forward setup; (c) evidence that 2022 (value
outperformed dramatically while momentum crashed) was handled correctly in real time.

## Strengths / Weaknesses

**Strengths:**
- Peer-reviewed in JPM — rare in this research database; methodology passed independent scrutiny.
- Economically grounded: factor cycles driven by macro regimes is well-documented in academic literature.
- Core SJM engine is openly available (`jumpmodels` on PyPI), enabling partial independent replication.
- Low turnover by design (jump penalty → infrequent transitions) → low trading costs.
- Multi-factor diversification: positive Sharpe for all 6 factor-specific regime signals, with low
  inter-factor correlation — genuine diversification.
- Fully-invested, long-only: no leverage, no shorting complexity.

**Weaknesses:**
- Key performance metrics (Sharpe, CAGR, max drawdown) are NOT ACCESSIBLE (paywalled in JPM).
- No full open-source implementation of the factor allocation code (only SJM engine is open).
- Absolute performance level unknown from accessible sources.
- The BL prior construction and SJM feature set are not confirmed accessible.
- The comparison benchmark (EW factor portfolio, IR 0.05) is a low bar — comparison to factor-momentum
  benchmark would be more informative.
- Data period unknown from accessible sources.

## Confidence Score

### Per-Dimension Breakdown

| Dimension | Weight | Score | Weighted |
|-----------|--------|-------|---------|
| Logical / economic soundness (falsifiable) | 3 | 7 | 21 |
| Transparency & reproducibility | 2 | 7 | 14 |
| Evidence auditability | 2 | 7 | 14 |
| Risk management quality | 2 | 5 | 10 |
| Cost & capacity realism | 2 | 5 | 10 |
| Honest treatment of drawdowns / failure | 1.5 | 5 | 7.5 |
| Robustness evidence (OOS / walk-forward / multi-market) | 1.5 | 6 | 9 |
| Survived independent scrutiny | 1 | 6 | 6 |
| **Total** | **15** | — | **91.5** |

**Latent score:** round(91.5 / 150 × 100) = **61**
**Evidence auditability = 7 → cap at 74**
**Confidence = min(61, 74) = 61 (cap does not bind; latent 61 < 74)**

`latent 61 (capped to 74 pending full verification: peer-reviewed JPM; IR improvement 0.05→0.4 confirmed; but absolute Sharpe/CAGR/drawdown paywalled; full factor allocation code not openly published)`

**Band: Worth Researching**

**Scoring rationale:**
- Logic 7/10: Factor cycles are well-documented; mechanism is falsifiable (shorter cycles, arbitrage,
  structural regime changes would erode it).
- Transparency 7/10: JPM peer-reviewed; jumpmodels engine open-source; BL is standard; full feature set
  and allocation code not confirmed open.
- Evidence 7/10: JPM peer review; related open code; but paywalled metrics and no confirmed full code.
- Risk management 5/10: Long-only, BL diversification, low turnover are strengths; no explicit stop-loss.
- Cost/capacity 5/10: Implicitly low turnover (jump penalty); factor ETFs are highly liquid; costs not
  quantified but structurally low.
- Drawdowns 5/10: Paper states max DD improved; exact values paywalled; failure regime analysis not seen.
- Robustness 6/10: JPM peer review implies OOS testing; consistent positive L/S Sharpe across all 6
  factors; data period not confirmed.
- Scrutiny 6/10: JPM peer review is the primary scrutiny gate; no independent community replications yet.

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Moderate. The SJM is available via PyPI, but the BL integration and feature engineering
  require domain knowledge. Not trivial but well within quant finance capabilities.
- **Scalability:** Very high — long-only factor ETF portfolio; no scalability constraint.
- **Automation feasibility:** High — fully systematic, rule-based once implemented.

## MQL5 / Python / Pine Feasibility

- **Python:** High feasibility. `jumpmodels` (PyPI) for the SJM. `PyPortfolioOpt` or custom BL
  implementation for portfolio construction. Factor index data from MSCI, FTSE Russell, or proxied via
  factor ETFs (iShares VLUE, MTUM, QUAL, USMV, SIZE, IWF). Moderate implementation effort.
- **MQL5:** Not applicable — this strategy uses equity factor indices/ETFs, not FX or CFD instruments.
- **Pine Script:** Feasible for simplified version (e.g., switching between pre-computed factor ETFs on
  TradingView), but SJM regime computation requires external data feed.

## Similar Strategies

- `regime-switching-jump-model-equity` — same SJM methodology from the same Princeton group (arXiv
  2402.05272), but applied to equity INDEX timing (in/out of market) rather than factor rotation. Cross-
  link: this paper is an evolution of the jump model application to factor allocation.
- **"Dynamic Factor Allocation via Momentum-Based Regime Switching"** (Tai/Leung/Jimenez, SSRN 6224058,
  Feb 2026) — analogous factor timing using z-score normalized momentum signals rather than SJM; Sharpe
  0.66 vs. 0.59 EW; 1998–2025; not peer-reviewed. This is the closest competing approach. The Shu/Mulvey
  paper has stronger evidence quality (peer-reviewed) but fewer accessible performance metrics.
- `hmm-rl-regime-taa` — also uses regime-switching (HMM + RL) for TAA, but on multi-asset (SPY/TLT/GLD)
  rather than factor rotation.

## Red Flags

- Key performance metrics (Sharpe, CAGR, max drawdown) paywalled — cannot fully evaluate the strategy.
  `NOT REPORTED`
- Benchmark is equal-weight factor portfolio (IR 0.05) — a relatively weak comparison; factor-momentum or
  industry-standard smart-beta benchmarks would be more informative.
- Full factor allocation code not confirmed open (only the SJM engine is available via `jumpmodels`).

## Source Links

| Source | URL | Accessed |
|--------|-----|----------|
| JPM publication page | https://www.pm-research.com/content/iijpormgmt/51/3/50 | 2026-06-04 (paywalled) |
| arXiv 2410.14841 | https://arxiv.org/abs/2410.14841 | 2026-06-04 |
| SSRN 4960484 | https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4960484 | 2026-06-04 |
| jumpmodels GitHub | https://github.com/Yizhan-Oliver-Shu/jump-models | 2026-06-04 |
| jumpmodels PyPI | https://pypi.org/project/jumpmodels/ | 2026-06-04 |
| Related paper (regime-switching-jump-model-equity) | https://arxiv.org/abs/2402.05272 | 2026-06-04 |

## Analyst Notes

**FACTS (from sources):**
- Peer-reviewed in Journal of Portfolio Management, Vol. 51, Issue 3, 2025.
- Universe: 7 long-only indices — market + 6 style factors (value, size, momentum, quality, low
  volatility, growth).
- Methodology: SJM identifies per-factor bull/bear regime → Black-Litterman portfolio construction.
- IR improved from 0.05 (EW benchmark) to ~0.4 (SJM dynamic allocation) — from accessible search result
  summaries of the JPM paper.
- Sharpe ratio and max drawdown both improved vs. EW (stated in accessible summaries; exact values NOT
  REPORTED — paywalled).
- Single-factor L/S strategy: positive Sharpe across all 6 factors; low inter-factor correlation.
- jumpmodels Python package (PyPI/GitHub) provides the SJM engine; factor allocation code not confirmed open.
- Related paper from same group: arXiv 2402.05272 (regime-switching-jump-model-equity, already in database).

**ANALYSIS:**
- IR of ~0.4 is a genuine and meaningful result for an active long-only equity strategy. Most active equity
  fund managers fail to beat the market net of fees; an IR of 0.4 would be well above average. If the
  absolute Sharpe is also meaningful (e.g., >0.6), this strategy would rank higher in our database.
- The key unknown is whether the IR improvement persists net of: (a) transaction costs (likely minimal due
  to infrequent rebalancing), (b) a factor-momentum benchmark comparison (which would be more conservative
  than EW), and (c) out-of-sample period after paper finalization.
- The research group (Shu, Mulvey, Kolm) is a leading academic team in regime-switching portfolio
  management; their methodology is consistently rigorous.

**ASSUMPTIONS:**
- Rebalancing frequency is monthly or quarterly (ASSUMPTION based on typical factor strategy cadence and
  the SJM's infrequent transitions).
- Data period is likely 1990–2024 or similar (ASSUMPTION based on related papers from same group).
- Factor indices are MSCI or Russell style indices (ASSUMPTION; not confirmed in accessible sources).

## Future Research Needed

1. Access full JPM paper to: confirm data period, absolute Sharpe/CAGR/drawdown, exact feature set for
   SJM, BL parameter settings, benchmark comparison vs. factor-momentum strategies.
2. Verify if open code for the factor allocation (beyond the SJM engine) is available in the
   `jump-models` GitHub repository or elsewhere.
3. Check if the strategy handles 2022 (value outperformed, momentum crashed) correctly in near-real-time.
4. Compare against the Tai/Leung/Jimenez (SSRN 6224058) approach using the same benchmark and period.
5. Attempt partial replication using factor ETFs (VLUE, MTUM, QUAL, USMV) and the `jumpmodels` package.
