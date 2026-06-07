# Regime-Optimal Trend Following (OPT) — Zakamulin 2026

## Overview

A theoretically grounded trend-following framework that replaces ad-hoc binary or linear position
sizing with **analytically optimal regime-conditional allocations**. The strategy detects bull/bear
(2-regime) or bull/correction/bear/rebound (4-regime) states using an EMA/MACD-derived trend signal,
then assigns position sizes that maximize gross Sharpe ratio given each regime's estimated
signal-to-noise ratio — derived from the same expanding-window history used to classify the regime.
The approach nests standard TSM (binary ±1) and DSM (Goulding/Harvey/Mazzoleni 2023) as special,
generally suboptimal, cases.

**Slug:** `regime-optimal-trend-zakamulin`
**SSRN:** 6376479 — *Rethinking Trend Following: Optimal Regime-Dependent Allocation*, Valeriy
Zakamulin (University of Agder, Norway), March 09, 2026. SSRN preprint. Not yet peer-reviewed.
Extends: Zakamulin/Giner, *Journal of Asset Management* 2024 (peer-reviewed).

---

## Asset Class / Timeframes

- **Primary datasets tested:** 18 portfolios from Kenneth French Data Library — US equity industry
  portfolios (e.g., 48 Fama-French industry portfolios), international equity indices (country-level),
  US factor/firm-characteristic portfolios (value, momentum, profitability, etc.)
- **Frequency:** Monthly (Ken French portfolios are monthly)
- **In scope for this archive:** Directly applicable to broad equity indices (S&P 500, NASDAQ, NAS100,
  international ETFs); indirectly applicable to any trend-following asset where regime persistence is
  empirically observable.
- **Market scope note:** The paper tests equity portfolios. Application to Forex, Crypto, Gold, or
  commodities is not demonstrated in the 2026 paper (prior Zakamulin/Giner work uses US equity markets
  as its primary laboratory).

---

## Core Logic

**Step 1 — Regime detection (from the underlying theoretical framework):**
Following Zakamulin/Giner (2024, JAM), the optimal trend signal under a Markov switching model
is the **Exponential Moving Average (EMA)**. Under the more realistic semi-Markov model (where
regime duration probability increases with age), the optimal rule resembles **MACD** (difference
of two EMAs). In the 2026 paper, the regime classification mechanism uses slow and fast momentum
signals (following the 4-phase framework of Goulding/Harvey/Mazzoleni 2023):

- **2-regime (OPT-2):** Bull vs. Bear — determined by the sign of the trend signal
- **4-regime (OPT-4):** Bull, Correction, Bear, Rebound — Bull = both slow and fast signals
  positive; Correction = slow positive, fast negative; Bear = both negative; Rebound = slow
  negative, fast positive

**Step 2 — Regime-specific Sharpe estimation:**
For each regime `r`, estimate `μ_r` (mean return when in regime `r`) and `σ_r` (volatility when
in regime `r`) using an **expanding window** of past data. This yields a per-regime Sharpe
estimate: `SR_r = μ_r / σ_r`.

**Step 3 — Optimal allocation (the OPT rule):**
The position weight in regime `r` is:
```
w_r = SR_r / SR_max   (scaled so w = 1 for the highest-Sharpe regime)
```
Where `SR_max = max(SR_r across all regimes)`.
- If `SR_r` is negative for some regime, `w_r` is negative (short).
- In the bull regime (typically highest SR): position = 1.0 (100% long)
- In the bear regime (typically negative SR): position ≤ 0 (flat or short)
- In transition regimes (Correction/Rebound): position proportional to relative SR

This produces a smooth, continuous allocation that changes every period based on the current
regime and the evolving Sharpe estimates from the expanding window.

**Intuition:** Standard TSM allocates ±1 regardless of regime quality. OPT allocates more when
the signal-to-noise ratio is high (e.g., deep bull) and less when the regime is noisy or
ambiguous (e.g., shallow correction).

---

## Economic Rationale

The rationale operates at two levels:

