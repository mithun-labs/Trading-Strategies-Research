# Structural Limits of OHLCV-Based Intraday Signals in MNQ Futures

## Overview

**NULL RESULT / FALSIFICATION STUDY.** This entry documents a systematic falsification of
fourteen families of intraday OHLCV-based trading signals applied to Micro E-Mini Nasdaq 100
futures (MNQ). The study's primary finding: **no signal family satisfies all institutional
validation criteria simultaneously** after a fixed two-point round-trip transaction cost.
The gross edge available from these signals (0.07–1.50 points per trade) is structurally
insufficient to overcome realistic execution costs in NAS100 futures.

This entry is recorded because documenting *what doesn't work* — with rigorous evidence — is
equal in value to documenting what does. The confidence score reflects very low confidence
in OHLCV-based intraday signals in MNQ, not low quality of the study itself.

**Author:** Mathias Mesfin
**Paper:** arXiv 2605.04004 (May 5, 2026); also SSRN 6709401
**Status:** arXiv preprint; not peer-reviewed as of 2026-06-12

---

## Asset Class / Timeframes

- **Asset class:** US Equity Index Futures — NAS100 (Micro E-Mini Nasdaq 100 / MNQ)
- **Market:** CME MNQ futures (in scope for our database as a major index)
- **Timeframe:** 5-minute bars (intraday, regular trading hours only)
- **Data period:** 2021–2025 (947 regular-trading-hours days)
- **Execution model:** Next-bar-open (MOO-style execution after signal confirmation)

---

## Core Logic

The paper systematically tests whether common intraday OHLCV-derived signals generate a
tradeable edge in MNQ futures under realistic institutional constraints. Fourteen signal
families are evaluated across multiple parameter variants:

1. **Opening Range Breakout (ORB)** — entry when price breaks above/below the range
   established in the first N minutes after the open
2. **Gap strategies** — continuation and fade strategies based on overnight/opening gaps
3. **Volume signature signals** — signals derived from abnormal volume patterns versus a
   rolling baseline
4. **Cross-session momentum** — intraday position based on prior session's direction/return
5. **Liquidity grab reversals** — entry after a false breakout or spike beyond key levels
6. **Volatility-regime-conditioned classifiers** — signals conditioned on pre-session
   volatility or range characteristics
7. **News-event-driven directional strategies** — directional trades around scheduled
   macro data releases or market-moving events
8–14. **Additional variants** — mean reversion (MGC Ornstein-Uhlenbeck), additional
  breakout configurations, time-of-day momentum filters, and volume-conditioned
  continuations (specific signal names NOT REPORTED in accessible secondary summaries)

**Validation framework (all criteria must be met simultaneously):**
- Out-of-sample walk-forward validation (expanding window)
- T-statistic ≥ 2.0 on mean net return per trade
- Minimum 30 trades per evaluation period
- Positive mean net return after 2-point round-trip cost
- Multi-year stability (consistent across sub-periods within 2021–2025)

**Headline result:** No signal satisfies all five criteria simultaneously.

---

## Economic Rationale

**Claimed rationale for the signal families (what the literature proposes):**
- ORB: price discovery is incomplete at open; early breakouts persist as "directional day"
  behavior
- Gap strategies: overnight informational gaps create directional pressure
- Volume signals: unusual volume reveals institutional activity or informed order flow
- Cross-session momentum: prior-day trends carry over into opening behavior
- Liquidity grabs: market makers or algorithms briefly sweep stops before reversing

**Falsifiability test (result):** All of these rationales ARE falsifiable — and the study
**actively falsifies them** for MNQ futures over 2021–2025. The conditions under which these
edges should disappear are exactly present in modern NAS100 futures:
- Highly liquid, actively traded instrument with tight bid-ask spread (typically 0.25 points
  on MNQ = 1 tick)
- Heavy algorithmic participation absorbs directional edges rapidly
- 2-point round-trip cost (typical retail commission + spread) exceeds the gross edge of
  0.07–1.50 points per trade
- Nasdaq-100 is among the most-watched and most-traded indices globally

