# ESG State-Dependent Momentum (Winners vs. Losers in Pro/Anti-ESG Regimes)

## Overview

A state-dependent long-short momentum framework that conditions portfolio construction on a
latent two-state ESG sentiment regime (pro-ESG vs. neutral/anti-ESG), identified via a
Markov chain. The central finding is **counterintuitive**: ESG loser portfolios outperform
ESG winner portfolios during pro-ESG policy regimes, suggesting market overreaction creates
short-term pricing inefficiencies that can be systematically exploited. Tested across Russell
3000 equities, Dow Jones 30, and 30 largest cryptocurrencies; 2-week formation / 2-week
holding period identified as optimal.

Source: arXiv 2505.24250 (May 2025). Preprint only; no peer review.

## Asset Class / Timeframes

- **Markets**: US Equities (Russell 3000; Dow Jones 30); Cryptocurrencies (30 largest by market cap)
- **Timeframes**:
  - Russell 3000: February 1, 2017 – February 21, 2025 (8 years; Bloomberg Terminal)
  - Dow Jones 30: November 23, 2020 – November 18, 2024 (~4 years)
  - Crypto: November 19, 2020 – November 17, 2024 (~4 years; 24/7 trading)
- **Rebalancing**: Biweekly (best: 2-week formation, 2-week holding period)
- **Portfolio implementation**: Via **30 principal components** of the Russell 3000 returns
  (not individual stocks); equivalent for DJ30 / Crypto (direct)

## Core Logic

1. **ESG regime identification**: Estimate a latent 2-state Markov chain (D_t ∈ {0,1}) from
   portfolio return dynamics. State 1 = pro-ESG; State 0 = anti/neutral-ESG. Transitions
   anchored to major ESG regulatory events (e.g., Biden administration's Jan 2021
   Paris Agreement recommitment, SEC climate disclosure proposals).
2. **Return forecasting**: Apply ARMA(1,1)-GARCH(1,1) model to decompose expected returns
   into a baseline market price of risk (λ₀) and an ESG-regime-dependent adjustment (λ₁).
3. **Portfolio construction**: 
   - Rank assets (or PCA factor portfolios) by ARMA-GARCH forward-looking return forecast.
   - Form **Winners**: top quantile of forecasted returns.
   - Form **Losers**: bottom quantile.
   - Long-short spread = Winners − Losers.
   - 4% turnover constraint to limit excessive churn.
4. **Key inversion**: In pro-ESG regimes, the historical ESG-sorted portfolios are re-oriented
   so that the "loser" portfolio (low-ESG firms) is expected to outperform — market overreaction
   creates a mean-reversion opportunity.
5. Equal-weighting within quantiles (inferred from paper; exact weighting not fully specified).

## Economic Rationale

**FACTS (from source):** ESG-winner stocks attract capital flows during pro-ESG sentiment
periods, potentially pushing valuations above fundamental value. ESG-loser stocks are
systematically avoided, creating undervaluation. When ESG policy sentiment is pro-ESG, the
overpriced winners are vulnerable to mean reversion, while the discounted losers have upside.

**ANALYSIS:** This is essentially a **sentiment-contrarian rationale** for cross-sectional
reversal within ESG categories. The mechanism is:

- Pro-ESG regime → investors overallocate to ESG winners → winners overpriced, losers
  underpriced → forward-looking returns: losers > winners.
- Anti-ESG regime → flows reverse → winners undervalued, losers overpriced.

Conditions when the edge should DISAPPEAR: (a) efficient markets immediately price ESG policy
risk without overreaction; (b) stable ESG sentiment with no regime transitions; (c) ESG
information is fully integrated into valuations (long-run equilibrium); (d) regulation changes
are anticipated rather than surprise news.

**Falsifiability verdict**: The mechanism is partially falsifiable (specifies regime as the
gating condition), but the regime is estimated endogenously from return data — making it
circular to some degree. The "pro-ESG → losers outperform" prediction could be tested against
known regulatory event windows, but this has not been done in the paper.

## Entry Conditions

- At each 2-week rebalance, compute forward-looking ARMA-GARCH return forecasts for all
  assets (or PCA factor portfolios).
