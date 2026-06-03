# EM vs US Tactical Allocation — Momentum-Based Approach

## Overview

A Quantpedia blog analysis (Vojtko/Dujava, April 2026) tests whether 6-to-12-month momentum
and trend signals on the EEM/SPY spread can produce positive risk-adjusted returns vs. passive
allocation. The hypothesis: relative performance between Emerging Markets and U.S. equities
follows persistent trends exploitable by simple SMA and ROC timing. Six strategy variants
are tested across three lookback periods; the best result — long-short SMA composite —
achieves CAR 7.13% and Sharpe 0.564 with max DD −20.31% over 1997–2026.

---

## Asset Class / Timeframes

- **Asset class:** Global Equities — Emerging Markets vs. U.S.
- **Instruments:** EEM (iShares MSCI Emerging Markets ETF) and SPY (SPDR S&P 500 ETF)
- **Timeframe:** Monthly (end-of-month signal generation, monthly rebalancing)
- **Data period:** 1997–2026; EEM ETF used from 2003 onwards; index/synthetic data
  implied for 1997–2002 pre-inception period

---

## Core Logic

Two signal types × three configurations:

**Signal types:**
1. **ROC (Rate of Change):** ROCt,n = (Price_t − Price_{t−n}) / Price_{t−n}. Positive ROC
   → momentum signal for that asset; negative → against.
2. **SMA (Simple Moving Average):** Price_t vs. n-month SMA. Above → positive trend;
   below → negative trend.

**Three configurations for each signal:**
1. **Long spread:** Long EEM / short SPY when EM momentum signal > US momentum signal
   (betting on EM outperformance). Cash otherwise.
2. **Short spread:** Long SPY / short EEM when US momentum signal > EM momentum signal
   (betting on US outperformance). Cash otherwise.
3. **Long-short spread:** Always takes a position — long the outperforming asset, short the
   underperforming one, regardless of absolute signal direction.

**Lookback periods tested:** 6, 9, 12 months. Composite blends all three horizons.

**Rebalancing:** Monthly, at end of month.

---

## Economic Rationale

Relative momentum between EM and US equities reflects persistent macro and structural cycles
(growth differentials, commodity super-cycles, dollar cycles, geopolitical risk premiums,
capital flow dynamics). These cycles tend to persist 6–18 months, making them exploitable
by intermediate-horizon momentum signals.

**When the edge should disappear:**
- When the EM-US relative performance cycle shortens to under 6 months (more capital
  responsive to micro changes) — momentum signals whipsaw.
- When the US dominance regime ends and EM mean-reverts sharply — long-short strategies
  built during the US-dominant era will underperform the reversal.
- When more capital deploys systematic EM/US rotation strategies, arbitraging the cycle
  speed up and compressing returns.
- Historically, 2000–2010 saw EM strongly outperform US; 2010–2026 reversed this. The
  strategy's best configurations have been implicitly "long US" for the most recent period.

---

## Entry Conditions

**ROC long-short (12-month, representative):**
- Compute 12-month ROC for both EEM and SPY
- If ROC_EEM > ROC_SPY: go long EEM, short SPY
- If ROC_SPY > ROC_EEM: go long SPY, short EEM
- Hold until next monthly signal

**SMA long-short (composite):**
- For each lookback n ∈ {6, 9, 12 months}, compute SMA_EEM,n and SMA_SPY,n
- Determine direction for each horizon: long EEM if EEM > SMA_EEM (and SPY < SMA_SPY),
  short EEM otherwise; reverse for US
- Blend signals across horizons (equal-weight or not reported specifically)

---

## Exit Conditions

- Signal flip at next monthly rebalancing date
- No explicit stop-loss or intra-month exit rules reported
- Long-short always has a position (no cash allocation in the long-short variant)

---

## Risk Management

- Long-short configuration is naturally hedged: one long leg offsets one short leg
- Monthly rebalancing limits whipsaw costs
- No explicit stop-loss, volatility targeting, or position sizing rules reported
- Max DD −20.31% for composite long-short SMA (reported)
- **Assessment:** Minimal explicit risk management; the long-short structure provides
  implicit hedging but is not equivalent to a full risk management framework.