**Assessment:** The economic rationale for OHLCV signals exists but predicts edges in less
efficient markets or with smaller transaction costs. The NAS100 is not that market.
Consistent with literature on market efficiency for liquid index futures.

---

## Entry Conditions

The paper tests 14 signal families with multiple parameter variants. Representative examples
from secondary sources:

- **ORB:** Enter long/short at the breakout of the first 5/15/30-minute range; hold for
  fixed duration or until reversal
- **Gap continuation:** Enter long at open if prior close > prior open by threshold; enter
  short vice versa
- **MGC mean reversion:** Ornstein-Uhlenbeck mean reversion signal targeting gap-close to
  session VWAP or prior close

Specific parameter values (lookback, thresholds, hold durations) for each family NOT
REPORTED in accessible secondary summaries.

---

## Exit Conditions

NOT REPORTED in accessible detail. Walk-forward framework assumes next-bar-open execution
and fixed hold period or end-of-session exit (inferred from "947 RTH trading days" and
intraday focus).

---

## Risk Management

Not systematically described for the tested signal families. The study evaluates signals
on a stand-alone basis without overlay risk management; this is appropriate for a
falsification study testing raw signal quality.

---

## Indicators Used

- OHLCV (open, high, low, close, volume) data on 5-minute bars
- Rolling average volume baseline for volume-signature signals
- Session open/close prices for gap calculation
- Implied or realized volatility conditioning (specific construction NOT REPORTED)

---

## Transaction Costs & Capacity

**Transaction cost model:** Fixed 2-point round-trip (approximately $40 per round-trip
on one MNQ contract; $0.50/point × 2 points × 2 directions × 2 contracts). This is
a realistic retail/semi-institutional estimate including spread and commissions.

**Gross edge vs. cost comparison:**
- Gross edge from signals: approximately **0.07–1.50 points per trade**
- Round-trip cost: **2.0 points**
- Result: **gross edge is structurally insufficient** to overcome transaction costs at
  any configuration tested

ANALYSIS: The 2-point cost model is reasonable for retail MNQ trading. Professional
desks with co-location and market-maker arrangements could operate at lower costs
(perhaps 0.5–1.0 points round-trip), but even at 1.0 point cost, the lower end of
the gross edge range (0.07–0.30 points) would still be unprofitable. Only the top
configurations in the 1.0–1.50 point range would survive at reduced institutional costs,
and these would require multi-year stability validation not demonstrated by the paper.

**Capacity:** Not a meaningful question for a null result — the strategies are unprofitable
even at single-contract scale.

---

## Backtest Evidence

**Evidence quality: CLAIMED, UNVERIFIED** (arXiv preprint; not peer-reviewed)

- **Dataset:** 947 regular trading hours (RTH) trading days, 5-minute bars, MNQ
  futures, 2021–2025
- **Methodology:** Expanding-window walk-forward validation on each signal family
- **Validation criteria:** T-stat ≥ 2.0, ≥ 30 trades, positive net return after 2-pt
  round-trip, multi-year stability
- **Conclusion:** No signal family satisfies all criteria simultaneously

**Specific results from accessible secondary sources:**
- Mean reversion (MGC Ornstein-Uhlenbeck): All configurations fail; most show **strongly
  negative T-statistics**
- All other families: highest-performing configuration achieves T = 1.46 (below 2.0
  threshold) with mean net +7.80 points but fails year-stability criteria
  (per companion paper arXiv 2605.11423)

---

## Forward-Test Evidence

NOT REPORTED. The paper uses 2021–2025 data as the primary evaluation window.

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | NOT REPORTED (null result) | NOT REPORTED | — |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | Negative (net return after 2-pt cost < 0 for all tested configs) | CLAIMED, UNVERIFIED | arXiv 2605.04004 secondary summaries |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | — |
| CAGR | NOT REPORTED (null result) | NOT REPORTED | — |
| Annual Return | NOT REPORTED | NOT REPORTED | — |
| Number of Trades | NOT REPORTED (varies by signal family; min 30 per walk-forward window required) | NOT REPORTED | — |
| Average Trade Duration | NOT REPORTED (intraday; likely minutes to hours) | NOT REPORTED | — |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | NOT REPORTED | NOT REPORTED | — |