- Identify ESG regime state D_t from Markov chain (via hidden-state inference / EM algorithm).
- Rank assets by forecasted return; long top quantile, short bottom quantile.
- In pro-ESG regime: apply regime-dependent adjustment (λ₁) to re-rank expectations toward
  ESG losers having higher expected returns.

## Exit Conditions

- Rebalance every 2 weeks (biweekly): exit positions when an asset moves out of the top/bottom
  quantile or when the ESG regime transitions.
- No intra-period stop-loss or take-profit rules defined.

## Risk Management

- Long-short construction provides market-neutral direction in principle (though the PCA
  implementation may not be dollar-neutral vs. the actual equity market).
- 4% turnover constraint limits excessive trading and associated costs.
- **GAPS:** No explicit stop-loss, no position sizing by volatility or risk, no max-drawdown
  circuit breaker. The regime gate is the sole risk mechanism; a misidentified regime state
  can lock in the wrong position for a full 2-week period.

## Indicators Used

- ARMA(1,1)-GARCH(1,1) return forecasts
- Latent 2-state Markov chain (ESG regime)
- 30 principal components (Russell 3000 PCA)
- STARR ratio (Stable Tail Adjusted Return Ratio at 50–99% confidence)
- Rachev ratio (tail ratio at 99%/99%)
- Sharpe ratio (conventional)

## Transaction Costs & Capacity

- **Costs explicitly NOT modeled.** Authors state: "we do not explicitly model slippage,
  bid-ask spreads, or liquidity-driven price impact" and treat results as "frictionless upper
  bounds on momentum profitability."
- 2-week rebalancing on Russell 3000 PCA-factor portfolios would involve significant bid-ask
  and market impact costs for any implementation with real AUM.
- **ANALYSIS:** The paper's own 4% turnover constraint acknowledges that unconstrained
  rebalancing is too costly, but the 4% cap is simply a constraint, not a modeled cost. The
  actual impact on returns is unknown. For any fund-level implementation, bi-weekly
  rebalancing of a cross-sectional long-short strategy on ~3,000 stocks via PCA would be
  operationally complex and expensive. Capacity is likely limited.

## Backtest Evidence

CLAIMED, UNVERIFIED — arXiv preprint, May 2025:

### Sharpe Ratios (2W/2W rebalancing)

| Portfolio | Russell 3000 | Dow Jones 30 | Cryptocurrencies |
|-----------|-------------|-------------|-----------------|
| Winners | 0.657 | 1.510 | 0.450 |
| Losers | 0.973 | 1.144 | 0.491 |
| Spread (W−L) | **-0.316** | 0.367 | -0.041 |

**Critical observation:** The momentum spread (long Winners, short Losers) has a NEGATIVE
Sharpe ratio in the two largest universes (Russell 3000: -0.316; Crypto: -0.041). This means
the classic momentum strategy **fails** in these samples. The paper's value-add is the regime
conditioning, which reverses the expected direction of the spread. This is a pre-existing
failure mode, not a successful strategy being enhanced.

### STARR Ratios at 99% Confidence (2W/2W)

| Portfolio | Russell 3000 | Dow Jones 30 | Cryptocurrencies |
|-----------|-------------|-------------|-----------------|
| Winners | 0.612 | 1.474 | 0.288 |
| Losers | 0.976 | 1.521 | 0.700 |
| Spread (W−L) | **-0.363** | -0.046 | -0.412 |

### Rachev Ratios at 99%/99% (2W/2W)

| Portfolio | Russell 3000 | Dow Jones 30 | Cryptocurrencies |
|-----------|-------------|-------------|-----------------|
| Winners | 0.997 | 1.502 | 0.564 |
| Losers | 0.540 | 1.361 | 0.313 |
| Spread (W−L) | 0.457 | 0.141 | 0.251 |

### Cumulative Return over Full Period (2W/2W)

| Portfolio | Russell 3000 | Dow Jones 30 | Cryptocurrencies |
|-----------|-------------|-------------|-----------------|
| Winners | +54.5% | +124.9% | +102.5% |
| Losers | +28.4% | +167.9% | +7.7% |
| Spread (W−L) | +26.1% | **-43.1%** | +94.9% |

