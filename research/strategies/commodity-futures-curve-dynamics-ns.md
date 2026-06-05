# Commodity Futures Curve Dynamics (Nelson-Siegel)

## Overview

Applies the Nelson-Siegel (NS) fixed-income term-structure model to commodity futures
price curves to extract three time-varying parameters (level, slope, curvature). Daily
or monthly long-short strategies trade the continuation of recent shifts in each
parameter. The **slope strategy** — betting that today's slope change in the futures
curve predicts tomorrow's slope change — is the dominant strategy, generating an
out-of-sample Sharpe of 1.41 before trading costs and 0.44–1.04 after costs (monthly
rebalancing variant). Published in *Journal of Banking & Finance*, Volume 154, 2023
(peer-reviewed).

## Asset Class / Timeframes

- **Primary market:** Commodity Futures (21 contracts inferred; specific list NOT
  REPORTED in accessible sources — energy, metals, agriculture sectors likely)
- **Timeframe:** Two variants described:
  - **Daily:** Signal formed at end of day t, position held for one day; Sharpe 1.41
    pre-cost; post-cost performance NOT REPORTED for this variant
  - **Monthly (rebalancing at end-of-month):** Turnover 1.36×/year; post-cost Sharpe
    0.44–1.04 across three transaction cost scenarios
- **Evaluation period:** NOT REPORTED precisely from accessible sources
  (paper published 2023; likely sample extends to ~2021)

## Core Logic

**Nelson-Siegel Term Structure Model:**
The NS model parameterizes the commodity futures price at maturity τ as:

```
f(τ) = L + S × [(1 − e^(−λτ)) / (λτ)] + C × [(1 − e^(−λτ)) / (λτ) − e^(−λτ)]
```

where:
- **L (Level):** overall height of the futures curve (parallel shifts)
- **S (Slope):** steepness of the curve; negative = backwardation (near > far),
  positive = contango (near < far)
- **C (Curvature):** hump or butterfly shape of the curve
- **λ:** decay parameter (typically fixed or estimated from the cross-section)

**Signal construction (for each commodity):**
1. Fit the NS model to the cross-section of available futures contracts to extract
   daily time series of L_t, S_t, C_t
2. Compute the one-period change: ΔL_t, ΔS_t, ΔC_t
3. Strategy signal: yesterday's parameter change → predicts tomorrow's parameter change
   (short-run autocorrelation in term structure movements)

**Portfolio construction:**
- Rank all commodities by the signal
- Go long top quintile (or top-N), short bottom quintile (or bottom-N)
- Equal capital to longs and shorts; fully collateralized (no leverage)
- Long-short construction makes the portfolio approximately market-neutral

**Three strategies:**
- **Level strategy:** Long commodities with largest positive ΔL (parallel upward shift);
  Short commodities with largest negative ΔL
- **Slope strategy:** Long commodities with largest positive ΔS (increasing
  steepening/contango shift); Short commodities with largest negative ΔS
- **Curvature/butterfly strategy:** Long commodities with largest positive ΔC;
  Short commodities with largest negative ΔC

**Headline result:** Slope strategy dominates. Level and curvature strategies performance
NOT REPORTED in accessible sources.

## Economic Rationale

**Why the edge should exist:**
Commodity futures curves reflect current and expected future supply/demand imbalances,
storage costs, and convenience yields. When market-wide information hits (e.g., a supply
shock, storage capacity constraint, or macro signal), the full term structure does not
adjust instantaneously — near-term contracts react first, and the adjustment propagates
along the curve over subsequent days. This creates short-term autocorrelation in curve
shape changes. The **slope signal** captures the steepening/flattening momentum:
when the curve steepens rapidly today (e.g., front-month shortage), it tends to steepen
further tomorrow before full arbitrage can occur.

Additionally, the slope is related to the net hedging pressure / convenience yield.
Systematic hedgers (producers) operate at specific maturities; their hedging creates
persistent pressure patterns that propagate slowly.

**When the edge should disappear:**
- When commodity markets become more electronically liquid and arbitrage occurs within
  hours rather than days (edge already degrading for the most liquid contracts)
- During instantaneous supply shocks (2022 Russia/Ukraine gas shock; COVID March 2020
  demand collapse) — term structure adjusts in one session, eliminating autocorrelation
