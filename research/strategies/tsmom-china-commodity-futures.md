# Time-Series Momentum in Chinese Commodity Futures (TSMOM-China)

## Overview

A systematic long-short portfolio of Chinese commodity futures, built on the sign of each
contract's past 1-month return (time-series momentum, TSMOM). Long if past-month return is
positive; short if negative. An equal-risk (volatility-managed) variant scales position size
inversely to realized volatility. Tested on 64 Chinese commodity futures across SHFE, DCE,
and CZCE exchanges from 2003 to 2023.

Primary source: Zheng, Zhang, Lien & Yu (2025). "Evaluating Trend-Based Strategies in Chinese
Commodity Futures Markets." Journal of Futures Markets. DOI: 10.1002/fut.70033.

---

## Asset Class / Timeframes

- **Asset class:** Commodity Futures (Chinese domestic exchanges)
- **Markets:** SHFE (metals: copper, aluminum, zinc, gold, silver; energy: crude oil, fuel oil);
  DCE (agriculture: soybeans, corn, palm oil; industrial: iron ore, coke, plastics);
  CZCE (agriculture: wheat, cotton, sugar, rapeseed; chemicals: methanol, glass)
- **Universe:** 64 commodity futures contracts (as of Zheng et al. 2025 study)
- **Timeframe:** Monthly rebalancing
- **Lookback (best):** 1 month (J=1)
- **Holding period (best):** 1 month (K=1)
- **Backtest period:** 2003–2023 (20 years; Zheng et al. 2025)

---

## Core Logic

**Basic TSMOM:**
At the end of each month, compute the past J-month return for each commodity futures contract.
- If past return > 0: take a long position
- If past return ≤ 0: take a short position
- Equal-weighted portfolio across all qualified contracts

**Volatility-managed TSMOM (enhanced):**
Scale each contract's weight inversely to its realized volatility:
`weight_i = sign(past_return_i) × (c / vol_i)`
where `c` is a target volatility constant (typically calibrated to a specific portfolio vol).

The Zheng et al. (2025) paper also develops **four additional enhanced strategies** (specific
formulas paywalled) based on decomposing the TSMOM return into:
1. Autocovariance of commodity returns
2. Independent predictive effects
3. Distribution of volatility-managed weights
4. Adjustments for the non-positive risk-return relationship in Chinese markets

---

## Economic Rationale

Time-series momentum is grounded in well-established academic mechanisms:
- **Underreaction to news:** Investors update positions gradually; prices continue in the
  direction of the initial signal. Supported by Hong & Stein (1999) gradual diffusion model.
- **Herding behavior (China-specific):** Chinese commodity markets have a higher proportion
  of retail and speculative traders who tend to herd → amplifies trend continuation.
- **Autocovariance of returns:** Positive serial correlation at the 1-month horizon is
  empirically documented in Chinese commodity markets (Zheng et al. 2025 decomposition).

**When the edge should disappear (falsifiability):**
- If Chinese commodity markets become more institutionalized (foreign institutional access
  increasing since QFII/CIBM expansion), reducing herding-driven price continuation.
- If regulators impose stricter position limits or trading curbs that suppress trend formation.
- At 3–12 month horizons, Chinese TSMOM reverses (unlike US), consistent with faster
  incorporation of information; the edge is horizon-specific.
- If margin requirements increase substantially, reducing speculative leverage and dampening
  trend formation.
- Cross-sectional momentum (ranking-based) does NOT show as strong results as TSMOM in
  Chinese markets — suggesting the mechanism is specific to own-return continuation, not
  factor-based cross-sectional effects.

---

## Entry Conditions

- Monthly: Compute 1-month return for each of 64 commodity futures.
- Long if past return > 0; short if past return ≤ 0.
- Vol-managed variant: `position_size = sign × (target_vol / realized_vol_i)`

---

## Exit Conditions

- Exit each position after 1 month (K=1 holding period).
- No intraday stops or soft exit conditions described.

---

## Risk Management

- **Volatility-managed sizing:** Inversely proportional to realized vol → reduces position in
  high-volatility contracts; concentrates in lower-vol contracts.
