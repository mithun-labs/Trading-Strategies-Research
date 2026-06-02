# Gold/Bitcoin Dual Momentum (Competing Stores of Value)

## Overview

A weekly-rebalanced tactical allocation strategy that applies Gary Antonacci's dual momentum
framework to a two-asset universe — SPDR Gold Trust (GLD) and iShares Bitcoin Trust (IBIT).
At each Wednesday rebalance, the strategy allocates to the asset with superior X-week relative
momentum **and** positive absolute momentum; if neither qualifies, it moves to cash. A 20%
annualized-volatility cap via position sizing provides an additional risk overlay.

Source: Quantpedia blog (Dujava, April 2026). Intellectual foundation: Antonacci (2014, 2016)
dual momentum / paired-switching methodology.

---

## Asset Class / Timeframes

- **Asset class:** Alternatives (Commodity — Gold; Crypto — Bitcoin)
- **Instruments:** GLD (SPDR Gold Trust) + IBIT (iShares Bitcoin Trust)
  - Pre-Jan 2024: BTC/USD proxy or BITO used (methodology not fully specified by source)
- **Rebalancing:** Weekly, on Wednesdays
- **Backtest period:** Dec 31, 2018 – April 2026 (~7.5 years)

---

## Core Logic

At every Wednesday rebalance:

1. Calculate the X-week return of GLD and IBIT.
2. **Relative momentum:** Identify which asset has the higher X-week return.
3. **Absolute momentum filter:** Require the selected asset's X-week return > 0%.
4. **Position:** Long the qualifying asset; cash if neither return is positive.
5. **Vol cap:** Calculate the selected asset's 12-week rolling standard deviation.
   Size the position such that portfolio annualized volatility ≤ 20%.

Lookback period X is the key parameter, tested at: 1, 2, 3, 4, 6, 8, 12, 20, 24, 28 weeks.
A composite variant averages signals across X = 4, 8, and 12 weeks simultaneously.

---

## Economic Rationale

Gold and Bitcoin occupy related but distinct "store of value" niches:
- **Gold:** centuries-old safe haven; inflation hedge; demand from central banks, jewelry, and
  risk-off investors. Performs best during systemic crisis and elevated real uncertainty.
- **Bitcoin:** speculative digital asset; sometimes characterized as "digital gold" with higher
  risk-on sensitivity, protocol-level scarcity, and retail/institutional speculative demand.

The dual momentum framework exploits the observation that assets with positive recent momentum
tend to continue outperforming (relative momentum premium; see Jegadeesh/Titman 1993,
Asness et al. 2013), while absolute momentum protects against holding both assets during
simultaneous bear markets.

**When the edge should disappear (falsifiability):**
- If Bitcoin and Gold become persistently highly correlated (both move as one risk cluster),
  relative switching adds no value.
- If the momentum premium decays as more systematic traders implement this exact signal.
- During fast regime reversals (e.g., a 50%+ Bitcoin flash crash within a single week),
  where weekly rebalancing cannot react in time.
- If neither asset sustains positive absolute momentum for extended periods (cash penalty).
- In low-volatility choppy regimes where momentum signals whipsaw repeatedly.

---

## Entry Conditions

- Signal: IBIT X-week return > GLD X-week return **AND** IBIT X-week return > 0% → long IBIT
- Signal: GLD X-week return > IBIT X-week return **AND** GLD X-week return > 0% → long GLD
- Otherwise: hold cash
- Position size: `(0.20 / (12-week rolling annualized vol of selected asset)) × portfolio`

---

## Exit Conditions

- Exit current position if the weekly rebalance signal shifts to the other asset or to cash.
- No intra-week hard stop defined by source.

---

## Risk Management

- **Absolute momentum filter (cash option):** Exits to cash if neither asset shows positive
  X-week momentum — provides implicit bear market protection.
- **20% annualized volatility cap:** 12-week rolling standard deviation used to size down
  positions during high-volatility regimes.
- **No explicit hard stop loss or max-drawdown halt** defined in source.
- **No leverage** implied by source.
- **Single-asset at a time:** never holds both simultaneously; concentration risk present.

---

## Indicators Used

- X-week simple return of GLD and IBIT
- 12-week rolling standard deviation of selected asset (for vol cap sizing)

---

## Transaction Costs & Capacity

- **Costs:** Weekly ETF trading (GLD, IBIT) incurs bid-ask spread and brokerage commission.
  GLD and IBIT are highly liquid ETFs with spreads typically < 0.03%; weekly turnover ~2×/year
  (when signal switches) is manageable. However, the source **does not explicitly model or
  report transaction costs**; this must be flagged.