---

## Indicators Used

- **ROC (Rate of Change):** standard price momentum, lookbacks 6/9/12 months
- **SMA (Simple Moving Average):** standard trend filter, periods 6/9/12 months
- No additional indicators reported

---

## Transaction Costs & Capacity

- **Costs modeled:** NOT REPORTED — no mention of spread, commission, or ETF borrowing costs
- **Practical cost assessment:**
  - Long ETF legs (EEM, SPY): negligible trading costs; both are highly liquid, spreads < 1bp
  - Short ETF legs: ETF borrowing cost for EEM ~50–80bps/year (varies by prime broker);
    SPY borrowing is near zero
  - Monthly rebalancing: approximately 1–2 trades/month; trading costs minimal
  - Net annual drag from EEM short borrow: ~0.5–0.8% per year when in short-EEM position
    (roughly 50% of the time in US-dominant regime) = ~25–40bps/year drag
  - This is material but unlikely to eliminate the reported Sharpe of 0.564
- **Capacity:** ETFs are highly liquid; no capacity constraint at retail scale; institutional
  scale would face minimal market impact

---

## Backtest Evidence

- Quantpedia blog analysis, not peer-reviewed
- Data: 1997–2026 (~29 years; EEM ETF from 2003; pre-2003 uses index data)
- No out-of-sample test, no walk-forward validation
- All metrics below are `CLAIMED, UNVERIFIED`

---

## Forward-Test Evidence

`NOT REPORTED` — no live or paper-trading validation described.

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | 0.564 (composite long-short SMA); 0.523 (6M long-short SMA); 0.497 (composite long-short ROC); 0.461 (6M short SMA); best single: 0.332 (9M short ROC) | CLAIMED, UNVERIFIED | Quantpedia blog (Vojtko/Dujava, Apr 2026) |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | −20.31% (composite long-short SMA, CLAIMED) | CLAIMED, UNVERIFIED | Quantpedia blog (Vojtko/Dujava, Apr 2026) |
| CAGR | 7.13% (composite long-short SMA); 7.84% (6M long-short SMA); 6.44% (12M long-short ROC); 5.69% (composite long-short ROC); 5.40% (6M short SMA) | CLAIMED, UNVERIFIED | Quantpedia blog (Vojtko/Dujava, Apr 2026) |
| Annual Return | NOT REPORTED | NOT REPORTED | — |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | NOT REPORTED (monthly signals → ~1 month average) | NOT REPORTED | — |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | ~100% (long-short always invested; long-only versions: ~50% estimated) | NOT REPORTED | — |

No automatic scrutiny triggers: CAGR < 50%, Sharpe < 3, Max DD > 5%.

---

## Community Sentiment

No independent community discussion found as of 2026-06-03. The Quantpedia blog is widely
read in the quant practitioner community (typically featured on Quantocracy aggregator), but
no independent replication or critique found. Allocate Smartly has independently evaluated
some Quantpedia strategies (e.g., gold-cross-asset-regimes), but no evidence they have
covered this specific blog post. The "short spread" dominant finding aligns with broader
consensus that US equities have structurally outperformed EM in the 2010–2026 period —
independently documented by Dujava/Vojtko in their background paper SSRN 4694624.

---

## Why This Might Be Nothing

### 1. Most likely benign explanation: US dominance regime disguised as momentum signal

The EM-US spread has been persistently negative (US outperforming EM) for most of the 2010–
2026 period. The best strategy configurations ("short spread" and "long-short") are
essentially "be long US equities for most of the period." The momentum signal adds relatively
little if the underlying regime was monotonically US-favorable.

**Test:** Remove the 2010–2026 "US dominance" period and check if the momentum signal still
adds value in the 1997–2009 period when EM and US alternated in leadership (2000–2003 US
underperformed; 2003–2007 EM strongly outperformed; 2008 both crashed; 2009 EM rebounded).
This sub-period test would be the most informative but is NOT reported.

