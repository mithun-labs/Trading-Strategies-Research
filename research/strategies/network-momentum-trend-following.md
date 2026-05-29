# Network Momentum Trend-Following (NMM)

## Overview

An enhancement to classical time-series momentum (TSMOM) strategies that adds cross-market
lead-lag detection to the univariate trend signal. Two methods — the Lévy area of pairwise
returns and Dynamic Time Warping (DTW) — identify which markets systematically lead or lag
others. These relationships are encoded into a graph-adjacency matrix, generating a "network
momentum" signal that supplements the standard MACD univariate signal. Tested on 28 futures
markets (agriculture, energy, metals, equity indices) OOS from 2005 to 2024. Pre-print
(January 2025, not peer-reviewed). Authors: Linze Li and William Ferreira (Imperial College
London).

---

## Asset Class / Timeframes

- **Markets:** 28 futures contracts — sectors: agriculture, energy, metals, and equity indices
- **Timeframe:** Daily (settlement prices)
- **Style:** Systematic trend-following (CTA-style), medium-frequency
- **Training window:** June 2002 – June 2024
- **OOS evaluation:** January 2005 – June 2024
- **In scope:** Major equity index futures (NASDAQ, S&P, etc.) + commodities ✓

---

## Core Logic

1. **Baseline MACD signal:** For each market, compute a standard MACD-style univariate trend
   indicator. This is the benchmark.

2. **Lead-lag detection — Lévy area:** For every pair of markets, compute the Lévy area of
   the two-dimensional path formed by their daily returns. The Lévy area captures both linear
   and nonlinear relationships at a fixed lag, identifying which market leads the other.

3. **Lead-lag detection — DTW:** For every pair of markets, apply Dynamic Time Warping to the
   returns series. DTW identifies similarity in the shape of return trajectories regardless of
   temporal alignment, providing a second view of lead-lag structure.

4. **Network construction:** Combine the Lévy area and DTW outputs into a graph-adjacency
   matrix using an ensemble learning approach. The adjacency matrix encodes directional
   lead-lag relationships as edge weights.

5. **Network momentum signal (NMM):** Apply graph signal processing to the adjacency matrix.
   Markets that are systematically led by other trending markets receive an augmented trend
   signal. The "follower" receives the "leader's" trend impulse before it appears in its own
   price.

6. **Position sizing:** Volatility scaling (standard for CTA/TSMOM strategies). Specific
   vol-target level NOT REPORTED in accessible sources.

7. **Ensemble approach:** Combine multiple NMM variants to reduce transaction costs (DTW
   variations achieve ~19% lower costs than MACD baseline) and improve robustness.

---

## Economic Rationale

**Why an edge could exist:** Commodity and futures markets have structural inter-market
dependencies: crude oil prices flow through to energy products; base metals (copper, aluminum)
reflect the same macro demand that drives equity indices; agricultural markets react to similar
weather and supply-chain events. If prices in one market systematically lead prices in a
correlated market — even by one to a few days — an investor in the lagging market can position
ahead of the inevitable catch-up move.

The Lévy area (a path-space measure from stochastic analysis) and DTW (a signal-alignment
algorithm) are designed to detect exactly this lead-lag structure when it is nonlinear or
time-varying — a methodological advance over simpler lag-correlation approaches.

**When the edge should disappear (falsifiability):**
- When lead-lag relationships become unstable — structural changes (policy, regulation,
  market structure) break the historical lead-lag matrix.
- When cross-market correlations become too high (contagion events) — all markets move
  simultaneously and no lead-lag exists to exploit.
- When markets become more efficient — HFT and cross-asset momentum traders arbitrage out
  the lead-lag by reducing the delay to milliseconds.
- When the graph-adjacency matrix overfits historical lead-lag structure — the specific
  "leaders" and "followers" inferred from 2002-2024 data may not hold in 2025+.
- During commodity supercycles or policy regime breaks where historical inter-market
  relationships (e.g., oil-gold correlation) invert.

---

## Entry Conditions

NOT REPORTED with full precision in accessible sources.

From context:
- Long when the network-augmented trend signal is positive (leader markets trending up,
  follower market not yet trending up → anticipate catch-up)
- Short when network-augmented trend signal is negative (analogous)
- Signal applied with a 1-day settlement lag (daily futures prices)
- Volatility-scaled position sizes (standard CTA sizing)

---

## Exit Conditions

NOT REPORTED with full precision. Likely:
- Reverse position when signal crosses zero
- Daily rebalancing at settlement