Note: DJ30 spread cumulative return is -43.1% despite positive Sharpe (0.367), indicating
high variance drag (arithmetic return positive but geometric compound negative). This implies
extreme volatility in the spread during the 2020-2024 period.

### Markov Chain Regime Transition Probabilities

| Portfolio | P(anti→anti) | P(pro→pro) |
|-----------|-------------|------------|
| Winners | 0.54 | **0.18** |
| Losers | 0.31 | 0.72 |
| Momentum spread | 0.87 | 0.81 |

The pro-ESG regime for the Winners portfolio has only 18% persistence (collapses to
anti-ESG 82% of the time), making regime-based conditioning highly unstable for this
portfolio. The momentum spread regime is highly persistent (87%/81%), which supports
the feasibility of conditioning on the spread's own regime state.

## Forward-Test Evidence

NOT REPORTED — No forward test or live trading data provided.

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | -0.316 (Russell 3000 W-L spread); 0.367 (DJ30 W-L spread) | CLAIMED, UNVERIFIED | arXiv 2505.24250 |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | — |
| CAGR | NOT REPORTED | NOT REPORTED | — |
| Annual Return | NOT REPORTED | NOT REPORTED | — |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | ~2 weeks (2-week holding period, stated) | CLAIMED, UNVERIFIED | arXiv 2505.24250 |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | NOT REPORTED | NOT REPORTED | — |

Note: STARR (Stable Tail Adjusted Return Ratio) and Rachev ratio reported in paper (see
Backtest Evidence section) but are non-standard metrics not in the above table. All standard
metrics (Sharpe for the spread, drawdown, CAGR) either NOT REPORTED or adverse.

## Community Sentiment

No community discussion found. The paper has no citations on Google Scholar or arXiv at the
time of retrieval. This is a May 2025 preprint with zero independent commentary.

## Why This Might Be Nothing

### 1. Most Likely Benign Explanation

The primary finding — ESG losers outperform winners in pro-ESG regimes — is likely explained
by **sample composition bias**: the Russell 3000 and DJ30 periods include the 2020–2022 energy
sector rebound (energy stocks are archetypal ESG losers that dramatically outperformed during
the post-COVID commodity rally and 2022 energy crisis). The Biden ESG transition in January
2021 is the identified "pro-ESG regime" onset — and that period (2021-2022) happened to see
energy (ESG losers) strongly outperform clean energy (ESG winners). This is regime-luck, not
a generalizable pattern.

### 2. The Cost Case

2-week rebalancing on a long-short cross-sectional strategy is costly. The paper explicitly
acknowledges it provides only "frictionless upper bounds." The 4% turnover constraint is not
the same as modeling bid-ask spreads, borrow costs, and market impact. For a strategy that
requires accurate regime timing, high-frequency implementation costs are the most likely
performance killer. The Russell 3000 W-L spread Sharpe of -0.316 gross leaves essentially
no room for any costs.

### 3. Regime Dependence

The strategy is entirely dependent on accurate real-time identification of the ESG regime
state D_t. The regime is estimated from portfolio return dynamics (endogenous) rather than
from independent ESG policy signals. In practice, you would need to know today whether you're
in a "pro-ESG" or "anti-ESG" regime to trade the correct direction — but the Markov chain
estimated from historical returns is a lagging signal, not a leading one.

### 4. Sample / Overfitting Concerns

- DJ30 and Crypto periods start November 2020 (COVID market trough) — one of the most
  regime-biased starting points possible.
- Russell 3000 period (2017-2025) spans approximately 2 major ESG policy transitions (Trump
  withdrawal 2017, Biden recommitment 2021, Trump withdrawal 2025). This is too few transitions
  for robust Markov chain estimation with 3 parameters per state.
- PCA reduces the Russell 3000 to 30 components: this is a substantial information reduction
  that changes the strategy from implementable stock selection to an abstract factor exercise.
- No grid search or multiple testing correction is disclosed, but the 3 tested formation/holding
  combinations still represent researcher degrees of freedom.

### 5. The Critical Test: Russell 3000 Momentum Spread Sharpe is NEGATIVE