### 2. The cost case

ETF borrowing cost for EEM short (~50–80bps/year) reduces realized performance. This is
not modeled. The composite long-short SMA CAR of 7.13% is likely overstated by ~25–40bps/
year due to omitted borrow costs when in the short-EEM position.

### 3. Multiple-testing concern

Six strategy variants (ROC/SMA × long/short/long-short) × three lookbacks = 18 combinations,
plus a composite. Without multiple-testing correction or walk-forward validation, the best-
performing combination (composite long-short SMA, Sharpe 0.564) benefits from parameter
selection from the same in-sample data.

### 4. Pre-2003 data quality

The 1997–2003 period uses index/synthetic data rather than the EEM ETF. Synthetic pre-
inception data frequently overstates actual performance (no tracking error, no liquidity
costs, no creation/redemption spreads). The ETF-only period (2003–2026) would be more
reliable but is not separately reported.

### 5. No explicit regime detection

The strategy treats momentum as valid in all regimes. In 2022 (dollar surge, EM sell-off),
the momentum signal likely pointed "short EM" already, providing protection. But a mean-
reversion shock in EM relative to US would produce sequential false signals.

### 6. The one piece of evidence that would most change my mind

A walk-forward test showing the strategy performed during BOTH the EM outperformance era
(2000–2010) AND the US outperformance era (2010–2026) with similar Sharpe ratios, would
confirm the signal adds value beyond regime persistence. Currently only the combined period
is reported.

---

## Strengths / Weaknesses

**Strengths:**
- Simple, reproducible rules using freely available ETF data (EEM, SPY)
- Monthly rebalancing means minimal trading costs
- Long-short structure provides inherent hedging
- Long historical data period (29 years)
- The phenomenon (EM-US relative momentum) is supported by academic literature on
  international cross-asset momentum
- Blending multiple horizons improves stability vs. single-parameter optimization
- Quantpedia is a reputable source with editorial standards

**Weaknesses:**
- Quantpedia blog only; no peer review; no underlying academic paper
- No OOS test or walk-forward
- Performance substantially driven by US dominance regime (2010–2026)
- Pre-2003 data is synthetic/index (not ETF)
- ETF borrowing costs not modeled
- Maximum drawdown for individual variants not reported (only composite)
- No position sizing or stop-loss rules

---

## Confidence Score

### Per-Dimension Breakdown

| Dimension | Raw Score | Weight | Contribution |
|-----------|-----------|--------|--------------|
| Logical / economic soundness (falsifiable) | 5 | 3 | 15 |
| Transparency & reproducibility | 8 | 2 | 16 |
| Evidence auditability | 4 | 2 | 8 |
| Risk management quality | 4 | 2 | 8 |
| Cost & capacity realism | 5 | 2 | 10 |
| Honest treatment of drawdowns / failure | 6 | 1.5 | 9 |
| Robustness evidence (OOS / walk-forward) | 4 | 1.5 | 6 |
| Survived independent scrutiny | 2 | 1 | 2 |

**Weighted sum:** 74 / 150 → **latent 49**

**Evidence auditability = 4 → cap at 59**

**`confidence = min(49, 59) = 49`** (Experimental)

*latent 49 (capped to 49 pending verification: Quantpedia blog only; no peer review;
no OOS test; performance driven by US dominance regime 2010–2026; pre-2003 data synthetic;
ETF borrowing costs not modeled)*

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Very low. Two ETFs, one momentum signal, monthly rebalancing.
- **Scalability:** High — EEM and SPY are among the most liquid ETFs globally; no capacity
  constraint at any retail or small institutional scale.
- **Automation:** Trivial to automate — compute N-month return for EEM and SPY, compare,
  allocate at month-end.

---

## MQL5 / Python / Pine Feasibility

- **Python:** PRIMARY — two lines of logic using yfinance/pandas; monthly backtesting
  trivial with vectorized operations. Simple to implement and verify.