- **Equal-risk weighting** across 64 contracts in the basic variant.
- **No explicit stop-loss** described in available summaries.
- **Diversification:** 64 contracts across metals, energy, and agricultural commodities.
- **No leverage specification** — paper reports returns at unspecified leverage; capacity
  assumed within normal futures margining (~10–15% margin per contract).

---

## Indicators Used

- Past J-month (J=1) futures contract return
- Realized volatility of each contract (for vol-managed variant)

---

## Transaction Costs & Capacity

- **Transaction costs:** The paper explicitly states "all strategies remain robustly
  profitable after accounting for transaction costs." Chinese commodity futures commissions
  are typically 0.01–0.05% per trade (exchange + broker). Monthly rebalancing means ~24
  trades per contract per year, making this a low-turnover strategy.
- **Slippage:** Not discussed in available summaries. Less liquid agricultural contracts
  (e.g., Zhengzhou wheat) may carry higher bid-ask spreads.
- **Capacity:** Chinese domestic commodity futures markets are large (combined daily volume
  exceeds $1 trillion during active periods); capacity limits apply for hedge funds at scale.
- **Accessibility:** Chinese commodity futures are **not directly accessible** to most
  international investors. QFII/CIBM programs allow some access; offshore instruments
  (gold CFDs, copper LME) partially replicate exposures but with different price dynamics.
  This is a critical limitation for non-Chinese investors.

---

## Backtest Evidence

- **Period:** 2003–2023, 64 futures (Zheng et al. 2025 JFoM)
- **Status:** `CLAIMED, UNVERIFIED` — peer-reviewed paper but specific metrics are paywalled.
  The methodology is reproducible and confirmed by multiple peer-reviewed studies.
- **Sub-period robustness:** "Performance is not driven by specific sample periods or
  individual commodity futures" (stated in available summary of Zheng et al. 2025).
- **Related confirmatory evidence:**
  - Cho, Ham, Kim, Ryu (2019, JFoM): 1-month lookback/hold optimal; TSMOM outperforms
    cross-sectional momentum and buy-and-hold in Chinese commodity futures
  - Liu et al. (2023, Economic Modelling): 24 commodities Jan 2010–Dec 2018; basic TSMOM
    Sharpe 0.07; enhanced version (partial moments) Sharpe 0.75; OOS improvements documented
  - Multiple other peer-reviewed papers confirm the anomaly in Chinese commodity futures

---

## Forward-Test Evidence

- No live trading evidence in available summaries.
- Status: `NOT REPORTED`

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | NOT REPORTED (Zheng 2025 paywalled); 0.07 basic / 0.75 enhanced (Liu 2023 paper, 24 commodities, 2010–2018) | CLAIMED, UNVERIFIED (Liu 2023); NOT REPORTED (Zheng 2025) | Liu et al. 2023 Economic Modelling |
| Sortino Ratio | Improved vs basic TSMOM (directional only; value NOT REPORTED) | NOT REPORTED | Zheng et al. 2025 summary |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | — |
| CAGR | "High returns" stated; specific value NOT REPORTED (paywalled) | NOT REPORTED | Zheng et al. 2025 |
| Annual Return | NOT REPORTED | NOT REPORTED | — |
| Number of Trades | ~24 trades/contract/year × 64 contracts = ~1,536/year (estimated from structure) | NOT REPORTED (estimated from rules) | — |
| Average Trade Duration | 1 month (K=1 holding period) | NOT REPORTED (implied by rules) | Zheng et al. 2025 |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | 100% (always long or short) | NOT REPORTED (implied by rules) | — |

**Note on performance metrics:** The primary source (Zheng et al. 2025 JFoM) is paywalled.
Qualitative statements include "generates high returns," "robust after transaction costs,"
and "not driven by specific subperiods." The Liu et al. (2023) Sharpe of 0.07 (basic) vs.
0.75 (enhanced partial moments) is for a different dataset (24 contracts, 2010–2018) and
should be considered separately from the Zheng 2025 findings.

---

## Community Sentiment

