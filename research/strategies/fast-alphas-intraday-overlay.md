# Fast Alphas: Short-Horizon Mean-Reversion as Intraday Trend Execution Overlay

## Overview

A two-component intraday strategy for SPY ETF that combines a base ATR-based trend-
following strategy with a "fast alpha" execution overlay. The key insight: a short-horizon
streak-based mean-reversion signal is unprofitable when traded directly (high turnover
erases the gross edge of CAGR 31.9% / Sharpe > 2), but adds net value when deployed as
an execution timing filter — waiting for short-term counter-moves (pullbacks) before
entering trend-aligned trades.

The baseline strategy achieves Sharpe 0.87; the enhanced (overlay) version achieves
Sharpe 0.99 with CAGR ~200 bps higher.

**Authors:** Carlo Zarattini, Alberto Pagani (Concretum Group / Swiss Finance Institute)
**Paper:** SSRN 6391638 (March 11, 2026); also at Concretum Group PDF (February 2026
working version)
**Status:** SSRN working paper; not peer-reviewed as of 2026-06-12
**Related prior work by same group:** SSRN 4824172 ("Beat the Market" — intraday SPY
momentum, score 50 in our database as `intraday-momentum-spy`)

---

## Asset Class / Timeframes

- **Asset class:** US Equities — ETF
- **Market:** SPY (SPDR S&P 500 ETF Trust)
- **Timeframe:** 5-minute bars (intraday)
- **Data period tested:** January 2007 – January 2026 (19 years)
- **Holding period:** Intraday (positions closed by EOD, inferred from prior Zarattini work
  on SPY intraday strategies)

---

## Core Logic

**Two-layer architecture:**

**Layer 1 — Base strategy (ATR-based intraday trend-following):**
- An ATR (Average True Range) volatility-based intraday trend-following framework on SPY
  5-minute bars
- The baseline strategy generates long/short entries based on intraday momentum/trend signals
- Standalone performance: Sharpe 0.87 (net of costs, 2007–2026)
- Specific ATR parameters and exact entry/exit rules NOT REPORTED in accessible sources

**Layer 2 — Fast alpha execution overlay (streak-based mean reversion):**
- A short-horizon mean-reversion signal constructed from price "streaks" (consecutive up or
  down 5-minute bars)
- Gross performance standalone: CAGR ~31.9%, Sharpe > 2 (before costs)
- After transaction costs: UNPROFITABLE due to high turnover
- As overlay: the signal conditions the TIMING of trend entries — the model waits for a
  short-term counter-move (pullback against the trend direction) before entering a
  trend-aligned position
- Effective result: better entry pricing within the trend direction; reduced adverse
  selection

**Combined performance:** Sharpe rises from 0.87 to 0.99; CAGR increases by ~200 bps.

---

## Economic Rationale

**Proposed mechanism:**
1. **Base trend edge:** SPY intraday trends persist within session (documented in multiple
   Zarattini papers). The ATR-based strategy captures this momentum.
2. **Fast alpha as execution improvement:** Intraday 5-minute returns exhibit short-horizon
   mean reversion. This creates predictable "better entry" windows within a trend: after a
   brief pullback, the next bar is more likely to move in the trend direction. Using this
   signal for entry timing improves the average entry price.

**Falsifiability test:**
The combined edge should disappear when:
- SPY intraday trends (the base edge) decay — this has been documented partially by the
  community (see `intraday-momentum-spy` notes on leverage inflation)
- Market microstructure changes reduce the short-horizon mean reversion (e.g., increased
  HFT activity absorbing the pullback pattern more quickly)
- SPY trading costs rise (they have historically trended to near-zero for large institutions)

**Assessment:** Partially falsifiable. The rationale for the overlay (better entry timing via
mean-reversion) is the more novel and interesting part. The base trend story is well-
documented in the Zarattini group's prior work. Combined, the rationale is specific enough
to predict when it should fail.

**Important cross-reference:** Mesfin (arXiv 2605.04004, `structural-limits-ohlcv-mnq` in
our database) found that short-horizon mean-reversion signals in MNQ futures FAIL as
standalone strategies after costs — consistent with Zarattini/Pagani's finding that the
fast alpha fails standalone. The two papers AGREE on the standalone failure and DISAGREE
only on whether the signal has value as an overlay. The overlay value is not tested by Mesfin.

---

## Entry Conditions

**Base strategy (ATR-based intraday trend):**
NOT REPORTED in accessible detail. Based on the Zarattini group's prior work:
- ASSUMPTION: ATR multiplier applied to determine intraday trend direction from a reference
  level (session open or prior close)
- ASSUMPTION: Long signal when price trends above ATR channel; short signal when below

