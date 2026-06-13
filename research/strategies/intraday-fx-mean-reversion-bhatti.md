# Regime-Conditioned Statistical Mean Reversion Framework for Intraday FX Markets

## Overview

A conceptual trading framework that conditions intraday mean-reversion entries in FX markets on a
higher-timeframe momentum (regime) filter. Z-score of price deviation triggers entry; a
multi-timeframe momentum state determines whether mean-reversion mode is active. The paper
explicitly self-describes as a "methodological contribution rather than a claim of guaranteed
profitability." No performance results are presented in accessible source materials.

**Author:** Amaanullah Bhatti (Hafzan Osmanoğlu)  
**Published:** January 17, 2026  
**Source:** SSRN 6087107  
**Status:** SSRN preprint only; no peer review; no published performance metrics

---

## Asset Class / Timeframes

- **Market:** Forex (currency pairs NOT SPECIFIED in accessible abstract; FX broadly implied)
- **Entry timeframe:** Intraday (exact timeframe NOT REPORTED)
- **Regime filter timeframe:** Higher timeframe than entry (specific TF NOT REPORTED)
- **MQL5 prototype:** Implies MetaTrader 5 / retail FX broker environment

---

## Core Logic

Short-horizon FX prices exhibit transient mean-reverting behavior driven by dealer inventory
imbalances and liquidity provider adjustments. This mean-reversion tendency degrades or reverses
entirely during trending/momentum regimes. The framework:

1. **Regime detection (higher TF):** Multi-timeframe momentum filter determines whether the
   market is in a mean-reverting or trending state. Parameters NOT REPORTED.
2. **Signal generation (entry TF):** Volatility-normalized Z-score of price deviation measures
   how far price has deviated from a rolling mean. Z-score threshold for entry NOT REPORTED.
   Lookback period NOT REPORTED.
3. **Position sizing:** Volatility-adjusted sizing (formula NOT REPORTED).
4. **Exit:** NOT REPORTED in accessible summaries; inferred to include Z-score reversion toward
   zero or a trailing stop.

Mean-reversion trades are only taken when the higher-TF regime filter signals a non-trending
(range/mean-reverting) environment. Signals during trending regimes are suppressed.

---

## Economic Rationale

**Stated mechanism:** Intraday FX mean reversion is driven by dealer inventory imbalances and
liquidity provider behavior — when dealers become directionally exposed, they widen spreads and
adjust quotes to attract offsetting flow, generating short-term price reversion. This is a
well-documented microstructure effect (Hasbrouck 1988, O'Hara 1995, Lyons 2001 on FX dealer
inventory models).

**Falsifiability:** The mechanism should fail (and the paper acknowledges this) when:
- The market enters a persistent trending regime (directional order flow dominates over
  inventory rebalancing)
- After macro news announcements when fundamental information overrides microstructure dynamics
- During low-liquidity sessions when dealer participation thins and mean-reversion is weaker

The regime filter is specifically designed to disable mean-reversion entries during these periods,
which is a theoretically defensible design. However, the specific conditions that define
"trending regime" are NOT REPORTED — without knowing the exact momentum indicator and threshold,
the falsifiability claim cannot be independently evaluated.

**Strength:** The underlying microstructure rationale is well-grounded in academic literature.
**Weakness:** The mechanism is broadly known and exploited by HFT market-makers; retail
implementation via MQL5 at 1-5 min resolution is unlikely to capture the sub-second dynamics
where dealer inventory reversion actually occurs.

---

## Entry Conditions

NOT REPORTED in accessible sources. From abstract description:
- Higher-TF momentum filter must signal mean-reverting (non-trending) regime
- Z-score of price deviation must exceed entry threshold (threshold NOT REPORTED)
- Direction: long when price below mean by threshold Z, short when above

Exact indicators, lookback periods, and thresholds NOT REPORTED.

---

## Exit Conditions