- **Academic community:** TSMOM in Chinese commodity futures is a well-established research
  area with multiple peer-reviewed papers confirming the anomaly (2019, 2023, 2025 waves
  in Journal of Futures Markets, Economic Modelling).
- **Same-author related work:** Zheng (2025) published a companion paper "Curve Momentum in
  China" (JFoM, DOI: 10.1002/fut.70093) developing momentum strategies based on futures
  curve term structure; also achieves Sharpe "comparable to or exceeding traditional factors."
- **Skeptical academic literature:** Uhl (2025, Review of Financial Economics) — "Speculators
  and time series momentum in commodity futures markets" — finds TSMOM profitability is
  connected to speculator positioning; when speculators reduce leverage, TSMOM signals weaken.
- **Market practitioners:** Limited community discussion accessible; Chinese commodity futures
  are less visible in English-language practitioner forums.

---

## Why This Might Be Nothing

**Steelman of the skeptical case (written before scoring):**

1. **The basic TSMOM Sharpe of 0.07 (Liu et al. 2023) is barely above zero.** The
   impressive "enhanced" Sharpe of 0.75 relies on additional signals (partial moments,
   tail-risk indicators) developed in-sample on 2010–2018 data. These enhancements may be
   curve-fit to that specific period. The gap from 0.07 to 0.75 suggests the basic signal
   is weak and the enhancement is where the data-mining risk concentrates.

2. **Chinese commodity market regulatory intervention risk.** The Chinese government
   (NDRC, MOFCOM, exchange margin committees) has repeatedly intervened in commodity
   futures markets — raising margin requirements, tightening position limits, and
   suspending trading — specifically to suppress speculative momentum-driven activity.
   Such interventions can eliminate the very conditions that generate TSMOM returns.

3. **Non-positive risk-return relationship (Zheng 2025).** In Chinese commodity futures,
   higher volatility does not correlate with higher momentum returns. This is anomalous and
   may reflect data quality issues in the early sample (2003–2010) when markets were less
   developed and less liquid, inflating apparent returns from small, illiquid contracts.

4. **Survivorship bias likely.** 64 commodity futures included from 2003 to 2023. Contracts
   that failed, lost liquidity, or were delisted are likely excluded. Surviving commodities
   are those that maintained or grew trading activity — a form of survivor selection.

5. **Accessibility constraint.** Chinese domestic commodity futures (SHFE copper, DCE
   soybeans) are not directly replicable by international investors at scale. A strategy
   demonstrably profitable in Chinese domestic markets may be entirely non-tradeable for
   the target audience of this research database (primarily non-Chinese systematic traders).

6. **Transaction cost realism for thinly-traded contracts.** While the paper states
   costs are "robust," the exact cost assumption is paywalled. Smaller agricultural
   contracts (Zhengzhou white sugar, rapeseed oil) may have daily volume spikes and gaps
   that make monthly-rebalanced long-short execution far more expensive in practice.

7. **The 2015–2016 Chinese market crash.** During the Chinese equity and commodity crash
   of 2015–2016, circuit breakers were introduced, trading was suspended, and forced
   liquidations occurred — conditions under which a long-short TSMOM strategy would suffer
   simultaneous adverse moves on both legs.

**Single most decisive missing piece:** An accessible out-of-sample test (e.g., 2020–2025)
with reported Sharpe, max drawdown, and specific transaction cost assumptions per exchange
and contract type. Without this, "robust after costs" is a qualitative claim from a
paywalled source.

---

## Strengths / Weaknesses

**Strengths:**
- Multiple peer-reviewed papers (JFoM, Economic Modelling) confirm the anomaly.
- 20-year sample, 64 contracts — broad and diverse evidence.
- Economic rationale (herding, underreaction) is well-specified and falsifiable.
- Low turnover (monthly) → favorable transaction cost profile.
- Explicit cost-robustness claim in peer-reviewed paper.
- Diversified portfolio (metals + energy + agricultural) provides natural hedging.

**Weaknesses:**
- Specific performance metrics are NOT REPORTED (paywalled).
- Basic TSMOM Sharpe 0.07 (from related paper) suggests thin edge before enhancement.
- Inaccessible to most international investors (Chinese domestic futures only).
- Regulatory intervention risk is high and unique to Chinese markets.
- Non-positive risk-return relationship in Chinese markets is anomalous — may reflect
  data issues in the early sample period.