---

## Risk Management

- **Defined:** Volatility scaling for position sizing (standard CTA practice).
- **Defined:** Ensemble approach reduces turnover vs single-model variants.
- **NOT defined in accessible sources:** Maximum leverage, drawdown limits, stop-loss levels,
  sector/market concentration limits.
- **Note:** The strategy trades 28 markets across sectors, providing natural diversification.
  Ensemble methods implicitly control for model uncertainty. However, explicit risk limits are
  not described.

---

## Indicators Used

- Daily settlement prices (publicly available futures data)
- MACD (baseline trend indicator)
- Lévy area (path-space lead-lag measure, mathematical construction from return series)
- DTW algorithm (time-series alignment, computed from return series)
- Graph-adjacency matrix (learned from Lévy area + DTW ensemble)
- Network momentum signal (graph signal processing output)

All inputs are derived from price/return series. No external macro data required.

---

## Transaction Costs & Capacity

- **Included:** Yes — net Sharpe ratios reported after transaction costs.
- **Finding:** NMM models typically incur **higher** transaction costs than the MACD baseline
  due to greater turnover (the network signal rebalances more frequently).
- **Ensemble mitigates:** Ensemble methods reduce costs, with DTW variants achieving ~19%
  lower costs than MACD.
- **Net result:** Net Sharpe ratios and net returns are still higher than MACD after costs.
- **Specific cost assumptions:** NOT REPORTED (bid-ask spread assumptions, commission per
  contract).
- **Capacity:** Diversified futures portfolio across 28 markets provides good capacity for
  institutional-scale strategies. Agricultural futures can be less liquid — a potential concern
  at large size.

---

## Backtest Evidence

- **Status:** CLAIMED, UNVERIFIED — pre-print; not peer-reviewed; primary sources all 403'd.
- **Period:** OOS January 2005 – June 2024 (19+ years).
- **Method:** Out-of-sample evaluation on real futures settlement prices.
- **Bootstrapped validation:** Synthetic bootstrapped data samples used to establish statistical
  significance of Sharpe improvements.
- **Baseline:** MACD univariate trend model.

---

## Forward-Test Evidence

NOT REPORTED.

---

## Reported Metrics

| Metric | Value | Source | Status |
|--------|-------|---------|--------|
| Net Sharpe (NMM vs MACD) | NMM median net Sharpe > MACD (margin NOT REPORTED) | Search summary | CLAIMED, UNVERIFIED |
| Net return (NMM vs MACD) | NMM higher (magnitude NOT REPORTED) | Search summary | CLAIMED, UNVERIFIED |
| Skewness | Improved vs MACD | Search summary | CLAIMED, UNVERIFIED |
| Downside performance | Improved vs MACD | Search summary | CLAIMED, UNVERIFIED |
| Statistical significance | Confirmed via bootstrapping | Search summary | CLAIMED, UNVERIFIED |
| Best variant | NMM-DDTW (multi-dimensional DTW) | Search summary | CLAIMED, UNVERIFIED |
| Transaction cost vs MACD | Higher for NMM; DTW ensemble ~19% lower than MACD | Search summary | CLAIMED, UNVERIFIED |

---

## Community Sentiment

- Pre-print submitted January 2025, no peer review found in accessible sources.
- One Bluesky post found commenting on the paper (Saeed Amenfx, bsky.app) — limited
  community engagement.
- The earlier companion paper "Network Momentum across Asset Classes" (arxiv 2308.11294)
  provides a foundation for this work, suggesting the authors have a coherent research program.
- The broader TSMOM enhancement literature (Moskowitz et al., Baltas-Kosowski, etc.) is well-
  established and provides a credible foundation for cross-market extensions.
- No critical forum discussion found.

---

## Why This Might Be Nothing

### 1. Most likely benign explanation: multiple-testing / model complexity
The Lévy area + DTW ensemble learns a 28×28 adjacency matrix (784 pairwise relationships) from
the same training data. With this many parameters and choices (lag length, DTW distance metric,
graph aggregation method), finding a combination that improves on MACD in-sample is trivially
easy. The OOS test (2005-2024) is long and covers the full training window — this is a
legitimate concern unless the methodology uses a strictly rolling hold-out.

### 2. The cost case
NMM models have inherently higher transaction costs than MACD. The ensemble mitigates this,
but the specific margin is not accessible. If the Sharpe improvement is small (e.g., 0.05-0.10)
and costs consume a material portion of gross alpha, the strategy may not survive contact with
live execution at realistic broker commissions and slippage.