NOT REPORTED in accessible sources. Implied from description:
- Z-score reverts toward zero (mean reversion completion)
- Possible trailing stop
- Possible fixed take-profit at a fraction of the Z-score entry level

All specific exit parameters NOT REPORTED.

---

## Risk Management

- **Position sizing:** Volatility-adjusted (formula NOT REPORTED)
- **Stop loss:** NOT REPORTED
- **Take profit:** NOT REPORTED
- **Max position/drawdown limits:** NOT REPORTED
- **Max concurrent positions:** NOT REPORTED

Only element confirmed: sizing is volatility-normalized, which is appropriate for intraday FX.

---

## Indicators Used

1. Z-score (volatility-normalized price deviation) — lookback NOT REPORTED
2. Multi-timeframe momentum indicator — specific indicator NOT REPORTED (EMA crossover? RSI? MACD?)
3. Rolling standard deviation (implied by Z-score formula)

No further indicator details accessible from abstract.

---

## Transaction Costs & Capacity

**Costs explicitly modeled:** NOT CONFIRMED from accessible sources. The abstract makes no
mention of spread, slippage, commission, or swap.

**Critical concern for intraday FX:**
- FX intraday mean reversion on retail platforms faces spread costs of 0.5–2+ pips per trade
  (pair-dependent) plus slippage during volatile periods
- Short-term mean reversion edges in FX are typically in the range of 0.5–1.5 pips gross;
  this is fully consumed by retail spreads on most pairs
- The MQL5 implementation note suggests retail execution, where spread + slippage is
  particularly punishing for mean-reversion strategies
- A strategy that is not demonstrably cost-positive on accessible evidence is
  **presumptively invalid for retail execution** — ANALYSIS

**Capacity:** Not discussed. FX spot is extremely liquid at institutional scale, but
retail mean-reversion signals have no meaningful capacity constraint at small size.

---

## Backtest Evidence

**Status: NOT REPORTED**

The paper explicitly self-describes as a "methodological contribution rather than a claim of
guaranteed profitability." No backtest results are presented in the accessible abstract or
any secondary source. This is a conceptual framework paper, not a performance study.

---

## Forward-Test Evidence

**Status: NOT REPORTED**

No forward-test or live trading results accessible.

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | — | NOT REPORTED | No results in paper |
| Sortino Ratio | — | NOT REPORTED | No results in paper |
| Calmar Ratio | — | NOT REPORTED | No results in paper |
| Profit Factor | — | NOT REPORTED | No results in paper |
| Win Rate | — | NOT REPORTED | No results in paper |
| Expectancy per Trade | — | NOT REPORTED | No results in paper |
| Maximum Drawdown | — | NOT REPORTED | No results in paper |
| CAGR | — | NOT REPORTED | No results in paper |
| Annual Return | — | NOT REPORTED | No results in paper |
| Number of Trades | — | NOT REPORTED | No results in paper |
| Average Trade Duration | — | NOT REPORTED | No results in paper |
| Recovery Factor | — | NOT REPORTED | No results in paper |
| Risk-Reward Ratio | — | NOT REPORTED | No results in paper |
| Exposure Percentage | — | NOT REPORTED | No results in paper |

No performance metrics of any kind appear in accessible summaries. The paper explicitly declines
to make performance claims.

---

## Community Sentiment

No community discussion of this paper found across ForexFactory, Reddit, MQL5 forums, or any
practitioner site. Zero citations or independent commentary accessible as of 2026-06-13.

The author (Amaanullah Bhatti / Hafzan Osmanoğlu) describes themselves as pursuing an M.Sc. in
Economics at Symbiosis International University and holds retail trading certifications
(NCIAC, NCIAP, NCMP Lv-3). Self-described "top 10% researcher on SSRN" — a very low bar
given SSRN volume and the platform's open upload model.

---

## Why This Might Be Nothing

**MANDATORY SKEPTIC STEELMAN — this section written before scoring.**

