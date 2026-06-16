# Dynamic Multi-Pair Trading Strategy in Cryptocurrency Markets with Deep Reinforcement Learning

## Overview

A hierarchical pairs-trading framework for cryptocurrency perpetual futures that combines
statistical pair selection (cointegration + Hurst exponent filtering) with a Deep Reinforcement
Learning (DRL) execution overlay. The DRL agent (PPO + LSTM) governs trade timing and sizing
within a fixed risk boundary, replacing the ad-hoc threshold rules of classical pairs trading.

**Source:** arXiv 2606.04574 — Lebiedź & Ślepaczuk, University of Warsaw (Faculty of Economic
Sciences, Department of Quantitative Finance and Machine Learning). Submitted June 3, 2026.
61 pages, 37 figures, 16 tables. Not peer-reviewed.

## Asset Class / Timeframes

- **Market:** Cryptocurrency perpetual futures — Binance USD-M Futures segment
- **Timeframe:** 1-hour candles
- **In-sample:** 2024
- **Out-of-sample (OOS):** 2025

## Core Logic

The strategy operates in three sequential layers:

**Layer 1 — Pair Selection ("Filter-then-Rank"):**
1. Apply a cointegration test to the full universe of available USD-M perpetual futures.
2. Filter cointegrated pairs using the **Hurst exponent** (computed via rescaled-range /
   R/S analysis): pairs with H < 0.5 are mean-reverting; pairs with H ≥ 0.5 (random walk
   or trending) are excluded. This separates genuine mean-reverting spreads from spurious
   cointegration driven by structural breaks or trending anomalies.
3. Rank surviving pairs by quality metrics (exact ranking formula NOT REPORTED from accessible
   sources) to select the active trading universe.

**Layer 2 — Execution ("Fixed Risk, Adaptive Mean"):**
A Proximal Policy Optimization (PPO) agent with a Long Short-Term Memory (LSTM) recurrent layer
governs execution decisions:
- **"Fixed Risk":** Deterministic position limits constrain the RL agent at all times — a
  safety/shielding layer that prevents the agent from taking positions beyond pre-set risk
  boundaries regardless of policy output. Exact parameters NOT REPORTED.
- **"Adaptive Mean":** The estimated mean of the spread (the reversion target) is dynamically
  updated — not fixed at a constant, allowing the agent to adjust to slow-moving regime shifts
  in the spread level.

**Layer 3 — Portfolio Construction:**
- Single-pair monthly backtests serve as building blocks; each pair is evaluated independently
  over a discrete one-month trading window.
- Capital is compounded trade-to-trade within each monthly window (strict sequential
  compounding; capital base updated on each trade close).
- Multiple active pairs are aggregated (aggregation logic NOT REPORTED from accessible sources).

## Economic Rationale

**Supporting rationale:** Cointegrated cryptocurrency futures pairs share common fundamental
drivers — both instruments are exposed to the same collateral dynamics, funding rate regimes,
market-maker liquidity provision, and systemic risk events (exchange failures, regulatory shock).
When a correlated pair's spread deviates from its mean, arbitrageurs (whether humans or
bots) typically close the gap. The Hurst exponent filter is theoretically motivated: it directly
tests whether the spread is mean-reverting vs. random-walking, which is the key structural
requirement for any pairs strategy.

**DRL timing rationale:** Classical pairs trading uses fixed entry/exit thresholds (e.g.,
enter at ±2σ, exit at 0σ). DRL replaces these hard rules with a learnable policy, potentially
capturing nonlinear dependencies, time-varying convergence speeds, and multi-period structure
that fixed rules miss.

**Falsifiability:** The mean-reversion rationale predicts the edge should disappear when:
(a) crypto correlation structure breaks down (during major exchange failures, liquidity crises,
de-pegging events), (b) the Hurst exponent rises toward 0.5+ across the pair universe (regime
shift to random-walk behavior), (c) perpetual futures funding rates make carry costs prohibitive,
or (d) arbitrage competition from HFT bots eliminates the spread before any 1-hour signal can
be acted upon. The DRL timing layer lacks an explicit falsification condition — this is a
concern (see Why This Might Be Nothing).

## Entry Conditions

- Selected pair passes Hurst filter (H < 0.5 for the spread) AND cointegration test.
- PPO+LSTM policy outputs a position signal given the current state observation (state
  features NOT REPORTED from accessible sources — likely include: spread z-score, realized
  volatility, time-of-day, position, P&L).
- "Fixed Risk" boundary must not be violated (safety layer pre-empts policy output if needed).