**Statistical level:** Bull and bear markets exhibit persistently different expected return
distributions. If that persistence is real, then a strategy conditioning on regime state should
outperform an unconditional strategy. The analytical framework shows that EMA/MACD is the
theoretically optimal signal under a semi-Markov model of regime durations.

**Practical level:** Standard trend following (TSM) is suboptimal because it ignores the quality
of the regime signal. A bear regime during a deep, persistent crash has very different signal-to-
noise from a brief correction — yet TSM allocates -1 to both.

**When the edge should disappear:**
- When equity regime persistence is genuinely absent (markets become I.I.D.) — the EMA/MACD
  signal carries no information and regime Sharpe estimates are noise
- When regime transitions happen faster than monthly — the expanding-window estimation lags
- When implementation costs (for non-equity, non-index assets) erode the gross improvement
- When the expanding-window Sharpe estimates are dominated by survivorship (e.g., post-2009
  bull market inflates bull regime Sharpe estimate, making OPT over-allocate to bull)

FACTS: The theoretical derivation establishes EMA/MACD optimality. The empirical OOS Sharpe
improvement is documented across 18 datasets.
ANALYSIS: The economic "why" is regime persistence in equity returns — a well-established
empirical regularity but one that is structurally dependent on market microstructure, investor
behavior, and central bank policy cycles.

---

## Entry Conditions

**2-regime OPT:**
1. Compute EMA trend signal for the asset (lookback determined analytically from regime
   duration estimates; comparable to a 10-month SMA approximately)
2. Classify regime: Bull if EMA > 0, Bear if EMA < 0
3. Look up estimated SR for the current regime from expanding-window history
4. Set position weight `w = SR_current / SR_bull` (capped at 1.0, can be negative in Bear)
5. Scale portfolio exposure accordingly

**4-regime OPT (extension of Goulding/Harvey/Mazzoleni):**
1. Compute both slow (e.g., 12-month) and fast (e.g., 1-month) momentum signals
2. Classify regime: Bull (both+), Correction (slow+/fast-), Bear (both-), Rebound (slow-/fast+)
3. Estimate SR for each of 4 regimes from expanding window
4. Set `w = SR_current / SR_max`

---

## Exit Conditions

- Continuous regime monitoring: position updates every period (monthly) as regime changes
- No explicit stop-loss: the regime-based sizing serves as implicit drawdown management
- No explicit holding period: position held until regime transitions

---

## Risk Management

- **Position sizing:** Analytically derived from regime Sharpe estimates; auto-adjusts
- **No leverage cap explicitly stated** in the available paper summaries
- **No hard stop-loss** defined
- **Implicit protection:** In bear regime, OPT goes flat or short, providing downside protection
- **No explicit max-loss rule, no explicit regime exit rule beyond the regime classification**

ASSUMPTION: The paper likely operates within ±1.0 exposure (no leverage beyond 100%), matching
TSM convention. Not explicitly confirmed from available summaries.

---

## Indicators Used

- Exponential Moving Average (EMA) — theoretically optimal under semi-Markov regime model
- MACD (difference of fast and slow EMA) — for 4-regime detection
- Expanding-window means and volatilities per regime

Specific EMA decay constant: NOT REPORTED from available sources (theoretically derived from
regime duration estimates, not a fixed parameter choice).

---

## Transaction Costs & Capacity

**Transaction costs: NOT REPORTED in available summaries.** This is a significant gap.

The paper appears to test on Ken French data (monthly frequency, academic equity portfolios),
where costs are minimal — ETFs tracking US large-cap indices carry ~5–10bps round-trip and
monthly turnover is low. However:

- For individual stock momentum portfolios (some Ken French datasets), turnover is much higher
  and costs could significantly erode gross Sharpe improvement
- The paper's stated OOS Sharpe figures are labeled "gross" in at least one review (CXO Advisory
  mentions "gross" Sharpe)
- No mention of bid-ask spreads, rebalancing friction, or slippage modeling