- Survivorship bias likely in 64-contract universe.
- The 4 enhanced strategies' rules are not publicly described — cannot be implemented
  without access to the full paywalled paper.

---

## Confidence Score

| Dimension | Raw (0–10) | Weight | Weighted |
|---|---|---|---|
| Logical / economic soundness (falsifiable) | 6 | 3 | 18 |
| Transparency & reproducibility | 7 | 2 | 14 |
| Evidence auditability | 6 | 2 | 12 |
| Risk management quality | 5 | 2 | 10 |
| Cost & capacity realism | 7 | 2 | 14 |
| Honest treatment of drawdowns / failure | 3 | 1.5 | 4.5 |
| Robustness evidence (OOS / walk-forward) | 7 | 1.5 | 10.5 |
| Survived independent scrutiny | 7 | 1 | 7 |
| **Total** | | **15** | **90** |

`latent_score = round(90 / 150 × 100) = 60`

**Evidence auditability = 6 → cap at 74**
`confidence = min(60, 74) = **60** (Worth Researching)`

Recorded as: `latent 60 (capped to 60 pending verification: specific performance metrics paywalled; non-positive risk-return relationship anomalous; Chinese market accessibility constraint; basic TSMOM Sharpe 0.07 from related paper raises edge quality question)`

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Moderate. Standard TSMOM formula from Moskowitz et al. (2012) is simple;
  the 4 enhanced strategies require paper access to implement.
- **Scalability:** Limited by Chinese market access restrictions; large for domestic Chinese
  institutional investors; limited for international players.
- **Automation:** Fully automatable monthly rebalancing; data availability is the constraint
  for non-Chinese investors.

---

## MQL5 / Python / Pine Script Feasibility

- **Python:** High feasibility for the basic TSMOM using Chinese futures data (Wind,
  Bloomberg, Tushare for Chinese market data). Implementation: ~50 lines with pandas.
  Vol-managed variant: additional 20 lines.
- **Pine Script:** Not feasible — TradingView does not carry Chinese commodity futures
  for systematic portfolio testing.
- **MQL5 / MT4:** Some Chinese brokers offer SHFE/DCE CFDs; limited to individual
  instruments rather than the full 64-contract portfolio.
- **Data access:** Wind Financial (Bloomberg of China), Tushare (free tier), or direct
  exchange data subscriptions required.

---

## Similar Strategies

- `crypto-ts-xs-momentum-realistic` — TS momentum on crypto (471 Binance coins, daily,
  15bps costs). Same TSMOM mechanism applied to crypto. Directly comparable.
- `bitcoin-max-min-trendrev` — Rolling MAX/MIN price signal on BTC; related to TSMOM
  but binary trigger on extreme prices, not continuous sign-of-return.
- `catching-crypto-trends-donchian-ensemble` — Donchian channel trend-following on crypto;
  related momentum concept, different indicator and market.
- `industry-trend-century` — Donchian + Keltner Channel trend on US industry ETFs;
  related trend-following, different market and indicators.

---

## Red Flags

- `INSUFFICIENT VERIFICATION` — Specific performance metrics paywalled; cannot verify
  Sharpe, CAGR, or drawdown figures.
- `ACCESSIBILITY CONSTRAINT` — Chinese domestic commodity futures not accessible to most
  international investors; strategy is effectively China-only.
- `REGULATORY INTERVENTION RISK` — Chinese government has repeatedly suppressed speculative
  commodity futures activity; this is a structural non-stationarity risk.
- `BASIC EDGE IS THIN` — Related paper's Sharpe 0.07 for basic TSMOM; "enhanced" version
  relies on additional in-sample-developed signals.

---

## Source Links