- **During Bitcoin stress:** IBIT spreads can widen during circuit breakers or extreme
  volatility; slippage may be non-trivial.
- **Capacity:** GLD and IBIT have >$50B AUM each; capacity constraints do not apply for
  retail or moderate institutional sizing.
- **Tax / short-term gains:** Weekly switching generates short-term capital gains in taxable
  accounts — a significant drag not modeled.

---

## Backtest Evidence

- **Period:** Dec 31, 2018 – April 2026 (single backtest window; no OOS split).
- **Source:** Quantpedia blog (Dujava, April 2026). Methodology: applied dual momentum
  rules to GLD + IBIT/BTC proxy; 10 lookback variants tested.
- **Status:** `CLAIMED, UNVERIFIED` — Quantpedia is a practitioner blog, not a peer-reviewed
  source. Methodology and data cannot be independently audited from this source.
- **Critical caveat:** IBIT launched January 11, 2024. Pre-2024 data necessarily uses a proxy
  (BTC/USD spot or BITO ETF). The proxy substitution introduces a dataset construction
  discontinuity that is not fully disclosed in available summaries.
- **10 lookbacks tested without OOS holdout:** selecting the best-performing lookback
  (8-week) ex post is a textbook form of data mining.

---

## Forward-Test Evidence

- IBIT has traded since January 2024 — approximately 15 months of live data available.
- No independent live-trading verification reported.
- Status: `NOT REPORTED`

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | 1.64 (8-wk); 1.47 (composite 4+8+12-wk); 1.37 (vol-capped composite) | CLAIMED, UNVERIFIED | Quantpedia blog (Dujava, Apr 2026) |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | 0.98 (vol-capped composite) | CLAIMED, UNVERIFIED | Quantpedia blog (Dujava, Apr 2026) |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | −49.40% (composite uncapped); −12.27% (vol-capped composite) | CLAIMED, UNVERIFIED | Quantpedia blog (Dujava, Apr 2026) |
| CAGR | 79.91% (8-wk); 64.73% (composite); 12.01% (vol-capped composite) | CLAIMED, UNVERIFIED | Quantpedia blog (Dujava, Apr 2026) |
| Annual Return | NOT REPORTED per year | NOT REPORTED | — |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | NOT REPORTED | NOT REPORTED | — |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | NOT REPORTED | NOT REPORTED | — |

**Benchmarks reported (CLAIMED, UNVERIFIED):**
- 50/50 GLD+IBIT static: 38.65% CAGR, Sharpe 1.12
- Bitcoin buy-and-hold: 46.66% CAGR, Sharpe 0.73
- Composite uncapped annualized vol: 44.14%; vol-capped: 8.77%

---

## Community Sentiment

- **Quantpedia community:** The blog post appears as part of Quantpedia's curated research
  library. Quantpedia has a reputation as a rigorous strategy tracker, though its blog posts
  are not peer-reviewed.
- **Antonacci dual momentum framework (parent strategy):** Widely discussed in the
  practitioner community. A Robot Wealth review notes Antonacci's broader GEM/AGM strategy
  "has continuously trailed its benchmark since [approximately] 2012" due to bond exposure
  changes. Criticism includes: (a) recency bias from the period used to develop the
  framework; (b) whipsaw in choppy markets; (c) heavy bond dependence in the original model.
- **Bitcoin/Gold tactical allocation:** Limited independent analysis. No academic paper
  specifically validates this exact two-asset dual momentum construct.
- **Substack posts claiming 72% return with 3.86% drawdown** on related Bitcoin momentum
  strategies exist but are cherry-picked short periods — not credible evidence.

---

## Why This Might Be Nothing

**Steelman of the skeptical case (written before scoring):**

1. **Data mining via 10 lookback variants.** The source explicitly tests X = 1, 2, 3, 4, 6, 8,
   12, 20, 24, 28 weeks and the composite averages of subsets. The headline 8-week result
   (79.91% CAGR, Sharpe 1.64) is the best-performing variant over this specific 7.5-year
   window. No OOS holdout test is performed. With 10 variants tested and no correction for
   multiple comparisons, finding one that dramatically outperforms by chance is likely.

2. **Regime luck — Bitcoin's structural bull market 2018–2026.** The backtest period coincides
   with Bitcoin's rise from ~$3,000 to $100,000+ (roughly 30× appreciation). A strategy that
   correctly allocated to Bitcoin for much of 2020–2021 would dominate the return profile
   regardless of signal quality. The dual momentum mechanism may be capturing a one-time
   structural run, not a repeatable edge.