The classic momentum strategy (long Winners, short Losers by past return) has a Sharpe of
-0.316 in the Russell 3000 over the full 2017-2025 period. This means the ESG-momentum
reversal is not being added on top of an already profitable strategy; it is being offered as
a way to extract alpha from a fundamentally failing signal. The evidence that regime
conditioning salvages this is entirely from the paper's own ESG-tagged backtesting — no
independent replication exists.

### 6. What Would Change My Mind

A peer-reviewed paper with cost-inclusive results (especially bid-ask for the 2-week W-L
spread), an independent OOS hold-out on data post-February 2025, and implementation via
actual individual stock long-short portfolios (not PCA) would substantially increase
confidence. A replication on a non-US equity market would also help establish generalizability.

## Strengths / Weaknesses

**Strengths:**
- Novel angle: ESG sentiment regime conditioning applied to standard momentum
- Multi-market testing (equities + crypto) provides some robustness check
- Rolling window methodology provides partial OOS quality
- Counterintuitive finding (ESG losers > winners) generates a testable, falsifiable hypothesis
- STARR and Rachev ratios provide richer tail-risk picture than Sharpe alone

**Weaknesses:**
- Russell 3000 momentum spread Sharpe is NEGATIVE (-0.316) in the raw data — strategy rescues
  a failing signal via regime conditioning, not a strong starting point
- No max drawdown reported — critical missing metric
- No CAGR reported — critical missing metric
- Frictionless ("upper bound") results only — biweekly rebalancing costs are substantial
- PCA implementation (30 factors from 3000 stocks) is not implementable as described for
  individual stock trading
- Short DJ30 and Crypto periods (Nov 2020 – Nov 2024) start at the COVID market bottom —
  extreme regime luck
- No peer review; arXiv preprint with no citations or community commentary
- Regime identification is endogenous (from return dynamics), not from independent ESG signals
- DJ30 spread has positive Sharpe (0.367) but negative 4-year cumulative return (-43.1%) —
  arithmetic mean ≠ geometric compound due to high volatility

## Confidence Score

**Per-dimension breakdown:**

| Dimension | Weight | Score | Weighted |
|-----------|--------|-------|----------|
| Logical / economic soundness (falsifiable) | 3 | 4 | 12 |
| Transparency & reproducibility | 2 | 5 | 10 |
| Evidence auditability | 2 | 3 | 6 |
| Risk management quality | 2 | 4 | 8 |
| Cost & capacity realism | 2 | 1 | 2 |
| Honest treatment of drawdowns / failure | 1.5 | 2 | 3 |
| Robustness evidence (OOS / walk-forward) | 1.5 | 3 | 4.5 |
| Survived independent scrutiny | 1 | 1 | 1 |

Weighted sum: 46.5 / 150 × 100 = **latent 31**

Evidence auditability = 3/10 (≤ 4) → verification cap = 59
**confidence = min(31, 59) = 31**

**latent 31 (capped to 31; cap does not bind — evidence auditability ≤ 4 caps at 59; latent already below cap)**

Band: **Low Confidence** (0–39)

NOT an implementation candidate (latent 31 < 60; PCA implementation not directly codeable
as individual-stock long-short without additional specification; no CAGR/drawdown reported).

## Complexity / Scalability / Automation Feasibility

High complexity. Requires:
- ARMA-GARCH modeling on a large equity universe
- PCA decomposition to 30 factors (Russell 3000)
- Hidden Markov Model / EM algorithm for regime estimation
- Bloomberg Terminal data access (not publicly available)
- Custom implementation of STARR / Rachev portfolio optimization

Not suitable for retail automation. Academic research prototype only.

## MQL5 / Python / Pine Feasibility

- **Python**: Possible in principle (statsmodels, pykalman, hmmlearn) but requires Bloomberg
  data and substantial development work. Not a retail-accessible strategy.
- **Pine Script**: Not feasible — regime detection at this complexity is not Pine-compatible.
- **MQL5**: Not feasible without Bloomberg data feed.

## Similar Strategies

- `currency-momentum-factor-menkhoff` — standard cross-sectional FX momentum; this paper's
  framework could theoretically extend to FX with ESG/policy regimes.