### 1. Most likely benign explanation
This is a conceptual sketch, not a validated strategy. The paper itself admits it makes no
performance claims. The Z-score + momentum-regime-filter combination is well-known in the
academic and practitioner literature; there is nothing novel enough here that it constitutes
evidence of an edge. The framework describes a category of strategy rather than a specific
implementable system.

### 2. The cost case (highest-information concern)
Intraday FX mean reversion is the environment most hostile to retail execution costs:
- Typical retail EUR/USD spread: 0.5–1.5 pips; GBP/USD: 1–2 pips; exotic pairs: 3–10 pips
- A mean-reversion trade harvesting a 1-pip gross edge at the Z-score level pays 1–3 pips
  in round-trip spread alone, never mind slippage
- This is almost certainly a negative-expectancy strategy after realistic retail costs on most
  FX pairs at short intraday timeframes — the paper makes no attempt to address this
- No transaction cost modeling is mentioned anywhere in accessible descriptions

ANALYSIS: The most probable outcome of implementing this framework is a net-negative return
after spread and slippage. This is the primary reason to be skeptical regardless of any
reported gross edge.

### 3. Regime dependence
- The strategy is explicitly long carry-free inventory rebalancing microstructure — which only
  exists reliably during liquid, range-bound, non-announcement periods
- Low-liquidity periods (Asian session, overnight) and high-impact news events (NFP, CPI, FOMC)
  create trending/momentum regimes that the filter is supposed to handle, but without knowing
  the filter's sensitivity, we cannot evaluate whether it correctly identifies these periods
- HFT market-makers already extract most of the dealer inventory mean-reversion premium at
  microsecond timescales; what remains at 1-5 min resolution is uncertain

### 4. Sample/overfitting concerns
- No backtest is disclosed; therefore no overfitting risk can be assessed — but the absence
  of any performance evidence is itself a red flag for a paper proposing a practical system
- Z-score lookback and threshold are the primary parameters; if these were tuned in-sample,
  any discovered edge is likely data-mined
- "Methodological contribution" framing may be the author's way of avoiding the replication
  problem by never showing results that need replicating

### 5. What would change my mind
An independent walk-forward backtest with realistic per-pair spread + slippage + commission
modeling showing positive Sharpe (> 0.5) across multiple FX pairs and multiple market regimes
including 2022–2025 (dollar trending, volatile rate environment). I do not have this evidence.

**Verdict:** Probably no verifiable edge as currently presented. The conceptual mechanism is
legitimate, but there is no evidence that the specific implementation generates positive returns
after costs in retail FX. This is a framework description, not a validated strategy.

---

## Strengths / Weaknesses

**Strengths:**
- Theoretically sound mechanism (dealer inventory microstructure is real and documented)
- Regime conditioning is a defensible design choice — avoids mean-reversion during trends
- Volatility-adjusted sizing is good practice
- Honest framing as "methodological contribution" rather than performance claim

**Weaknesses:**
- Zero performance evidence — not even a claimed metric
- Critical parameters undisclosed (Z-score lookback, momentum filter, thresholds, exit rules)
- MQL5/retail implementation is the wrong execution venue for a microstructure-driven edge
- No cost modeling mentioned
- No OOS testing
- No independent replication or scrutiny
- Single junior author with no finance PhD affiliation; no peer review

---

## Confidence Score

### Per-Dimension Breakdown