3. **CAGR >50% triggers automatic scrutiny.** The 8-week uncapped CAGR of 79.91% and
   composite uncapped CAGR of 64.73% both exceed the 50% threshold. These figures are
   structurally explained by Bitcoin's bull-run dominance in the backtest window, not by
   a robust systematic edge. Realized compound growth of 60–80%/year is not plausible as
   a forward expectation.

4. **IBIT proxy discontinuity (pre-2024).** IBIT only exists from January 2024. The pre-2024
   portion of the backtest uses a proxy (likely BTC/USD spot or BITO). BTC/USD spot and IBIT
   may differ in tracking error, fee drag, and accessibility for retail investors, introducing
   a methodology inconsistency.

5. **Transaction costs not modeled.** Weekly rebalancing in a taxable account generates
   short-term capital gains. Even at 0.03% spread per trade, annual round-trip costs on a
   weekly-rebalancing strategy are modest (~1.5% on 100% turnover/year), but this drag is
   unquantified and untested. More importantly, the short-term tax drag is significant for
   US taxable investors.

6. **Uncapped drawdown (-49.40%) reveals the real risk.** Bitcoin's bear market
   (2021–2022) produced drawdowns of ~65–75%. The strategy's -49.40% uncapped DD shows
   the momentum signal failed to exit before major damage. The vol-capped version reduces
   this at the cost of much lower returns, but the -12.27% figure relies on a 20% vol target
   during a period when Bitcoin often realized 60–80% annualized vol.

7. **No independent replication.** There is no academic paper or independent analysis
   confirming these results on the same instruments and time period.

**The single piece of evidence that would most change my view:** An independent, out-of-sample
backtest (e.g., 2013–2018 using BTC/USD + gold spot) combined with a pre-specified lookback,
showing Sharpe > 1.0 after transaction costs and short-term tax drag, would significantly
raise confidence. That evidence does not exist in available sources.

---

## Strengths / Weaknesses

**Strengths:**
- Rules are fully transparent and reproducible.
- Absolute momentum filter provides bear market protection (cash option).
- Vol cap addresses position sizing risk.
- Assets are liquid ETFs with minimal execution friction.
- The dual momentum framework is well-studied academically in other markets.
- Captures an interesting structural relationship between competing store-of-value narratives.

**Weaknesses:**
- All evidence is CLAIMED, UNVERIFIED (Quantpedia blog, no peer review).
- Extreme headline returns (79.91% CAGR) are driven by Bitcoin's structural bull run.
- 10 lookbacks tested without OOS — significant data mining risk.
- Short 7.5-year backtest window.
- IBIT proxy discontinuity pre-2024.
- Weekly switching generates short-term capital gains in taxable accounts.
- Concentrated single-asset exposure at all times.
- No stop loss; crypto flash crashes can produce significant intra-week losses.
- Antonacci's original dual momentum framework has underperformed benchmarks since ~2012
  in some applications, suggesting the concept may not translate reliably to new asset pairs.

---

## Confidence Score

| Dimension | Raw (0–10) | Weight | Weighted |
|---|---|---|---|
| Logical / economic soundness (falsifiable) | 5 | 3 | 15 |
| Transparency & reproducibility | 9 | 2 | 18 |
| Evidence auditability | 3 | 2 | 6 |
| Risk management quality | 6 | 2 | 12 |
| Cost & capacity realism | 4 | 2 | 8 |
| Honest treatment of drawdowns / failure | 7 | 1.5 | 10.5 |
| Robustness evidence (OOS / walk-forward) | 1 | 1.5 | 1.5 |
| Survived independent scrutiny | 2 | 1 | 2 |
| **Total** | | **15** | **73** |

`latent_score = round(73 / 150 × 100) = 49`

**Evidence auditability = 3 → cap at 59**
`confidence = min(49, 59) = **49** (Experimental)`

Recorded as: `latent 49 (capped to 49 pending verification: Quantpedia blog only; no peer review; 10 lookbacks tested without OOS)`

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Low. Two ETFs, one signal, weekly rebalancing.
- **Scalability:** Near-unlimited for retail and institutional given GLD/IBIT AUM.
- **Automation:** Fully automatable. Weekly Wednesday rebalance, single comparison, vol calc.
  Suitable for Python / MQL5 / Pine Script implementation.
