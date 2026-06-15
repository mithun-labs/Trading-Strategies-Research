# Enhanced Momentum with Momentum Transformers

## Overview

A short (7-page) arXiv preprint extending the original Momentum Transformer architecture
(Wood/Roberts/Zohren, arXiv 2112.08534, Oxford-Man Institute) from futures to US equities.
The authors combine an LSTM with a multi-head attention mechanism to learn an optimal
portfolio for the next trading day from time-series momentum features. Primary result:
annualized return 4.14% / Sharpe 1.12 CLAIMED after correcting an initial data-leakage bug.

Authors: Max Mason, Waasi A Jagirdar, David Huang, Rahul Murugan (submitted December 17, 2024).
Note: the queue entry attributed authorship to "Moraes et al." — this is incorrect.

## Asset Class / Timeframes

- Asset class: US Equities (individual stocks; specific index/universe NOT REPORTED)
- Timeframe: Daily (daily rebalancing implied)
- Holding period: NOT REPORTED

## Core Logic

1. Collect daily OHLCV for a universe of US equities.
2. Feed a lookback window of 21 trading days (reduced from the intended 21+126-day multi-scale
   window due to computational memory constraints) into an attention-LSTM hybrid.
3. The model outputs position weights (continuous, not binary signals) for each stock.
4. Positions are sized to target time-series momentum: long (short) on stocks with positive
   (negative) momentum signals.
5. Rebalance daily.

Architecture difference from baseline: the attention mechanism provides the LSTM with direct
access to all prior time steps in the training window (vs. sequential LSTM which decays earlier
information), intended to capture longer-term dependencies and adapt to regime shifts.

## Economic Rationale

Anchored to time-series momentum (Moskowitz/Ooi/Pedersen 2012): persistent positive
autocorrelation in returns exists due to underreaction to information, herding, and
institutional trend-following. The attention mechanism is theoretically motivated by allowing
the model to weight different historical lookback windows dynamically.

**Falsifiability:** The mechanism predicts the edge disappears during sharp momentum reversals
(e.g., 2009 "momentum crash," May 2020), during high-correlation regimes where cross-sectional
dispersion collapses, and at high transaction cost environments where daily rebalancing erases
gross signal. However, the paper does not formally state these failure conditions.

**ANALYST ANALYSIS:** The attention-based rationale is applied after the fact (why attention
outperforms LSTM on this dataset). The paper does not state when attention-LSTM should be
expected to *underperform* a simple TSMOM baseline, which weakens the falsifiability of the
mechanism claim.

## Entry Conditions

- NOT REPORTED explicitly.
- Implied: daily signal from the attention-LSTM model produces a continuous position weight
  (long or short) for each stock in the universe.

## Exit Conditions

- NOT REPORTED explicitly.
- Implied: daily rebalancing — positions are re-weighted each day by the model's new output.

## Risk Management

- NOT REPORTED. No stop-loss, position limits, max drawdown exit, or sizing rules described
  in accessible sources.
- The original Momentum Transformer (Wood et al.) used volatility-scaled position sizing;
  it is unclear whether the enhanced paper inherits this.

## Indicators Used

- Time-series momentum signal (TSMOM) features from 21-day lookback (reduced from multi-scale
  21+126 day due to computational constraints)
- LSTM + multi-head attention architecture learns optimal combination

## Transaction Costs & Capacity

- The paper claims the architecture "enhances performance in scenarios accounting for
  transaction costs." The exact cost model (bps/trade, spread, commissions) is NOT REPORTED
  in accessible sources.
- **Concern (ANALYST ANALYSIS):** Daily rebalancing on individual US equities involves
  substantially higher transaction costs than the futures contracts used by the original
  Momentum Transformer. For mid/small-cap stocks, bid-ask spreads of 5–20 bps per side are
  common, plus commissions. At daily rebalancing frequency, round-trip costs of 10–40 bps/day
  would annualize to 25–100% of portfolio turnover cost. A gross Sharpe materially higher than
  1.12 would be needed to net 1.12 after such costs — and the paper does not report a gross
  Sharpe or cost decomposition.