| Dimension | Wt | Score | Reasoning |
|---|---|---|---|
| Logical / economic soundness (falsifiable) | 3 | 5 | Dealer inventory mechanism real; regime conditioning is correctly specified at concept level; but "when the edge disappears" only partially stated (trending regime — not quantified) |
| Transparency & reproducibility | 2 | 3 | High-level description only; specific parameters (Z-score lookback, momentum indicator, thresholds) NOT REPORTED; MQL5 prototype referenced but not confirmed public |
| Evidence auditability | 2 | 2 | No performance results in the paper; SSRN preprint only; no peer review; methodology described in prose at high level |
| Risk management quality | 2 | 4 | Volatility-adjusted sizing mentioned (good); stop/TP/max-loss NOT REPORTED |
| Cost & capacity realism | 2 | 1 | No cost modeling mentioned; intraday FX is high-cost regime; retail spread almost certainly consumes any gross edge |
| Honest treatment of drawdowns / failure | 1.5 | 5 | Paper honestly disclaims performance claims; but absence of drawdown data scores mid rather than high |
| Robustness evidence (OOS / walk-forward / multi-market) | 1.5 | 0 | No evidence whatsoever |
| Survived independent scrutiny | 1 | 0 | No community discussion or independent review found |

**Weighted sum:** (3×5) + (2×3) + (2×2) + (2×4) + (2×1) + (1.5×5) + (1.5×0) + (1×0)
= 15 + 6 + 4 + 8 + 2 + 7.5 + 0 + 0 = **42.5**

**Max possible:** 150

**Latent score:** round(42.5 / 150 × 100) = **28**

**Verification cap:** Evidence auditability = 2/10 (≤ 4) → cap at 59

**Capped confidence:** min(28, 59) = **28**

**latent 28 (capped to 28 pending verification: evidence auditability 2/10 — no performance data reported; paper self-describes as methodological framework only; SSRN preprint, no peer review)**

**Band:** Low Confidence (0–39)

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Moderate — Z-score calculation straightforward; multi-TF regime filter adds
  complexity but is implementable; the challenge is parameter selection with no guidance
- **Scalability:** FX spot is liquid; strategy is inherently small-size only (retail mean
  reversion on 1-5 min bars); scales poorly because it competes with HFT market-makers
- **Automation feasibility:** High in principle (MQL5 prototype described); but no open code
  available; parameter uncertainty makes blind implementation risky

---

## MQL5 / Python / Pine Feasibility

- **MQL5:** Explicitly mentioned; a prototype exists per the paper, but it is not confirmed
  to be publicly available
- **Python:** Standard implementation — Z-score is trivial; multi-TF regime filter
  implementable with pandas + ta-lib; but parameters are unknown
- **Pine Script (TradingView):** Feasible for signal visualization; execution quality on
  intraday FX would suffer from TradingView alert delays

---

## Similar Strategies

- `london-breakout-forex` — same market (intraday FX), opposite mechanism (breakout vs.
  mean reversion); similar evidence quality
- `fast-alphas-intraday-overlay` (SSRN 6391638, Zarattini/Pagani) — most similar: also
  applies streak-based mean-reversion overlay with trend regime conditioning on intraday bars;
  on US equities (SPY) not FX, but architecturally very close; scored 55 vs. Bhatti's 28
- `fx-cointegration-pairs-trading` — FX/ETF mean reversion via different signal (cointegration)
- `intraday-momentum-spy` — same intraday timeframe, opposite mechanism (momentum vs. reversion)

---

## Red Flags

- **No performance evidence:** Paper explicitly declines to claim profitability; zero metrics
- **Parameters undisclosed:** Key implementation details (Z-score lookback, momentum filter
  type/period, entry threshold, exit rule) NOT REPORTED — not a deployable specification
- **Retail execution venue:** MQL5 / MT5 implementation in a high-spread retail environment
  is structurally disadvantaged for microstructure-based mean reversion
- **Transaction costs ignored:** No mention of spread, slippage, or commission modeling
- **Junior author, no affiliation:** Pursuing undergraduate/master's degree; no finance PhD
  or quant research group affiliation; single-author SSRN preprint
- **No community scrutiny:** Zero independent discussion found

---

## Source Links

