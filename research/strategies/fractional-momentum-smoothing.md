# Fractional Momentum and Turnover Smoothing (Chitsiripanich, Paolella, Polak, Walker)

## Overview

Two related papers by Chitsiripanich, Paolella, Polak, and Walker from the University of
Zurich and Stony Brook present a cross-sectional equity momentum strategy that applies
fractional differencing (parameter d ∈ [0,1]) to price series, combining standard momentum
(d=1) and short-term reversal (d=0) into a hybrid signal. A follow-up paper adds path-
dependent constraints in a sequential portfolio optimization framework to reduce the turnover
of daily-rebalanced strategies by 95–99%.

**Core papers:**
- "Momentum Without Crashes" — SSRN 4280465, SFI RP 22-87, November 2022
- "Smoothing Out Momentum and Reversal" — SSRN 4955388, SFI RP 24-47, August 2024

Both papers are Swiss Finance Institute research papers; neither has confirmed peer-reviewed
journal publication as of 2026-06-12 (the 2022 paper has been on SSRN for 3+ years).

---

## Asset Class / Timeframes

- **Asset class**: US Equities (cross-sectional; long-short; daily or monthly rebalancing)
- **Additional markets**: "Different asset universes and foreign markets" (pervasive across
  markets — specific markets NOT REPORTED in accessible summaries)
- **Timeframe**: Daily rebalancing (with turnover reduction to monthly-equivalent frequency)
- **Portfolio type**: Cross-sectional long-short (winners minus losers)

---

## Core Logic

**Paper 1 — Fractional Momentum (d-parameter):**

Classical 12-month momentum uses only the first and last price in the lookback window
(12-month return). Short-term reversal uses only the most recent return. The fractional
differencing approach applies a fractional-difference filter to the price series, producing
exponentially decaying weights that span the entire lookback period. The parameter d controls
the blend:
- d = 1 → classical momentum (standard 12M-1M signal)
- d = 0 → short-term reversal
- 0 < d < 1 → hybrid: gradually decaying weights across all past returns, preserving
  long-memory structure

Winners (stocks with high fractional-momentum scores) are held long; losers are held short.

**Paper 2 — Path-Dependent Turnover Smoothing:**

Introduces sequential portfolio optimization with path-dependent constraints that classify
stocks into groups based on (1) attractiveness from the signal perspective and (2) need for
rebalancing. The constraints prevent excessive trading when a stock is near the
boundary of the long/short threshold, effectively creating a "hysteresis band" around the
signal threshold. This reduces daily portfolio turnover by 95–99%, bringing it to the level
of traditionally monthly-rebalanced strategies.

The specific value of d that maximizes risk-adjusted performance is NOT REPORTED in
accessible summaries. The exact path-dependent constraint formulation is NOT REPORTED
in accessible summaries (HTTP 403 on primary papers).

---

## Economic Rationale

**Why the fractional momentum signal might work:**
Returns may exhibit long-range dependence (fractional integrated structure), meaning past
returns beyond the 12M window carry predictive content. Classical momentum discards this
information by using only the 12M-minus-1M return. The fractional filter preserves and
weights all past returns, theoretically extracting more signal from the available history.
The combination with reversal (d < 1) reduces the explosive crash behavior of pure momentum
by damping the most recent over-extensions.

**When the edge should disappear:**
- If returns follow a random walk (no long memory), fractional differencing adds no
  information vs. standard momentum or reversal
- During momentum crashes (2008–2009, 2020 March): the fractional signal still has large
  negative positions in former winners at peak crash; the d < 1 weighting reduces but does
  not eliminate this exposure
- If the long-memory structure of returns is non-stationary or regime-dependent
- Under increased market efficiency as the signal becomes widely implemented

**Critical caveat:** The premise of "long memory in equity returns" is empirically contested.
Lo (1991, Review of Financial Studies) found weak evidence for long-range dependence in
stock returns after controlling for short-range autocorrelation. The fractional differencing
approach assumes this memory structure exists; the economic rationale is therefore PARTIALLY
SUPPORTED by the literature.

---

## Entry Conditions

