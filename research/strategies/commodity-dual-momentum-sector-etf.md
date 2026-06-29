# Commodity Dual Momentum — Sector ETF (Antonacci Framework Applied to 4 Commodity ETFs)

## Overview

A monthly-rebalancing dual momentum strategy applied to four broad commodity sector ETFs.
Relative momentum selects the top M assets by recent return; an absolute trend filter (price
vs. X-month SMA) gates each selected asset — if the asset is below its own SMA, the
allocation goes to cash instead. Tested January 2007 – February 2026. Source: Quantpedia blog
post by Sona Beluska, June 15, 2026, citing Antonacci (2014) dual momentum framework,
Moskowitz/Ooi/Pedersen (2012) TSMOM, and Jegadeesh/Titman (1993) cross-sectional momentum.

## Asset Class / Timeframes

- **Market**: Commodities (US-listed sector ETFs)
- **Instruments**: DBA (agriculture), DBB (base metals), DBE (energy), DBP (precious metals)
- **Timeframe**: Monthly rebalancing (end-of-month)
- **Backtest period**: January 2007 – February 2026

## Core Logic

1. At each month-end, compute the X-month Rate of Change (ROC) for all 4 ETFs.
2. Rank ETFs from highest to lowest X-month ROC.
3. Select the top M ETFs by ROC (relative momentum component).
4. For each selected ETF, check whether its price is above its X-month Simple Moving Average
   (absolute trend filter). If yes, allocate equally. If no, hold cash for that slot.
5. Rebalance monthly. Remaining allocation (cash slots) earns 0% (assumed).

Best parameters identified: X = 3 months, M = 3 assets (out of 4).

The mechanism reduces drawdown relative to pure relative momentum by eliminating exposure to
commodity sectors in downtrends, replacing them with cash.

## Economic Rationale

**FACTS (from source):** Commodity sectors exhibit cross-sectional momentum (past relative
winners tend to continue outperforming in the near term), and individual commodities exhibit
time-series momentum (trend persistence). The absolute SMA filter operationalizes the latter
at the sector level.

**ANALYSIS:** The edge — if genuine — should arise because: (a) commodity supply-demand
imbalances adjust slowly, sustaining price trends; (b) sector-level diversification reduces
idiosyncratic noise that kills momentum at single-commodity level. The edge should be absent
when: (a) macro shocks reverse all commodity sectors simultaneously (correlation spike);
(b) commodity cycle turning points create whipsaws; (c) sectors move together and ranking
has no discriminatory power. This is adequately falsifiable: the mechanism predicts failure
in sudden synchronous reversals and in range-bound, low-trend regimes.

**ASSUMPTIONS:** The dual momentum framework (Antonacci 2014) has been applied to many asset
classes, and this test extends it to a 4-sector commodity universe. The assumed mechanism is
plausible but the evidence base for this specific application is blog-level only.

## Entry Conditions

- Monthly (end-of-month): compute 3-month ROC for DBA, DBB, DBE, DBP.
- Rank by 3-month ROC; identify top 3.
- For each of the top 3, check if closing price > 3-month SMA.
  - Yes → allocate 1/3 of portfolio.
  - No → that slot is cash (0% return).

## Exit Conditions

- An allocated ETF exits at next month-end rebalance if: (a) it falls out of the top-M rank,
  or (b) its price falls below the X-month SMA at rebalance date.
- No intra-month stops or exits — monthly only.

## Risk Management

- Absolute trend filter provides de facto regime exit: below SMA → cash, avoiding holding
  into prolonged drawdowns.
- Equal weighting (1/3 per selected ETF) limits concentration.
- Implicit cash allocation when sector in downtrend acts as capital preservation.
- **GAPS:** No explicit stop-loss, no volatility targeting, no max-loss rule, no position sizing
  by risk. The SMA filter is the sole risk mechanism.

## Indicators Used