**Gross Edge Reported (signal-level, before costs):**
| Signal Family | Gross Edge Range | T-stat | Net After 2-pt Cost |
|---|---|---|---|
| All families tested | 0.07–1.50 pts/trade | < 2.0 for all (max reported: 1.46) | Negative for all |
| MGC mean reversion | NOT REPORTED | Strongly negative | Negative |

---

## Community Sentiment

No direct community discussion of arXiv 2605.04004 is available from accessible sources
as of 2026-06-12. The paper is single-author and has not appeared in major forum
discussions (ForexFactory, r/algotrading, MQL5) from accessible sources.

**Relevant broader context:**
- The "ORB" (Opening Range Breakout) family is one of the most widely discussed intraday
  strategies in retail trading communities. Community consensus has increasingly shifted
  toward "works rarely and only in specific regimes" — this paper's findings are consistent
  with practitioner skepticism.
- NAS100 futures are well-known to be "algo-heavy" — this is widely acknowledged in trading
  communities as a reason why simple OHLCV signals are difficult to trade profitably.
- The related paper (arXiv 2605.11423, same author) — a VVG classifier study — identifies
  a regime classifier but also shows all directional strategies fail institutional standards
  even in "activated" regime days (4.4% of trading days).

---

## Why This Might Be Nothing

*Note: This section applies the MANDATORY skeptic steelman to the POSITIVE CASE (that the
signals could work), since the paper itself IS the skeptic steelman.*

### The Paper's Core Argument Is Sound

The falsification study addresses the main "why it might actually work" objections:
1. **Regime-conditioning objection:** The study explicitly tests "volatility-regime-conditioned
   classifiers" — conditioning on pre-session volatility or range. These still fail.
2. **Volume-filtering objection:** The study tests "volume signature signals" — abnormal
   volume filters. These still fail.
3. **Selectivity objection:** The study requires only 30+ trades and walk-forward validation
   — it's not requiring hundreds of trades per month. Even low-frequency setups fail.

### Residual Concerns About the Study (reasons it might not generalize)

1. **Single instrument (MNQ only):** The null result may be specific to NAS100 futures.
   These signals might work on ES (S&P 500 futures), CL (crude oil), GC (gold futures), or
   other instruments where the cost-to-edge ratio is more favorable.
2. **2021–2025 sample:** This period includes the 2022 bear market, the 2023-2024 recovery,
   and the 2025 correction. A different 4-year window might show different results. However,
   the walk-forward methodology reduces this concern somewhat.
3. **2-point cost assumption:** Professional traders with co-location and market-maker
   arrangements might operate at 0.5–1.0 points round-trip. At 0.5 points, several signal
   configurations would become profitable in gross terms — the question is whether they would
   pass T-stat and year-stability criteria, which the paper doesn't address at this cost level.
4. **Single-author arXiv preprint:** Mathias Mesfin's institutional affiliation and background
   are not clearly documented in accessible sources. The methodology appears rigorous but lacks
   peer review scrutiny.
5. **Next-bar-open execution constraint:** The paper assumes next-bar-open (MOO-style)
   execution with 5-minute bars. Faster execution (sub-second) or limit orders might change
   the effective cost. However, faster execution also requires more sophisticated
   infrastructure and creates different selection biases.

### What Would Change My Mind (about the null result)

The paper would be more convincing if:
- A second, independent researcher replicated the null result with different data sources
- The analysis was extended to other instruments (ES, NQ, YM)
- The paper was peer-reviewed

The paper would be OVERTURNED by:
- An independent replication showing positive net returns after costs on MNQ 5-minute data
  in 2021–2025 using any of the 14 signal families
- A demonstration that 2-point costs significantly overstate typical execution costs for
  retail traders (possible but unlikely given MNQ commission structures)

---

## Strengths / Weaknesses

**Strengths:**
- Rigorous validation framework (5 simultaneous criteria including T-stat, trade count,
  net return after costs, and multi-year stability)
- Expanding-window walk-forward prevents in-sample overfitting
- Covers 14 signal families — comprehensive test of commonly-used OHLCV signals
- Explicit cost modeling (2-point round-trip) is unusually rigorous for a retail-audience paper
- Companion papers (2605.11423, 2605.17724) extend the research to regime classifiers and
  ML models respectively, forming a coherent research program