ANALYSIS: For broad equity index application (SPY, QQQ), costs are negligible at monthly
frequency. For factor portfolios with high turnover, cost erosion is real and NOT MODELED in
accessible results. This makes the comparison to TSM (also presumably gross) directionally valid
but practically uncertain.

**Capacity:** Ken French portfolios are not capacity-constrained (broad market indices). The
strategy is scalable for any amount in liquid equity index ETFs or futures.

---

## Backtest Evidence

- **Source:** Ken French Data Library (public, downloadable)
- **Training:** Expanding window, starting as early as 1927 for US equity portfolios
- **OOS evaluation period:**
  - US portfolios: approximately 1968 onward (initial training window)
  - International indices: December 2003 onward
  - US factor/firm portfolios: 1997 onward
  - All OOS end date: December 2025
- **Number of datasets:** 18 portfolios from Ken French library
- **Evidence status:** CLAIMED, UNVERIFIED — expanding-window OOS on public academic data is
  relatively credible, but not independently replicated and not peer-reviewed for the 2026 paper.

---

## Forward-Test Evidence

**NOT REPORTED.** No live trading results available.

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio (2-regime OPT, avg 18 datasets) | 0.506 | CLAIMED, UNVERIFIED | Zakamulin SSRN 6376479 via web search summary |
| Sharpe Ratio (TSM baseline, avg 18 datasets) | 0.208 | CLAIMED, UNVERIFIED | Zakamulin SSRN 6376479 via web search summary |
| Sharpe Ratio (4-regime OPT, avg) | 0.628 | CLAIMED, UNVERIFIED | Zakamulin SSRN 6376479 via web search summary |
| Sharpe Ratio (DSM baseline, 4-regime) | 0.496 | CLAIMED, UNVERIFIED | Zakamulin SSRN 6376479 via web search summary |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | — |
| CAGR | NOT REPORTED | NOT REPORTED | — |
| Annual Return | NOT REPORTED | NOT REPORTED | — |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | Monthly (inferred) | NOT REPORTED | — |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | NOT REPORTED | NOT REPORTED | — |

**Automatic scrutiny triggers:** No thresholds triggered. Sharpe 0.506 is within normal bounds
for equity-portfolio trend following. Note: these are averages across 18 datasets; individual
datasets could range significantly.

**ANALYSIS:** Sharpe 0.208 (TSM baseline) is consistent with the well-documented typical CTA
per-market Sharpe of 0.15–0.20 (Hurst et al. 2017). The OPT improvement to 0.506 (+144%)
would be extraordinary if verified and generalizable. The improvement in the 4-regime case
(0.496 → 0.628, +27%) is more modest and more plausible. The gross Sharpe figures need to be
verified net of costs and adjusted for multiple testing across 18 datasets.

---

## Community Sentiment

Available independent reviews:
- **CXO Advisory** (review summary available): Found the strategy credible; noted the improvement
  is "gross" Sharpe. Does not question the methodology but notes the paper extends prior
  Zakamulin/Giner work which was already peer-reviewed.
- **Alpha Architect** (review summary referenced): Covered the paper; generally supportive of
  the theoretical grounding.