1. Compute the fractional-difference signal for each stock using parameter d ∈ (0,1)
   (optimal d NOT REPORTED in accessible summaries).
2. Rank stocks by their fractional-momentum signal.
3. Long top decile/quintile; short bottom decile/quintile (standard cross-sectional
   portfolio construction; exact cutoffs NOT REPORTED).
4. Paper 2 adds path-dependent hysteresis: only rebalance stocks that have moved
   significantly past the threshold since last trade.

---

## Exit Conditions

- Daily rebalancing (Paper 2: effectively monthly due to path-dependent constraints)
- Standard cross-sectional: at each rebalancing, replace current holdings with new signal-
  ranked portfolio (filtered by path-dependent constraints in Paper 2)

---

## Risk Management

- Long-short construction hedges broad market beta
- Path-dependent constraints limit concentration in boundary stocks (reduces flash-trading)
- No explicit stop-loss or max-loss rules mentioned
- Maximum drawdown reduced from 76–99% to 22–49% (CLAIMED) via path-dependent constraints
- No leverage or position limits explicitly stated

---

## Indicators Used

- Fractional-difference filter applied to stock price series (parameter d)
- No technical indicators, fundamental factors, or macro signals explicitly mentioned

---

## Transaction Costs & Capacity

- **Transaction costs**: Explicitly modeled under "realistic transaction cost assumptions"
  (specific cost assumption: NOT DISCLOSED in accessible summaries)
- **Turnover**: Reduced from daily to monthly-equivalent (95–99% reduction in daily turnover)
- **Results claim**: Net risk-adjusted returns improve 38–149% after transaction costs vs.
  classical daily-rebalanced momentum/reversal
- **Capacity**: Cross-sectional long-short on US equities; capacity depends on universe size
  and position concentration (NOT REPORTED)
- **Assessment**: The turnover reduction is the single strongest feature of this paper —
  if the monthly-equivalent turnover claim is accurate, round-trip costs are manageable.
  However, the 38–149% improvement range is wide and likely driven by comparison to the
  most expensive baseline (daily reversal), not to the fair baseline (monthly momentum).

---

## Backtest Evidence

Two SFI research papers. Neither is confirmed peer-reviewed as of 2026-06-12. "Momentum
Without Crashes" (2022) has been an SSRN working paper for 3+ years without journal
publication — a significant concern.

- **"Momentum Without Crashes"**: "Extensive out-of-sample analysis" stated; specific
  period NOT REPORTED in accessible summaries.
- **"Smoothing Out Momentum and Reversal"**: Period NOT REPORTED in accessible summaries.
- Comparison baseline: classical daily-rebalanced momentum and short-term reversal.

---

## Forward-Test Evidence

NOT REPORTED in accessible summaries.

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | NOT REPORTED | NOT REPORTED | — |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | 22–49% (post-smoothing); 76–99% classical baseline | CLAIMED, UNVERIFIED | SSRN 4955388 secondary summaries |
| CAGR | NOT REPORTED | NOT REPORTED | — |
| Annual Return | NOT REPORTED | NOT REPORTED | — |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | Monthly-equivalent (implied by turnover reduction) | CLAIMED, UNVERIFIED | Secondary summaries |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | NOT REPORTED | NOT REPORTED | — |

**Comparative improvement claims (CLAIMED, UNVERIFIED from secondary summaries):**
- Turnover reduction: 95–99% vs. daily-rebalanced baseline
- Max drawdown reduction: from 76–99% to 22–49%
- Net risk-adjusted returns improvement: 38–149%
- "Significantly higher risk-adjusted returns" vs. classical momentum (Paper 1)
- "Pervasive across different asset universes and foreign markets"

**Note on missing Sharpe ratio**: Despite being available since 2022, no secondary source
reports the absolute Sharpe ratio of the fractional momentum strategy. This is unusual for
a high-quality strategy and may indicate either (a) the results are in the paper's tables
but not in accessible abstracts, or (b) the results are less impressive in absolute terms
than the comparative percentages imply.

---

## Community Sentiment