- X-month ROC (Return of Change): `(Price_t / Price_{t-X_months} - 1)`
- X-month Simple Moving Average (SMA): average closing price over past X months
- Best parameters: X = 3 months, M = 3

## Transaction Costs & Capacity

- **Costs NOT modeled.** This is an explicit weakness acknowledged by the source.
- Monthly ETF rebalancing costs are relatively low in practice (ETF bid-ask spreads are
  typically 1–5bps for liquid ETFs like DBA/DBB/DBE/DBP) but slippage and any friction
  are absent from the backtest.
- **ANALYSIS:** Monthly turnover on 4 ETFs with equal-weight rebalancing is moderate. At
  ~5bps round-trip per trade per ETF, annual cost impact is modest for monthly holding.
  However, the absolute-trend filter can cause frequent cash switching, potentially adding
  transaction frequency. Even so, for monthly ETF trading, costs are unlikely to erase a
  Sharpe 0.63 gross result entirely — but the precise net figure is unknown.
- **Capacity**: Large, as all 4 ETFs have high AUM and daily trading volumes ($100M+ AUM).
  Capacity is not a constraint.

## Backtest Evidence

CLAIMED, UNVERIFIED — Quantpedia blog post, January 2007 – February 2026:

| Variant | Sharpe | Annual Return | Volatility | Max Drawdown | Calmar |
|---------|--------|---------------|------------|--------------|--------|
| Dual Momentum (X=3, M=3) | 0.63 | 7.54% | 11.92% | -30.07% | 0.25 |
| Relative Momentum (X=6, M=1) | 0.49 | 11.51% | 23.44% | -63.94% | 0.18 |
| Static B&H Benchmark (EW, no rebalance) | 0.30 | 4.76% | 15.79% | -53.25% | — |

All 12 Dual Momentum parameter combinations (varied X and M) outperform the static benchmark
on Sharpe ratio. Only 5 of 12 pure relative momentum variants do so.

## Forward-Test Evidence

NOT REPORTED — No live trading results or forward-test data provided.

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | 0.63 | CLAIMED, UNVERIFIED | Quantpedia blog (Beluska, Jun 2026) |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | 0.25 | CLAIMED, UNVERIFIED | Quantpedia blog (Beluska, Jun 2026) |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | -30.07% | CLAIMED, UNVERIFIED | Quantpedia blog (Beluska, Jun 2026) |
| CAGR | NOT REPORTED | NOT REPORTED | — |
| Annual Return | 7.54% | CLAIMED, UNVERIFIED | Quantpedia blog (Beluska, Jun 2026) |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | ~1 month (inferred from monthly rebalance) | CLAIMED, UNVERIFIED | Quantpedia blog (Beluska, Jun 2026) |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | NOT REPORTED | NOT REPORTED | — |

Note: CAGR is NOT REPORTED despite Annual Return being stated; these may differ slightly
depending on whether compounding or arithmetic averaging is used. Annual Return (7.54%) as
reported is treated as CLAIMED, UNVERIFIED.

## Community Sentiment

No community discussion found. This is a single Quantpedia blog post with no forum thread,
Reddit discussion, or academic commentary. The author (Sona Beluska, Quantpedia Junior Quant
Analyst) has published similar commodity and multi-asset momentum papers (e.g., the active
dual-momentum GTAA post from May 2026, already in this database as `active-dual-momentum-gtaa`
with score 30).

## Why This Might Be Nothing

### 1. Most Likely Benign Explanation

Pure data-mining on a 4-ETF universe with 12 tested parameter combinations is the dominant
concern. With only 4 assets, the "momentum ranking" has almost no discriminatory power —
there is typically only one meaningful position (top vs. bottom ETF) since ranking 4 objects
is trivial. "All 12 combinations outperform the benchmark" is not surprising when the strategy
has many ways to avoid the worst performer and the benchmark is unweighted with no rebalancing.

### 2. The Cost Case

