# Causal Inference Directional Trading Framework

## Overview

A multi-stage causal inference pipeline applied to a tiny stock universe (3 US equities)
over a 2-month backtest window. The framework uses Gaussian Mixture Models to cluster stocks
by volatility profile, then applies three sequential causal detection methods (Granger
causality, PCMCI, Effective Transfer Entropy) to identify lead-lag pairs, and uses DTW +
KNN to determine trade timing. Claims 100% win rate on certain pairs. The 2-month test
period renders all reported metrics statistically meaningless.

**VERDICT: LOW CONFIDENCE — effectively a null result dressed as a positive result. The
test period (~65 trading days, 3 stocks) is far too small to support any statistical claim.**

- **Authors:** NOT REPORTED (arXiv preprint)
- **Source:** arXiv:2507.09347 (July 12, 2025, preprint — not peer-reviewed)
- **Backtest period:** June 8, 2023 – August 12, 2023 (~65 days)

## Asset Class / Timeframes

- **Market:** US Equities (QCOM, TSLA, AMZN — 3 stocks from a 9-stock universe)
- **Timeframe:** Intraday (exact resolution NOT REPORTED)
- **Style:** Lead-lag arbitrage / directional trading

## Core Logic

**Step 1 — Universe construction:**
9 stocks are clustered using Gaussian Mixture Models (GMM) based on their mid-range
historical volatility profiles over a 3-year lookback. The 9 stocks and their selection
criteria are NOT REPORTED.

**Step 2 — Causal inference pipeline (three sequential layers):**
1. Granger Causality Test (GCT) — linear Granger causality to identify which stocks
   predict others at each lag.
2. PCMCI (Peter-Clark Momentary Conditional Independence) — conditional independence test
   to remove spurious Granger links, identifying true "momentary" causal links net of
   confounders.
3. Effective Transfer Entropy (ETE) — information-theoretic measure of directional
   information flow; adds a nonlinear component to the pipeline.

**Step 3 — Trade execution:**
Dynamic Time Warping (DTW) combined with K-Nearest Neighbours (KNN) classifier determines
the optimal lag for trade execution. Exact entry/exit rules: NOT REPORTED.

**Net output:** A set of causal pairs (stock A leads stock B) with a timing lag. Trade
by following A's move with B at the identified lag.

## Economic Rationale

Lead-lag relationships between related equities have legitimate economic grounding:
information diffusion, supply chain linkages, shared macro sensitivities, and institutional
cross-asset flow can create predictable short-term lead-lag patterns. This is the basis of
statistical arbitrage in pairs trading.

However, **the rationale fails the falsifiability test in this paper:** no conditions are
stated under which the identified causal links should disappear. In a 2-month window on 3
stocks, GMM + GCT + PCMCI + ETE + DTW + KNN collectively have far more parameters than
the data can constrain. Any pattern found is virtually guaranteed to be in-sample curve-fitting.

## Entry Conditions

NOT REPORTED (specific entry trigger, position direction, size: all NOT REPORTED).

## Exit Conditions

NOT REPORTED.

## Risk Management

NOT REPORTED. No stop-loss, position limit, or drawdown management described in available
sources.

## Indicators Used

- GMM volatility clustering (3-year lookback)
- Granger Causality Test
- PCMCI conditional independence test
- Effective Transfer Entropy (ETE)
- Dynamic Time Warping (DTW)
- K-Nearest Neighbours (KNN) classifier

## Transaction Costs & Capacity

NOT REPORTED. No mention of bid-ask spread, commissions, slippage, or market impact in any
available source. Given the lead-lag framework's dependence on short time lags, slippage
would be a material cost — and its omission is an additional reason to distrust the results.

## Backtest Evidence

**CLAIMED, UNVERIFIED:** June 8 – August 12, 2023 (~65 calendar days, ~45 trading days).
This is not a backtest in any statistically meaningful sense — it is a proof-of-concept run
on a 2-month window. No OOS test exists; the 2-month window IS the entire evaluation.