- In markets dominated by financial (not physical) participants, who arb curve shape
  differences immediately
- When investor sentiment is low/fearful — the paper explicitly documents that
  profitability of the slope strategy increases with sentiment; low-sentiment periods
  are documented underperformance regimes
- During economic slowdowns: the paper states the strategy "is in part a compensation
  for drawdowns incurred during economic slowdowns" — this IS a documented failure regime

## Entry Conditions

For each commodity at the end of day t (or end of month):
1. Fit NS model to available futures curve → extract S_t
2. Compute ΔS_t = S_t − S_{t-1}
3. Rank all commodities by ΔS_t
4. Go long top quartile/quintile (largest positive ΔS), short bottom quartile/quintile
   (largest negative ΔS) → equal capital to longs and shorts
5. Hold for one day (daily version) or until end of next month (monthly version)

## Exit Conditions

Position held for the formation period (1 day or 1 month). No intraperiod stops.
Full rebalancing at each period end.

## Risk Management

- **Market neutrality:** Long-short construction with equal capital each side
- **Full collateralization:** No embedded leverage
- Fully-collateralized positions imply T-bill returns on the collateral (common in
  commodity futures research)
- No explicit per-position stop-losses or max-loss limits described
- No position-size scaling by volatility described

## Indicators Used

- **Nelson-Siegel slope parameter (S_t):** Time-varying slope of the futures curve
  extracted by NS regression
- **One-day change ΔS_t:** The momentum signal
- **NS level parameter (L_t) and curvature (C_t):** Used in level and curvature
  strategy variants
- **Investor sentiment:** Used to condition profitability analysis (not explicitly
  a trading signal — described as an explanatory variable for realized returns)

## Transaction Costs & Capacity

- **Costs modeled?** YES — explicitly tested across three transaction cost scenarios
- **Turnover:** ~1.36× annual portfolio turnover (for monthly rebalancing variant)
- **Net-of-cost Sharpe range:** 0.44–1.04 (from 1.41 gross pre-cost) across
  three scenarios
- **Cost survival:** The monthly rebalancing variant "survives reasonable transaction
  costs" across all three scenarios (positive net mean excess returns confirmed)
- **Daily variant:** Cost survival NOT confirmed — post-cost performance for the daily
  rebalancing variant is NOT REPORTED in accessible sources; daily commodity futures
  round-trip costs of 5–15bps applied at daily frequency could severely erode the
  gross Sharpe of 1.41
- **Capacity:** Small-to-medium; depends on the specific commodity futures included.
  Most major commodity futures (crude oil, gold, corn, etc.) are highly liquid and
  can support moderate position sizes. Smaller/less liquid contracts could face
  slippage above the cost scenarios modeled.

**ANALYSIS:** The degradation from Sharpe 1.41 (gross) to 0.44–1.04 (net, monthly)
represents a 27–69% Sharpe reduction from costs alone. The lower bound (0.44) is
barely above the typical CTA per-market Sharpe benchmark of ~0.15–0.20. The headline
Sharpe of 1.41 is pre-cost and, if interpreted for the daily variant, would likely not
survive daily commodity futures costs at all. The monthly version with confirmed 0.44–
1.04 is the more credible number.

## Backtest Evidence

Peer-reviewed in Journal of Banking & Finance (2023), Volume 154. OOS slope strategy
Sharpe = 1.41 (before costs). Multiple specification robustness checks (alternative NS
models). Timing extension magnifies profitability. Specific evaluation period NOT
REPORTED from accessible sources. China extension paper (Zheng, Journal of Futures
Markets) applies the framework to Chinese commodity futures, suggesting multi-market
persistence.

## Forward-Test Evidence