**Overlay (fast alpha):**
- ASSUMPTION: Streak-based signal counts consecutive 5-minute bars in the same direction
- When the base strategy signals long AND a short-term pullback (down streak) occurs,
  enter long at the pullback
- When the base strategy signals short AND a short-term up streak occurs, enter short at
  the up streak
- Specific streak length, threshold, and timing rules NOT REPORTED

---

## Exit Conditions

NOT REPORTED in accessible detail. Based on the Zarattini group's prior work, likely
end-of-session close or ATR-trailing stop.

---

## Risk Management

NOT REPORTED in full detail from accessible sources.

ASSUMPTION: Some form of position sizing linked to ATR (standard in trend-following
frameworks). The paper explicitly models transaction costs (the key finding requires it),
suggesting realistic position sizing and not extreme leverage.

---

## Indicators Used

- ATR (Average True Range) — base trend signal
- Streak-based mean-reversion signal (consecutive 5-minute bars in same direction)
- SPY 5-minute OHLCV data

---

## Transaction Costs & Capacity

**Transaction costs:** Explicitly modeled. The paper's central contribution is showing that
the fast alpha signal is UNPROFITABLE standalone due to costs but PROFITABLE as an overlay.
Specific cost model (spread, commission, slippage per trade) NOT REPORTED from accessible
sources.

**Capacity:** Limited to SPY ETF. SPY is the most liquid US ETF with essentially unlimited
capacity for retail and institutional sizes within the base strategy's position sizing.
However, the overlay generates frequent small position adjustments, which may have different
capacity characteristics at institutional scale.

**Analysis:** The explicit modeling of transaction costs is a significant positive relative
to most SSRN working papers. The cost-inclusive analysis is the paper's core contribution.

---

## Backtest Evidence

**Evidence quality: CLAIMED, UNVERIFIED** (SSRN working paper; March 2026)

- **Dataset:** SPY 5-minute bars, January 2007 – January 2026 (19 years)
- **Backtest type:** Full-sample; no explicit walk-forward split confirmed from accessible
  sources
- **Baseline Sharpe:** 0.87 (net of costs, base ATR trend strategy)
- **Enhanced Sharpe:** 0.99 (net of costs, with fast alpha overlay)
- **CAGR improvement:** ~200 bps above baseline
- **Standalone fast alpha gross CAGR:** ~31.9%; Sharpe > 2 (gross, before costs)
- **Standalone fast alpha net:** Unprofitable after costs

---

## Forward-Test Evidence

NOT REPORTED. The paper uses 2007–2026 as the primary evaluation window.

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | 0.99 (enhanced); 0.87 (baseline) | CLAIMED, UNVERIFIED | SSRN 6391638 secondary summaries |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | — |
| CAGR | Baseline + ~200 bps (exact CAGR NOT REPORTED) | CLAIMED, UNVERIFIED | SSRN 6391638 secondary summaries |
| Annual Return | NOT REPORTED | NOT REPORTED | — |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | Intraday (estimated hours; exact NOT REPORTED) | NOT REPORTED | — |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | NOT REPORTED | NOT REPORTED | — |

**Standalone fast alpha (gross, before costs):**
| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| CAGR | ~31.9% | CLAIMED, UNVERIFIED | SSRN 6391638 secondary summaries |
| Sharpe Ratio | > 2 | CLAIMED, UNVERIFIED | SSRN 6391638 secondary summaries |

**Notes:**
- The 31.9% gross CAGR of the standalone fast alpha is below the 50% automatic scrutiny
  trigger. Sharpe > 2 is below the 3.0 trigger. No automatic scrutiny applies.
- The Sharpe improvement (0.87 → 0.99) is modest (+0.12 or ~14% relative improvement)
  but over a 19-year period in a liquid, well-studied market, this is meaningful if robust.

---

## Community Sentiment

No direct independent community validation found for SSRN 6391638 as of 2026-06-12.

**Relevant broader context:**
- Zarattini is a well-known practitioner researcher with multiple credible papers from
  Swiss Finance Institute / Concretum Group. Prior papers (including `intraday-momentum-spy`,
  `industry-trend-century`, `catching-crypto-trends-donchian-ensemble`) show a consistent
  methodology and tend toward conservative, well-documented claims.
- The concept of "execution alpha" (using fast signals for timing rather than direction)
  has some support in market microstructure literature (arrival price algorithms, VWAP
  slippage reduction). This paper extends that idea to retail-accessible strategy design.
- QuantifiedStrategies.com and QuantSeeker have referenced Zarattini's broader SPY intraday
  work positively, indicating community recognition.

---

## Why This Might Be Nothing

### 1. In-Sample Optimization (Streak Parameters)

The streak-based mean-reversion signal likely has key parameters (streak length, threshold
for counter-move magnitude) that were optimized on the same 2007–2026 sample. A +200bps
CAGR improvement over 19 years could be entirely explained by in-sample fitting of these
parameters. Without a walk-forward confirmation (no specific mention in accessible
summaries), this is the primary concern.