## Exit Conditions

- PPO+LSTM policy outputs a close/reverse signal.
- "Fixed Risk" risk boundary triggers forced exit (stop-loss component).
- End of monthly window (position closed regardless).

## Risk Management

- **Deterministic safety layer ("deterministic shielding"):** Hard position limits imposed
  regardless of policy output. Described as "safe reinforcement learning via deterministic
  shielding for pair trading." Specific limits NOT REPORTED.
- **Monthly window isolation:** Each pair trades within a one-month window; this provides
  a natural limit on maximum holding period.
- **"Fixed Risk" model:** Likely implies fixed position sizing (dollar or percent of capital
  per trade); exact mechanism NOT REPORTED.

## Indicators Used

- **Cointegration test** (method NOT REPORTED — Engle-Granger or Johansen likely)
- **Hurst exponent** (rescaled-range / R/S analysis)
- **PPO+LSTM policy** (state features, network architecture, hyperparameters NOT REPORTED)
- Likely: spread z-score, rolling mean/volatility of the spread (implicit in adaptive mean)

## Transaction Costs & Capacity

- **Exchange:** Binance USD-M Futures. Binance maker/taker fees for futures: typically
  0.02%/0.04% maker/taker (as of 2025). For a 1-hour strategy with active rebalancing,
  transaction costs are relevant.
- **Funding rate:** Perpetual futures carry a funding rate (typically ±0.01%/8h = ~1.1%/yr
  when flat, but can be extreme during trend periods). For pairs, funding rates may partially
  offset if the pair is long/short symmetric, but divergence is possible.
- **Cost modeling:** "Transaction costs" are mentioned in abstract-level descriptions, but
  the specific assumptions (spreads, slippage, funding rates) are NOT REPORTED from accessible
  sources. Whether costs are fully embedded in the simulated P&L is CLAIMED but not verifiable.
- **Capacity:** Not discussed. Crypto perpetual futures have high liquidity for top pairs
  (BTC-USD, ETH-USD), but exotic pairs may have significant slippage at any size.

## Backtest Evidence

**In-sample (2024):** PPO+LSTM agent trained on 2024 data. Specific IS metrics NOT REPORTED.

**Out-of-sample (2025):** The optimized RL policy "achieved out-of-sample performance that
substantially outperformed the heuristic baseline" — CLAIMED, UNVERIFIED.

A stationary circular block bootstrap robustness check confirms that the agent's
"risk-adjusted outperformance is statistically significant at the 10% level" — CLAIMED, UNVERIFIED.

Primary source (arXiv PDF) returned HTTP 403 on all access attempts; specific metrics below are
from web search abstracts and search snippets only. Full tables in the 61-page paper are
inaccessible.

## Forward-Test Evidence

NOT REPORTED. No live trading or paper-trading results referenced in accessible sources.

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | NOT REPORTED | NOT REPORTED | arXiv 2606.04574 (inaccessible) |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | arXiv 2606.04574 (inaccessible) |
| Calmar Ratio | NOT REPORTED (mentioned as evaluation metric) | NOT REPORTED | arXiv 2606.04574 (inaccessible) |
| Profit Factor | NOT REPORTED | NOT REPORTED | arXiv 2606.04574 (inaccessible) |
| Win Rate | NOT REPORTED | NOT REPORTED | arXiv 2606.04574 (inaccessible) |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | arXiv 2606.04574 (inaccessible) |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | arXiv 2606.04574 (inaccessible) |
| CAGR | NOT REPORTED | NOT REPORTED | arXiv 2606.04574 (inaccessible) |
| Annual Return | NOT REPORTED | NOT REPORTED | arXiv 2606.04574 (inaccessible) |
| Number of Trades | NOT REPORTED | NOT REPORTED | arXiv 2606.04574 (inaccessible) |
| Average Trade Duration | NOT REPORTED (≤ 1 month max) | NOT REPORTED | arXiv 2606.04574 (inaccessible) |
| Recovery Factor | NOT REPORTED | NOT REPORTED | arXiv 2606.04574 (inaccessible) |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | arXiv 2606.04574 (inaccessible) |
| Exposure Percentage | NOT REPORTED | NOT REPORTED | arXiv 2606.04574 (inaccessible) |

**Qualitative result:** "Substantially outperforms heuristic baseline; statistically significant
at 10% level (circular block bootstrap)" — CLAIMED, UNVERIFIED.

## Community Sentiment