- Capacity: NOT REPORTED. Likely limited by universe size and daily liquidity at daily
  rebalancing frequency.

## Backtest Evidence

NOT REPORTED formally. Training period was 7 years (reduced from 10 due to memory
constraints). The exact train/test split dates are NOT REPORTED. Results are on a held-out
test set of unknown duration.

**FACT:** A data leakage bug was detected and corrected during the research: "inflated Sharpe
ratios due to improper handling of dataset timestamps, particularly with companies having
multiple stock instances." The reported Sharpe 1.12 is the post-correction result.

## Forward-Test Evidence

NOT REPORTED.

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | 1.12 | CLAIMED, UNVERIFIED | arXiv 2412.12516 (post-leakage correction) |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | — |
| CAGR | NOT REPORTED | NOT REPORTED | — |
| Annual Return | 4.14% (avg) | CLAIMED, UNVERIFIED | arXiv 2412.12516 |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | NOT REPORTED | NOT REPORTED | — |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | NOT REPORTED | NOT REPORTED | — |
| Benchmark Improvement | +5.21% annual return vs base momentum (CLAIMED) | CLAIMED, UNVERIFIED | Secondary source summarizing arXiv 2412.12516 |

**Note:** The "Sharpe ratio improvement of 1.3" cited in one secondary source is ambiguous —
it could mean the absolute Sharpe is 1.12 while the baseline TSMOM is approximately −0.18
(improvement of ~1.3), or that the improvement *is* 1.3 Sharpe units. This inconsistency
cannot be resolved from accessible sources and is noted as ANALYST ANALYSIS uncertainty.

## Community Sentiment

No community discussion, forum threads, replications, or citations found in accessible sources
as of June 2026. The paper is 18 months old on arXiv with no peer-reviewed publication and
no independent critique found.

## Why This Might Be Nothing

1. **Data leakage was detected and corrected — residual risk persists (FACT):** The authors
   found inflated Sharpe ratios from improper timestamp handling with multi-instance companies.
   After correction, Sharpe dropped to 1.12. This is a significant red flag: it demonstrates
   the authors were not initially aware of a fundamental data hygiene problem. The probability
   of residual subtle leakage (e.g., point-in-time accounting data, survivorship bias, index
   constituency look-ahead) in a 7-page paper cannot be dismissed.

2. **Course-project-level paper, no established research group:** The paper is 7 pages with
   5 figures. The original Momentum Transformer (Wood/Roberts/Zohren) came from the Oxford-Man
   Institute with 20+ pages, open code (GitHub), peer review process underway, and eventual
   Risk.net publication. This extension has none of those attributes. The short format precludes
   adequate methodology documentation.

3. **Computational constraints degraded the methodology:** The lookback was reduced from the
   intended multi-scale (21+126 day) to 21 days only; the training period was reduced from 10
   to 7 years. Both of these constraint-driven decisions create a mismatch between the original
   architecture's design and what was actually tested. A Sharpe of 1.12 on a constrained version
   of the architecture tells us little about the full architecture's performance.

4. **Transaction cost model opaque:** Daily rebalancing on individual equities is extremely
   expensive. A claim that transaction costs are "accounted for" without specifying the cost
   model is not verifiable. If the cost model is 0–1 bps (futures-style), it would materially
   underestimate actual equity trading costs and invalidate the result.

5. **Survivorship bias in equity universe likely (ANALYST ANALYSIS):** The paper does not
   specify the stock universe or confirm point-in-time index membership. Standard survivorship
   bias on US equities (using current index members as if they were always in the index)
   inflates momentum returns by 1–3% per year in the literature.

6. **No out-of-sample framework:** The paper uses a train/test split, not walk-forward or
   time-series cross-validation. The test set period is unknown, making it impossible to assess
   whether results are from a favorable regime.

7. **The most likely benign explanation:** A Sharpe 1.12 on a daily-rebalanced equity momentum
   strategy is plausible if (a) survivorship-biased universe, (b) minimal transaction costs
   assumed, and (c) favorable test period. None of these can be confirmed.

8. **What would change my mind:** An independent replication on a survivorship-corrected
   universe with point-in-time data, explicit cost model ≥10 bps round-trip per rebalance,
   and a walk-forward OOS result over ≥5 years including a momentum-crash period.