### 2. Data-Mining on a Single Instrument

The overlay is tested on SPY only. If the mean-reversion overlay was selected from among
many possible intraday signals, the 200bps improvement could reflect multiple testing bias.
The Zarattini group likely has robust methodology, but accessible sources don't confirm
whether this was the only signal tested or one of many.

### 3. Base Strategy Decay Risk

The base trend strategy's Sharpe of 0.87 is itself based on `intraday-momentum-spy` (SSRN
4824172), which our database rates 50 (Experimental) with a note about leverage-inflated
Sharpe and concerns about strategy decay. If the base strategy's edge decays post-2026,
the overlay becomes irrelevant.

### 4. Transaction Cost Sensitivity

The overlay is designed for a cost-inclusive analysis, but the specific cost assumptions
(spread, commissions per trade) are not confirmed in accessible sources. If the overlay
generates many additional "entry timing" adjustments, each with transaction costs, the
200bps improvement could be sensitive to cost assumptions.

### 5. Maximum Drawdown Not Reported

The overlay improves Sharpe (0.87 → 0.99), but max drawdown is NOT REPORTED. If the
improvement comes with higher tail risk (e.g., the overlay occasionally doubles down on
a losing trend), the risk-adjusted improvement may be illusory.

### 6. What Would Change My Mind

- Walk-forward OOS validation showing the 200bps improvement holds in unseen data
- Replication on similar instruments (QQQ, IWM, ES futures)
- Open code with exact signal construction

---

## Strengths / Weaknesses

**Strengths:**
- Transaction costs explicitly modeled and central to the paper's thesis
- Novel concept: explains how signals that fail standalone can add execution value
- Credible authors (Zarattini group; proven track record of rigorous intraday SPY research)
- 19-year backtest provides substantial data
- The core insight (execution timing overlay) is intellectually coherent and supported by
  microstructure theory
- Related prior work (`intraday-momentum-spy`) has been consistently documented

**Weaknesses:**
- SSRN working paper; no peer review
- No confirmed walk-forward OOS validation
- Specific signal parameters (streak length, ATR period) NOT REPORTED
- Single instrument tested (SPY only)
- Max drawdown NOT REPORTED
- The +200bps / Sharpe 0.87→0.99 improvement is modest — within in-sample noise range
- Overlap with prior Zarattini work may mean the improvement is marginal over what is
  already documented

---

## Confidence Score

| Dimension | Score (0–10) | Weight | Weighted | Notes |
|---|---|---|---|---|
| Logical / economic soundness (falsifiable) | 7 | 3 | 21 | Clear mechanism (execution timing); falsifiable (base trend decay, microstructure change); well-specified |
| Transparency & reproducibility | 6 | 2 | 12 | Framework described; specific parameters (ATR period, streak threshold) NOT REPORTED from accessible sources |
| Evidence auditability | 3 | 2 | 6 | SSRN working paper; not peer-reviewed; no confirmed walk-forward; 19-year single-instrument sample |
| Risk management quality | 5 | 2 | 10 | ATR-linked sizing implied; costs explicitly modeled; max DD and stop rules NOT REPORTED |
| Cost & capacity realism | 7 | 2 | 14 | Costs explicitly modeled — central to paper; SPY capacity not a constraint; overlay cost sensitivity unclear |
| Honest treatment of drawdowns / failure | 6 | 1.5 | 9 | Baseline underperformance acknowledged; standalone failure documented; max DD NOT REPORTED |
| Robustness evidence (OOS / walk-forward / multi-market) | 4 | 1.5 | 6 | 19-year period; single instrument; no confirmed OOS split or multi-market testing |
| Survived independent scrutiny | 4 | 1 | 4 | Zarattini group has credibility; no independent replication of this specific paper |

**Weighted sum:** 82 / 150 maximum

**Latent score:** round(82 / 150 × 100) = **55**

**Verification cap:** Evidence auditability = 3 ≤ 4 → cap at 59. Latent 55 < 59, cap does not bind.

**Confidence: 55 (Experimental)**

`latent 55 (capped to 55 — evidence auditability 3/10; SSRN working paper; no confirmed walk-forward OOS; specific parameters NOT REPORTED; max DD NOT REPORTED; single instrument backtest 2007–2026)`

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Moderate — requires ATR-based trend framework plus streak counter logic;
  manageable in any systematic trading platform
- **Scalability:** High for SPY (unlimited liquidity); not tested on other instruments
- **Automation:** Feasible in Python (5-min OHLCV + ATR + streak counter); MQL5 feasible
  for MT5 backtesting

---

## MQL5 / Python / Pine Feasibility