The paper was submitted June 3, 2026 — approximately two weeks before this research pass. No
community discussion, forum threads, or independent replications have been identified. The
Ślepaczuk group at University of Warsaw has a track record of serious quantitative finance
research (multiple arXiv papers on systematic trading), which modestly increases credibility
relative to an unknown single-author preprint — but peer review has not occurred.

No criticism threads found.

## Why This Might Be Nothing

**1. Extremely short training and testing windows — most likely benign explanation:**
IS: 2024 only (1 year). OOS: 2025 only (1 year). Crypto 2025 was a bull market year
(BTC peaked above $100k). In bull markets, crypto pair correlations tend to be higher and
more stable, making pairs trading easier. The strategy may be implicitly long bull-market
conditions. One year of training for a DRL model (PPO+LSTM) on 1-hour bars of crypto pairs
is extremely short — the model likely memorizes patterns specific to 2024 market microstructure
rather than learning a generalizable policy.

**2. DRL overfitting — the most dangerous failure mode for RL in finance:**
PPO with LSTM has many learnable parameters. With only ~8,760 hourly data points per pair
in the IS period, the ratio of parameters-to-observations is unfavorable. Small changes in
hyperparameters, random seed, or data preprocessing can produce large swings in OOS
performance. The "statistically significant at 10% level" result from a block bootstrap is
weaker than the standard 5% threshold and provides only marginal comfort against data snooping.

**3. Cost case — the silent killer of crypto pairs strategies:**
Perpetual futures funding rates fluctuate dramatically. During trending markets (which 2025 BTC
was), long-side funding rates can reach 0.01–0.10%/8h, adding 5–50% in annualized carry cost
for net long positions. For pairs strategies, if the long leg is consistently the higher-funding
asset, this can erode edge significantly. Whether funding rates were modeled is NOT REPORTED.

**4. Regime dependence — the pairs assumption:**
Crypto correlation structure is highly unstable. Major disruptions (exchange failures like
FTX Nov 2022, de-pegging events like LUNA May 2022, regulatory shocks) cause all correlations
to surge toward 1 simultaneously and then fracture into new patterns. A strategy trained on
2024 correlation structure may fail immediately when the next structural break occurs. The
Hurst filter helps at any snapshot but does not protect against sudden regime change.

**5. 10% significance is insufficient for structural claims:**
The block bootstrap test confirms "statistically significant at the 10% level" — but not 5%.
In a 61-page paper testing many pairs and hyperparameter combinations, 10% significance from
a single robustness check does not rule out data snooping as the source of apparent performance.

**6. The one piece of evidence that would change my mind:**
An independent live trading log (not simulated) with actual exchange trade records across
multiple 6-month windows covering at least two different crypto market regimes — one bullish
(as in 2024-2025) and one bearish or highly correlated crash regime — with explicit
transaction cost audit (including funding rates).

## Strengths / Weaknesses

**Strengths:**
- Hurst exponent filter is a theoretically motivated improvement over cointegration alone
- "Safe RL" / deterministic shielding concept addresses a real failure mode of naive DRL
- Ślepaczuk group has published rigorously before; 61-page paper suggests depth
- Monthly window isolation limits catastrophic divergence exposure
- OOS test on unseen 2025 data (not full-sample in-sample)

**Weaknesses:**
- All specific performance metrics (Sharpe, Calmar, max DD) are NOT REPORTED from accessible sources
- IS and OOS periods are each only 1 year — far too short for statistical reliability
- DRL hyperparameter search and architecture choices not disclosed
- Funding rates and precise transaction cost model not confirmed
- "10% significance" is a weak statistical threshold
- No open code; no independent replication; 2 weeks from publication
- Crypto 2025 bull regime may not persist

## Confidence Score

**Per-dimension breakdown:**

| Dimension | Score (0–10) | Weight | Weighted |
|-----------|-------------|--------|---------|
| Logical / economic soundness (falsifiable) | 5 | 3 | 15 |
| Transparency & reproducibility | 4 | 2 | 8 |
| Evidence auditability | 2 | 2 | 4 |
| Risk management quality | 5 | 2 | 10 |
| Cost & capacity realism | 3 | 2 | 6 |
| Honest treatment of drawdowns / failure | 3 | 1.5 | 4.5 |
| Robustness evidence (OOS / walk-forward / multi-market) | 3 | 1.5 | 4.5 |
| Survived independent scrutiny | 1 | 1 | 1 |

**Weighted sum:** 53 / 150 → latent 35

**Verification cap:** Evidence auditability = 2/10 (arXiv preprint; all primary content HTTP 403;
no open code confirmed; no peer review) → cap at 59.