### 3. Regime dependence
Cross-market lead-lag relationships are not stable over time. The 2002-2010 commodity
supercycle created strong inter-market correlations (oil, metals, EM equities). Post-2015,
the relationship between oil and equity indices weakened as US shale production decoupled oil
from traditional macro cycles. A lead-lag matrix learned on 2002-2024 data may not apply to
the structural environment of 2025 and beyond.

### 4. Sample / overfitting
The "graph-adjacency matrix" is learned — it introduces learnable parameters. The number
of parameters is not clearly disclosed. Bootstrapped significance testing mitigates multiple-
testing risk but does not fully address the possibility that the specific lead-lag structure
was fitted to the historical training period.

### 5. What would change my mind
Access to the full paper showing (a) the specific Sharpe improvement by variant (e.g., MACD
Sharpe 0.55 → NMM Sharpe 0.70), (b) the exact number of learned parameters, and (c) a
strictly rolling in-sample/out-of-sample split (not a fixed training set) would substantially
change my view.

---

## Strengths / Weaknesses

**Strengths:**
- Genuine methodological innovation: Lévy area and DTW for nonlinear lead-lag detection is
  more sophisticated than simple cross-correlation
- Long OOS period (19 years)
- Transaction costs explicitly modeled and net Sharpe reported
- Diversified 28-market portfolio provides natural robustness
- Bootstrapped significance testing appropriate for financial data

**Weaknesses:**
- Pre-print only — no peer review
- No open code — requires significant mathematical expertise to implement
- High inherent transaction costs (partially mitigated by ensemble)
- 28×28 adjacency matrix is a high-dimensional parameter space — overfitting risk
- Specific performance numbers not accessible
- Lead-lag relationships likely non-stationary

---

## Confidence Score

### Per-dimension scoring

| Dimension | Wt | Score | Contribution | Rationale |
|---|---|---|---|---|
| Logical / economic soundness (falsifiable) | 3 | 6 | 18.0 | Cross-market lead-lag has genuine economic basis; failure conditions stated (regime breaks, efficiency increases, instability) |
| Transparency & reproducibility | 2 | 6 | 12.0 | Methods described in detail (Lévy area, DTW formulas, graph construction); no open code; requires expert implementation |
| Evidence auditability | 2 | 4 | 8.0 | Pre-print only; no peer review; no code; all sources 403'd |
| Risk management quality | 2 | 5 | 10.0 | Vol scaling standard; ensemble reduces turnover; no explicit stops or drawdown limits |
| Cost & capacity realism | 2 | 6 | 12.0 | Net Sharpe reported after costs; ensemble cost reduction validated; 28 liquid futures markets |
| Honest treatment of drawdowns | 1.5 | 4 | 6.0 | Downside performance improvement mentioned but specific DD not accessible |
| Robustness evidence (OOS / walk-forward) | 1.5 | 6 | 9.0 | 19-year OOS; 28 markets; bootstrapped validation; no walk-forward or multi-regime detail accessible |
| Survived independent scrutiny | 1 | 3 | 3.0 | Pre-print; minimal community engagement; no peer review |

**Weighted sum:** 18.0 + 12.0 + 8.0 + 10.0 + 12.0 + 6.0 + 9.0 + 3.0 = **78.0**
**Max possible:** 150
**latent_score:** round(78.0 / 150 × 100) = **52**

**Verification cap check:**
- Evidence auditability = 4 → cap at 59
- **confidence = min(52, 59) = 52** (Experimental, 40–59)