| Source | URL | Retrieved |
|--------|-----|-----------|
| SSRN 6087107 (primary paper — HTTP 403) | https://papers.ssrn.com/sol3/papers.cfm?abstract_id=6087107 | 2026-06-13 |
| SSRN 6650958 (Bhatti Gold paper — HTTP 403) | https://papers.ssrn.com/sol3/papers.cfm?abstract_id=6650958 | 2026-06-13 |
| ResearchGate (HTTP 403) | https://www.researchgate.net/publication/400200138 | 2026-06-13 |
| DeepDyve (HTTP 403) | https://www.deepdyve.com/lp/unpaywall/a-regime-conditioned-statistical-mean-reversion-framework-for-intraday-Tq2X1EbWTi | 2026-06-13 |
| Google Scholar profile (HTTP 403) | https://scholar.google.com/citations?user=gK6fBqUAAAAJ | 2026-06-13 |

---

## Analyst Notes

**FACTS** (from search engine summaries of the abstract):
- Paper title: "A Regime-Conditioned Statistical Mean Reversion Framework for Intraday FX Markets"
- Author: Amaanullah Bhatti (also known as Hafzan Osmanoğlu)
- Published: January 17, 2026, SSRN 6087107
- Framework uses: Z-score signal, multi-timeframe momentum regime filter, volatility-adjusted sizing
- MQL5 prototype described
- Paper self-describes as "methodological contribution rather than a claim of guaranteed profitability"
- Author credentials: Bachelor's in Financial Markets, pursuing M.Sc. in Economics at Symbiosis International University; retail trading certifications

**FACTS** (from Bhatti's separate Gold paper SSRN 6650958, NOT the FX paper):
- The Gold paper (April 2026) uses 200-period EMA as binary regime classifier, 50-period EMA as pullback zone + trailing stop, session VWAP as primary structural reference
- Search engine snippet for Gold paper cited: win rate 45.3%, mean expectancy +0.414R, profit factor 1.76, Sharpe 3.99, max DD 5.1% — **these metrics are from the GOLD paper, not the FX paper, and cannot be applied to the FX paper**
- The Gold paper Sharpe of 3.99 would trigger automatic scrutiny (threshold > 3) and warrants deep skepticism — these results are NOT attributed to the FX paper being reviewed here

**ANALYSIS:**
- The general approach (Z-score + regime filter) is a well-established framework; similar structures are discussed in Zarattini/Pagani (SSRN 6391638) for US equities
- The dealer inventory microstructure rationale is legitimate but the edge is captured at HFT timescales, not retail intraday timescales
- A paper that explicitly disclaims performance evidence should not be treated as evidence of a working strategy; it is a conceptual sketch
- The MQL5 prototype claim without open code access means the implementation cannot be verified
- The author's Gold paper Sharpe of 3.99 with max DD of 5.1% is highly suspicious if those numbers prove real — patterns of implausible metrics from an author reduce overall credibility of the research program

**ASSUMPTIONS (flagged):**
- Assumed the paper uses 1-min to 15-min intraday timeframes based on "intraday FX" description and retail MQL5 context
- Assumed Z-score uses rolling window on close prices; formula unknown
- Assumed momentum filter is EMA-based given the Gold paper context, but this is speculation

---

## Future Research Needed

1. **Access full paper:** Direct download of SSRN PDF to extract specific parameters, backtest
   methodology, and any performance tables
2. **Replicate from methodology:** If parameters are disclosed in the full paper, replicate
   the Z-score + regime filter on EURUSD/GBPUSD M5/M15 with realistic spread modeling
3. **Author's Gold paper review:** Evaluate SSRN 6650958 independently — Sharpe 3.99 and
   max DD 5.1% would trigger scrutiny if from accessible data; if those numbers are real, what
   explains the anomaly?
4. **Academic precedent comparison:** Compare to established intraday FX mean-reversion
   literature (Osler 2005 round-number clustering, Chaboud et al. algorithmic trading studies,
   Darragh/Harris MR studies) to assess whether Bhatti's framework adds anything novel