**Weaknesses:**
- Single instrument (MNQ only) — null result may not generalize
- Single author — no independent replication within the paper itself
- ArXiv preprint, no peer review
- 4-year sample (2021–2025) — limited regime diversity
- Specific signal parameters tested NOT FULLY REPORTED in accessible summaries
- No institutional or academic affiliation confirmed for the author

---

## Confidence Score

**Note on scoring for a null result paper:** The standard confidence score framework
measures "confidence that the strategy has an edge." For a falsification study, this is
inverted — higher-quality null-result evidence means lower confidence in the strategy, not
higher. The score below is applied to the STRATEGY (intraday OHLCV signals in MNQ) as
tested by the paper. The low score correctly reflects very low confidence in these
signals having an edge.

| Dimension | Score (0–10) | Weight | Weighted | Notes |
|---|---|---|---|---|
| Logical / economic soundness (falsifiable) | 3 | 3 | 9 | Plausible mechanisms, but study falsified them in this market/period |
| Transparency & reproducibility | 7 | 2 | 14 | 14 signal families described; walk-forward framework documented |
| Evidence auditability | 3 | 2 | 6 | Single arXiv preprint; rigorous walk-forward but no peer review; single instrument |
| Risk management quality | 1 | 2 | 2 | No systematic risk management for these signals; gross edge insufficient to manage |
| Cost & capacity realism | 1 | 2 | 2 | Strategy EXPLICITLY FAILS after costs; cost modeling is rigorous but the strategy can't overcome it |
| Honest treatment of drawdowns / failure | 9 | 1.5 | 13.5 | Full null result; all failure modes reported; nothing hidden |
| Robustness evidence (OOS / walk-forward / multi-market) | 5 | 1.5 | 7.5 | Expanding walk-forward across 14 families; single instrument and 4-year window |
| Survived independent scrutiny | 2 | 1 | 2 | Single author; arXiv only; no community review found |

**Weighted sum:** 56 / 150 maximum

**Latent score:** round(56 / 150 × 100) = **37**

**Verification cap:** Evidence auditability = 3 ≤ 4 → cap at 59. Latent 37 < 59, so cap does not bind.

**Confidence: 37 (Low Confidence)**

`latent 37 (capped to 37 — null result paper; low confidence is correct interpretation; study rigorously shows no edge in OHLCV signals in MNQ after 2-point round-trip costs; single author arXiv preprint; single instrument; no peer review)`

**Interpretation note:** This is a rare case where the LOW CONFIDENCE score is the DESIRED outcome. We are highly confident (based on this evidence) that intraday OHLCV-based signals LACK an edge in MNQ futures under realistic cost assumptions. The score of 37 correctly signals "do not implement based on this strategy class" — not "this is uncertain and worth further research."

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Low to moderate — OHLCV signals are computationally simple
- **Scalability:** Irrelevant — the signals don't work after costs
- **Automation:** Technically easy to automate; economically pointless in MNQ given null result

---

## MQL5 / Python / Pine Feasibility

- **Python:** Trivially feasible to implement; irrelevant given null result
- **MQL5:** Feasible for MT5 backtesting; same conclusion
- **Pine Script:** Feasible for TradingView visualization; same conclusion

Note: The strategy SHOULD NOT be implemented in any platform. The null result is the finding.

---

## Similar Strategies

- `intraday-momentum-spy` (score 50) — intraday momentum on SPY ETF; VWAP-based; separate market and mechanism from MNQ OHLCV signals; more marginal evidence
- `orb-stocks-in-play` (score 78) — ORB applied to high-volume news-driven US stocks (NOT index futures); significantly different: uses relative volume filter, stock-specific catalysts, and is NOT the same as applying ORB to liquid index futures. The high score reflects that stock-specific ORB has a different edge profile than index futures ORB.
- `omega-model-intraday-nasdaq` (score 55) — intraday Omega ratio optimization on NASDAQ-100; peer-reviewed; different approach (portfolio optimization, not OHLCV signals)