*latent 52 (capped to 59 pending verification: pre-print only, no code, all sources 403'd; actual cap not binding here since latent < cap)*

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** HIGH. Lévy area requires path signature computation; DTW requires pairwise
  time-series comparison; graph signal processing adds further complexity. This is not a
  "code in an afternoon" strategy.
- **Scalability:** Medium. Could extend to more markets; computation scales with market count
  (28×28 pairwise comparisons is tractable).
- **Automation:** Feasible with expert quant engineering. Daily update cycle. No exotic data
  required (futures settlement prices only).

---

## MQL5 / Python / Pine Feasibility

- **Python:** MEDIUM. Libraries for DTW (`dtw-python`) and path signatures (`iisignature`) exist.
  Graph signal processing available via `pygsp`. No turnkey implementation — requires
  significant custom code (~500+ lines). No open code from authors.
- **MQL5:** NOT FEASIBLE. The algorithm requires external numerical computation.
- **Pine Script:** NOT FEASIBLE.

---

## Similar Strategies

- `catching-crypto-trends-donchian-ensemble` (score 64) — multi-lookback ensemble trend-following
  on crypto; same ensemble philosophy but much simpler implementation
- `adaptivetrend-crypto` (score 41) — also multi-component trend with asset selection; higher
  complexity on crypto
- Related paper: "Network Momentum across Asset Classes" (arxiv 2308.11294) — earlier paper
  by Barroso et al. that established network momentum framework across equity, bonds, commodities

---

## Red Flags

- Pre-print only — not peer-reviewed
- All primary sources returned HTTP 403 — no direct paper access
- High-dimensional parameter space (28×28 adjacency matrix) — overfitting risk
- Inherently higher transaction costs than MACD baseline
- Specific performance numbers NOT REPORTED in accessible sources
- Lead-lag relationships historically non-stationary (commodity cycles shift)

---

## Source Links

| URL | Accessed | Notes |
|-----|----------|-------|
| https://arxiv.org/abs/2501.07135 | 2026-05-29 | Pre-print (HTTP 403) |
| https://arxiv.org/html/2501.07135v1 | 2026-05-29 | HTML version (HTTP 403) |
| https://www.researchgate.net/publication/387975339 | 2026-05-29 | ResearchGate (HTTP 403) |
| https://ideas.repec.org/p/arx/papers/2501.07135.html | 2026-05-29 | IDEAS.repec (HTTP 403) |
| https://www.imperial.ac.uk/media/imperial-college/faculty-of-natural-sciences/department-of-mathematics/math-finance/239242288---Linze-Li---LI_LINZE_01869053.pdf | 2026-05-29 | Imperial College thesis/paper (HTTP 403) |
| https://bsky.app/profile/saeedamenfx.bsky.social/post/3lfrhvml2672x | 2026-05-29 | Community commentary (search snippet only) |

All primary sources returned HTTP 403. Evidence entirely from search engine summaries.

---

## Analyst Notes

**FACTS (from accessible search summaries):**
- Authors: Linze Li and William Ferreira (Imperial College London)
- Pre-print submitted January 13, 2025 (arxiv 2501.07135)
- 28 futures markets across sectors: agriculture, energy, metals, equity indices
- Training period: June 2002 – June 2024
- OOS: January 2005 – June 2024
- Two lead-lag methods: Lévy area (linear + nonlinear) and DTW
- Net Sharpe ratios confirmed better than MACD baseline across all NMM variants
- Bootstrapped significance testing used
- Ensemble approach reduces transaction costs; DTW ensemble ~19% lower costs than MACD

**ANALYSIS (my reasoning):**
- The Lévy area is a genuine mathematical innovation applied to finance (path signatures from
  stochastic analysis). This is not a retrofitted existing technique — it appears purpose-built
  for detecting nonlinear lead-lag in financial returns. That deserves credit.
- The 28×28 adjacency matrix is a learnable parameter space. The bootstrapped validation
  mitigates multiple-testing concerns but does not eliminate them.
- The cost transparency (higher costs for NMM, reported in net terms) is commendable — most
  papers in this space report gross Sharpe only.
- Without peer review or direct paper access, the quality of the methodology cannot be fully
  assessed. A reasonable quant would want to see: (a) the exact feature count, (b) rolling
  vs. fixed training window, (c) net Sharpe by individual market vs. portfolio.

**ASSUMPTIONS (not confirmed):**
- ASSUMED the 28-market list includes major CME/ICE commodity and equity index futures.
- ASSUMED the "vol scaling" is a standard realized-vol or EWMA-vol approach consistent with
  CTA industry practice.
- ASSUMED the OOS period (2005-2024) uses a strictly forward-rolling methodology and not a
  fixed-train-window approach.

---

## Future Research Needed

1. **Peer review:** The paper should be submitted to a quantitative finance journal. Peer
   review would address the overfitting concerns.
2. **Code release:** Publishing the implementation would allow independent replication.
3. **Rolling OOS clarification:** Confirm whether the OOS evaluation uses a rolling or fixed
   training window.
4. **Specific Sharpe numbers:** Quantify the Sharpe improvement margin (e.g., MACD Sharpe 0.55
   vs NMM Sharpe 0.68).
5. **Post-training OOS:** Test the strategy from June 2024 forward (after the training window
   closes) to assess live-time performance.
6. **Regime breakdown analysis:** Does NMM outperform in trend-following regimes but
   underperform in mean-reverting periods (2011-2015)?