## Forward-Test Evidence

NOT REPORTED.

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | Up to 2.17 (best pair) | CLAIMED, UNVERIFIED | arXiv:2507.09347 |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | Up to 100% (certain pairs) | CLAIMED, UNVERIFIED | arXiv:2507.09347 |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | — |
| CAGR | NOT REPORTED | NOT REPORTED | — |
| Annual Return | 15.38% total return (2-month period) | CLAIMED, UNVERIFIED | arXiv:2507.09347 |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | NOT REPORTED | NOT REPORTED | — |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | NOT REPORTED | NOT REPORTED | — |

**Benchmark:** Buy-and-Hold returned 10.39% over the same 2-month period.

**Critical note on "up to 100% win rate":** This means the *best-performing pair* (selected
post-hoc) had 100% win rate. Reporting the maximum of a cherry-picked subset is not a
meaningful performance statistic. The win rates of all pairs over the full evaluation would
be the relevant figure — NOT REPORTED.

**Critical note on 15.38% total return:** This is a 2-month return. Annualizing it would
yield ~90%+ CAGR, which is not stated but is deeply implausible on a sustainable basis.

## Community Sentiment

No community discussion found for this preprint (July 2025). No forum threads, replications,
or critical reviews identified.

## Why This Might Be Nothing

**1. Fatally small test period (primary reason):**
June 8 – August 12, 2023 is approximately 45 trading days on 3 stocks. Statistical tests
at this sample size have essentially no power to distinguish genuine signal from noise.
A "100% win rate on certain pairs" across ~45 days is almost certainly the result of
data-mining — the pipeline has enough free parameters (GMM components, Granger lag order,
PCMCI parameters, ETE bandwidth, DTW window, KNN k) to fit any 45-day window on any 3 stocks.

**2. Selective reporting of the best pair:**
"Up to 100% win rate" means the best-performing pair achieved 100%. The distribution of
win rates across all pairs, and the performance of a portfolio of all identified causal
pairs, are NOT REPORTED. This is textbook cherry-picking.

**3. No out-of-sample testing:**
The 2-month window is simultaneously the training data for the causal pipeline (which uses
3 years of historical vol for GMM clustering) and the backtest. Without a separate hold-out
period, there is no evidence of generalization.

**4. Transaction costs absent:**
Lead-lag strategies based on intraday execution require strict cost accounting. Even at
institutional spreads, a short-lag arbitrage strategy is likely negative after costs on
3 actively-traded large-cap tech stocks, which are among the most informationally efficient
equities in the world.

**5. Most likely benign explanation:**
Random statistical noise over 45 days + enough model parameters + reporting the best
outcome from a search over pairs = apparent 100% win rate with no persistent edge. This
is multiple comparisons on a tiny sample.

**What would change my mind:** An independent replication of the causal pipeline on a
new 12-month hold-out period, applied to all pairs (not just the best), with realistic
transaction costs modeled, on a broader universe of at least 30 stocks.

## Strengths / Weaknesses

**Strengths:**
- The causal inference methodology (GCT + PCMCI + ETE) is academically sophisticated
- Combining multiple causal tests to reduce spurious links is a reasonable design principle
- Lead-lag arbitrage between equities has legitimate theoretical grounding

**Weaknesses:**
- 2-month backtest on 3 stocks: fatally insufficient
- 100% win rate is a red flag, not a positive result
- No OOS testing; no transaction costs; no risk management documented
- Not peer-reviewed; no open code
- Vast parameter space relative to data — virtually guaranteed overfitting

## Confidence Score

**Per-dimension breakdown:**

| Dimension | Raw (0–10) | Weight | Weighted |
|-----------|-----------|--------|----------|
| Logical/economic soundness (falsifiable) | 3 | 3 | 9 |
| Transparency & reproducibility | 3 | 2 | 6 |
| Evidence auditability | 1 | 2 | 2 |
| Risk management quality | 1 | 2 | 2 |
| Cost & capacity realism | 1 | 2 | 2 |
| Honest treatment of drawdowns | 1 | 1.5 | 1.5 |
| Robustness evidence (OOS / walk-forward) | 0 | 1.5 | 0 |
| Survived independent scrutiny | 0 | 1 | 0 |