## Strengths / Weaknesses

**Strengths:**
- Builds on a well-motivated and established architecture (Wood et al. 2021) with theoretical
  grounding in attention-based temporal pattern learning.
- Addresses a genuine extension gap: the original Momentum Transformer focused on futures;
  equities are a natural but untested extension.
- Authors acknowledged and corrected a data leakage bug rather than reporting inflated results.

**Weaknesses:**
- 7-page paper insufficient for full methodology documentation.
- No peer review.
- Data leakage detected — corrected version still at risk of subtler issues.
- Computational constraints forced methodology degradation (shorter lookback, smaller training set).
- No open code, no replication data.
- Missing metrics: max drawdown, win rate, volatility, trade count, OOS period.
- No community scrutiny or citation.
- Transaction cost model opaque.

## Confidence Score

Per-dimension scoring:

| Dimension | Raw (0–10) | Weight | Weighted |
|-----------|-----------|--------|----------|
| Logical/economic soundness (falsifiable) | 4 | 3 | 12 |
| Transparency & reproducibility | 2 | 2 | 4 |
| Evidence auditability | 2 | 2 | 4 |
| Risk management quality | 1 | 2 | 2 |
| Cost & capacity realism | 2 | 2 | 4 |
| Honest treatment of drawdowns/failure | 2 | 1.5 | 3 |
| Robustness evidence (OOS/walk-forward) | 2 | 1.5 | 3 |
| Survived independent scrutiny | 1 | 1 | 1 |

**Notes on dimension scores:**
- *Logical soundness (4):* TSMOM rationale is legitimate but the attention mechanism's
  advantage is argued post-hoc; failure conditions not formally stated.
- *Transparency (2):* 7-page format cannot document methodology; no code; lookback/dataset
  unstated.
- *Evidence auditability (2):* arXiv preprint, data leakage detected, computational
  compromises, no independent replication.
- *Risk management (1):* No stop, no sizing, no max-DD control described.
- *Cost realism (2):* Costs claimed but model opaque; daily equity rebalancing at meaningful
  scale is expensive.
- *Drawdowns/failure (2):* Max drawdown NOT REPORTED; no failure regime analysis.
- *Robustness (2):* Single train/test split of unknown duration; no walk-forward; no
  multi-market.
- *Independent scrutiny (1):* No citations, no community discussion after 18 months.

**Weighted sum:** 12 + 4 + 4 + 2 + 4 + 3 + 3 + 1 = 33
**Max possible:** 150
**Latent score:** round(33/150 × 100) = **22**

**Verification cap:** Evidence auditability = 2 (≤ 4) → cap at 59.
**Capped confidence score:** min(22, 59) = **22 (Low Confidence)**

Recorded as: `latent 22 (capped to 22 — evidence auditability 2/10: arXiv preprint,
data leakage detected/corrected, computational scope reductions, no open code, no peer review,
no independent replication, max drawdown and cost model NOT REPORTED)`

## Complexity / Scalability / Automation Feasibility

- **Complexity:** High — requires training a deep learning model (attention-LSTM) on
  equity time series, daily inference, and rebalancing.
- **Scalability:** Unknown — no universe size or capacity stated. Daily rebalancing on a
  large equity universe would require significant infrastructure.
- **Automation feasibility:** In principle yes (the original Momentum Transformer has open
  code on GitHub); this extension has no code released.

## MQL5 / Python / Pine Feasibility

- **Python:** Feasible in principle using PyTorch/TensorFlow + the open code from the
  original Momentum Transformer (kieranjwood/trading-momentum-transformer on GitHub) as a
  starting point. The equities extension adds data pipeline complexity (point-in-time universe,
  survivorship-corrected prices).
- **MQL5:** Not feasible — deep learning inference at scale not practical in MQL5.
- **Pine Script:** Not feasible — no deep learning support in TradingView.

## Similar Strategies

- `cmm-daily-return-weighted-momentum` — also ML-weighted momentum on US equities; CMM uses
  sparse ML weighting of daily returns within formation window; this paper uses attention-LSTM
  on 21-day sequences; both are daily momentum variants with ML signal enhancement.