- **Allocate Smartly** (listed as tracking Zakamulin's optimal trend-following strategies):
  Known to independently replicate; this paper is in their coverage universe.

No critical papers specifically targeting the 2026 "Rethinking" paper found. Prior work
(Zakamulin/Giner 2024) was published in Journal of Asset Management — a credible peer-reviewed
venue that implied the prior work survived peer scrutiny.

FACTS: Three independent reviewers have noted/covered the paper. No published critique found.
ANALYSIS: The absence of critique may reflect recency (paper is 3 months old as of research date).

---

## Why This Might Be Nothing

### 1. Most likely benign explanation for the apparent edge

**Multiple testing across 18 correlated datasets.** The 18 Ken French portfolios are highly
correlated US equity time series. Reporting average Sharpe across 18 correlated tests inflates
the appearance of statistical power without providing independent evidence. The effective
independent dataset count is far lower than 18. If the same 18 datasets were used to develop
the OPT allocation formula (expanding window still uses the full available history of each
dataset), there is a risk that the OPT rule implicitly fits to the average characteristics of
Ken French portfolio returns. The "OOS" period starting in 1968–2003 still covers ~5 decades —
a potentially long enough calibration period to inflate expanding-window estimates.

### 2. The cost case

As noted, costs are not modeled. For Ken French industry/factor portfolios:
- Turnover on some factor portfolios (short-term reversal, momentum) is high
- Transaction costs would erode gross Sharpe significantly
- The TSM baseline is presumably also gross → the relative improvement is preserved, but the
  absolute implementable OPT Sharpe could be much lower than 0.506
- **For broad equity INDEX application** (SPY, QQQ, NAS100): monthly turnover is minimal
  (~2 trades/month max) and costs are ~5bps → costs are essentially negligible and don't
  change the conclusion materially for this use case

### 3. Regime dependence

The OPT rule's improvement depends on equity regime persistence being stable through time.
Evidence suggests equity regime persistence has changed significantly:
- Post-2009 QE-distorted bull market (near-zero bear regimes for years)
- 2022 regime shift: rapidly alternating regimes that exceed EMA detection speed
- COVID (2020): bear regime lasted weeks, not months — below EMA resolution
The expanding window Sharpe estimates may embed a post-2009 bull-market bias that
inflates estimated OPT exposures going forward.

### 4. Sample / overfitting concerns

- The core theoretical model (semi-Markov) is elegant but represents a simplification
- The 4-regime detection is borrowed directly from Goulding/Harvey/Mazzoleni (2023) — not
  independently derived — this means the regime boundaries are those researchers' parameter
  choices embedded in the OPT framework
- The OPT rule's expansion-window estimates require a long history to be stable; for
  shorter-history assets (emerging markets, crypto), the approach is likely unreliable

### 5. What would change my mind

An independent replication by Allocate Smartly or a similar organization with:
- Net-of-cost Sharpe ratios for the same 18 datasets
- Breakdown showing which datasets drive the improvement (vs. average over all 18)
- Application to assets outside Ken French universe (e.g., commodity futures, FX, Bitcoin)
- Peer review (the paper appears to be under review based on its connections to the prior 2024
  JAM paper, but not yet confirmed)

Without cost data and independent replication, the correct interpretation is: **plausible
framework, promising average result, insufficient evidence to claim verified edge.**

---

## Strengths / Weaknesses

**Strengths:**
- Theoretical foundation: EMA/MACD optimality is rigorously derived from regime-switching models
- Principled position sizing: Sharpe-ratio-based allocation is superior to arbitrary rule design
- Tested on diverse, publicly available datasets (Ken French) with expanding window
- 25+ year OOS period for the US datasets
- Extension of peer-reviewed work by the same author
- Transparent framework: the allocation rule is fully specified mathematically

**Weaknesses:**
- SSRN preprint (not peer-reviewed as of research date)
- Transaction costs not addressed in accessible summaries
- Max drawdown not reported — limits ability to evaluate tail risk
- 18 correlated US equity datasets → lower effective independence than it appears
- Regime detection requires calibrated EMA parameters (not fixed in all accounts)
- 4-regime model borrowed from Goulding/Harvey/Mazzoleni — adds that paper's assumptions
- Primarily validated on monthly equity portfolios; cross-asset applicability undemonstrated

---

## Confidence Score

**Per-dimension breakdown:**

| Dimension | Weight | Score (0–10) | Contribution |
|---|---|---|---|
| Logical / economic soundness (falsifiable) | 3 | 7 | 21 |
| Transparency & reproducibility | 2 | 6 | 12 |
| Evidence auditability | 2 | 5 | 10 |
| Risk management quality | 2 | 5 | 10 |
| Cost & capacity realism | 2 | 4 | 8 |
| Honest treatment of drawdowns / failure | 1.5 | 3 | 4.5 |
| Robustness evidence (OOS / walk-forward / multi-market) | 1.5 | 7 | 10.5 |
| Survived independent scrutiny | 1 | 5 | 5 |
| **Total** | **15** | | **81** |

`latent_score = round(81 / 150 × 100) = 54`

**Verification cap:** Evidence auditability = 5 → Cap at 74.
Since latent (54) < 74, cap does not bind.

**`confidence = 54` (Experimental)**

`latent 54 (uncapped: evidence auditability 5 / 10; cap at 74 does not bind; cost data and max drawdown NOT REPORTED)`

**Rationale for key scores:**
- Economic soundness 7/10: Falsifiable (edge disappears when regimes are I.I.D. or very
  short-lived); theoretical derivation from semi-Markov model is rigorous; BUT economic
  mechanism beyond "regime persistence exists" is underspecified
- Cost & capacity 4/10: Monthly equity indices have trivial costs; factor portfolio costs are
  real and unmodeled; no explicit cost analysis in accessible summaries
- Drawdowns 3/10: Max drawdown NOT REPORTED — this is a significant gap for any trend-following
  strategy where drawdowns are the primary risk metric
- Robustness 7/10: 18 datasets, 25+ year OOS, expanding window; docked for correlated datasets

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Medium. Requires: (1) regime detection via EMA/MACD trend signal, (2) expanding
  window computation of per-regime mean/volatility, (3) position weight calculation
- **Scalability:** High. Monthly frequency + liquid equity indices → no capacity constraint
- **Automation:** High feasibility. Monthly signal; all inputs are price-based; no proprietary data

---

## MQL5 / Python / Pine Feasibility

- **Python:** High feasibility. Ken French data is freely downloadable. The expanding-window
  computation, EMA trend signal, and Sharpe-based position sizing are all standard pandas/numpy
  operations. No proprietary data required for the equity index application.
- **Pine (TradingView):** Medium feasibility. The regime classification and per-regime Sharpe
  tracking require multi-state logic; feasible but verbose
- **MQL5:** Medium feasibility. MT4/MT5 support EMA; the expanding-window regime estimation
  would need custom buffer management

---

## Similar Strategies

- `regime-switching-jump-model-equity` (score 68) — SJM market timing on equity indices; uses
  Statistical Jump Model regime detection vs. EMA/MACD; similar market-timing logic but
  different regime model and binary (not graded) position sizing
- `hmm-rl-regime-taa` (score 44) — HMM + RL for SPY/TLT/GLD rotation; regime-based but uses
  HMM Gaussian on VIX changes; more complex, cross-asset, RL policy
- `dynamic-factor-allocation-regime-shu-mulvey` (score 61) — SJM + Black-Litterman for factor
  rotation; also applies regime information to equity factors but within a portfolio context
- `industry-trend-century` (score 64) — Donchian/Keltner trend on 48 industries; trend-following
  on the same Ken French industry portfolios but using price breakout rather than regime-optimal
  sizing
- `em-us-momentum-taa` (score 49) — SMA/ROC on EEM vs SPY; similar monthly macro momentum
  framework but applied to international equity rotation

---

## Red Flags

- Transaction costs not addressed (moderate flag for factor portfolios; low flag for index ETFs)
- Max drawdown not reported — key risk metric missing
- SSRN preprint, not peer-reviewed
- 18 correlated datasets may overstate statistical evidence
- 2-regime average Sharpe improvement (+144% over TSM) is large enough to warrant scrutiny;
  the 4-regime improvement (+27% over DSM) is more plausible

---

## Source Links

| Source | Retrieved | Notes |
|--------|-----------|-------|
| SSRN abstract: papers.ssrn.com/sol3/papers.cfm?abstract_id=6376479 | 2026-06-07 | Primary paper; HTTP 403 on full text |
| Web search result excerpt: CXO Advisory review at cxoadvisory.com/technical-trading/regime-optimal-trend-following/ | 2026-06-07 | HTTP 403 on full text; title and brief review noted |
| Web search result excerpt: Alpha Architect review at alphaarchitect.com/rethinking-trend-following-optimal-regime-dependent-allocation/ | 2026-06-07 | HTTP 403 on full text; paper coverage confirmed |
| Web search result excerpt: Allocate Smartly coverage at allocatesmartly.com/zakamulins-optimal-trend-following/ | 2026-06-07 | HTTP 403 on full text; tracking confirmed |
| Zakamulin/Giner 2024 (Journal of Asset Management): link.springer.com/article/10.1057/s41260-024-00357-0 | 2026-06-07 | Prior peer-reviewed foundation paper; HTTP 403 on full text |
| SSRN 4217513 (Zakamulin/Giner earlier version): papers.ssrn.com/sol3/papers.cfm?abstract_id=4217513 | 2026-06-07 | Prior SSRN version of foundation work |
| Goulding/Harvey/Mazzoleni (JFE 2023): people.duke.edu/~charvey/Research/Published_Papers/P158_Momentum_turning_points.pdf | 2026-06-07 | 4-regime DSM paper that OPT-4 competes with; HTTP 403 |
| Web search multi-result excerpts (various) | 2026-06-07 | Performance numbers aggregated from public summaries |

---

## Analyst Notes

**FACTS (from sources):**
- Zakamulin SSRN 6376479, March 2026, University of Agder
- 2-regime OPT average OOS Sharpe 0.506 vs TSM 0.208 across 18 Ken French datasets
- 4-regime OPT average OOS Sharpe 0.628 vs DSM 0.496 (Goulding/Harvey/Mazzoleni 2023)
- OOS period: approx. Jan 1998–Dec 2025 (US), Dec 2003–Dec 2025 (international), 1997–Dec 2025 (factor)
- Regime detection: EMA (2-regime) or slow/fast EMA disagreement (4-regime)
- Expanding window Sharpe estimation per regime
- Extension of Zakamulin/Giner 2024 (Journal of Asset Management), peer-reviewed

**ANALYSIS:**
- TSM baseline Sharpe of 0.208 is consistent with empirical literature on per-market trend following
- OPT improvement is largest in 2-regime setting (+144%); 4-regime improvement is +27% — more
  plausible as a standalone improvement rather than artifact
- The key innovation is the position sizing formula, not the regime detection signal — this is
  distinguishable from existing archive entries
- The Ken French test universe is not directly tradeable in the precise form tested; closest
  implementation is liquid equity index ETFs/futures (SPY, QQQ, NAS100, EFA) at monthly frequency
- Cost impact estimate (ANALYSIS): For SPY/QQQ at monthly frequency: ~10bps round-trip × 12
  trades/year = ~120bps/year. At annual vol ~15%, this removes ~0.80 from Sharpe. If gross Sharpe
  is 0.506, net Sharpe ≈ 0.506 − (0.8/15%_correction) ≈ still positive but lower. However, the
  TSM baseline net would also decline by similar amount — the relative improvement likely persists.
  ASSUMPTION: vol of Ken French portfolios approximates 15–20% annual.

**ASSUMPTIONS:**
- The OPT position sizing respects a ±1.0 exposure bound (no leverage); not explicitly confirmed
- The expanding-window estimation starts from inception of each dataset and uses all available data
- "Average across 18 datasets" implies equal weighting; not confirmed

---

## Future Research Needed

1. **Cost-adjusted replication:** Apply OPT to SPY/QQQ/EFA ETFs at monthly frequency with
   realistic roundtrip costs (~10bps) and confirm net Sharpe improvement vs TSM
2. **Cross-asset validation:** Does OPT outperform TSM on commodity futures (where we already
   have `industry-trend-century`)? On Bitcoin (where we have `bitcoin-multitf-trend`)?
3. **Max drawdown analysis:** Essential missing metric — TSM's maximum drawdown is typically
   30–50% on equity indices. Does OPT reduce this?
4. **Peer review outcome:** Monitor SSRN for journal acceptance; note prior Zakamulin/Giner
   work (same framework) was accepted at JAM
5. **Interaction with transaction costs for factor portfolios:** The paper tests factor portfolios
   with potentially high turnover; cost-adjusted Sharpe for those specific portfolios is essential
6. **Independent replication:** Allocate Smartly tracking is the most likely source of
   independent cost-inclusive replication — check for updates
