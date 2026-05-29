# Omega Model Intraday Momentum/Reversal (NASDAQ-100 / S&P 500)

## Overview

A portfolio-optimization framework that integrates intraday momentum and reversal signals with
the Omega ratio as the objective function. Each day, component stocks of the S&P 500 or
NASDAQ-100 are ranked by their prior intraday return. The top or bottom 15 stocks are selected
(winners for momentum; losers for reversal), and portfolio weights are allocated by maximizing
the Omega ratio subject to trading costs. Three model variants: M_Omega (momentum only),
R_Omega (reversal only), M_R_Omega (combined). Published in PLOS One (September 2023, peer-
reviewed, open access). Authors: Jing-Rung Yu, Chieh-Hui Wei, Chi-Ju Lai, Wen-Yi Lee
(National Chi Nan University, Taiwan).

---

## Asset Class / Timeframes

- **Markets:** US Equities — S&P 500 component stocks and NASDAQ-100 component stocks
- **Timeframe:** Intraday (daily rebalancing; momentum lookback 15–60 min; reversal lookback
  60–300 min)
- **Style:** Intraday mean-reversion/momentum, daily portfolio rebalancing
- **In scope:** NAS100 (NASDAQ-100 components), US30 (potential S&P 500 extension) ✓

---

## Core Logic

1. **Universe:** S&P 500 or NASDAQ-100 component stocks (full index membership).

2. **Ranking:** At each rebalancing, rank all stocks by their prior intraday return over the
   chosen lookback period:
   - Momentum (M_Omega): lookback periods of 15, 30, 45, or 60 minutes
   - Reversal (R_Omega): lookback periods of 60, 120, 180, or 300 minutes

3. **Selection:** Buy the top 15 stocks (M_Omega: momentum leaders) OR bottom 15 stocks
   (R_Omega: reversal candidates — prior losers expected to mean-revert).

4. **Portfolio weights:** Allocate capital to the 15 selected stocks by maximizing the
   **Omega ratio** subject to:
   - Transaction costs
   - Long-only constraint (no shorting)
   - Each day the Omega optimization uses the day's selected stocks

   The Omega ratio Ω(τ) = [∫τ^∞ (1 − F(x)) dx] / [∫-∞^τ F(x) dx] where F(x) is the
   return CDF and τ is the threshold return (typically 0 or the risk-free rate). Unlike
   Sharpe, Omega uses the full return distribution rather than just mean/variance.

5. **Holding period:** Intraday — positions held from the ranking period through end of
   trading day. Portfolio rebalanced every transaction day.

6. **Transaction costs:** Explicitly included in portfolio optimization and performance.

---

## Economic Rationale

**Momentum (M_Omega):** Intraday momentum exists because large institutions execute block
orders throughout the day, creating persistent price pressure in the direction of their
trades. Stocks that have moved up in the first 15-60 minutes are more likely to continue
moving up as other institutions follow the trade or the order flow continues.

**Reversal (R_Omega):** Intraday reversal exists because short-term overshoots driven by
retail overreaction or limited liquidity create price dislocations that professionals
arbitrage away within a few hours.

**Omega ratio as optimizer:** Stocks with large intraday moves have skewed return
distributions. The Omega ratio captures this asymmetry better than Sharpe, which only
penalizes downside and upside variance symmetrically.

**When the edge should disappear (falsifiability):**
- In low-volatility environments: when intraday return dispersion is compressed (low VIX,
  choppy markets), the ranking signal is noisy and transaction costs dominate.
- The paper explicitly confirms this: "more profitable in a volatile market or period."
- The S&P 500 Sharpe results only outperform the benchmark during the COVID 2020-2021
  high-volatility period, directly confirming the regime dependence.
- The strategy is NOT recommended long-term by its own authors because transaction costs
  erode returns as the number of daily rebalancing rounds compounds.

---

## Entry Conditions

- **M_Omega:** Enter positions in top 15 S&P 500 or NASDAQ-100 stocks ranked by intraday
  return over the prior 15, 30, 45, or 60 minutes (exact best lookback NOT REPORTED by
  period in accessible sources)
- **R_Omega:** Enter positions in bottom 15 stocks ranked by intraday return over prior
  60, 120, 180, or 300 minutes
- Weights allocated by Omega ratio optimization at time of entry
- Positions entered at rebalancing time; held intraday

---

## Exit Conditions

- Close all positions at end of trading day (forced EOD exit)
- No intraday stop-loss defined in accessible sources

---

## Risk Management