- `attention-factors-statistical-arbitrage` — Pelger group's attention-based factor model;
  better evidence quality (ICAIF 2025 paper, peer-reviewed conference; 24-year OOS);
  conceptually related (attention for equity alpha).
- `network-momentum-trend-following` — another deep learning extension of TSMOM (lead-lag
  networks); higher evidence quality (OOS documented).

## Red Flags

- **Data leakage detected:** Initial Sharpe ratios were inflated due to timestamp handling
  errors. (FACT from accessible sources)
- **Methodology scope reduced under computational constraint:** Not a design choice but a
  hardware limitation. Results represent a degraded version of the intended architecture.
- **7-page preprint:** Insufficient for methodology verification.
- **Transaction cost model opaque:** "Accounted for" is unverifiable without specification.
- **Maximum drawdown NOT REPORTED:** Cannot assess tail risk.
- **18 months on arXiv with zero citations or community engagement found.**

## Source Links

- arXiv 2412.12516 (primary): https://arxiv.org/abs/2412.12516 — retrieved 2026-06-15
- arXiv PDF: https://arxiv.org/pdf/2412.12516 — HTTP 403 (inaccessible)
- Original Momentum Transformer (arXiv 2112.08534): https://arxiv.org/abs/2112.08534 — retrieved 2026-06-15
- Original code (GitHub): https://github.com/kieranjwood/trading-momentum-transformer — retrieved 2026-06-15
- IDEAS/RePEC mirror: https://ideas.repec.org/p/arx/papers/2412.12516.html — HTTP 403
- Secondary summary (aimodels.fyi): https://www.aimodels.fyi/papers/arxiv/enhanced-momentum-momentum-transformers — HTTP 403

## Analyst Notes

**FACTS (from accessible sources):**
- Paper is 7 pages with 5 figures, submitted December 17, 2024.
- Authors: Max Mason, Waasi A Jagirdar, David Huang, Rahul Murugan. (Queue entry "Moraes et al." was incorrect.)
- Architecture: LSTM + multi-head attention hybrid, extending Wood et al. (arXiv 2112.08534).
- Claimed results: Sharpe 1.12, average annual return 4.14%.
- Claimed benchmark improvement: +5.21% annual return vs base momentum strategy.
- Lookback: 21 days (reduced from 21+126 due to memory constraints).
- Training period: 7 years (reduced from 10 due to memory constraints).
- Data leakage was detected (timestamp handling with multi-instance companies) and corrected.
- Corrected Sharpe 1.12 is the post-fix result.

**ANALYSIS:**
- The Sharpe improvement of 1.12 over a base TSMOM is plausible but the cost model is
  opaque. At daily rebalancing on individual US equities, 10 bps round-trip per turn is
  a very conservative assumption; 20–40 bps is realistic for mid-cap stocks.
- The data leakage detection increases my concern about residual subtle biases (survivorship,
  point-in-time accounting, index constituency). A 7-page paper cannot document all safeguards.
- The original Momentum Transformer (Wood et al.) achieved Sharpe ~2.54 on futures over
  1995–2020. The equity extension's Sharpe of 1.12 is plausible given higher equity volatility
  but is a meaningful step down in evidence quality (no peer review, no open code, data issues).
- This is a student-project-grade extension of solid Oxford research. The base idea is sound;
  this specific paper cannot support deployment.

**ASSUMPTIONS:**
- The "average annual return of 4.14%" appears to be annualized; this is assumed (not confirmed).
- The test set is a held-out portion of the 7-year dataset (assumed); total dataset dates NOT REPORTED.

## Future Research Needed

1. Obtain full text of arXiv 2412.12516 to verify exact dataset, cost model, and train/test
   split.
2. Replicate using the kieranjwood/trading-momentum-transformer codebase on a
   survivorship-corrected equity universe with explicit cost model.
3. Check if an updated version (v2+) addresses methodology gaps.
4. Compare to CMM (cmm-daily-return-weighted-momentum) which has superior evidence
   (peer-reviewed JBF) on a similar domain.
