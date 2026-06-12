# Factor Timing via Momentum-Based Regime Switching (Tai, Leung, Jimenez 2026)

## Overview

Tai, Leung, and Jimenez (SSRN 6224058, February 2026) present a systematic framework for
dynamic allocation across five US equity style factors (Value, Size, Momentum, Quality,
Growth) by identifying Bull and Bear regimes for each factor using a z-score normalization
of that factor's trend signal. In Bull-regime factors, the portfolio overweights; in Bear-
regime factors, it underweights or equal-weights. The claim is a modest improvement in
risk-adjusted returns (Sharpe 0.66 vs. 0.59 for equal-weighting) with consistent calendar-
year outperformance over 1998–2025.

This is a variation on the "factor momentum" literature (Gupta/Kelly 2019 JFE: "Factor
Momentum Everywhere") — the core idea that past-period factor returns predict near-future
factor returns — implemented via a two-parameter z-score regime detector rather than a
simple trailing return rank.

---

## Asset Class / Timeframes

- **Asset class**: US Equity Factor Indices / Factor ETFs (long-only, multi-factor)
- **Factors**: Value, Size, Momentum, Quality, Growth (5 factors; market index as 6th
  benchmark)
- **Timeframe**: Monthly (regime rebalancing frequency implied; NOT explicitly stated)
- **Market**: US equity long-only factors

---

## Core Logic

For each of the five factor indices, compute a z-score of the factor's recent return
relative to its own history, using two hyperparameters (presumably: lookback window for
mean/std estimation, and z-score threshold for Bull/Bear classification). A factor in a
"Bull" regime is overweighted in the final portfolio; a "Bear"-regime factor receives no
additional weight (or is underweighted toward equal-weight). The authors report statistical
significance for regime identification across all five factors, with Size and Growth showing
strong significance (p < 0.01); Bull-regime cross-sectional forward returns are significant
at t = 1.98, p = 0.047.

The specific hyperparameter values (lookback period and z-score threshold) are NOT REPORTED
in accessible summaries. Portfolio construction rules (how much to tilt per regime, whether
Bear factors are zero-weighted or set to a floor) are NOT REPORTED in accessible summaries.

---

## Economic Rationale

Factor momentum — the tendency of factors that have recently outperformed to continue
outperforming over the near term — is documented in the academic literature (Gupta/Kelly
2019 JFE; Ehsani/Linnainmaa 2022 RFS). The economic driver is likely investor
under-reaction to changing factor premiums: as a factor's regime shifts, allocators are
slow to adjust, allowing the trend to persist long enough to be exploited.

**When the edge should disappear:**
- During rapid regime reversals (e.g., the 2022 momentum crash as rate regimes flipped)
- When factor autocorrelations break down after volatility shocks or macro regime shifts
- When the strategy is widely adopted and the predictability is arbitraged away
- During momentum crashes when momentum factor (one of the five) simultaneously
  switches while the model is still in Bull — the strategy suffers a within-regime shock

This rationale is **falsifiable**: if factor return autocorrelations are near zero (white
noise), the z-score regime signal carries no information and the strategy degrades to equal-
weighting. The Dichtl et al. (2019, FAJ) study specifically documents that in-sample
factor timing predictability does not survive transaction costs, which is the most direct
empirical threat to this idea.

---

## Entry Conditions

1. At each rebalancing date (monthly, implied), compute the z-score of each factor's return
   relative to its own historical distribution using the two specified hyperparameters.
2. A positive z-score above the threshold → Bull regime → overweight that factor.
3. A negative z-score below (−threshold) → Bear regime → underweight or equal-weight.
4. Exact threshold values and overweight magnitudes NOT REPORTED in accessible summaries.

---

## Exit Conditions

- Regime switch from Bull to Bear triggers a weight reduction at the next rebalancing.
- Long-only constraint: minimum weight is zero (no shorting factors).
- The strategy is always fully invested; no cash allocation mentioned.

---

## Risk Management

- Long-only construction provides implicit floor (no short factor positions).
- No explicit stop-loss or max-loss rules mentioned in accessible summaries.
- No leverage mentioned; implies 1× gross exposure.
- No position limits per factor explicitly stated.
- Overall risk management quality: BASIC (weight tilting only; no stops).

---

## Indicators Used

- Z-score of factor return relative to factor's own historical mean and standard deviation
  (2 hyperparameters: lookback window and z-score threshold)
- No external macro or market indicators mentioned

---

## Transaction Costs & Capacity

- **Transaction costs**: NOT MODELED in accessible summaries. Monthly factor ETF trading
  costs are low (ETF bid-ask spread + commission ≈ 0.03–0.15% round-trip), but the paper
  does not report whether these are included.
- **Dichtl et al. (2019, FAJ) empirical finding**: factor timing approaches fail to
  outperform after transaction costs in that study's sample — a direct precedent for the
  primary skeptical concern.
- **Capacity**: Factor ETFs (IVE, IWM, MTUM, QUAL, IVW-class) are highly liquid; capacity
  is not a binding constraint at most portfolio sizes.
- **Assessment**: Presumptively valid at ETF turnover costs given monthly rebalancing, but
  the paper does NOT confirm this.

---

## Backtest Evidence

SSRN working paper (February 2026). No peer review. Primary paper inaccessible (HTTP 403
on multiple attempts). All metrics from secondary search engine summaries.

**Period**: 1998–2025 (27 years). Full in-sample; no separate OOS holdout described.

---

## Forward-Test Evidence

NOT REPORTED. No live trading or paper-trading results mentioned.

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | 0.66 (vs. 0.59 EW benchmark) | CLAIMED, UNVERIFIED | SSRN 6224058 abstract summaries |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | — |
| CAGR | 13.0% (vs. 11.3% EW benchmark) | CLAIMED, UNVERIFIED | SSRN 6224058 abstract summaries |
| Annual Return | NOT REPORTED | NOT REPORTED | — |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | NOT REPORTED | NOT REPORTED | — |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | ~100% (long-only, always invested) | CLAIMED, UNVERIFIED | Paper implies full investment |

**Calendar-year outperformance vs. EW benchmark**: 78.6% of calendar years (1998–2025)
— CLAIMED, UNVERIFIED.

**Statistical significance of regime identification**: Size and Growth p < 0.01; all five
factors statistically significant; Bull-regime cross-sectional forward returns t = 1.98,
p = 0.047 — CLAIMED, UNVERIFIED.

---

## Community Sentiment

No community discussion found. The paper was posted February 2026 and there are no forum
threads, r/algotrading posts, or practitioner write-ups identified in search results.

The broader factor timing literature is skeptical:
- Dichtl, Drobetz, Lohre, Rother, Vosskamp (FAJ 2019): "Optimal Timing and Tilting of
  Equity Factors" — found in-sample factor timing predictability but characterized the
  results as a "cautionary tale" after transaction costs.
- Gupta/Kelly (2019, JFE) "Factor Momentum Everywhere" — documents factor momentum as a
  stand-alone factor, but this is distinct from using it to tilt multi-factor portfolios.
- The general consensus in the academic literature is that factor timing is extremely
  difficult to implement profitably out-of-sample.

---

## Why This Might Be Nothing

**Strongest skeptical case:**

1. **In-sample fitting on 2 hyperparameters over 27 years**: Even two hyperparameters are
   sufficient to find a z-score threshold that improves Sharpe from 0.59 to 0.66 over a
   27-year history, especially when the full 1998–2025 period was dominated by Growth and
   Momentum factors. The 2025 bull market for Growth specifically inflated the strategy's
   apparent performance.

2. **Regime luck (2009–2021 secular bull market)**: US factors showed strong autocorrelation
   during 2009–2021. A momentum-following regime model benefits enormously from this era and
   may be explicitly calibrated to it.

3. **The marginal t-statistic (t = 1.98, p = 0.047)**: This barely clears the 5% threshold.
   In a multi-factor universe with multiple regime configurations tested, even 2 hyperparameters
   can produce p ≈ 0.05 by chance. The finding does not survive even mild Bonferroni
   correction.

4. **Maximum drawdown NOT REPORTED**: The Sharpe improvement from 0.59 to 0.66 could be
   entirely explained by the strategy having a catastrophically worse drawdown in Bear regimes
   if the regime model lags behind actual factor reversals. Without max drawdown data, the
   comparison to the EW benchmark is incomplete and potentially misleading.

5. **The Dichtl et al. (2019) cautionary finding**: The closest precedent study — covering
   20 factor portfolios with timing and tilting — concluded that factor timing fails after
   costs. The Tai et al. approach does not address why their model would be immune to this
   documented failure mode.

6. **No transaction costs modeled**: Monthly rebalancing across 5 factors with regime
   switches likely produces 50–100% annual turnover; at 0.10% round-trip cost per switch,
   this erodes the 1.7% CAGR improvement by an unknown but potentially material amount.

7. **The evidence that most change my mind**: An independent walk-forward test on post-2025
   data, with explicit transaction cost modeling and comparison to the raw factor momentum
   strategy (long Bull factors, short Bear factors), with full disclosure of hyperparameter
   values and drawdown statistics.

---

## Strengths / Weaknesses

**Strengths:**
- Grounded in well-documented factor momentum literature (Gupta/Kelly 2019 JFE)
- Simple framework (only 2 hyperparameters) — less overfitting risk than complex models
- Long-only construction is realistic for most portfolios
- 27-year sample period provides some statistical depth
- Monthly rebalancing limits transaction costs

**Weaknesses:**
- Maximum drawdown NOT REPORTED — critical gap
- Transaction costs NOT MODELED in accessible summaries
- No explicit OOS holdout; full-sample results only
- Regime detection by momentum z-score will lag reversals — creates model risk during
  rapid regime transitions
- The improvement (Sharpe 0.59 → 0.66) is modest and marginal significance (p = 0.047)
- No open code; primary paper inaccessible
- Factor timing specifically called a "cautionary tale" in the most relevant precedent study

---

## Confidence Score

**Dimension breakdown:**

| Dimension | Score (0-10) | Weight | Weighted |
|-----------|-------------|--------|---------|
| Logical / economic soundness (falsifiable) | 6 | 3 | 18 |
| Transparency & reproducibility | 3 | 2 | 6 |
| Evidence auditability | 2 | 2 | 4 |
| Risk management quality | 4 | 2 | 8 |
| Cost & capacity realism | 3 | 2 | 6 |
| Honest treatment of drawdowns / failure | 2 | 1.5 | 3 |
| Robustness evidence (OOS / walk-forward / multi-market) | 3 | 1.5 | 4.5 |
| Survived independent scrutiny | 1 | 1 | 1 |
| **Total** | | **15** | **50.5 / 150** |

**latent_score** = round(50.5 / 150 × 100) = **34**

**Verification cap**: Evidence auditability = 2 ≤ 4 → cap at 59
**confidence** = min(34, 59) = **34** (Low Confidence)

Recorded as: latent 34 (capped to 34 — evidence auditability 2/10, SSRN working paper
only, primary paper inaccessible, no peer review, max drawdown NOT REPORTED, transaction
costs NOT MODELED; Dichtl et al. 2019 cautionary finding on factor timing is the
most relevant precedent)

---

## Complexity / Scalability / Automation Feasibility

- **Complexity**: Low. Monthly z-score computation on 5 factor indices.
- **Scalability**: High. Factor ETFs are highly liquid; no capacity constraints.
- **Automation**: Straightforward (monthly rebalancing trigger; rule-based).
- **Implementation feasibility**: HIGH — data from Bloomberg/MSCI factor indices or ETF
  proxies (IVE/IWM/MTUM/QUAL/IVW); z-score computation trivial.

---

## MQL5 / Python / Pine Feasibility

- **Python**: HIGH — pandas/numpy; monthly factor return z-scores; 10–20 lines of logic
- **MQL5**: LOW — not typical MQL5 use case (factor ETF universe, monthly)
- **Pine Script**: LOW — factor indices not easily tracked on TradingView

---

## Similar Strategies

- [`dynamic-factor-allocation-regime-shu-mulvey`](./dynamic-factor-allocation-regime-shu-mulvey.md) —
  CLOSELY RELATED but distinct: uses Statistical Jump Model (SJM) + Black-Litterman; peer-
  reviewed JPM 2025; 6 factors (includes Low Volatility); daily features; more sophisticated
  regime detection; scored 61 (Worth Researching). Cross-link: Tai et al. is a simpler
  two-parameter analog to the Shu/Mulvey framework with weaker evidence. Recommendation:
  access the Shu/Mulvey code first (jumpmodels PyPI available).
- [`regime-switching-jump-model-equity`](./regime-switching-jump-model-equity.md) — Uses SJM
  for overall market timing (not factor timing); open code (jumpmodels PyPI).
- [`systematic-reversal-industry-momentum`](./systematic-reversal-industry-momentum.md) —
  Orthogonal decomposition of industry momentum; different decomposition approach.

---

## Red Flags

- **Maximum drawdown NOT REPORTED** — this is a critical omission for any portfolio timing strategy
- **Transaction costs NOT MODELED** — Dichtl et al. (2019) found factor timing fails after costs
- **No explicit OOS split** — 27-year period is all in-sample
- **Marginal statistical significance** (t = 1.98, p = 0.047) — barely exceeds 5% threshold
- **Primary paper inaccessible** — HTTP 403 on multiple attempts; all metrics from secondary summaries
- **No open code** — strategy cannot be independently verified
- **No peer review** — SSRN working paper only

---

## Source Links

| URL | Retrieved | Used For |
|-----|-----------|----------|
| [SSRN 6224058 — Tai/Leung/Jimenez (2026)](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=6224058) | 2026-06-12 | Primary source; HTTP 403; secondary summaries used |
| [Dichtl et al. (2019, FAJ) — Factor Timing Cautionary Tale](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3277887) | 2026-06-12 | Precedent study on factor timing failure |
| [Gupta/Kelly (2019, JFE) — Factor Momentum Everywhere](https://www.aqr.com/-/media/AQR/Documents/Insights/Journal-Article/Factor-Momentum-Everywhere-JPM-Quant-19.pdf) | 2026-06-12 | Economic rationale foundation |
| [Web search summaries (Google/Bing snippets)](https://www.google.com) | 2026-06-12 | All quantitative claims above from secondary snippets |

---

## Analyst Notes

**FACTS (from accessible sources):**
- Paper authored by Jim Tai, Stephanie Leung, Justin Jimenez; posted SSRN Feb 12, 2026
- Strategy: z-score of factor return with 2 hyperparameters; Bull/Bear regime → weight tilting
- 5 factors: Value, Size, Momentum, Quality, Growth
- Period: 1998–2025; Sharpe 0.66 vs 0.59 EW; CAGR 13.0% vs 11.3%; 78.6% calendar year wins
- Statistical significance: all 5 factors significant; Size and Growth p < 0.01; Bull regime t=1.98, p=0.047
- Primary paper inaccessible (HTTP 403 on all attempts); no open code; no peer review

**ANALYSIS:**
- CAGR improvement = +1.7% annualized; Sharpe improvement = +0.07. This is well within the range
  of data-mining noise over a 27-year period with any 2-hyperparameter model.
- The 2010–2025 period was exceptionally favorable to Growth and Momentum factors (tech secular
  bull). A regime model that learned to stay long Growth/Momentum during this era would have
  outperformed without any genuine predictive content.
- IMPLIED ANALYSIS: If we assume total portfolio volatility ≈ 15% (typical US multi-factor),
  the Sharpe difference implies ~0.7% additional annual return per unit vol. This is below the
  threshold of statistical detectability in most realistic sample sizes.
- The Dichtl et al. (2019, FAJ) study is the single most important precedent: it tested factor
  timing with exactly the set of approaches this paper uses (technical indicators, momentum-based
  signals) and concluded that the practical benefit is negligible after costs.

**ASSUMPTIONS:**
- Monthly rebalancing assumed (not stated); costs assumed ~0.10% round-trip for ETF implementation
- OOS split absent; assumed full-sample in-sample optimization

---

## Future Research Needed

1. Access the full paper PDF (SSRN 6224058) when available without paywall to verify:
   - Exact hyperparameter values (lookback window and z-score threshold)
   - Max drawdown and other risk metrics
   - Whether transaction costs were modeled
   - Whether any OOS holdout was performed
2. Compare directly to raw factor momentum strategy (Gupta/Kelly 2019) — is the z-score
   regime approach better than simply buying the last-period winning factor?
3. Test on 2022–2025 subsample: the 2022 momentum crash is the critical stress test for
   any momentum-following regime model.
4. If code becomes available, replicate the z-score computation and check sensitivity to
   hyperparameter values (if the 2 parameters can be varied within ±20% without losing
   significance, the result is more credible).