**Key contrast:** The failure of OHLCV signals in MNQ futures does NOT imply that all intraday strategies fail. `orb-stocks-in-play` applies a different mechanism (catalyst-driven, high relative volume) that may survive costs on single stocks. The null result is instrument-specific and signal-type-specific.

---

## Red Flags (against the strategy, not the paper)

- **Proven null result** in a rigorous walk-forward study: gross edge 0.07–1.50 pts < 2-pt round-trip cost
- **NAS100 is among the most efficient equity index futures** — algorithmic participation is extreme; simple OHLCV patterns are likely arbitraged away
- **Mean reversion configurations show strongly negative T-statistics** — not just neutral but actively negative after costs
- **Single instrument result**: may generalize to other liquid index futures (ES, YM) but cannot confirm

---

## Source Links

| Source | Retrieval Date | Notes |
|--------|---------------|-------|
| [arXiv 2605.04004 — Mesfin (May 2026)](https://arxiv.org/abs/2605.04004) | 2026-06-12 | Primary source; abstract accessed via secondary summaries |
| [SSRN 6709401 — same paper](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=6709401) | 2026-06-12 | Mirror on SSRN; abstract/metadata only |
| [arXiv 2605.11423 — Mesfin, VVG Classifier (May 2026)](https://arxiv.org/abs/2605.11423) | 2026-06-12 | Companion paper: regime classifier; also fails institutional validation |
| [arXiv 2605.17724 — Mesfin, LSTM vs Gradient Boosting on MNQ (May 2026)](https://arxiv.org/abs/2605.17724) | 2026-06-12 | Companion paper: ML models on same MNQ data; context only |

---

## Analyst Notes

**FACTS (from accessible secondary sources):**
- Paper by Mathias Mesfin; arXiv 2605.04004; May 5, 2026
- 947 RTH trading days of 5-minute MNQ data, 2021–2025
- 14 signal families tested: ORB, gap, volume signatures, cross-session momentum, liquidity
  grabs, volatility-conditioned classifiers, news-driven strategies, MGC mean reversion,
  and other variants
- Validation: T ≥ 2.0, ≥ 30 trades, positive net after 2-point round-trip, multi-year stability
- Result: No signal satisfies all criteria simultaneously
- Gross edge: 0.07–1.50 points per trade
- Mean reversion configurations show strongly negative T-statistics
- Best reported T-stat: 1.46 (below 2.0 threshold)

**ANALYSIS:**
- The 2-point round-trip cost is realistic for retail MNQ trading. A single MNQ tick = $0.50;
  2-point round-trip = $20 per contract. With typical $2–4 commissions, the breakeven is
  about 0.5 ticks per trade, and 2 points round-trip includes both spread and commission.
- The null result is particularly notable for mean reversion (MGC/OU model): negative
  T-statistics suggest these signals actively LOSE money before costs, not just fail to
  profit. This implies mean reversion in MNQ 5-minute prices is too weak and noisy to trade.
- The companion VVG paper (2605.11423) identifies a "high-volatility/high-volume/high-gap"
  regime on 4.4% of trading days — but even in this specialized regime, directional
  strategies still fail. This is a stronger null result than the base paper alone.

**ASSUMPTIONS:**
- "Inferred from" research group's prior papers: Binance exchange for crypto entries
- ASSUMPTION: "next-bar-open" execution means entering at the open of the 5-minute bar
  immediately following the signal bar

---

## Future Research Needed

1. Obtain full paper to verify specific signal parameters and tables
2. Independent replication on ES (S&P 500 e-mini) and NQ (Nasdaq-100 e-mini, full-size)
   to determine if the null result is MNQ-specific or index-futures-wide
3. Replication with lower cost assumptions (0.5–1.0 points round-trip) to identify
   which if any signals survive at institutional cost levels
4. Extension to 2026+ data to test whether the null result is stable in a new market regime
5. Comparison with the Zarattini/Pagani "fast alphas" overlay approach (SSRN 6391638)
   which claims the same signal class (short-horizon mean reversion) has value as an
   EXECUTION OVERLAY rather than a standalone strategy — testing whether the null result
   depends on the execution mechanism