- **Python:** Feasible with yfinance or broker API for 5-min SPY data; ATR and streak
  counter are standard indicators. Signal construction requires full paper for exact
  parameters.
- **MQL5:** Feasible; SPY can be approximated by US500 CFD in MT5
- **Pine Script (TradingView):** Feasible for visualization and development; execution
  research would then move to Python/MQL5

**Implementation note:** The paper's full signal construction (ATR parameters, streak
definition) is NOT ACCESSIBLE from secondary sources. Implementation requires obtaining
the full paper.

---

## Similar Strategies

- `intraday-momentum-spy` (score 50) — Zarattini/Aziz/Barbon (SSRN 4824172); the BASE
  intraday SPY strategy that this paper likely builds upon. The ATR-based trend in this
  paper is almost certainly the same or similar system.
- `structural-limits-ohlcv-mnq` (score 37, NULL RESULT) — Mesfin (arXiv 2605.04004);
  shows that the same type of signal (short-horizon OHLCV mean reversion) fails standalone
  in MNQ futures — consistent with Zarattini/Pagani's standalone failure finding.

---

## Red Flags

- **No confirmed OOS validation** — the 200bps improvement may reflect in-sample
  optimization of streak parameters over 19 years of SPY data
- **Parameters not disclosed** — cannot replicate or independently verify
- **Max drawdown not reported** — incomplete risk profile for the enhanced strategy
- **Single instrument** — SPY-specific; no multi-market robustness
- **SSRN working paper only** — no peer review
- **Modest improvement** (Sharpe 0.87 → 0.99): statistically marginal in a single
  backtest without bootstrap or permutation testing confirmation

---

## Source Links

| Source | Retrieval Date | Notes |
|--------|---------------|-------|
| [SSRN 6391638 — Zarattini/Pagani (March 2026)](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=6391638) | 2026-06-12 | Primary source; HTTP 403 on direct access |
| [Concretum Group QuanTip post](https://concretumgroup.com/quantip-improving-performance-with-fast-alphas-a-tactical-overlay-for-intraday-trend-trading/) | 2026-06-12 | Summary (HTTP 403); metadata from secondary search |
| [intraday-momentum-spy strategy file](../strategies/intraday-momentum-spy.md) | 2026-06-12 | Related base strategy by same group |
| [structural-limits-ohlcv-mnq strategy file](../strategies/structural-limits-ohlcv-mnq.md) | 2026-06-12 | Cross-reference: standalone mean-reversion failure confirmed in MNQ |

---

## Analyst Notes

**FACTS (from accessible secondary sources):**
- Paper by Zarattini and Pagani; SSRN 6391638; March 11, 2026
- Data: 5-minute SPY, January 2007 – January 2026
- Fast alpha: streak-based mean-reversion signal; gross CAGR ~31.9%, Sharpe > 2; unprofitable after costs as standalone
- Base strategy: ATR-based intraday trend-following on SPY
- Combined Sharpe: 0.87 (baseline) → 0.99 (with overlay); CAGR +~200bps
- Mechanism: overlay waits for short-term pullbacks before executing trend-aligned entries

**ANALYSIS:**
- The improvement from 0.87 to 0.99 Sharpe is statistically modest over a 19-year sample.
  ANALYSIS: Standard error of Sharpe estimate ≈ 1/√T where T is years; over 19 years, the
  SE ≈ 0.23, so the improvement of 0.12 is about half a standard error — not statistically
  significant on its own without bootstrap tests.
- This is an EXECUTION ALPHA, not a traditional strategy edge. The paper's contribution
  is methodological (showing how failed fast signals can still add value) as much as it is
  a specific strategy recommendation.
- Cross-reference with `structural-limits-ohlcv-mnq`: Mesfin showed that short-horizon
  mean-reversion in MNQ 5-min data fails standalone and is ACTIVELY NEGATIVE for mean-
  reversion configurations. Zarattini/Pagani show the same failure in SPY but argue the
  signal is valuable as a timing overlay. These findings are consistent and complementary.

**ASSUMPTIONS:**
- The base "ATR-based intraday trend" is the same or a close variant of the strategy in
  SSRN 4824172 (`intraday-momentum-spy`)
- "Streak" is defined as consecutive same-direction 5-minute bars (exact count NOT CONFIRMED)

---

## Future Research Needed

1. Obtain full paper for exact signal construction (ATR parameters, streak definition)
2. Walk-forward OOS validation: split 2007–2026 into in-sample/out-of-sample periods
3. Multi-instrument replication: QQQ, IWM, ES futures
4. Bootstrap/permutation testing on the 200bps improvement to confirm statistical
   significance
5. Full risk metrics: max drawdown, Sortino, recovery factor for the enhanced strategy
6. Specific transaction cost model: how sensitive is the improvement to cost assumptions?