- Pawel Polak (Stony Brook) presented the 2022 paper at Stanford FinTech Laboratory —
  institutional visibility
- Quantpedia referenced "Momentum Without Crashes" in their blog covering momentum crash
  fixes (not a full strategy review)
- No forum discussion on r/algotrading, QuantLib, or MQL5 found in searches
- No independent replication found
- No hostile criticism found; no strong endorsement found
- The 3+ year SSRN residence without journal publication is the strongest implicit signal
  from the community — peer review has apparently not been kind

---

## Why This Might Be Nothing

**Strongest skeptical case:**

1. **Three-plus years on SSRN, no journal publication (most important red flag)**: "Momentum
   Without Crashes" was posted in November 2022. By June 2026 (3.5 years later), it remains
   a working paper. Papers with genuinely superior, robust results that survive peer review
   typically receive journal acceptance within 1–2 years when submitted to journals. The
   extended SSRN residence is the strongest single signal that peer reviewers have identified
   fundamental problems — potentially in the statistical methodology, the benchmark comparison,
   or the robustness of the OOS evidence.

2. **Turnover reduction misdefines the benchmark**: The 95–99% turnover reduction from a
   *daily-rebalanced* strategy is compared to classical momentum. However, the fair comparison
   is not daily-rebalanced momentum (which no practitioner implements) but monthly-rebalanced
   momentum — a well-documented strategy since Jegadeesh/Titman (1993). The dramatic
   improvement may be entirely explained by "stop doing something foolish (daily rebalancing)
   and rebalance monthly instead." The fractional d-filter may add nothing incremental.

3. **Fractional d is a one-parameter in-sample optimization**: The optimal d is presumably
   selected from data. Even one hyperparameter optimized over decades of US equity returns
   can produce spuriously high Sharpe ratios if the true optimal d varies across regimes.
   A value of d that worked well in 1970–2010 may not be optimal in 2010–2025.

4. **The improvement range (38–149%) is suspicious**: A 4× spread in improvement suggests
   extreme configuration dependence. The best-case (149%) likely represents comparison to
   the worst baseline (daily reversal with high costs) in the best market. The worst-case
   (38%) is more representative of what a fair comparison yields.

5. **Maximum drawdown of 22–49% remains severe**: Even after the dramatic reduction, a
   22–49% maximum drawdown on a long-short equity strategy is still a very significant
   drawdown. A 49% drawdown (upper bound) would essentially destroy a leveraged
   implementation. The comparison to 76–99% baseline makes the improvement look better
   than the absolute numbers warrant.

6. **Long-memory premise is contested**: Lo (1991, RFS) found that apparent long-range
   dependence in stock returns is largely explained by short-range autocorrelation (GARCH
   effects). If the long-memory structure is spurious, fractional differencing adds no
   information — the cross-correlation with the d parameter would then be data-mining.

7. **What would change my assessment**: Full paper access showing (a) the data period and
   out-of-sample window, (b) the absolute Sharpe ratio vs. the monthly-rebalanced benchmark,
   (c) a peer-reviewed journal acceptance, and (d) the specific value of d used.

---

## Strengths / Weaknesses

**Strengths:**
- Grounded in fractional calculus literature (established mathematical framework)
- Transaction costs explicitly modeled; results claimed to survive realistic costs
- Turnover reduction is practically valuable for implementation
- Multi-market validation claimed
- Reputable institutional authors (UZH/SFI, Stony Brook)
- Max drawdown explicitly compared and stated (rare transparency)

**Weaknesses:**
- Neither paper has confirmed peer-reviewed journal publication (3.5 years as working paper)
- Absolute Sharpe ratio NOT REPORTED in any accessible source — major gap
- Optimal d value NOT REPORTED — cannot implement independently
- Path-dependent constraint details NOT ACCESSIBLE
- Benchmark comparison (vs. daily-rebalanced) may not be the fair baseline
- Long-memory premise is contested in the empirical literature
- No open code; primary papers HTTP 403
- Improvement range (38–149%) excessively wide for credible claim

---

## Confidence Score

**Dimension breakdown:**