- `maxing-out-reversal-max-stocks` — another regime-conditioned reversal within momentum
  framework (condition = MAX daily return in prior week); similar structure, higher evidence quality.
- `cmm-daily-return-weighted-momentum` — sparse ML weighting of momentum components; also
  addresses momentum enhancement via selective conditioning.
- `regime-switching-jump-model-equity` — cleaner regime identification methodology (Statistical
  Jump Model); stronger evidence; higher score (68).

## Red Flags

- **Costs omitted** — explicitly flagged as "frictionless upper bound"
- **Momentum spread Sharpe is negative** in the primary test universe (Russell 3000: -0.316)
- **No max drawdown or CAGR reported** — critical missing metrics
- **Short data periods** for DJ30 and Crypto with extreme regime bias (COVID recovery start)
- **Regime identification endogenous** — estimated from same return data used to measure performance
- **PCA-based implementation** — not directly implementable as individual-stock strategy
- **Positive Sharpe / negative cumulative return** in DJ30 spread — arithmetic-geometric divergence
  signals extremely high strategy volatility during 4-year period

## Source Links

| URL | Retrieved | Notes |
|-----|-----------|-------|
| https://arxiv.org/abs/2505.24250 | 2026-06-29 | Abstract and metadata |
| https://arxiv.org/html/2505.24250v1 | 2026-06-29 | Full paper HTML; primary data source |

## Analyst Notes

**FACTS (from source):**
- Paper: "Winners vs. Losers: Momentum-based Strategies with Intertemporal Choice for ESG
  Portfolios" — arXiv 2505.24250 (May 2025). No author names appear in available HTML version.
- Three universes: Russell 3000 (Feb 2017–Feb 2025), DJ30 (Nov 2020–Nov 2024), 30 largest
  crypto (Nov 2020–Nov 2024). Data from Bloomberg Terminal.
- 2-week/2-week formation-holding optimal across all tail-risk metrics.
- Market price of risk decomposed: λ₀ (baseline) + λ₁ (ESG regime) via ARMA(1,1)-GARCH(1,1).
- Markov chain: Winners P(pro→pro) = 0.18 (very transient), Losers P(pro→pro) = 0.72 (stable).
- Key finding: ESG loser Sharpe > ESG winner Sharpe in Russell 3000 (0.973 vs 0.657) and crypto (0.491 vs 0.450).
- 4% turnover constraint imposed; no slippage/bid-ask modeled.

**ANALYSIS:**
- This paper identifies a real but likely ephemeral pattern: ESG losers outperformed winners
  2017-2025 because of the 2021-2022 energy price supercycle, not because of a structural
  ESG-sentiment mechanism.
- The ARMA-GARCH + Markov chain framework is well-established in academic finance, but
  applying it to ESG regime detection creates circularity: you detect the ESG regime from
  returns, then use it to explain returns.
- The PCA-based implementation is a significant barrier to practical deployment. Most
  practitioners would need to trade individual stocks, which requires additional specification
  not provided.
- Score 31 (Low Confidence) reflects: interesting idea + multi-market testing + rolling OOS,
  offset by: negative primary market Sharpe, no costs, no drawdown, endogenous regime,
  short biased periods, and zero community scrutiny.

**ASSUMPTIONS:**
- The Markov chain parameters were estimated over the full Russell 3000 sample (implied), not
  on rolling hold-out windows — this likely overstates regime identification accuracy.
- The "pro-ESG" and "anti-ESG" regimes anchored to known regulatory events may introduce
  look-ahead bias in parameter estimation if the transitions were chosen after observing returns.

## Future Research Needed

- Independent OOS test on data post-February 2025 using publicly available ESG policy event
  calendar to identify regimes out-of-sample.
- Cost-inclusive implementation: explicit bid-ask, borrow cost for shorts, market impact.
- Reformulate as individual-stock long-short (not PCA) to enable practical backtesting.
- Test on non-US equities (Europe, Asia) where ESG regulation has followed a different path.
- Extend to longer history (pre-2017) using factor-based ESG proxies.
- Decompose performance attribution: how much comes from ESG regime conditioning vs. from
  the energy sector rebound (2021-2022)?
