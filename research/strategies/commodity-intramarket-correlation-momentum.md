# Commodity Intra-Market Correlation Momentum-Reversal Filter

## Overview

A Quantpedia/SSRN working paper (Vojtko/Pauchlyová, Sep 2024) proposes using the ratio of
short-term to long-term intra-market correlation among 4 sector commodity ETFs (DBA, DBB,
DBE, DBP) to switch between a momentum and a reversal strategy. When the 20-day average
pairwise correlation exceeds the 200-day average, momentum is applied (long top performers,
short bottom performers). When short-term correlation falls below long-term correlation,
reversal is applied. The combined strategy "nearly doubles" standalone momentum/reversal
returns and outperforms across all reported metrics vs. the baseline momentum strategy.
Specific Sharpe/CAGR/drawdown numbers are not accessible in search summaries.

---

## Asset Class / Timeframes

- **Asset class:** Commodities (sector ETFs)
- **Instruments:** 4 sector commodity ETFs:
  - DBA — Invesco DB Agriculture Fund (inception: Jan 2007)
  - DBB — Invesco DB Base Metals Fund (inception: Jan 2007)
  - DBE — Invesco DB Energy Fund (inception: Feb 2007)
  - DBP — Invesco DB Precious Metals Fund (inception: Jan 2007)
- **Timeframe:** Monthly (end-of-month signal, monthly rebalancing)
- **Data period:** 2007–2024 (~17 years; constrained by ETF inception dates)

---

## Core Logic

**Step 1 — Momentum ranking (baseline):**
- Compute the trailing N-month return for each of the 4 ETFs (N tested from 1–12 months;
  12 months appears to be the primary specification)
- Rank ETFs by return; go long top 2, short bottom 2 (equal-weighted within each leg)
- Result: baseline commodity momentum performs poorly (slightly above benchmark,
  higher volatility)

**Step 2 — Intra-market correlation filter:**
- Compute the average pairwise correlation among all 4 ETFs over a 20-day rolling window
  (short-term correlation, STCorr)
- Compute the average pairwise correlation over a 200-day rolling window (long-term
  correlation, LTCorr)
- Compare STCorr vs. LTCorr:
  - If STCorr > LTCorr: apply MOMENTUM (long top 2, short bottom 2)
  - If STCorr < LTCorr: apply REVERSAL (long bottom 2, short top 2)

**Step 3 — Combined strategy (Mom+Rev):**
- Always in the market: applies the correlation-appropriate strategy every month
- Alternative simplified version: if STCorr > LTCorr, go long [the momentum portfolio];
  else go to cash (momentum-only with cash filter)

---

## Economic Rationale

When commodity sector correlations are rising above their historical baseline (STCorr >
LTCorr), macro/risk-off factors dominate — all commodities are moving together, driven by
common drivers (dollar, global growth, risk sentiment). In these regimes, relative momentum
persists because the macro driver causes systematic trending. When sector-specific factors
dominate (STCorr < LTCorr), idiosyncratic supply/demand forces drive individual commodity
returns — performance leaders tend to mean-revert as supply responds, while laggards recover
as demand stays stable.

**When the edge should disappear:**
- As commodity index funds and ETFs proliferate, all four sectors become more correlated with
  the broader commodity index, making the STCorr signal less informative about momentum regime.
- If the 20-day vs 200-day correlation relationship becomes random (no predictive power for
  subsequent momentum profitability), the filter adds no value.
- In a period of prolonged low or high correlation (no regime switches), the strategy
  produces no signal variation and degenerates to a static position.
- The four-sector ETF universe is extremely small; with more sectors, the momentum signal
  becomes more statistically meaningful.

---

## Entry Conditions

**Momentum signal active (STCorr > LTCorr):**
- Long: top 2 ETFs by trailing 12-month return
- Short: bottom 2 ETFs by trailing 12-month return

**Reversal signal active (STCorr < LTCorr):**
- Long: bottom 2 ETFs by trailing 12-month return
- Short: top 2 ETFs by trailing 12-month return

**Monthly rebalancing:** at end of month

---

## Exit Conditions

- Signal flip at next monthly rebalancing
- No intra-month exits or stop-losses reported

---

## Risk Management

- Long-short structure (2 long, 2 short of 4 assets) provides implicit market-neutral
  positioning within the commodity sector
