# Short-Term Basis Reversal in Commodity Futures

## Overview

A cross-sectional (and time-series) strategy exploiting systematic negative autocorrelation in the spread
between front-month (M1) and second-month (M2) commodity futures contracts. Weekly rebalancing on 39
commodity futures. The mechanism is claimed to be differential information-incorporation speed across the
futures curve: front-month contracts react faster to news, temporarily over-shooting relative to second-month
contracts, with the spread reverting within one week.

**Source:** Rossi, Alberto G., Yingguang Zhang, and Yandi Zhu. "Short-Term Basis Reversal."
Georgetown McDonough School of Business Research Paper. SSRN 5250499. May 2025.
*Alberto Rossi: Georgetown University McDonough School of Business.*

## Asset Class / Timeframes

- **Asset class:** Commodity Futures (Agriculture, Energy, Metals)
- **Universe:** 39 commodity futures contracts from Norgate Data (specific contract list NOT REPORTED in
  accessible sources; Norgate Data covers major CBOT, CME, NYMEX, COMEX contracts)
- **Timeframe:** Weekly (1-week holding period)
- **Rebalancing:** Weekly

## Core Logic

The spread between the first-nearby and second-nearby commodity futures contract (M1 − M2 spread return)
exhibits significant negative autocorrelation from one week to the next. If the spread fell sharply last week
(M1 underperformed M2), it tends to rebound this week; if the spread rose sharply, it tends to fall back.

**Cross-sectional strategy (primary):**
1. Each week, compute the M1−M2 spread return for all 39 commodities.
2. Rank all 39 commodities from lowest (most negative spread return) to highest (most positive).
3. Long the spread (long M1, short M2) of the 4 commodities with the *most negative* prior-week spread
   return (anticipating a rebound).
4. Short the spread (short M1, long M2) of the 4 commodities with the *most positive* prior-week spread
   return (anticipating a reversal).
5. Hold for one week; rebalance.

**Time-series variant:** If the aggregate spread return across all commodities was negative last week, go long
the spread portfolio; if positive, go short. Both variants are claimed profitable.

**Position sizing:** Equal weight across the 4 long and 4 short positions (assumed; not explicitly described in
available summaries).

## Economic Rationale

Front-month futures are more actively traded and react more rapidly to new information (macro data releases,
weather events, demand shocks) than second-month contracts. This differential in information-processing speed
creates a temporary overreaction in the front month relative to the second month, producing negative
autocorrelation in the M1−M2 spread that reverts over the following week.

**When the edge should disappear (falsifiability test):**
- If algorithmic traders simultaneously monitor and trade the full futures curve, eliminating cross-maturity
  price-discovery lags.
- In very liquid, electronically traded markets where M1 and M2 depth is comparable and arbitrageurs
  routinely trade calendar spreads to maintain pricing consistency.
- If fundamental news volatility declines dramatically (reducing the frequency of M1 overreactions).
- During regimes of financialization where speculative flows dominate both contract months simultaneously,
  narrowing the spread autocorrelation.
- ANALYSIS: The mechanism implies the edge should be *stronger* in less-liquid (agricultural, specialty)
  contracts versus highly liquid (energy) contracts, and in higher-volatility regimes — both are stated in
  the paper and represent testable cross-sectional predictions.

## Entry Conditions

- Beginning of each week: compute prior-week spread return (M1 close − M2 close return) for each commodity.
- Rank the 39 commodities by this spread return.
- Long: 4 commodities with the most negative (bottom quintile) prior-week spread return.
- Short: 4 commodities with the most positive (top quintile) prior-week spread return.
- Positions are in the *calendar spread* (long M1 / short M2 for a long position; short M1 / long M2 for
  short), not outright long/short directional positions.

## Exit Conditions

- Exit after one week (fixed holding period). No intra-week exits described in available sources.

## Risk Management

- **Market neutrality:** The long and short legs are spread trades (long M1/short M2 vs. short M1/long M2),
  so the portfolio is approximately market-neutral with limited directional commodity exposure.
- **Diversification:** 8 active positions (4 long spreads, 4 short spreads) across 39 possible commodities.
- **No explicit stop-loss:** NOT REPORTED in available summaries.
- **Position sizing:** Equal weight assumed. NOT REPORTED explicitly.
- **Leverage:** NOT REPORTED. Calendar spread positions use less margin than outright futures.

## Indicators Used

- M1−M2 weekly spread return (prior week): the sole signal.
- No technical indicators, no macro overlays.