- **Taxable account drag:** Significant concern for US investors due to short-term gains.
  Tax-advantaged accounts (IRA, 401k) preferred.

---

## MQL5 / Python / Pine Script Feasibility

- **Python:** Trivial with pandas/yfinance. ~30 lines.
- **Pine Script (TradingView):** Implementable on weekly IBIT/GLD charts with relative return
  crossover and vol-scaling.
- **MQL5 / MT4:** Applicable if broker offers GLD and BTC CFDs, which some do.
- **Note:** Live trading requires a broker offering both US ETFs (GLD, IBIT) simultaneously,
  or equivalent CFDs for non-US brokers.

---

## Similar Strategies

- `gold-cross-asset-regimes` — Long GLD when BOTH GLD and IEF 12M return positive; otherwise
  cash. Different: bond-based regime filter, no Bitcoin, monthly rebalancing.
- `catching-crypto-trends-donchian-ensemble` — Multi-period Donchian channel trend on BTC.
  Different: BTC-only, no gold, technical breakout signal.
- `bitcoin-max-min-trendrev` — Rolling MAX/MIN price triggers on BTC daily. Different:
  BTC-only, no gold, different signal.

---

## Red Flags

- `CLAIMED, UNVERIFIED` — Quantpedia blog, not peer-reviewed; no open code.
- `DATA MINING` — 10 lookback variants tested; best (8-week) selected; no OOS.
- `REGIME LUCK` — 7.5-year window dominated by Bitcoin structural bull run.
- `INSUFFICIENT VERIFICATION` — No independent replication; IBIT proxy pre-2024.
- CAGR > 50% on uncapped variants triggers automatic scrutiny.

---

## Source Links

| Source | Retrieved | Notes |
|--------|-----------|-------|
| Quantpedia blog — "Dual Momentum Allocation Between Physical Gold and Bitcoin (Digital Gold)" (https://quantpedia.com/dual-momentum-allocation-between-physical-gold-and-bitcoin-digital-gold/) | 2026-06-02 | Primary source; strategy rules and all performance metrics |
| Antonacci (2014) — "Dual Momentum Investing" (book) | N/A — cited by source | Intellectual framework |
| Web search summary — Gold vs BTC backtest details | 2026-06-02 | Quantitative metrics extracted |
| Web search — Community criticism of Antonacci dual momentum (Robot Wealth, QuantifiedStrategies) | 2026-06-02 | Criticism context |

---

## Analyst Notes

**FACTS (from sources):**
- Strategy rules as described: relative + absolute momentum, weekly rebalancing, 20% vol cap
- Data period: Dec 31, 2018 – April 2026
- 10 lookback periods tested (1–28 weeks)
- Composite = average of 4, 8, 12-week signals
- IBIT launched January 11, 2024

**ANALYSIS (my reasoning):**
- The uncapped 79.91% CAGR is almost entirely explained by Bitcoin's structural appreciation
  during the 2018–2026 window. A simple buy-and-hold BTC strategy returned 46.66% CAGR in
  the same period — the "added value" of the signal is ~33pp of CAGR, which is plausible
  as genuine momentum alpha but impossible to confirm without OOS testing.
- The vol-capped composite (12.01% CAGR, 1.37 Sharpe) is a more realistic forward
  expectation and is the only version worth considering for implementation planning.
- Implied Calmar calculation from capped version: CAGR 12.01% / MDD 12.27% = 0.98, which
  matches the reported figure and suggests internal consistency in the metrics.
- 10 lookback variants with no multiple-testing correction implies that at the 5% significance
  level, at least 1 of the 10 would appear significant by chance alone.

**ASSUMPTIONS (flagged):**
- Assumes Bitcoin proxy pre-2024 adequately represents IBIT behavior.
- Assumes weekly Wednesday close prices are available without slippage for rebalancing.

---

## Future Research Needed

1. OOS backtest on 2013–2018 period (pre-strategy development) using BTC/USD proxy + gold.
2. Walk-forward analysis with pre-specified lookback (e.g., 8 weeks, to test if the best
   lookback remains stable across sub-periods).
3. Cost-inclusive backtest: add 0.03% round-trip spread per trade + annual 25bps for tracking.
4. Evaluation during the 2022 crypto winter specifically: did the cash option protect? What
   was the max drawdown during that regime?
5. Test on non-US gold proxies (XAUUSD spot) to check whether ETF wrapping affects signal.
6. Peer-reviewed academic study applying dual momentum to competing store-of-value assets
   across longer global history (e.g., silver vs gold, BTC vs ETH).