- No explicit position sizing, stop-loss, or maximum drawdown protection reported
- Portfolio is fully invested at all times (in the full Mom+Rev version)
- Cash version reduces drawdown (go to cash when reversal signal) but omits reversal returns
- **Assessment:** Minimal explicit risk management; the long-short structure provides
  commodity-market neutrality but no protection against correlated commodity sector crashes.

---

## Indicators Used

- **N-month trailing return** (momentum ranking, N = 1–12; 12-month primary)
- **20-day pairwise correlation** (short-term intra-market correlation)
- **200-day pairwise correlation** (long-term intra-market correlation)
- No additional price indicators

---

## Transaction Costs & Capacity

- **Costs modeled:** NOT REPORTED — no mention of bid-ask spread or ETF borrow costs
- **Practical assessment:**
  - Monthly rebalancing with 4 ETFs: very low trading costs; ETF bid-ask spreads < 5bps
  - Short ETF legs: DBA/DBB/DBE/DBP borrow costs ~20–50bps/year per short position
  - At 50% of months in each regime, annual borrow drag ≈ 10–25bps — small but not zero
  - ETF management expense ratios: DBA (0.85%), DBB (0.85%), DBE (0.85%), DBP (0.75%) —
    these are embedded in ETF price returns and do not add extra cost for the strategy
- **Capacity:** Highly liquid commodity ETFs; no constraint at retail or small institutional scale

---

## Backtest Evidence

- SSRN working paper (not peer-reviewed)
- Data: 4 sector commodity ETFs, 2007–2024 (~17 years)
- No out-of-sample test, no walk-forward validation
- Multiple lookbacks tested (1–12 months momentum + correlation filter) — parameter selection
  risk
- All metrics below are `CLAIMED, UNVERIFIED`

---

## Forward-Test Evidence

`NOT REPORTED` — no live or paper-trading validation described.

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | NOT REPORTED (described as "significantly increased" vs baseline momentum) | NOT REPORTED | — |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED (described as "outperforms" baseline momentum) | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | NOT REPORTED (described as "outperforms" baseline momentum on this metric) | NOT REPORTED | — |
| CAGR | NOT REPORTED (described as "nearly doubles" standalone momentum/reversal) | NOT REPORTED | — |
| Annual Return | NOT REPORTED | NOT REPORTED | — |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | NOT REPORTED (monthly rebalancing → ~1 month) | NOT REPORTED | — |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | ~100% (always long-short in Mom+Rev version) | NOT REPORTED | — |

**Note:** The paper's "outperforms across all metrics" claim is `CLAIMED, UNVERIFIED` —
comparisons are against the standalone (weak) baseline commodity momentum, not against a
strong benchmark like a commodity index buy-and-hold.

No automatic scrutiny triggers fire (no specific numbers reported to cross thresholds).

---

## Community Sentiment

- SSRN: 1,612 downloads, 4,312 abstract views — relatively high interest for a working paper
- Featured on Quantpedia blog (same authors) and QuantSeeker aggregator
- StockViz (independent Indian quant blog) independently tested a related concept
  (intra-stock correlation and momentum in Indian equities, Nov 2024) — a conceptually
  adjacent analysis, not a direct replication
- No critical forum discussions or independent performance replications found
- No rejection or peer-review criticism found (not yet peer-reviewed)

---

## Why This Might Be Nothing

### 1. Universe size is fatally small

With only 4 ETFs, the long-short strategy takes top 2 / bottom 2 positions. This is
essentially a binary bet: which 2 of 4 commodity sectors outperform. There are only 6
possible orderings of 4 assets, making this more of a sector pair-trade than a momentum
strategy. The statistical power to identify a genuine momentum signal from 4 assets over
17 years is very limited — the t-statistics for any outperformance will be low. A proper
commodity momentum study requires 10–50+ contracts or ETFs.

### 2. The "nearly doubles" claim is against a weak baseline

The baseline commodity momentum strategy on DBA/DBB/DBE/DBP "yielded poor performance"
(roughly matching the commodity benchmark at higher volatility). "Nearly doubling" returns
from a near-zero base means very little in absolute terms. The absolute CAGR of the
combined strategy is not reported and may still be modest.

### 3. Multiple testing with small universe