## Transaction Costs & Capacity

**Transaction costs:**
- The paper reports "pre-cost Sharpe ratios above 1" for both cross-sectional and time-series strategies
  (per QuantSeeker's summary of the paper). This phrasing implies the headline 1.42/1.45 figures may be
  *pre-cost*, with net performance lower.
- Weekly rebalancing of 8 calendar spread positions across 39 commodities means approximately 8 full
  calendar-spread round-trips per week.
- Calendar spreads (trading M1 and M2 as a spread) are typically executed at tighter bid-ask spreads than
  outright futures due to directional risk netting; CBOT corn, soybean, and NYMEX crude oil calendar spreads
  have very tight markets. Agricultural specialty and smaller energy spreads may be wider.
- ANALYSIS: At 8 round-trips/week × 52 weeks = ~416 spread round-trips/year, even a 2–3bps cost per round-
  trip would consume 8–12bps/year of alpha — modest. However, if calendar spreads must be legged in
  separately (trading M1 and M2 independently), costs could be 4–8× higher (20–50bps/year or more per
  unit of risk), potentially eroding a significant fraction of the reported edge.
- ASSUMPTION: The independent QuantReturns implementation (9.68%/0.92 Sharpe, 2007–2025) is likely
  post-cost or close to it, since it represents an actual implementation attempt on available data.

**Capacity:**
- Calendar spread strategies in major commodity futures have high capacity — institutional size is feasible
  for energy (crude oil, natural gas, refined products) and major grains (corn, soybeans, wheat).
- Smaller contracts (coffee, cocoa, specialty metals) may have limited capacity.
- ANALYSIS: The equal-weight cross-sectional approach on 39 commodities with weekly rebalancing is
  reasonably scalable for most institutional managers.

## Backtest Evidence

- **Period (claimed):** "Several decades" per Quantitativo note. Exact start year NOT REPORTED in accessible
  sources. One QuantReturns reviewer independently implemented the strategy for 2007–2025 (18 years); the
  paper likely covers a longer period (possibly starting in the early 1990s when Norgate commodity futures
  data begins in depth, though this is ASSUMPTION).
- **Status:** CLAIMED, UNVERIFIED — SSRN working paper, not peer-reviewed, no open code confirmed.

## Forward-Test Evidence

NOT REPORTED.

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | 1.42 (paper); 1.45 (QuantReturns review); 0.92 (independent impl., 2007–2025) | CLAIMED, UNVERIFIED | SSRN 5250499; QuantReturns.com review |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | −24.4% (one source); −17% (QuantReturns impl., 2007–2025) | CLAIMED, UNVERIFIED | Search-result summary; QuantReturns.com |
| CAGR | 17.6–18% (paper); 9.68% (QuantReturns impl., 2007–2025) | CLAIMED, UNVERIFIED | SSRN 5250499; QuantReturns.com |
| Annual Return | See CAGR row | CLAIMED, UNVERIFIED | — |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | 1 week (stated) | CLAIMED, UNVERIFIED | SSRN 5250499 |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | ~100% (always 8 positions open; portfolio is nearly fully invested in spreads) | CLAIMED, UNVERIFIED | ASSUMPTION from strategy description |
| Volatility | 12.6% (one source); 10.6% (QuantReturns impl., 2007–2025) | CLAIMED, UNVERIFIED | Search-result summary |
| Factor exposure | Zero exposure to carry or momentum (claimed) | CLAIMED, UNVERIFIED | SSRN 5250499 |
| Benchmark Sharpe | 0.62 (one source, vs. commodity index benchmark) | CLAIMED, UNVERIFIED | QuantReturns.com review |
| Benchmark Max DD | −56.2% (benchmark) | CLAIMED, UNVERIFIED | QuantReturns.com review |

**Critical discrepancy:** The paper claims Sharpe 1.42 and 17.6–18% annual return "over several decades."
An independent QuantReturns implementation for 2007–2025 shows Sharpe 0.92 and 9.68% annual return.
This ~35–45% reduction in both metrics between the full-sample claim and the shorter independent
implementation must be explained before the strategy's true performance can be assessed. Possible
explanations include: (1) the paper's "several decades" includes an earlier, more profitable period
(e.g. pre-2007 or the 2000s commodity supercycle); (2) the independent implementation uses a smaller
universe or different data source; (3) the paper's 1.42 is pre-cost while 0.92 is post-cost; or
(4) implementation methodology differs.

## Community Sentiment

**Positive:**
- QuantSeeker (May 2025 tweet): "New paper by Rossi et al. on short-term basis reversals in commodity
  futures markets. Strategies that exploit this predictability, either in the cross-section or the time-
  series, generate substantial risk-adjusted returns."
- Quantitativo (Substack note): "One-week holding. 17.6% annual returns and 1.42 Sharpe. Zero exposure to
  known risk factors."
- QuantScraper (tweet): "Adjacent maturity spreads show negative autocorrelation — a basis reversal driven
  by differential price sensitivity to news across the futures curve."
- QuantReturns review: implemented independently on 2007–2025 data; confirms the directional finding (0.92
  Sharpe vs. 0.62 benchmark); broadly endorses the concept.

**Critical discussion:**
- CXO Advisory reviewed the paper but the detailed analysis is paywalled.
- No published academic critiques yet found (paper posted May 2025, run date June 2026). The strategy is
  new and has not faced sustained academic scrutiny.
- No independent replication that publicly disputes the findings (though the lower 0.92 Sharpe in
  independent implementation vs. 1.42 claimed in the paper is implicitly a partial challenge).

**No known criticism threads on ForexFactory, r/algotrading, or MQL5 forums** (the strategy is in academic
finance rather than retail forums).

## Why This Might Be Nothing

### 1. Most likely benign explanation: liquidity premium, not information asymmetry

The front-month futures contract is more liquid and has more trader participation than the second month. If
noise traders and less-sophisticated speculators primarily trade the front month, the spread overreaction to
news may be compensating for providing liquidity to the front-month market — i.e., the strategy earns a
liquidity risk premium rather than an information arbitrage return. If so, it is a genuine risk-adjusted
return, but it is not "pure alpha" — it requires being a liquidity provider during high-volatility events
when the spread moves most.

### 2. The cost case: costs may be material for some implementations

The paper states "pre-cost Sharpe ratios above 1" — if the headline 1.42 is pre-cost, the net performance
depends critically on how the calendar spread is traded. If executed as a spread order (M1 vs M2
simultaneously), costs are minimal (1–3bps/round-trip for major commodity spreads). But if M1 and M2 are
legged separately, or for illiquid agricultural contracts, costs could be 5–20bps per leg, significantly
eroding the net edge. The strategy involves ~416 round-trip spread trades per year. ANALYSIS: At 5bps/RT
this is ~21bps/year of costs. At 20bps/RT it is ~83bps/year. Given a reported 10.6% volatility and Sharpe
around 1.0, even 50bps/year of costs would reduce Sharpe from 1.0 to ~0.9. This is not catastrophic but
is not negligible.

### 3. Regime dependence: commodity supercycle and financialization

The "several decades" of data almost certainly includes the 2000s commodity supercycle (2003–2008) where
commodity futures curves were structurally in backwardation across many sectors due to supply shocks. In
that regime, information-driven M1 volatility was very high, likely inflating the spread autocorrelation
signal. The independent 2007–2025 implementation (Sharpe 0.92, 9.68% annual) still shows the edge but at
reduced magnitude — consistent with regime decay after the supercycle ended.

Also, financialization of commodity futures (increasing participation of algorithmic traders, index funds,
and institutional investors since 2005–2010) may have partially arbed the information-speed differential
between M1 and M2 contracts, explaining the gap between paper claims and independent implementation.

### 4. Sample/overfitting concerns

The strategy has only TWO parameters (number of long/short commodities to select = 4). This is very
parsimonious, which argues against overfitting. The monotonic relationship between spread-return rank and
forward return (described in community summaries) is a good sign. However:
- The 39-commodity universe creates 39 × (39−1)/2 = 741 possible pairs — but the strategy uses only 8
  positions, which is appropriately small relative to the universe.
- The paper has not been peer-reviewed and the full parameter robustness analysis is unavailable in
  accessible sources.
- The "several decades" claim is vague; without knowing the exact data period, it is impossible to rule out
  regime cherry-picking (e.g., if the sample ends December 2024 and the strategy started working in 1990
  when commodity market structure first allowed systematic spread trading).

### 5. What single piece of evidence would most change my mind

A peer-reviewed publication with: (a) explicit transaction cost modeling per contract type, (b) a formally
declared post-publication OOS period (e.g. 2020 onward tested after paper was finalized in 2024), and (c)
the edge surviving after controlling for the commodity liquidity factor and the VIX-conditional volatility
regime. Without this, the strategy is a credible but unconfirmed working paper finding.

## Strengths / Weaknesses

**Strengths:**
- Mechanistically plausible: differential information incorporation speed is a documented feature of futures
  curves (especially in agricultural and energy markets with localized supply shocks).
- Very simple signal (one input: prior-week M1−M2 spread return) with only one free parameter (number of
  holdings = 4), making overfitting relatively unlikely for the core finding.
- Approximately market-neutral: spread positions have limited net commodity directional exposure.
- Cross-asset generalizability: the effect is also found in stock index futures and bonds, which is a
  robustness test across different markets.
- Independent replication confirms the direction (Sharpe 0.92 vs. 0.62 benchmark over 2007–2025).
- Zero exposure to carry and momentum (claimed): genuine diversification if true.

**Weaknesses:**
- Not peer-reviewed (SSRN working paper, Georgetown).
- Key performance metrics likely pre-cost (Sharpe 1.42 possibly pre-cost; independent impl. Sharpe 0.92 is
  lower and more likely post-cost or near-cost).
- Significant gap between paper-claimed performance (17.6%/1.42) and independent implementation (9.68%/0.92)
  unexplained.
- No explicit stop-loss, position sizing details, or drawdown-management rules in available sources.
- "Several decades" is too vague to assess sample selection precisely.
- Universe limited to Norgate Data's 39 contracts; some specialty commodities may lack adequate liquidity.
- No open code confirmed.

## Confidence Score

### Per-Dimension Breakdown

| Dimension | Weight | Score | Weighted |
|-----------|--------|-------|---------|
| Logical / economic soundness (falsifiable) | 3 | 6 | 18 |
| Transparency & reproducibility | 2 | 7 | 14 |
| Evidence auditability | 2 | 4 | 8 |
| Risk management quality | 2 | 5 | 10 |
| Cost & capacity realism | 2 | 4 | 8 |
| Honest treatment of drawdowns / failure | 1.5 | 6 | 9 |
| Robustness evidence (OOS / walk-forward / multi-market) | 1.5 | 6 | 9 |
| Survived independent scrutiny | 1 | 5 | 5 |
| **Total** | **15** | — | **81** |

**Latent score:** round(81 / 150 × 100) = **54**
**Evidence auditability = 4 → cap at 59**
**Confidence = min(54, 59) = 54 (cap does not bind; latent 54 < 59)**

`latent 54 (capped to 59 pending verification: SSRN working paper only; not peer-reviewed; likely pre-cost metrics; no open code confirmed; performance gap vs. independent implementation unexplained)`

**Band: Experimental**

**Scoring rationale:**
- Logic 6/10: Plausible differential-speed mechanism with stated falsifiability conditions; not fully
  specified (speed differential not modeled quantitatively).
- Transparency 7/10: Rules are fully reproducible from descriptions; no open code but 100% rule clarity.
- Evidence 4/10: Georgetown SSRN working paper; credible authors; but not peer-reviewed; no open code.
- Risk management 5/10: Market-neutral structure good; no explicit stop or sizing beyond equal-weight.
- Cost/capacity 4/10: "Pre-cost" qualifier on Sharpe; costs likely manageable for spread trades but not
  explicitly modeled in accessible summaries.
- Drawdowns 6/10: Max DD −17% to −24% stated; regime dependence (stronger in high volatility) disclosed.
- Robustness 6/10: Works in bond futures and stock index futures (cross-asset); independent replication
  confirms directional finding; no formal walk-forward or declared OOS split.
- Scrutiny 5/10: Community attention is positive; independent replication confirms; no critical academic
  response yet; CXO analysis paywalled.

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Low — one signal, weekly computation, straightforward sorting.
- **Scalability:** High for major commodity futures; moderate for smaller agricultural contracts.
- **Automation feasibility:** High — fully rule-based, no discretion required, weekly frequency.

## MQL5 / Python / Pine Feasibility

- **Python:** High feasibility. Data from Norgate Data (paid), Quandl/Bloomberg/Reuters for commodity
  futures M1/M2. Weekly computation of spread returns, rank sort, position construction.
- **MQL5:** Low — MQL5 is designed for FX/CFD on MT4/MT5. Commodity futures calendar spreads require
  futures broker infrastructure (Interactive Brokers, Schwab, TradeStation, etc.).
- **Pine Script:** Low feasibility. TradingView has some commodity futures data but not all 39 contracts
  with M1/M2 split available in a form suitable for cross-sectional ranking.

## Similar Strategies

- `commodity-intramarket-correlation-momentum` — also a commodity systematic strategy; uses 4-ETF
  correlation signal to switch between momentum and reversal (vs. direct spread reversal here).
- `tsmom-china-commodity-futures` — time-series momentum on commodity futures (China-specific); uses
  1-month return sign, not M1-M2 spread dynamics.
- `bitcoin-max-min-trendrev` — uses max/min price signals for mean-reversion/continuation in crypto;
  analogous in spirit but different asset class and mechanism.

## Red Flags

- Performance gap between paper claim (1.42 Sharpe) and independent implementation (0.92 Sharpe):
  unexplained; requires scrutiny. `CLAIMED, UNVERIFIED`
- "Pre-cost Sharpe ratios above 1" language in community summaries suggests headline 1.42 is pre-cost.
  Cost treatment in the paper itself is NOT REPORTED from accessible sources.
- "Several decades" is too vague; exact period not confirmed.
- Not peer-reviewed.
- No open code.

## Source Links

| Source | URL | Accessed |
|--------|-----|----------|
| SSRN 5250499 (Rossi, Zhang, Zhu) | https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5250499 | 2026-06-04 |
| Quantitativo Substack note | https://substack.com/@quantitativo/note/c-117932838 | 2026-06-04 |
| QuantReturns review | https://quantreturns.com/strategy-review/short-term-basis-reversal/ | 2026-06-04 |
| QuantReturns Substack | https://quantreturns.substack.com/p/when-futures-overreact-a-weekly-edge | 2026-06-04 |
| QuantSeeker tweet | https://x.com/quantseeker/status/1923330759202377791 | 2026-06-04 |
| QuantScraper tweet | https://x.com/QuantScraper/status/1974406171101393206 | 2026-06-04 |
| CXO Advisory (paywalled) | https://www.cxoadvisory.com/commodity-futures/commodity-futures-term-structure-reversals/ | 2026-06-04 |

## Analyst Notes

**FACTS (from sources):**
- Paper by Rossi, Zhang, Zhu (Georgetown), SSRN 5250499, posted May 11, 2025.
- Strategy: weekly cross-sectional ranking of M1−M2 spread returns; long 4 bottom, short 4 top; 1-week hold.
- 39 commodity futures from Norgate Data.
- Paper performance (directly from Quantitativo and QuantReturns summaries): 17.6–18% annual return,
  Sharpe 1.42–1.45, "several decades."
- QuantReturns independent implementation (2007–2025): 9.68% annual return, Sharpe 0.92, vol 10.6%,
  max DD −17%.
- Community summaries state "pre-cost Sharpe ratios above 1" for both variants.
- Effect also found in stock index futures and bonds (paper claim).
- "Zero exposure to carry or momentum" (paper claim).

**ANALYSIS:**
- The discrepancy between paper metrics and independent implementation is the key unresolved question.
  Most likely explanations (in order of probability): regime decay (paper includes stronger pre-2007 period),
  different data availability (paper: 39 contracts from inception; QuantReturns: may have fewer contracts in
  early period), and cost treatment (pre-cost vs. post-cost).
- The mechanism (front-month overreaction) is economically sound and falsifiable. The effect being present
  in bonds and equity index futures supports that this is a genuine market microstructure phenomenon, not
  commodity-specific data mining.
- The strategy is attractive for its simplicity (one parameter, one signal), but the evidence base
  (single unreviewed working paper + one independent implementation) is insufficient to place high confidence.

**ASSUMPTIONS:**
- Norgate Data coverage begins with sufficient commodity contracts for this strategy to be live-tradeable
  starting mid-1990s (ASSUMPTION).
- Equal-weight position sizing assumed throughout; the paper may use vol-targeted sizing (NOT CONFIRMED).
- The 1.42 Sharpe is pre-cost (inferred from "pre-cost Sharpe ratios above 1" language; ASSUMPTION).

## Future Research Needed

1. Access full SSRN paper to: confirm exact data period, confirm cost treatment (pre/post), obtain per-
   sector or per-commodity performance breakdown.
2. Peer review status: check if paper has been submitted to JFE, RFS, JFM, or similar journal.
3. Check if costs survive across the full commodity spectrum (especially energy vs. agricultural).
4. Replicate independently using freely available data (CME Group open data, Quandl futures).
5. Verify the cross-asset generalization claim (bonds, equity index futures) with independent data.
6. Assess sensitivity to the number of long/short commodities (k=4) — is the choice optimal or robust?