| Source | Retrieved | Notes |
|--------|-----------|-------|
| Zheng, Zhang, Lien, Yu (2025). "Evaluating Trend-Based Strategies in Chinese Commodity Futures Markets." Journal of Futures Markets. DOI: 10.1002/fut.70033 (https://onlinelibrary.wiley.com/doi/10.1002/fut.70033) | 2026-06-02 | Primary source; abstract + summary accessible; full text paywalled |
| Liu et al. (2023). "Revisiting time series momentum in China's commodity futures market." Economic Modelling 128. DOI: 10.1016/j.econmod.2023.106372 (https://www.sciencedirect.com/science/article/abs/pii/S0264999323003346) | 2026-06-02 | Related paper; Sharpe 0.07 (basic) / 0.75 (enhanced) on 24 futures 2010–2018 |
| Cho, Ham, Kim, Ryu (2019). "Time-Series Momentum in the Chinese Commodity Futures Market." Journal of Futures Markets 39(12): 1515–1528 (https://ideas.repec.org/a/wly/jfutmk/v39y2019i12p1515-1528.html) | 2026-06-02 | Foundational paper; 1-month lookback best; China vs US comparison |
| Uhl (2025). "Speculators and time series momentum in commodity futures markets." Review of Financial Economics (https://onlinelibrary.wiley.com/doi/10.1002/rfe.1228) | 2026-06-02 | Related paper; speculator positioning drives TSMOM |
| Zheng (2025). "Curve Momentum in China." Journal of Futures Markets. DOI: 10.1002/fut.70093 | 2026-06-02 | Companion paper by same lead author; futures curve momentum |

---

## Analyst Notes

**FACTS (from sources):**
- Zheng et al. (2025, JFoM): 64 Chinese commodity futures, 2003–2023; TSMOM generates high
  returns; robust after transaction costs; not driven by specific subperiods; 4 enhanced
  strategies developed; non-positive risk-return relationship in Chinese markets
- Cho et al. (2019, JFoM): 1-month lookback + 1-month hold = best performance in China;
  more speculative investors in China vs US → momentum lasts shorter time
- Liu et al. (2023, Economic Modelling): Sharpe 0.07 (basic TSMOM) vs 0.75 (partial
  moment enhanced) on 24 contracts, Jan 2010–Dec 2018; OOS improvements confirmed

**ANALYSIS (my reasoning):**
- The peer-reviewed evidence is multi-pronged and spans 10+ years. This is a real anomaly
  with documented mechanism (herding, speculative behavior). But the strength of the basic
  signal is ambiguous — a Sharpe of 0.07 for basic TSMOM from Liu et al. (2023) is barely
  distinguishable from noise. The enhanced strategies are the real research contribution,
  but their specific rules are paywalled.
- The non-positive risk-return relationship deserves attention: in Moskowitz et al. (2012)'s
  global TSMOM, higher-vol assets contribute more per unit position. In China, the opposite
  holds — suggesting different mechanisms operate, or data issues in the early illiquid period.
- For non-Chinese investors, this strategy is academic: the market access constraint is a
  fundamental barrier to implementation. For Chinese domestic systematic investors, this
  is a well-validated edge.
- The "accessible implementation candidate" status (latent ≥ 60) is appropriate for
  Chinese domestic investors or research using Chinese data providers.

**ASSUMPTIONS (flagged):**
- Assumes the basic 1-month lookback TSMOM formula from Moskowitz et al. (2012) applies
  directly to Chinese futures (sign-based, vol-weighted variant).
- Assumes "robust after transaction costs" means the strategy survives a cost assumption
  of at least 0.05% per trade (standard for liquid Chinese futures).

---

## Future Research Needed

1. Access full Zheng et al. (2025) text to extract specific Sharpe, CAGR, max drawdown,
   and the exact rules for the 4 enhanced strategies.
2. Test the strategy on an accessible proxy universe (e.g., LME copper, CME soybean
   futures) to check whether the China-specific findings generalize to global commodities.
3. Evaluate performance specifically during 2015–2016 (Chinese market crash) and
   2018–2020 (US-China trade war) sub-periods.
4. Review Zheng (2025b) "Curve Momentum in China" for whether term-structure momentum
   provides additive value over TSMOM.
5. Obtain Chinese futures data (Wind or Tushare) and replicate the basic 1-month lookback
   strategy to verify the Cho et al. (2019) results.