12 possible momentum lookbacks × 2 correlation periods (could be varied) × 2 strategies
(momentum/reversal vs. cash fallback) = many parameter combinations. With only 4 assets and
17 years of monthly data (~200 monthly observations), overfitting is a serious concern.

### 4. Correlation filter mechanism is circular at short horizons

The 20-day correlation is computed over roughly the same period as the momentum signal. In a
strongly trending period for all commodities (all rising or all falling), both correlation
and momentum signals would be aligned — but this alignment may be definitional rather than
causal. The filter may add no genuine information beyond the momentum signal itself.

### 5. No OOS test

The entire 2007–2024 period is used in-sample. Commodity markets underwent structural
changes during this period (growth of commodity index funds 2007–2012, commodity super-cycle
end 2014–2016, COVID demand shock 2020, energy crisis 2022). A walk-forward test would
reveal whether the correlation filter was genuinely predictive or was selected to fit
retrospective regimes.

### 6. The one piece of evidence that would most change my mind

An independent replication on a different commodity universe (e.g., 20+ commodity futures,
or a different set of commodity ETFs like PDBC, COMB, COMT) with a held-out 2023–2025 OOS
period, showing that the combined strategy achieves Sharpe > 0.5 after realistic costs.
This would address both the small-universe concern and the in-sample overfitting concern.

---

## Strengths / Weaknesses

**Strengths:**
- Simple, reproducible rules using freely available ETF data
- Monthly rebalancing means minimal trading costs
- Long-short structure provides commodity-market neutrality
- Economic rationale is plausible (correlation regimes predict momentum/reversal)
- SSRN paper with high download count suggests practitioner interest
- The concept is generalizable to other markets (sector equities, FX, etc.)

**Weaknesses:**
- Only 4 ETFs — extremely small universe; statistical power is minimal
- SSRN working paper only; not peer-reviewed
- No specific performance numbers accessible (no Sharpe, CAGR, or max drawdown figures)
- No OOS test or walk-forward
- "Nearly doubles" claim is against a weak baseline
- Pre-publication overfitting risk is high with 12 lookback combinations tested
- ETF borrow costs not modeled
- Results not independently replicated

---

## Confidence Score

### Per-Dimension Breakdown

| Dimension | Raw Score | Weight | Contribution |
|-----------|-----------|--------|--------------|
| Logical / economic soundness (falsifiable) | 5 | 3 | 15 |
| Transparency & reproducibility | 7 | 2 | 14 |
| Evidence auditability | 4 | 2 | 8 |
| Risk management quality | 4 | 2 | 8 |
| Cost & capacity realism | 4 | 2 | 8 |
| Honest treatment of drawdowns / failure | 5 | 1.5 | 7.5 |
| Robustness evidence (OOS / walk-forward) | 3 | 1.5 | 4.5 |
| Survived independent scrutiny | 2 | 1 | 2 |

**Weighted sum:** 67 / 150 → **latent 45**

**Evidence auditability = 4 → cap at 59**

**`confidence = min(45, 59) = 45`** (Experimental)

*latent 45 (capped to 45 pending verification: SSRN working paper only; no peer review;
universe of only 4 ETFs — statistically underpowered; no specific Sharpe/CAGR/drawdown
reported; no OOS test; "nearly doubles" claim against weak baseline; latent 45 = tied with
vix-cmf-ml-walkforward and bitcoin-max-min-trendrev)*

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Low. Four ETFs, one momentum signal, one correlation filter, monthly
  rebalancing. Simple to implement.
- **Scalability:** Constrained by universe — scaling to more commodities would require
  more ETFs or futures. Current 4-ETF version is trivially implementable.
- **Automation:** Very easy to automate — compute monthly momentum and rolling pairwise
  correlations, apply signal, place 4 ETF orders.

---

## MQL5 / Python / Pine Feasibility

- **Python:** PRIMARY — trivial with yfinance/pandas; compute 20-day and 200-day
  rolling pairwise correlations + return rankings; 20 lines of code.
- **Pine Script (TradingView):** Feasible but limited (DBA/DBB/DBE/DBP available);
  custom correlation calculation needed.
- **MQL5:** Feasible with ETF CFDs; less natural.
- **Latent score (45) is below implementation candidate threshold (latent ≥ 60); will
  not be added to implementation_candidates. However, the simplicity means a DIY
  verification pass would be low effort — consider extending the universe to 10+ ETFs
  or commodity futures before any serious implementation test.**