| Dimension | Score (0-10) | Weight | Weighted |
|-----------|-------------|--------|---------|
| Logical / economic soundness (falsifiable) | 6 | 3 | 18 |
| Transparency & reproducibility | 4 | 2 | 8 |
| Evidence auditability | 3 | 2 | 6 |
| Risk management quality | 5 | 2 | 10 |
| Cost & capacity realism | 7 | 2 | 14 |
| Honest treatment of drawdowns / failure | 7 | 1.5 | 10.5 |
| Robustness evidence (OOS / walk-forward / multi-market) | 6 | 1.5 | 9 |
| Survived independent scrutiny | 3 | 1 | 3 |
| **Total** | | **15** | **78.5 / 150** |

**latent_score** = round(78.5 / 150 × 100) = **52**

**Verification cap**: Evidence auditability = 3 ≤ 4 → cap at 59
**confidence** = min(52, 59) = **52** (Experimental)

Recorded as: latent 52 (capped to 52 — evidence auditability 3/10, SFI working papers
without confirmed peer review; neither paper journal-published after 3+ years on SSRN;
absolute Sharpe ratio NOT REPORTED; optimal d value NOT REPORTED; full paper HTTP 403;
path-dependent constraint details NOT ACCESSIBLE)

**NOTE**: If either paper achieves peer-reviewed publication AND the absolute Sharpe ratio
is disclosed in accessible form, this strategy is worth deeper investigation. The 52 latent
score is below the 60 threshold for implementation candidates, but it's among the higher
scores in the Low Confidence / Experimental cluster.

---

## Complexity / Scalability / Automation Feasibility

- **Complexity**: Moderate. Fractional differencing requires scipy/statsmodels (fracdiff
  function); cross-sectional ranking is standard. Path-dependent constraints require custom
  optimization code.
- **Scalability**: Dependent on universe size. Long-short on hundreds/thousands of stocks.
- **Automation**: The fractional signal is computable; the path-dependent constraints require
  knowing the specific formulation (NOT ACCESSIBLE).

---

## MQL5 / Python / Pine Feasibility

- **Python**: MODERATE — fractional differencing implementable via `fracdiff` library;
  cross-sectional ranking straightforward; path-dependent constraint unknown without full paper
- **MQL5**: LOW — cross-sectional long-short requires multi-ticker framework; not typical
  for MQL5
- **Pine Script**: VERY LOW — cross-sectional analysis across a stock universe not feasible
  in Pine

---

## Similar Strategies

- [`crypto-ts-xs-momentum-realistic`](./crypto-ts-xs-momentum-realistic.md) — Cross-sectional
  momentum on crypto; SSRN 4675565; 15bps costs modeled; score 55. Different market and
  signal (standard return-based momentum vs. fractional).
- [`catching-crypto-trends-donchian-ensemble`](./catching-crypto-trends-donchian-ensemble.md) —
  Multi-period momentum ensemble; daily crypto; score 64. Different mechanism.
- [`industry-trend-century`](./industry-trend-century.md) — Long-only Donchian momentum on
  industry portfolios; SSRN 4857230; score 64. Different scope and signal.

---

## Red Flags

- **3+ years on SSRN without journal acceptance** — peer review has apparently found problems
- **Absolute Sharpe ratio NOT REPORTED** in any accessible secondary source
- **Optimal d value NOT REPORTED** — strategy cannot be independently implemented or tested
- **Path-dependent constraint details NOT ACCESSIBLE** (HTTP 403 on primary papers)
- **Wide improvement range (38–149%)** — configuration-dependent; cherry-picking possible
- **Fair baseline unclear**: comparison to daily-rebalanced (worst practice) not monthly

---

## Source Links