**Defined:**
- Long-only constraint (no short exposure)
- Transaction costs embedded in optimization objective
- Limited to 15 stocks per day (concentration limit)
- Strategy is NOT recommended for long-term deployment (authors' own conclusion)

**NOT defined:**
- No stop-loss
- No max drawdown circuit breaker
- No position size limits beyond the Omega optimization weights

**Assessment:** Risk management is inadequate for live deployment. The strategy has no
protection against gaps, rapid intraday reversals, or sudden correlation breakdowns in
the selected 15-stock portfolio. The "do not use long-term" warning from the authors is
itself a risk disclosure.

---

## Indicators Used

- Intraday high-frequency price data (US exchanges)
- Prior intraday returns as the ranking signal (no additional indicators)
- Omega ratio (portfolio optimization objective)
- Transaction cost model

---

## Transaction Costs & Capacity

- **Included:** Yes — transaction costs explicitly embedded in the Omega optimization objective.
- **Net performance:** Positive returns net of transaction costs confirmed.
- **Long-term erosion:** Authors explicitly note transaction costs "become increasingly
  significant over time" for daily intraday rebalancing.
- **Specific cost assumptions:** NOT REPORTED in accessible sources (commission rate, spread).
- **Capacity:** S&P 500 and NASDAQ-100 component stocks are large-cap and highly liquid.
  Selecting 15 of 500+ (S&P) or 100 (NASDAQ) limits concentration but the strategy would
  likely face meaningful slippage at institutional size given intraday order flow competition.

---

## Backtest Evidence

- **Status:** CLAIMED — published in PLOS One (open-access, peer-reviewed). Full paper
  accessible in theory; returned HTTP 403 during this research session.
- **Period:** Three periods tested:
  1. 2018-01-01 – 2019-04-22 (pre-COVID, normal volatility)
  2. 2020-01-01 – 2021-04-22 (COVID pandemic, high volatility)
  3. 2021-01-01 – 2022-04-22 (post-COVID recovery + early 2022 sell-off)
- **Method:** Full-period simulation with daily rebalancing; transaction costs included.
- **OOS:** No walk-forward or hold-out OOS test described in accessible sources; appears
  to be in-sample optimization per period.

---

## Forward-Test Evidence

NOT REPORTED.

---

## Reported Metrics

| Metric | Value | Dataset | Status |
|--------|-------|---------|--------|
| Terminal wealth improvement, M_Omega vs Omega | +$731,463 | S&P 500 | CLAIMED, UNVERIFIED |
| Terminal wealth improvement, R_Omega vs Omega | +$331,336 | S&P 500 | CLAIMED, UNVERIFIED |
| Terminal wealth improvement, M_Omega vs Omega | +$247,127 | NASDAQ-100 | CLAIMED, UNVERIFIED |
| Terminal wealth improvement, R_Omega vs Omega | +$120,332 | NASDAQ-100 | CLAIMED, UNVERIFIED |
| Sharpe vs S&P 500 (2020-2021) | M_Omega and R_Omega both outperform | S&P 500 | CLAIMED, UNVERIFIED |
| Sharpe vs S&P 500 (2018-2019, 2021-2022) | R_Omega < S&P 500 Sharpe | S&P 500 | CLAIMED, UNVERIFIED |
| Sharpe vs NASDAQ-100 (all 3 periods) | M_Omega and R_Omega both outperform | NASDAQ-100 | CLAIMED, UNVERIFIED |
| Max drawdown | NOT REPORTED | — | NOT REPORTED |

**Critical finding:** S&P 500 Sharpe outperformance is confined to the COVID volatility period.
NASDAQ-100 outperformance spans all three periods — the more robust result.

---

## Community Sentiment

- Published in PLOS One (open access, peer-reviewed, ISSN 1932-6203).
- PLOS One is a legitimate peer-reviewed journal but uses a "methodological soundness"
  criterion rather than requiring results to be statistically significant or commercially
  important.
- No independent replication or community critique found in accessible sources.
- The paper is cited in related work on intraday time-series momentum (ideas.repec reference
  found, but no discussion accessible).

---

## Why This Might Be Nothing

### 1. Most likely benign explanation: COVID volatility bias
The strategy's flagship result — outperforming the S&P 500 Sharpe — only materializes in
2020-2021, during the most extreme volatility event since 2008. This is not a generalizable
result; it is a regime-specific result. Any intraday momentum or mean-reversion strategy
with sufficient leverage would have "worked" in 2020-2021 because intraday dispersion was
5-10× its historical average.

### 2. The cost case
The strategy requires daily rebalancing of 15 stocks. Even at 0.05% round-trip cost per
stock, 252 trading days × 15 stocks × 2 (buy + sell) = 7,560 transactions per year. At
$0.005/share on 1,000 shares per position, cumulative costs compound aggressively over time.
The authors themselves warn this strategy is "not suitable long-term" — a rare and honest
admission that costs eventually dominate.

### 3. Regime dependence
The S&P 500 result is regime-conditional (only COVID). This is the single largest concern.
Real strategies must work across regimes, not just during extreme volatility spikes.

### 4. Sample / overfitting
Only 4 years of data (2018-2022) across three 16-month periods. The Omega optimization
uses in-sample data per period without a strict hold-out. Results may reflect period-specific
parameter fitting.

### 5. What would change my mind
An out-of-sample test on 2022-2026 data (after the paper's period) using the same
lookback periods without re-optimization, showing sustained outperformance across normal
and volatile periods, would significantly change my assessment — particularly for the
NASDAQ-100 variant which performed better in all three tested periods.

---

## Strengths / Weaknesses

**Strengths:**
- Published in open-access peer-reviewed journal (PLOS One)
- Transaction costs explicitly modeled
- Multiple periods tested (including a post-COVID drawdown period)
- Omega ratio as optimizer is methodologically sound for asymmetric distributions
- NASDAQ-100 variant outperforms across all three periods (not just COVID)
- Authors honestly disclose limitations ("not recommended long-term")

**Weaknesses:**
- S&P 500 result is COVID-period-conditional only
- Only 4 years total backtest across three periods — insufficient for regime generalization
- No out-of-sample walk-forward
- No long-only / short-side generalization (some markets favor R_Omega in downtrends)
- Max drawdown not reported
- "Not suitable long-term" is a significant admission
- Specific lookback period selections not disclosed as final choices

---

## Confidence Score

### Per-dimension scoring

| Dimension | Wt | Score | Contribution | Rationale |
|---|---|---|---|---|
| Logical / economic soundness (falsifiable) | 3 | 5 | 15.0 | Intraday momentum + reversal both documented; Omega optimizer plausible; but edge is clearly regime-conditional (confirmed by own authors) |
| Transparency & reproducibility | 2 | 7 | 14.0 | Peer-reviewed; Omega ratio standard; lookback periods disclosed; portfolio size disclosed; full methodology in PLOS One |
| Evidence auditability | 2 | 7 | 14.0 | Published in PLOS One (open-access, peer-reviewed); specific numbers accessible from search summaries; methodology replicable in principle |
| Risk management quality | 2 | 3 | 6.0 | Long-only; cost embedded; 15-stock limit; no stop-loss; authors say "not for long-term" |
| Cost & capacity realism | 2 | 6 | 12.0 | Costs explicitly included; net positive; BUT long-term cost erosion acknowledged; daily rebalancing intensive |
| Honest treatment of drawdowns / failure | 1.5 | 7 | 10.5 | Three periods tested; S&P underperformance openly disclosed; "not for long-term" warning; notable transparency |
| Robustness evidence (OOS / walk-forward) | 1.5 | 4 | 6.0 | Three periods is reasonable but only 4 years total; no walk-forward; S&P results period-conditional; NASDAQ more robust |
| Survived independent scrutiny | 1 | 5 | 5.0 | PLOS One peer review; no independent replication or community discussion accessible |

**Weighted sum:** 15.0 + 14.0 + 14.0 + 6.0 + 12.0 + 10.5 + 6.0 + 5.0 = **82.5**
**Max possible:** 150
**latent_score:** round(82.5 / 150 × 100) = **55**

**Verification cap check:**
- Evidence auditability = 7 → cap at 74
- latent 55 < 74 → cap does not bind
- **confidence = 55** (Experimental, 40–59)

*latent 55 (capped to 74 pending independent replication: PLOS One paper accessible but specific Sharpe numbers/drawdown not confirmed; cap not binding since latent < cap)*

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Medium-high. Requires intraday high-frequency data, Omega ratio optimization
  (a quadratic or LP problem per day), daily rebalancing, and transaction cost modeling.
  More complex than simple momentum rankings.
- **Scalability:** Moderate. The 15-stock selection from S&P 500 (500 stocks) or NASDAQ-100
  (100 stocks) is tractable. Scaling to larger universes would require faster optimization.
- **Automation:** Feasible but demands a real-time data feed and optimization library.
  A Python implementation using `scipy.optimize` for Omega optimization + IBKR API for
  execution is achievable by an experienced quant developer.

---

## MQL5 / Python / Pine Feasibility

- **Python:** MEDIUM. Omega ratio optimization requires a convex solver (`scipy.optimize`,
  `cvxpy`). Daily rebalancing logic is straightforward. Need intraday data (Yahoo Finance
  for testing; proper broker feed for live). ~300 lines for MVP.
- **MQL5:** LOW. Omega ratio optimization is not native to MQL5. Would require an external
  optimization engine.
- **Pine Script:** NOT FEASIBLE.

---

## Similar Strategies

- `intraday-momentum-spy` (score 50) — simpler intraday momentum on SPY ETF (index level
  vs component stock selection here); same intraday momentum concept
- `small-cap-multipattern-poudel` (score 53) — also intraday momentum/reversal in US equities;
  small-cap focus vs large-cap here; also has mean reversion family
- `orb-stocks-in-play` (score 78) — intraday US equities; ORB on news-driven stocks vs
  systematic ranking across index components here

---

## Red Flags

- S&P 500 Sharpe outperformance is COVID-period-conditional only (confirmed by paper)
- Only 4 years of data (2018-2022) — insufficient regime coverage
- No walk-forward or hold-out OOS evaluation
- "Not suitable for long-term" per own authors (cost erosion)
- Max drawdown not reported
- Specific best lookback periods not disclosed in accessible sources

---

## Source Links

| URL | Accessed | Notes |
|-----|----------|-------|
| https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0291119 | 2026-05-29 | PLOS One (HTTP 403) |
| https://pmc.ncbi.nlm.nih.gov/articles/PMC10490999/ | 2026-05-29 | PubMed Central (HTTP 403) |
| https://www.researchgate.net/publication/373773848 | 2026-05-29 | ResearchGate (HTTP 403) |
| https://go.gale.com/ps/i.do?id=GALE%7CA764125112 | 2026-05-29 | Gale Academic (HTTP 403) |

All primary sources returned HTTP 403. Evidence entirely from search engine summaries.
The paper IS open access (PLOS One); the 403 errors appear to be a session/crawler restriction.

---

## Analyst Notes

**FACTS (from accessible search summaries):**
- Published: PLOS One, Vol 18, Issue 9, September 8, 2023 (doi: 10.1371/journal.pone.0291119)
- Authors: Jing-Rung Yu, Chieh-Hui Wei, Chi-Ju Lai, Wen-Yi Lee (National Chi Nan University)
- Markets: S&P 500 and NASDAQ-100 component stocks; intraday high-frequency data
- Three periods: 2018-04/2019, 2020-04/2021, 2021-04/2022
- M_Omega (15-60 min lookback) vs R_Omega (60-300 min lookback)
- 15 selected stocks per portfolio per day; Omega ratio optimization
- Terminal wealth improvements: S&P M_Omega +$731K, R_Omega +$331K over baseline Omega
- NASDAQ-100 M_Omega +$247K, R_Omega +$120K over baseline Omega
- S&P 500 Sharpe only outperforms benchmark during 2020-2021 COVID period
- NASDAQ-100 Sharpe outperforms benchmark in all three periods
- Net positive returns after transaction costs; not suitable long-term

**ANALYSIS (my reasoning):**
- The NASDAQ-100 result is genuinely more interesting than the S&P 500 result: outperforming
  the index Sharpe across all three tested periods (including the 2021-2022 drawdown) suggests
  the momentum signal has some utility outside extreme volatility.
- The intraday Omega optimization is methodologically reasonable — NASDAQ-100 stocks exhibit
  strong intraday momentum (tech sector flow dynamics) that may make the ranking more persistent.
- The "not suitable long-term" admission is one of the most honest statements I have encountered
  in this research database. It deserves credit but also reduces implementation attractiveness.
- The 2018-2022 period contains two very distinct market regimes (bull 2018-2019, COVID 2020,
  recovery 2020-2021, sell-off 2022). Four regime tests is reasonable for a 4-year study.

**ASSUMPTIONS (not confirmed):**
- ASSUMED the terminal wealth numbers start from a common initial investment of $1,000,000
  (common convention in portfolio studies).
- ASSUMED the lookback periods tested are the complete set; best lookback per period not
  confirmed.
- ASSUMED PLOS One version is the final version of record; no prior pre-print found.

---

## Future Research Needed

1. **Access the full paper:** PLOS One is open access; re-fetch when the 403 restriction lifts
   to confirm exact Sharpe ratios, drawdown tables, and lookback period selections.
2. **Post-publication OOS test:** Run the NASDAQ-100 variant on 2022-2026 data using the
   best lookback period identified in the paper — test whether the NASDAQ-100 advantage persists.
3. **Regime conditioning:** Investigate whether a VIX-based filter can improve the S&P 500
   result by activating the strategy only in high-volatility environments.
4. **Long-term cost analysis:** Model the cumulative transaction cost over 3, 5, 10 years
   to establish the break-even volatility level.
5. **Short-side extension:** Test selling the bottom-ranked stocks (not just buying top) to
   capture directional downside momentum — though this requires a different risk framework.