NOT REPORTED. Post-2022 live or out-of-sample evidence not available.

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | 1.41 (pre-cost, slope strategy, OOS) | CLAIMED, UNVERIFIED | Multiple search result summaries of JBF 2023 paper |
| Sharpe Ratio (net) | 0.44–1.04 (monthly variant, 3 cost scenarios) | CLAIMED, UNVERIFIED | Search result summary of JBF 2023 paper |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | |
| Profit Factor | NOT REPORTED | NOT REPORTED | |
| Win Rate | NOT REPORTED | NOT REPORTED | |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | Paywalled |
| CAGR | NOT REPORTED | NOT REPORTED | Paywalled |
| Annual Return | NOT REPORTED | NOT REPORTED | Paywalled |
| Number of Trades | NOT REPORTED | NOT REPORTED | |
| Average Trade Duration | 1 day (daily) or ~1 month (monthly) | NOT REPORTED | Inferred from description |
| Recovery Factor | NOT REPORTED | NOT REPORTED | |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | |
| Exposure Percentage | ~100% long + 100% short (fully collateralized L/S) | NOT REPORTED | Inferred |

**Automatic scrutiny note:** Sharpe of 1.41 exceeds the 3× trigger threshold only after
cost adjustment (net 0.44–1.04 does not trigger). Pre-cost 1.41 triggers scrutiny.
See `## Why This Might Be Nothing`.

## Community Sentiment

No direct community replication (Quantpedia, r/algotrading, forum) found. Published in
Journal of Banking & Finance (rigorous academic peer review). The China extension
(Zheng, JFoM) cites and extends the framework, implying positive reception in the
academic community. No critical commentary found in accessible sources. Joëlle Miffre
(co-author) is a well-known commodity research expert (multiple JBF/JFQA papers on
commodity premia), lending credibility. Quantpedia has catalogued related Bianchi work
(microscopic momentum in commodities) but this specific paper not confirmed as a
Quantpedia strategy entry.

## Why This Might Be Nothing

**1. The 1.41 Sharpe is pre-cost for the daily variant — costs could eliminate it entirely:**
The headline Sharpe of 1.41 is pre-trading-costs, explicitly stated. For a daily
rebalancing strategy on commodity futures (even liquid ones), round-trip costs of 5–15bps
per trade applied DAILY would accumulate to 12–38% annualized cost drag. Even at the
lower bound, a Sharpe of 1.41 gross might survive costs — but this is not confirmed. The
monthly variant's 0.44–1.04 is the confirmed net range, and 0.44 at the pessimistic end
barely justifies the strategy overhead.

**2. Large cost degradation raises questions about robustness:**
The Sharpe declines from 1.41 to as low as 0.44 — a 69% reduction. If the net edge is
this cost-sensitive, then small errors in cost assumptions (slightly higher bid-ask
spreads, price impact, roll costs) could push the net Sharpe below zero. The paper tests
three scenarios, but "reasonable" cost assumptions vary significantly by commodity and
market regime.

**3. Sample-period luck / regime specificity:**
The evaluation period is unknown from accessible sources. If the sample predominantly
covers the commodity supercycle (2002–2014) when commodity curve dynamics were
particularly persistent, results might be overstated for more recent periods. The paper
itself documents that profitability is sentiment-dependent (low sentiment = worse
performance) and drawdown-prone in recessions — these are documented regimes where the
strategy fails.

**4. Modest OOS evidence:**
The statement "out-of-sample Sharpe of 1.41" may refer to an expanding-window OOS test
within the sample, not a true holdout period. Without accessing the full paper, whether
this represents genuine future-period holdout OOS or is merely an in-sample OOS split
is unclear.

**5. No open code or independent replication:**
No public code implementation found. The commodity universe is not confirmed. Without
these, independent verification is impossible.

**6. Falsifiability concern about the sentiment relationship:**
"Profitability increases with investor sentiment" — this means in high-sentiment bull
markets, the strategy works; in bear markets, it underperforms. This is a form of
implicit long-market beta. If the backtested period had more high-sentiment time than
low-sentiment time, the Sharpe overestimates the unconditional long-run edge.

**What single piece of evidence would change my mind:** An independent replication on
a different commodity universe (e.g., European/Asian futures) showing net-of-cost
Sharpe ≥ 0.5 in both high- and low-sentiment regimes, with explicit out-of-sample
period 2015–2023 (to capture post-supercycle and post-COVID regimes).

## Strengths / Weaknesses

**Strengths:**
- Peer-reviewed in Journal of Banking & Finance — commodity research specialist venue
- Transaction costs explicitly modeled with three scenarios — net-of-cost analysis is
  a genuine strength
- NS model has decades of yield-curve application precedent; adaptation to commodity
  futures is principled and relatively transparent