NOT EXPLICITLY MODELED. Monthly rebalancing on liquid ETFs imposes modest friction (~5bps
per round-trip per ETF). At 4 ETFs changing monthly, the annual cost estimate is manageable.
However, the absolute-trend switching from ETF to cash and back can amplify the number of
trades. Net Sharpe is unknown but gross Sharpe 0.63 likely survives modest ETF-level costs —
this is not a presumptive invalidation, but confirmation is required.

### 3. Regime Dependence

The strategy is implicitly long commodity sector beta (it selects the top 3 of 4 ETFs almost
all the time if any are above SMA). A prolonged commodity bear market (e.g., 2011–2015 energy
and metals) where all 4 ETFs trend below SMA simultaneously would place the entire portfolio
in cash — this is actually beneficial protection, but the strategy earns nothing during those
periods. The strategy also relies on the commodity sector exhibiting some cross-sectional
dispersion; if all 4 ETFs move together, ranking is noise.

### 4. Sample / Overfitting Concerns

The backtest starts in 2007 (post-commodity bull run beginning). The 2007–2011 commodity
super-cycle dominated early performance. The parameters (X=3, M=3) were selected in-sample
over 12 combinations with no hold-out OOS period. The "best" combination is effectively
cherry-picked after observing outcomes.

### 5. What Would Change My Mind

An independent OOS backtest (e.g., 2007–2015 training, 2016–2026 test) with explicit
transaction costs and a rebalanced benchmark showing Sharpe > 0.5 would substantially
increase confidence. A peer-reviewed paper replicating this on similar commodity universes
(futures instead of ETFs, other geographies) would be the strongest evidence.

## Strengths / Weaknesses

**Strengths:**
- Simple, transparent, fully reproducible rules
- Antonacci dual momentum is a well-documented framework with multi-asset track record
- Substantially reduces max drawdown vs. relative momentum alone (-30% vs -64%)
- Large-capacity, low-cost instruments (liquid ETFs)
- Intuitive mechanism: avoid commodity sectors in downtrends

**Weaknesses:**
- 4-ETF universe is trivially small; momentum ranking has minimal discriminatory power
- No transaction costs modeled — gross only
- No OOS test; all 12 parameter combinations evaluated in-sample
- Quantpedia blog source only; no academic peer review
- Zero risk-free rate assumption unrealistic (cash slots earn nothing)
- Non-rebalanced static benchmark understates passive alternative performance
- Annual return 7.54% vs 4.76% B&H static — the outperformance is modest in absolute terms
- Max drawdown -30.07% is still substantial despite the trend filter

## Confidence Score

**Per-dimension breakdown:**

| Dimension | Weight | Score | Weighted |
|-----------|--------|-------|----------|
| Logical / economic soundness (falsifiable) | 3 | 6 | 18 |
| Transparency & reproducibility | 2 | 9 | 18 |
| Evidence auditability | 2 | 2 | 4 |
| Risk management quality | 2 | 4 | 8 |
| Cost & capacity realism | 2 | 2 | 4 |
| Honest treatment of drawdowns / failure | 1.5 | 5 | 7.5 |
| Robustness evidence (OOS / walk-forward) | 1.5 | 2 | 3 |
| Survived independent scrutiny | 1 | 1 | 1 |

Weighted sum: 63.5 / 150 × 100 = **latent 42**

Evidence auditability = 2/10 (≤ 4) → verification cap = 59
**confidence = min(42, 59) = 42**

**latent 42 (capped to 42; cap does not bind at latent 42 — evidence auditability ≤ 4 caps at 59; latent is already below cap)**

Band: **Experimental** (40–59)

NOT an implementation candidate (latent 42 < 60).

## Complexity / Scalability / Automation Feasibility

Low complexity. Monthly rebalancing on 4 ETFs. Automation is trivial: compute 3-month ROC
and 3-month SMA at month-end, rank, allocate. Any Python script or spreadsheet can implement
this. No data vendor required beyond free-tier EOD prices.

## MQL5 / Python / Pine Feasibility