**confidence = min(35, 59) = 35 → Low Confidence**

`latent 35 (capped to 35 — verification cap is 59 but latent score itself is 35; dominated by
short training window, all metrics NOT REPORTED, no peer review, no open code, single regime OOS)`

## Complexity / Scalability / Automation Feasibility

- **Complexity:** High. Requires DRL training infrastructure (GPU), cointegration + Hurst
  screening at each monthly reset, real-time spread monitoring and execution.
- **Scalability:** Limited by Binance API rate limits and liquidity in exotic pairs.
- **Automation:** In principle automatable via Binance API; but DRL retraining cadence
  (monthly?) adds operational complexity. Model drift between training cycles is a real risk.

## MQL5 / Python / Pine Feasibility

- **Python:** Feasible in principle — stable-baselines3 (PPO), statsmodels (cointegration),
  hurst library (R/S analysis). But exact architecture and hyperparameters NOT REPORTED.
- **MQL5:** Impractical for DRL component (DRL inference in MQL5 is non-standard).
- **Pine Script:** Not feasible for DRL.

## Similar Strategies

- `fx-cointegration-pairs-trading` — same cointegration-based stat arb approach; equity ETF context; limited profitability
- `crypto-ts-xs-momentum-realistic` — different signal (TSMOM vs. pairs mean reversion); same Binance universe
- `catching-crypto-trends-donchian-ensemble` — same market, opposite signal (trend vs. mean reversion)
- `graph-clustering-pairs-trading` — ML-enhanced pairs selection for US equities

## Red Flags

- **All specific metrics NOT REPORTED** — cannot evaluate actual risk/return profile
- **Very short IS and OOS windows** — 1 year each; insufficient for regime coverage
- **DRL overfitting risk** — high parameter count relative to short training period
- **10% significance only** — weak statistical evidence
- **Crypto perpetual funding rates not confirmed modeled** — silent cost
- **No open code, no peer review, published 2 weeks ago** — zero community scrutiny

## Source Links

| URL | Retrieved | Used For |
|-----|-----------|----------|
| https://arxiv.org/abs/2606.04574 | 2026-06-16 | Main paper abstract |
| https://arxiv.org/html/2606.04574 | 2026-06-16 | HTML version (HTTP 403) |

## Analyst Notes

**FACTS (from accessible web search snippets):**
- Authors: Damian Lebiedź and Robert Ślepaczuk (University of Warsaw)
- Submission: arXiv, June 3, 2026; arXiv:2606.04574
- Market: Binance USD-M Futures perpetual contracts, 1-hour data
- IS: 2024, OOS: 2025
- Pair selection: cointegration + Hurst exponent ("Filter-then-Rank")
- Execution: PPO+LSTM with "Fixed Risk, Adaptive Mean" model
- Risk: "deterministic shielding" (safety layer)
- Architecture: single-pair monthly windows; trade-to-trade compounding
- Bootstrap test: stationary circular block bootstrap; significant at 10% level
- 61 pages, 37 figures, 16 tables

**ANALYSIS:**
- Hurst exponent filtering is genuinely novel compared to standard cointegration-only approaches
  and addresses a known weakness (spurious cointegration in trending markets)
- "Safe RL" via deterministic shielding is a principled contribution to RL-in-finance
- However, with IS=1yr and OOS=1yr, no multi-regime evidence, and a 10% significance threshold,
  the paper is preliminary. The methodology is interesting but the evidence is insufficient.
- Crypto perpetual funding rates (not mentioned in accessible sources) are a hidden cost that
  could substantially alter net-of-cost performance, especially in 2025's positive-funding environment.

**ASSUMPTIONS:**
- Assumed that "transaction costs" mentioned in abstract include at least some fee modeling,
  but this could mean bid-ask half-spread only, not funding rates.
- Assumed monthly window reset implies monthly retraining or model re-deployment; if the
  DRL model is never retrained and runs fixed after 2024 IS period, this is even weaker.

## Future Research Needed

1. **Full paper access** (arXiv PDF currently HTTP 403) — read Tables and results sections for
   specific Sharpe, Calmar, max drawdown figures.
2. **Code release** — Ślepaczuk's group sometimes releases code; monitor for GitHub release.
3. **2026 OOS extension** — if results are updated to include 2026 data (including potential
   bearish regime), that would be far more informative.
4. **Funding rate sensitivity analysis** — understand net returns after perpetual funding costs.
5. **Peer review / citation** — if paper receives peer review and appears in a journal, confidence
   should be reassessed upward.