| URL | Retrieved | Used For |
|-----|-----------|----------|
| [SSRN 4955388 — Smoothing Out Momentum and Reversal (2024)](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4955388) | 2026-06-12 | Primary source; HTTP 403; secondary summaries used |
| [SSRN 4280465 — Momentum Without Crashes (2022)](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4280465) | 2026-06-12 | Foundational paper; HTTP 403; secondary summaries used |
| [SFI Publication No. 24-47](https://www.sfi.ch/en/publications/n-24-47-smoothing-out-momentum-and-reversal) | 2026-06-12 | Institutional listing; HTTP 403 |
| [IDEAS/RePEC — Momentum Without Crashes](https://ideas.repec.org/p/chf/rpseri/rp2287.html) | 2026-06-12 | Metadata; HTTP 403 |
| [ZORA — Momentum Without Crashes (UZH repository)](https://www.zora.uzh.ch/id/eprint/232025/) | 2026-06-12 | Screened; secondary summaries used |
| [CXO Advisory — Stock Momentum Exploiting All Price Data](https://www.cxoadvisory.com/momentum-investing/stock-momentum-exploiting-all-price-data-in-a-lookback-interval/) | 2026-06-12 | Review; HTTP 403 |
| [Stanford FinTech Lab — Pawel Polak presentation](https://fintech.stanford.edu/events/pawel-polak-stony-brook-momentum-without-crashes) | 2026-06-12 | Community sentiment signal |
| [Web search summaries (Google/Bing snippets)](https://www.google.com) | 2026-06-12 | All quantitative claims from secondary summaries |

---

## Analyst Notes

**FACTS (from accessible sources):**
- Two SFI research papers: "Momentum Without Crashes" (2022) and "Smoothing Out Momentum
  and Reversal" (2024)
- Authors: Chitsiripanich (UZH), Paolella (UZH/SFI), Polak (Stony Brook), Walker (UZH/SFI)
- Core mechanism: Fractional differencing parameter d blends momentum (d=1) and reversal (d=0)
- Paper 2 adds path-dependent constraints → 95–99% turnover reduction
- Max drawdown: reduced from 76–99% to 22–49% (CLAIMED)
- Risk-adjusted net returns: +38–149% vs. classical daily baseline (CLAIMED)
- Transaction costs: explicitly modeled under "realistic" assumptions
- Multi-market validation claimed; factor-model robustness claimed
- Neither paper confirmed as peer-reviewed journal publication as of 2026-06-12
- Optimal d value not accessible; constraint details not accessible

**ANALYSIS:**
- ANALYSIS: The comparison baseline (daily-rebalanced momentum) is not the practitioner-
  standard implementation. Monthly-rebalanced momentum (Jegadeesh/Titman 1993) is the
  canonical benchmark. If the 38–149% improvement is vs. monthly-rebalanced, that is
  impressive. If it is vs. daily-rebalanced, it is mostly just avoiding unnecessary trading.
  The available summaries do not clarify this critical distinction.
- ANALYSIS: 22–49% max drawdown on a long-short equity portfolio implies very large single
  losses. At 2× leverage (common for institutional long-short), this implies 44–98%
  drawdown. This is still catastrophic and limits leverage deployment.
- ANALYSIS: The 3.5-year SSRN residence implies that peer reviewers have raised concerns
  that the authors have not fully resolved. The most common criticism of momentum improvement
  papers is that they use in-sample d-parameter optimization without proper OOS validation.

**ASSUMPTIONS:**
- Data period assumed to include standard CRSP cross-sectional US equity sample (~1963–2020
  or similar); NOT CONFIRMED
- Monthly-rebalanced momentum assumed to be the implicit standard benchmark; NOT CONFIRMED
- The 22–49% drawdown assumed to be peak-to-trough on a long-short portfolio; NOT CONFIRMED

---

## Future Research Needed

1. Access the full paper PDFs when available without paywall to verify:
   - Exact data period and OOS specification
   - Optimal d value (and sensitivity to d)
   - Absolute Sharpe ratio vs. both daily and monthly-rebalanced benchmarks
   - Whether the improvement holds in 2010–2025 (post-GFC era)
2. Check whether either paper is now published in a peer-reviewed journal
3. Test the fractional differencing approach independently using the `fracdiff` Python
   library on US equities (CRSP data) with d ∈ {0.1, 0.2, ..., 0.9} to see if improvements
   are robust or parameter-sensitive
4. Verify whether the path-dependent constraints add value beyond simply rebalancing monthly