- Market-neutral (long-short) with full collateralization — limited tail risk from
  leverage
- Multiple robustness tests (alternative NS specs, timing overlay)
- China extension paper provides multi-market validation
- Well-known co-authors (Bianchi, Miffre) with established commodity premia research
  track record

**Weaknesses:**
- Pre-cost Sharpe of 1.41 degrades significantly to 0.44–1.04 after costs
- Sentiment-dependent profitability → implicit long bull-market risk
- Economic slowdown documented as a failure regime → pro-cyclical risk
- Full commodity universe and evaluation period not accessible
- No open code for implementation
- Daily variant's post-cost performance not confirmed

## Confidence Score

| Dimension | Weight | Score | Weighted |
|-----------|--------|-------|----------|
| Logical / economic soundness (falsifiable) | 3 | 7 | 21.0 |
| Transparency & reproducibility | 2 | 5 | 10.0 |
| Evidence auditability | 2 | 7 | 14.0 |
| Risk management quality | 2 | 5 | 10.0 |
| Cost & capacity realism | 2 | 6 | 12.0 |
| Honest treatment of drawdowns / failure | 1.5 | 6 | 9.0 |
| Robustness evidence (OOS / walk-forward / multi-market) | 1.5 | 7 | 10.5 |
| Survived independent scrutiny | 1 | 6 | 6.0 |
| **Total** | **15** | | **92.5** |

`latent_score = round(92.5 / 150 × 100) = 62`

**Verification cap:** Evidence auditability = 7 → cap at 74 (range 5–7)
`confidence = min(62, 74) = 62`

**latent 62 (capped to 62 pending full metric verification: specific CAGR/max
drawdown paywalled; daily variant post-cost performance not confirmed; evaluation
period not accessible)**

**Band:** Worth Researching (60–74)

**Note on automatic scrutiny trigger:** The 1.41 pre-cost Sharpe technically exceeds
the > 3 threshold for automatic discussion, but the paper itself transparently
labels it as pre-cost and provides post-cost figures (0.44–1.04). The scrutiny is
addressed above: the pre-cost number is a gross edge, and the post-cost range is what
matters for implementation. The post-cost range (0.44–1.04) does not trigger the > 3
Sharpe threshold. The presentation is honest.

## Complexity / Scalability / Automation Feasibility

**Complexity:** MEDIUM. NS model fitting is standard econometrics (non-linear LS or
Diebold-Li two-step linear regression). Long-short portfolio construction is simple.
Data requirement: full commodity futures curves (multiple maturities per commodity).

**Scalability:** Moderate. Depends on data availability for the full term structure of
21 commodity futures across multiple maturities. Most institutional data providers
(Bloomberg, Refinitiv, Quandl/Norgate) provide this.

**Automation:** Fully automatable in Python. NS fitting: scipy.optimize or statsmodels.
Position construction: pandas/numpy. No ML required.

## MQL5 / Python / Pine Feasibility

- **Python:** FEASIBLE. Diebold-Li NS fitting is well-documented in Python
  (e.g., using scipy for non-linear LS or the two-step Diebold-Li linearization).
  Long-short commodity futures construction: straightforward with pandas.
  Data challenge: need multi-maturity commodity futures data (not in Yahoo Finance).
- **MQL5:** NOT FEASIBLE. MQL5 lacks multi-maturity futures curve data and NS fitting.
- **Pine Script:** NOT FEASIBLE. Pine Script cannot access futures curve data.

## Similar Strategies

- `commodity-basis-reversal-futures` (SSRN 5250499; score 54 Experimental) — also
  a commodity term structure strategy but uses only the M1-M2 spread (single-point
  curve measure) vs. this paper's full 3-factor NS decomposition. The NS slope is a
  richer analog of the M1-M2 spread.
- `tsmom-china-commodity-futures` (JFoM 2025; score 60 Worth Researching) — commodity
  futures time-series momentum on price; similar market (commodity futures) but
  completely different signal (return sign vs. NS slope changes)
- `fx-carry-value-momentum-200yr` (score 51 Experimental) — similar in spirit:
  yield-curve-analog (FX carry) in a non-equity asset class