---

## Similar Strategies

- `tsmom-china-commodity-futures` — monthly time-series momentum on 64 Chinese commodity
  futures; peer-reviewed; same monthly TSMOM mechanism but larger universe and different market
- `industry-trend-century` — Donchian/Keltner breakout on 48 US industries; related concept
  (systematic commodity/industry rotation) but different mechanism
- `catching-crypto-trends-donchian-ensemble` — multi-period Donchian ensemble; similar idea of
  using correlation/regime to time momentum

---

## Red Flags

- **Only 4 ETFs:** Statistically underpowered for momentum strategy; results may be noise.
- **No specific performance numbers:** CAGR, Sharpe, and max drawdown are all NOT REPORTED
  in any accessible source. "Outperforms" and "nearly doubles" are qualitative only.
- **No OOS test:** 17-year in-sample period with multiple parameters tested.
- **Not peer-reviewed:** SSRN working paper without editorial filter.
- **Borrow costs not modeled:** Short ETF positions incur annual borrow costs.

---

## Source Links

| URL | Retrieved | Notes |
|-----|-----------|-------|
| https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4964417 | 2026-06-03 | Primary source — SSRN abstract (HTTP 403 — not directly accessible) |
| https://quantpedia.com/how-to-improve-commodity-momentum-using-intra-market-correlation/ | 2026-06-03 | Quantpedia blog coverage (HTTP 403 — not directly accessible) |
| https://medium.com/@quantpedia/how-to-improve-commodity-momentum-using-intra-market-correlation-8b2d10a33a41 | 2026-06-03 | Quantpedia Medium post (HTTP 403) |

---

## Analyst Notes

**FACTS (from sources via web search summaries):**
- Authors: Radovan Vojtko and Margaréta Pauchlyová (Quantpedia)
- Paper: SSRN 4964417 (September 16, 2024)
- Universe: DBA, DBB, DBE, DBP (4 sector commodity ETFs, all started 2007)
- Backtest: 2007–2024 (approximately)
- Momentum: 1–12 month lookback; 12-month appears primary
- Correlation filter: 20-day avg pairwise correlation vs. 200-day (or 250-day) avg pairwise correlation
- Signal: STCorr > LTCorr → momentum; STCorr < LTCorr → reversal (or cash)
- Baseline momentum: "poor performance" vs. benchmark
- Combined (Mom+Rev): "nearly doubles" standalone returns; "outperforms across all metrics
  including annual returns, volatility, max drawdown, Sharpe, Calmar"
- SSRN: 1,612 downloads, 4,312 views

**ANALYSIS:**
- "Nearly doubles" with a near-zero baseline means very little in absolute terms. If the
  baseline momentum strategy earns 2% CAGR at 15% volatility, "nearly doubling" gives
  ~4% CAGR — still underwhelming against a commodity buy-and-hold or diversified portfolio.
- The universe size problem is fundamental. Academic research on commodity momentum (e.g.,
  Asness/Moskowitz/Pedersen 2013) requires 27 commodity futures contracts to generate
  meaningful cross-sectional momentum. 4 ETFs is well below any defensible threshold.
- The correlation filter concept is theoretically interesting and has academic precedent
  (see Cho et al. 2019 for time-series momentum and correlation regimes). However, the
  application to 4-ETF universe substantially weakens the case.

**ASSUMPTIONS (flagged):**
- ASSUMED: 12-month momentum lookback is the primary specification tested.
- ASSUMED: Equal weighting within long and short legs.
- ASSUMED: Monthly end-of-month rebalancing (Quantpedia standard).

---

## Future Research Needed

1. Access full paper for specific Sharpe, CAGR, and max drawdown figures.
2. Test concept on broader commodity universe: 10+ individual commodity ETFs (GLD, SLV,
   USO, UNG, CORN, WEAT, SOYB, etc.) or commodity futures (27+ contracts per Asness 2013).
3. Independent OOS test on 2022–2025 data (commodity supercycle period).
4. Compare to the simpler benchmark: just long DJP/PDBC/COMB (commodity index ETF) — does
   the active strategy add value beyond passive commodity exposure?
5. Investigate whether the correlation filter works on other asset classes (sectors, FX,
   equity factors) as a generalizable timing signal.