Weighted sum = 9 + 6 + 2 + 2 + 2 + 1.5 + 0 + 0 = **22.5**
Max possible = 150
`latent_score = round(22.5 / 150 × 100) = round(15) = **15**`

**Verification cap:** Evidence auditability = 1 (≤ 4) → cap at 59.
`confidence = min(15, 59) = **15**`

**Band: Low Confidence (0–39)**

`latent 15 (capped to 15: 2-month backtest on 3 stocks; 100% win rate = curve-fitting; no OOS; no costs; arXiv preprint)`

## Complexity / Scalability / Automation Feasibility

**Complexity:** High — six distinct methodological components (GMM, GCT, PCMCI, ETE, DTW,
KNN). All are standard Python libraries but require significant calibration.

**Not recommended for implementation** given the evidence quality.

## MQL5 / Python / Pine Feasibility

**Python:** Implementable with standard libraries (`statsmodels`, `tigramite` for PCMCI,
`pyinform` for ETE, `dtaidistance` for DTW, `scikit-learn` for KNN). However, implementing
this without evidence of OOS validity would be a wasted effort.

**Not recommended until genuine evidence of edge exists.**

## Similar Strategies

- Conceptually related to `attention-factors-statistical-arbitrage` (lead-lag in equities)
  and `network-momentum-trend-following` (cross-market causal network), both of which have
  substantially stronger evidence bases.

## Red Flags

- `INSUFFICIENT VERIFICATION`: 2-month backtest (~45 trading days) on 3 hand-picked stocks
- `CURVE-FITTING`: 100% win rate on selected pairs from a large causal detection parameter space
- `SELECTIVE REPORTING`: "Up to 100% win rate" = cherry-picked best pair, not full distribution
- `COSTS OMITTED`: No transaction cost modeling for intraday lead-lag strategy
- `NO OOS`: 2-month window is both fit period and evaluation period
- `NO RISK MANAGEMENT`: No stops, no position limits documented

## Source Links

| Source | Retrieved | Notes |
|--------|-----------|-------|
| arXiv:2507.09347 abstract | 2026-05-29 | Primary source; HTTP 403 for full fetch |
| Search snippets arXiv:2507.09347 | 2026-05-29 | Backtest dates, win rate, total return, Sharpe extracted |

## Analyst Notes

**FACTS (from search result snippets):**
- arXiv:2507.09347, submitted July 12, 2025
- Backtest period: June 8, 2023 – August 12, 2023 (~65 calendar days)
- 9 stocks clustered by GMM; QCOM, TSLA, AMZN among the universe
- Causal pipeline: GCT → PCMCI → ETE → DTW/KNN for lag
- Total return 15.38% (vs 10.39% buy-and-hold over same 2 months)
- Sharpe "up to 2.17"; win rate "up to 100%" for certain pairs

**ANALYSIS:**
- 15.38% total return in 2 months → implied annualized return ≈ 15.38% × (252/45) ≈ 86%.
  This is not reported by authors (wisely) but illustrates why a 2-month window is
  non-comparable to multi-year strategies.
- The multiple causal methods applied sequentially create a very flexible model that can
  find signal in any short time series by chance alone. With 9 stocks and multiple pairs,
  the probability of finding at least one pair with 100% win rate in 45 days is extremely
  high under the null hypothesis of no edge.

## Future Research Needed

1. Replicate on 12+ month hold-out period (2024–2025) to test OOS persistence.
2. Apply the causal pipeline to all pairs simultaneously and report the *distribution*
   of win rates, not just the maximum.
3. Model realistic transaction costs for short-lag intraday lead-lag execution.
4. Expand universe to 50+ stocks from the same sectors (technology/semiconductors) to test
   whether causal links are stable across the broader category or specific to these 3 stocks.