- Prior curve momentum literature: Koijen et al. (2018) "Carry" (Journal of Financial
  Economics) — related term structure momentum applied to FX, bonds, commodities, and
  equities; NS slope is a specific instance of the carry signal family

## Red Flags

- Pre-cost Sharpe 1.41 headline may overstate live implementation performance
  significantly (triggers Sharpe > 3 scrutiny only pre-cost — post-cost is clean)
- Sentiment-dependent: underperforms in low-sentiment markets
- Daily variant post-cost performance: NOT CONFIRMED
- Full commodity universe and evaluation period: NOT ACCESSIBLE from open sources

## Source Links

| URL | Retrieved | Notes |
|-----|-----------|-------|
| https://arxiv.org/abs/2308.00383 | 2026-06-05 | arXiv preprint (403) |
| https://www.sciencedirect.com/science/article/abs/pii/S0378426623001632 | 2026-06-05 | JBF 2023 publication (403 — paywalled) |
| https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3749061 | 2026-06-05 | SSRN (403) |
| https://ideas.repec.org/a/eee/jbfina/v154y2023ics0378426623001632.html | 2026-06-05 | IDEAS/REPEC (403) |
| https://hal.science/hal-04174414/ | 2026-06-05 | HAL archive (403) |
| https://www.bayes-cid.com/pdf/issues/2023-winter/publications/CID-Winter-2023-Bianchi-etal.pdf | 2026-06-05 | Bayes CID winter 2023 publication copy (403) |
| https://onlinelibrary.wiley.com/doi/10.1002/fut.70093 | 2026-06-05 | China extension paper — Zheng, JFoM (403) |

## Analyst Notes

**FACTS (from sources):**
- Paper: "Exploiting the dynamics of commodity futures curves"
- Authors: Robert J. Bianchi, John Hua Fan, Joëlle Miffre, Tingxi Zhang
- Published: Journal of Banking & Finance, Volume 154, September 2023 (article 106965)
- arXiv: 2308.00383; SSRN: 3749061
- Strategy: NS model applied to commodity futures term structure → level/slope/curvature
  signals → daily or monthly long-short portfolio
- Slope strategy OOS Sharpe: 1.41 (pre-cost) per multiple search result summaries
- Post-cost Sharpe (monthly variant, 3 scenarios): 0.44–1.04
- Annual portfolio turnover: 1.36× (monthly rebalancing variant)
- Profitability correlated with investor sentiment
- Drawdowns documented during economic slowdowns
- China extension paper exists (Zheng, Journal of Futures Markets)

**ANALYSIS (my reasoning):**
- The NS model is the right tool for term structure analysis — its application to
  commodity futures is methodologically sound and well-motivated
- The 0.44–1.04 post-cost range is the economically honest benchmark, not the 1.41
  gross Sharpe. A minimum net Sharpe of 0.44 is weak but positive.
- The upper end of the post-cost range (1.04) is attractive for a commodities strategy;
  the lower end (0.44) is marginal. The 3-scenario range reflects genuine cost
  uncertainty in commodity futures markets.
- The sentiment/slowdown dependence is both a strength (it gives a theoretical
  explanation for when to expect underperformance) and a weakness (the strategy has
  implicit cyclical risk)

**ASSUMPTIONS (flagged):**
- The 21-commodity universe figure comes from the queue note — this was not confirmed
  from accessible sources during this research session
- "End-of-month rebalancing" appears to be a specific variant for the cost analysis;
  the daily variant's cost picture is uncertain
- The OOS Sharpe may be from an expanding-window in-sample OOS design (standard in
  term structure papers) rather than a true future holdout

## Future Research Needed

1. Access full JBF 2023 paper (or HAL open access version) to retrieve:
   - Specific commodity universe (confirm/deny 21 contracts)
   - Exact evaluation period and OOS period dates
   - Max drawdown and CAGR
   - Post-cost Sharpe for the DAILY variant specifically
   - Sub-period performance (pre/post-2014 commodity supercycle end)
2. Check whether the China extension paper (Zheng, JFoM) provides independent
   validation with different commodity data
3. Test whether the NS slope signal is still correlated with conventional term
   structure signals (carry, backwardation) — if so, the "unrelated to previously
   documented risk factors" claim may require examination
4. Assess whether this is codeable from the Diebold-Li (2006) methodology alone
   without the full paper