- **Pine Script (TradingView):** Feasible — SPY and EEM are available; monthly SMA/ROC
  and long/short logic is straightforward.
- **MQL5:** Feasible if using CFDs; less natural for ETF-based rotation strategies.
- **Latent score (49) is below implementation candidate threshold (latent ≥ 60); will not
  be added to implementation_candidates. However, the simplicity of the rules means
  replication is trivial — a future OOS verification pass would be low effort.**

---

## Similar Strategies

- `hmm-rl-regime-taa` — SPY/TLT/GLD rotation via HMM regime; similar multi-asset TAA structure
- `gold-cross-asset-regimes` — Long GLD when both GLD and IEF positive; similar binary rotation
- `gold-bitcoin-dual-momentum` — Antonacci dual momentum on GLD/IBIT; same framework applied differently

---

## Red Flags

- **Regime dependence:** US dominated EM for most of the 2010–2026 portion of the sample;
  the best strategy configurations are effectively "long US" for most of this period.
- **No OOS test:** 18 combinations tested without walk-forward; best selected in-sample.
- **Pre-2003 synthetic data:** EEM did not exist before 2003; pre-inception data inflates apparent performance.
- **Borrowing costs omitted:** Short EEM borrow costs (~50–80bps/year) reduce realized returns.

---

## Source Links

| URL | Retrieved | Notes |
|-----|-----------|-------|
| https://quantpedia.com/systematic-tactical-allocation-in-emerging-markets-vs-u-s-a-momentum-based-approach/ | 2026-06-03 | Primary source — Quantpedia blog (Vojtko/Dujava, Apr 2026); HTTP 403 — not directly accessible |
| https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4694624 | 2026-06-03 | Background paper — "Why Do US Stocks Outperform EM and EAFE Regions?" (Dujava/Vojtko); provides context for EM vs US cycles |

---

## Analyst Notes

**FACTS (from sources via web search summaries):**
- Authors: Radovan Vojtko and Cyril Dujava (Quantpedia)
- Published: April 2026 (Quantpedia blog); no separate SSRN/arXiv paper identified for this specific strategy
- ETFs: EEM (EM) and SPY (US), monthly rebalancing
- Signal: ROC and SMA on 6/9/12-month horizons
- Three configurations: long spread (EM outperformance), short spread (US outperformance), long-short
- Best composite long-short SMA: CAR 7.13%, Sharpe 0.564, Max DD -20.31%
- Data period: 1997–2026; EEM ETF from 2003
- "Short spread consistently outperforms long spread" (US regime dominance)

**ANALYSIS:**
- The Sharpe of 0.564 is modest but reasonable for a TAA strategy. For context, SPY buy-and-hold
  Sharpe over 1997–2026 is approximately 0.50–0.60; the long-short strategy offers a similar
  Sharpe but with ~6x lower correlation to US equities (since it's long-short), providing
  genuine diversification value.
- However, the Max DD of -20.31% is substantial for what appears to be a "hedged" strategy,
  suggesting that both legs can move in the same direction in crises (e.g., March 2020: both
  EEM and SPY fell simultaneously; the long SPY / short EEM position would have been
  hurt on the short EEM leg as EM also sold off while SPY recovered faster).
- The pre-2003 synthetic data is a concern. If restricted to 2003–2026, performance would
  likely differ materially.

**ASSUMPTIONS (flagged):**
- ASSUMED: Composite blends the three horizons (6/9/12M) equally.
- ASSUMED: Transaction costs are not modeled (stated "not reported").
- ASSUMED: Rebalancing is monthly end-of-month (typical for Quantpedia TAA studies).

---

## Future Research Needed

1. Sub-period analysis: separately report 1997–2009 (EM outperformance era) vs 2010–2026
   (US outperformance era) performance. This would reveal regime dependence.
2. ETF-only period analysis: 2003–2026 to avoid synthetic data.
3. Model ETF borrowing costs for short-EEM positions.
4. Walk-forward test (e.g., train on 2003–2015, test on 2016–2026).
5. Check whether the strategy is independently covered or replicated by Allocate Smartly.