- **Python**: Trivial. 10–20 lines with pandas/yfinance.
- **Pine Script**: Feasible. Monthly rebalancing on ETFs is doable in TradingView multi-chart
  setup with alerts.
- **MQL5**: Feasible but less natural (ETF data requires MT5 with CFD feeds).

## Similar Strategies

- `commodity-intramarket-correlation-momentum` — same 4 ETFs (DBA/DBB/DBE/DBP), monthly rebalancing,
  but different mechanism: short-term vs. long-term pairwise correlation switches between
  momentum and reversal rather than dual momentum. Score 45.
- `active-dual-momentum-gtaa` — same Antonacci dual momentum framework applied to 9 multi-asset
  ETFs (SHY, IEF, UUP, GLD, USO, SPY, EFA, QQQ, EEM) with weekly rebalancing. Score 30.
- `gold-bitcoin-dual-momentum` — Antonacci dual momentum on GLD+IBIT (2 assets). Score 49.
- `em-us-momentum-taa` — similar TAA momentum logic on EEM/SPY. Score 49.

## Red Flags

- **No OOS testing** — all 12 parameter combinations evaluated in-sample
- **Costs omitted** — non-trivial for monthly-rebalancing ETF strategy
- **4-ETF universe** — trivially small; ranking provides minimal discrimination
- **Single Quantpedia blog source** — no academic peer review, no independent replication

## Source Links

| URL | Retrieved | Notes |
|-----|-----------|-------|
| https://quantpedia.com/dual-vs-single-momentum-in-commodities-enhancing-risk-adjusted-returns-through-absolute-trend-filtering/ | 2026-06-29 | Primary source: Beluska (June 2026) |

## Analyst Notes

**FACTS (from source):**
- Dual momentum framework from Antonacci (2014): combine relative and absolute momentum.
- 4 commodity ETFs: DBA, DBB, DBE, DBP. Monthly end-of-month rebalancing.
- Best parameters: X=3 month lookback, M=3 assets selected (of 4).
- Best dual momentum result: Sharpe 0.63, Ann. Return 7.54%, Vol 11.92%, Max DD -30.07%,
  Calmar 0.25.
- Benchmark: equal-weight static (no rebalance): Sharpe 0.30, Return 4.76%, Max DD -53.25%.
- All 12 dual momentum combos beat benchmark Sharpe. Only 5 of 12 pure relative momentum
  combos do.
- Backtest period: January 2007 – February 2026.

**ANALYSIS:**
- Latent score 42 reflects the honest picture: the mechanism is sound, rules are clear, but
  the evidence base is a blog post with a trivially small 4-ETF universe and no costs.
- Dual momentum's key value add is drawdown reduction (-30% vs. -64%), not return enhancement
  (7.54% vs 11.51% for relative momentum alone). This is a defensible trade-off, but the
  absolute return improvement over the static benchmark is modest (7.54% vs 4.76%).
- The strategy's main appeal is its simplicity and the 47% drawdown reduction vs. relative
  momentum. For a commodity satellite position with high risk aversion, this framing makes
  sense.
- Closely related to the author's previous work on `active-dual-momentum-gtaa` (score 30),
  which applied the same framework to 9 multi-asset ETFs with worse metrics (weekly rebalancing
  with no cost modeling). This version is modestly better evidence.

**ASSUMPTIONS:**
- 3-month lookback and M=3 are in-sample optimal; OOS performance likely reverts toward the
  12-combo average, which was not reported.
- Zero risk-free rate underestimates the opportunity cost of cash allocations, especially in
  2022–2024 with high rates.

## Future Research Needed

- Independent OOS backtest on a hold-out period (2016–2026 would be a natural split).
- Apply to commodity futures (instead of ETFs) to avoid ETF-specific tracking error and test
  a broader universe.
- Model transaction costs explicitly.
- Add a rebalanced equal-weight benchmark to properly isolate the momentum contribution.
- Test whether the 3-month lookback advantage persists in a walk-forward framework, or whether
  it is an artifact of in-sample optimization.
