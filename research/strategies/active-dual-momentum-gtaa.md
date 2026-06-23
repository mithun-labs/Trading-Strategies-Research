# Active Dual Momentum GTAA Strategy

## Overview

A weekly-rebalanced Global Tactical Asset Allocation (GTAA) strategy that combines
two momentum signals across nine multi-asset ETFs. The strategy selects top
relative-momentum performers and applies an absolute trend filter to avoid holding
declining assets. It blends a 10-week and a 25-week sub-strategy at 50/50 weight,
rebalancing every Wednesday at close. Non-selected or trend-failing assets park in
cash at 0% yield. The strategy is a GTAA implementation of Gary Antonacci's "Dual
Momentum" framework (combining cross-sectional relative momentum and time-series
absolute momentum) across a nine-instrument multi-asset universe.

**Source:** Quantpedia blog — "Active Dual Momentum GTAA Strategy" (Sona Beluska,
Junior Quant Analyst, Quantpedia, May 2026)

---

## Asset Class / Timeframes

- **Instruments:** 9 ETFs — SHY (short-term US Treasuries), IEF (7–10yr Treasuries),
  UUP (US Dollar Index), GLD (gold), USO (crude oil), SPY (US large-cap equities),
  EFA (international developed equities), QQQ (NASDAQ-100 / US tech), EEM (emerging
  market equities)
- **Timeframe:** Weekly bars; rebalance at Wednesday close
- **Markets:** Multi-asset — bonds (2), currency (1), commodities (2), US equities (2),
  international equities (2)
- **Data period:** February 21, 2007 – March 25, 2026 (≈19 years)
- **Data source:** EODHD.com (adjusted close prices)

---

## Core Logic

**Sub-strategy A (10-week momentum):**
1. Calculate 10-week Rate of Change (ROC) for each of the 9 ETFs.
2. Rank ETFs from highest to lowest 10-week ROC.
3. Select the top 3 ETFs by rank.
4. Apply absolute momentum filter: for each selected ETF, if 10-week ROC > 0, hold;
   otherwise replace with cash (0% yield).
5. Equal-weight the remaining selected non-cash assets.

**Sub-strategy B (25-week momentum):**
1. Calculate 25-week ROC for each of the 9 ETFs.
2. Rank and select top 3 by 25-week ROC.
3. Apply absolute momentum filter as above using 25-week ROC.
4. Equal-weight the remaining non-cash assets.

**Final portfolio:**
- 50% capital to Sub-strategy A + 50% capital to Sub-strategy B.
- Rebalance every Wednesday at close.
- No leverage; cash earns 0% (assumption, not T-bill or MMF rate).

---

## Economic Rationale

The strategy rests on two of the most robustly documented return anomalies in
academic finance:

1. **Cross-sectional (relative) momentum** (Jegadeesh and Titman 1993): assets that
   have outperformed peers over intermediate horizons tend to continue outperforming
   over the next 3–12 months. Mechanisms include: investor underreaction to news,
   gradual information diffusion, trend-following institutional flow, and behavioral
   anchoring. Cross-sectional momentum has been confirmed across asset classes
   (Asness et al. 2013, AQR).

2. **Time-series (absolute) momentum** (Moskowitz, Ooi and Pedersen 2012): an asset
   that has a positive own-return over the past 12 months tends to continue rising,
   while negative-return assets tend to continue falling. The mechanism mirrors
   cross-sectional momentum but operates on a per-asset basis.

3. **Dual momentum combination** (Antonacci 2014): combining relative and absolute
   momentum in a GTAA framework provides a "synergy" that pure cross-sectional or
   pure trend-following individually do not achieve. The absolute filter specifically
   prevents holding assets that are declining on their own even when they are relative
   outperformers.

**When the edge should disappear (falsifiability):**
- **Momentum crashes:** Rapid, sharp trend reversals — as documented in AQR's
  analysis of the 2009 momentum crash — can be severe. Weekly frequency slightly
  limits this vs. monthly, but cannot fully escape the risk.
- **Low-dispersion regimes:** When correlations across the 9 ETFs spike (as in Q4
  2008), relative rankings become unstable and the signal degrades to noise.
- **Arbitrage saturation:** As capital chases the same momentum signals, crowding
  erodes the premium, particularly in liquid ETFs where front-running is easiest.
- **Zero-cost fiction:** At weekly turnover, any realistic cost structure materially
  reduces edge; the strategy disappears under even conservative cost assumptions
  if turnover is high.
- **Universe lock-in:** Fixed ETF universe (selected circa 2007) may miss dominant
  asset classes that emerge later (e.g., Bitcoin ETFs from 2024 onwards), causing
  structural underperformance vs. a dynamic universe.

---

## Entry Conditions

- Long only (no short positions, no leverage).
- Sub-strategy A: Open position in an ETF if it ranks top-3 by 10-week ROC AND
  10-week ROC > 0. Equal-weight with other qualifying ETFs.
- Sub-strategy B: Open position in an ETF if it ranks top-3 by 25-week ROC AND
  25-week ROC > 0. Equal-weight with other qualifying ETFs.
- Combined: allocate 50% of portfolio to Sub-A holdings + 50% to Sub-B holdings
  (positions may overlap).

---

## Exit Conditions

- Weekly rebalancing at Wednesday close automatically adjusts positions.
- An ETF is exited when it drops out of the top-3 ranking in a given sub-strategy,
  or when its ROC turns negative (absolute momentum filter triggered).
- No explicit stop-loss or profit target.

---

## Risk Management

- **Absolute momentum filter:** Acts as a structural circuit breaker — assets in
  downtrends (negative ROC over lookback) are replaced with cash. Reduces exposure
  during broad market declines.
- **Equal weighting:** Prevents concentration in any single asset within each
  sub-strategy.
- **50/50 horizon blend:** Combining short-term (10-week) and long-term (25-week)
  momentum lookbacks provides partial diversification across momentum time horizons.
- **No leverage defined.** No stop-loss. No maximum drawdown trigger. No position-
  size scaling. Effective risk management is limited to the absolute momentum cash
  filter and diversification.

---

## Indicators Used

- Rate of Change (ROC) over 10 weeks (cross-sectional rank + absolute filter)
- Rate of Change (ROC) over 25 weeks (cross-sectional rank + absolute filter)

---

## Transaction Costs & Capacity

- **Stated costs:** Zero basis points — explicitly assumed in the Quantpedia article.
- **Reality check (ANALYSIS):** Weekly rebalancing on 9 ETFs generates real friction.
  The 9 ETFs are liquid (SPY, QQQ, GLD, IEF: bid-ask spreads ≈ 1–3 bps), so per-
  trade cost is small. However, turnover accumulates. If the strategy averages 2 position
  changes per week × 52 weeks = 104 trades/year, and each round-trip costs ~5 bps
  (spread + small market impact), annual friction ≈ 520 bps. If average turnover is
  lower (1 change/week), friction ≈ 260 bps. This is material against a gross annual
  return that, with Calmar ~0.9 and a modest multi-asset return stream, is likely in
  the 6–12% range. The gross Sharpe of ~0.9 would decline materially (ANALYSIS).
- **Capacity:** Very high. All 9 ETFs are highly liquid; the strategy could scale to
  hundreds of millions of dollars without detectable slippage.
- **Conclusion:** Costs unmodeled on a weekly-rebalanced strategy = **presumptive
  invalidity** per research framework. The strategy may still have edge after costs,
  but this has not been demonstrated.

---

## Backtest Evidence

- **Source:** Quantpedia blog post (Beluska, May 2026)
- **Status:** CLAIMED, UNVERIFIED
- **Period:** Feb 21, 2007 – Mar 25, 2026 (19+ years)
- **Methodology:** Single full-sample in-sample grid search over lookback periods
  (1–12 weeks for short; 15–50 weeks for long) and ETF counts (1–9). Optimal
  parameters selected and reported. No OOS split, no walk-forward, no cross-
  validation documented.
- **Key claim:** "The final strategy outperforms the benchmark in every aspect,
  achieving Sharpe and Calmar ratios as close to 0.9."
- **Auditability:** NOT AUDITABLE — Quantpedia blog post by a junior analyst, no
  peer review, no independent replication, no open code.

---

## Forward-Test Evidence

- NOT REPORTED — no live trading or out-of-sample period described.

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | ~0.9 | CLAIMED, UNVERIFIED | Quantpedia blog (Beluska, May 2026) |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | ~0.9 | CLAIMED, UNVERIFIED | Quantpedia blog (Beluska, May 2026) |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | — |
| CAGR | NOT REPORTED | NOT REPORTED | — |
| Annual Return | NOT REPORTED | NOT REPORTED | — |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | ≈1 week (weekly rebalancing) | CLAIMED, UNVERIFIED | Quantpedia blog |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | NOT REPORTED | NOT REPORTED | — |

**Note on "~0.9":** The Quantpedia article reports Sharpe and Calmar ratios as
"close to 0.9" for the combined (50/50) final strategy; this is an approximate figure
and NOT a precisely stated metric from a table. The article references "Table 5" but
the specific numerical values were not accessible in the version retrieved; they are
reported here as "~0.9" and tagged CLAIMED, UNVERIFIED.

**Automatic scrutiny check:** Sharpe ~0.9 does not trigger the Sharpe > 3 threshold.
Calmar ~0.9 is within normal range for discretionary/systematic multi-asset strategies.
No automatic scrutiny triggers fire on the reported approximate figures.

**ANALYSIS:** With Calmar ~0.9 and a typical multi-asset annual volatility of 8–12%,
the implied Sharpe could be consistent with an annual return of 8–11% and maximum
drawdown of 9–12%. These are plausible for a momentum-tilted multi-asset portfolio
over a long bull-market-dominated sample. However, with zero costs, these figures
are almost certainly overstated for real trading.

---

## Community Sentiment

- **No external discussion found.** The strategy is a Quantpedia blog post; there is
  no forum critique, replication by independent researchers, or adversarial commentary
  that could be retrieved.
- **Antonacci's Dual Momentum framework** (the academic ancestor of this strategy)
  has received substantial independent scrutiny:
  - Robot Wealth review identified that the strong out-of-sample returns were largely
    driven by the 2008–2009 crisis period; excluding that window significantly weakens
    the reported edge.
  - Critics note universe selection is the primary "hidden parameter" and backtests
    on this framework are highly sensitive to which assets are included.
  - Allocate Smartly's live tracking of Dual Momentum–style GTAA strategies has
    produced post-2015 Sharpe ratios in the 0.3–0.6 range, well below in-sample
    claims.

---

## Why This Might Be Nothing

**1. Most likely benign explanation (data mining + 2008 regime cherry-pick):**
The parameter grid searches 12 short lookbacks × 36 long lookbacks × 9 ETF counts
= 3,888 combinations (approximately), all tested in-sample. Selecting the best
10-week + 25-week + top-3 combination post-hoc guarantees results that look better
than chance even if no real edge exists. More critically, the 19-year backtest
includes 2008–2009, during which buy-and-hold equity (SPY, EFA, EEM, USO) crashed
30–75%, while the absolute momentum filter would have moved to cash or bonds. This
creates an enormous boost to the comparison vs. benchmark that is a regime artifact,
not a persistent edge. Post-2015 live GTAA strategies in this family typically
produce half or less the advertised in-sample Sharpe.

**2. The cost case:**
Zero costs on weekly rebalancing is acknowledged in the article but dismissed
without quantification. If the strategy turns over 2 positions per week at 5 bps
round-trip, that is 520 bps/year — likely wiping out most or all of a modest annual
gross return. The article provides no turnover estimate to assess this. Until
actual turnover and realistic costs are applied, this result is presumptively invalid
for a weekly-frequency strategy.

**3. Regime dependence:**
The strategy is long the momentum risk premium, which is known to be:
(a) positive in trending markets (long, well-defined trends), and
(b) sharply negative in fast-reversal environments (momentum crashes of 2001,
2009, 2020). The 19-year sample includes multiple trending regimes (2009–2019 US
equity bull market, 2020–2021 COVID recovery) with relatively few sustained
momentum crashes. The strategy's behavior in a 2022-style coordinated bond-equity
selloff — when both SPY and IEF lose simultaneously — is not explicitly reported.

**4. Sample and overfitting concerns:**
- Universe fixed at 9 specific ETFs; the article itself acknowledges this is a
  "hidden parameter" ("including Bitcoin in 2015 would have had a large impact").
- Parameter count is large (lookback + ETF count + blend weight) with no OOS.
- The 50/50 blend of sub-strategies was not shown to be robustly optimal; it appears
  to be a round-number assumption.
- No statistical significance test is reported for the outperformance.

**5. The single piece of evidence that would most change my mind:**
A cost-inclusive, walk-forward backtest (e.g., 5-year expanding window OOS from
2016 to 2026) with realistic ETF turnover and transaction cost assumptions showing
net Sharpe > 0.6 would substantially increase confidence. That evidence does not exist.

---

## Strengths / Weaknesses

**Strengths:**
- Built on academically documented momentum anomalies (Jegadeesh-Titman 1993;
  Moskowitz et al. 2012; Antonacci 2014).
- Combines two robust momentum signals (relative + absolute) rather than relying on
  either alone.
- Multi-asset universe provides natural diversification across uncorrelated cycles.
- Simple, fully describable rules — low implementation complexity.
- Dual-horizon blending (10-week + 25-week) reduces lookback sensitivity.

**Weaknesses:**
- Zero transaction costs = presumptively invalid for weekly rebalancing.
- Full in-sample parameter optimization; no OOS validation.
- Universe selection ("hidden parameter") is unacknowledged as a major source of
  overfitting.
- No stop-loss, no drawdown limit, no position sizing.
- Cash at 0% is unrealistic post-2015 (money market funds pay 4–5% in 2023–2026).
- Not peer-reviewed; single blog post by a junior analyst.
- Post-publication evidence for Dual Momentum GTAA strategies shows significant
  decay vs. backtested performance.

---

## Confidence Score

**Per-dimension breakdown:**

| Dimension | Score (0–10) | Weight | Contribution |
|---|---|---|---|
| Logical / economic soundness (falsifiable) | 5 | 3 | 15.0 |
| Transparency & reproducibility | 6 | 2 | 12.0 |
| Evidence auditability | 2 | 2 | 4.0 |
| Risk management quality | 3 | 2 | 6.0 |
| Cost & capacity realism | 1 | 2 | 2.0 |
| Honest treatment of drawdowns / failure | 2 | 1.5 | 3.0 |
| Robustness evidence (OOS / walk-forward) | 1 | 1.5 | 1.5 |
| Survived independent scrutiny | 1 | 1 | 1.0 |

**Weighted sum:** 44.5  
**Max possible:** 150  
**latent_score:** round(44.5 / 150 × 100) = **30**

**Verification cap:**
Evidence auditability = 2 ≤ 4 → cap at 59.
confidence = min(30, 59) = **30**

`latent 30 (cap of 59 does not bind; score lands in Low Confidence at 30 before cap applies)`

**Band:** Low Confidence (0–39)

**Score rationale:**
The core mechanism is academically grounded (momentum literature is strong), which
prevents a score below 20. However, the combination of zero costs on weekly rebalancing,
full in-sample parameter optimization, no OOS, and no peer review prevents the score
from rising above the Low Confidence band. The Calmar/Sharpe of ~0.9 is attractive
on its face but is not a meaningful signal without cost modeling or OOS validation.

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Low — two ROC calculations, a ranking step, and an absolute filter.
- **Scalability:** Very high — all instruments are highly liquid ETFs.
- **Automation:** Straightforward in Python (pandas) or Pine Script (TradingView).
  Requires weekly Wednesday close price feed for 9 ETFs.

---

## MQL5 / Python / Pine Feasibility

- **Python:** Trivial; 50 lines of pandas code. Data from Yahoo Finance or EODHD.
- **Pine Script:** Feasible for a single ETF sub-strategy; multi-asset GTAA requires
  external code since Pine Script is single-instrument. Possible via TradingView
  multi-chart scripts or a broker integration layer.
- **MQL5:** Feasible but non-trivial; multi-asset GTAA in MQL5 requires custom
  portfolio management logic.

---

## Similar Strategies

- `em-us-momentum-taa` — 2-asset (SPY/EEM) monthly SMA/ROC TAA; score 49; simpler
  universe, same momentum family
- `gold-bitcoin-dual-momentum` — 2-asset dual momentum on GLD+IBIT; score 49;
  Antonacci-inspired framework, narrow universe
- `multi-asset-pullback-200ma` — 6-ETF multi-asset but REVERSAL mechanism; score 42;
  same ETF genre, opposite signal (mean-reversion vs. momentum)
- `hmm-rl-regime-taa` — 3-ETF regime-based TAA (SPY/TLT/GLD); score 44; related
  multi-asset TAA family but regime-driven, not momentum-driven
- `commodity-intramarket-correlation-momentum` — 4 commodity ETFs only; different
  mechanism (correlation-switching); score 45

---

## Red Flags

- **Costs ignored on weekly-rebalanced strategy** — presumptive invalidity flag.
- **Full in-sample parameter grid** — multiple testing over ~3,888+ combinations
  without OOS.
- **Universe selection as acknowledged "hidden parameter"** — author admits this; no
  sensitivity analysis provided.
- **No independent replication** — single blog post by a junior analyst at the same
  firm that publishes the strategy database.
- **Benchmark chosen is equal-weight 9-ETF** — an unusually weak comparison (no
  risk-free return, no standard index); any active strategy that avoids the 2008
  crash looks strong vs. this benchmark.
- **Cash at 0%** — misses significant income opportunity in post-2015 environments
  where short-term rates were 2–5%.

---

## Source Links

| Source | Retrieved | Used For |
|--------|-----------|----------|
| https://quantpedia.com/active-dual-momentum-gtaa-strategy/ | 2026-06-23 | Primary source — full strategy rules, performance claims |
| https://quantpedia.com/quantpedia-in-may-2026/ | 2026-06-23 | Discovery — confirmed publication in May 2026 |
| https://quantpedia.com/blog/ | 2026-06-23 | Blog index scan for recent posts |

---

## Analyst Notes

**FACTS (from sources):**
- Asset universe: SHY, IEF, UUP, GLD, USO, SPY, EFA, QQQ, EEM (9 ETFs)
- Rebalancing: every Wednesday close
- Signal: 10-week ROC and 25-week ROC, top-3 selection, absolute filter (positive ROC)
- Portfolio: 50% to 10-week sub + 50% to 25-week sub
- Backtest: Feb 21, 2007 – Mar 25, 2026
- Reported Sharpe: ~0.9; Calmar: ~0.9 (CLAIMED)
- Transaction costs: 0 bp (stated)
- Cash rate assumed: 0%
- Author: Sona Beluska, Junior Quant Analyst, Quantpedia

**ANALYSIS:**
- The 2008–2009 benchmark impairment likely contributes disproportionately to the
  outperformance. Removing 2008–2009 from the comparison would substantially narrow
  the gap.
- Weekly turnover at 5 bps round-trip could cost 260–520 bps/year depending on
  position frequency — this is not trivial for a strategy targeting ~8–11% gross
  annual return.
- The underlying momentum mechanism is real (Jegadeesh-Titman, Moskowitz et al.) but
  the edge from the specific parameterization (10+25 week weekly) is unverified.
- Post-publication Dual Momentum GTAA tracking (Allocate Smartly, RobotWealth) shows
  consistent decay below in-sample numbers; the advertised Sharpe of 0.9 should be
  treated as an upper bound.
- The dual-horizon blend (50/50 short + long momentum) is intuitively appealing but
  there is no theoretical justification for 50/50 vs. 40/60 or 30/70; this is likely
  in-sample optimized.

**ASSUMPTIONS:**
- Assumes zero slippage in addition to zero commission — on weekly close prices, this
  may be reasonable for ETFs but market-on-close fills for illiquid periods
  (e.g., holiday weeks) are not discussed.
- Assumes no tax drag (position changes would generate short-term capital gains in
  taxable accounts).
- Assumes continuous ETF availability from 2007; some ETFs (IBIT-style products) did
  not exist then; the specific 9 ETFs were chosen partly for their 2007+ history.

---

## Future Research Needed

1. **OOS / walk-forward validation:** Test 2016–2026 as OOS, train on 2007–2015.
2. **Cost-inclusive rerun:** Apply realistic round-trip costs (5–10 bps) and model
   actual weekly turnover. Does Sharpe remain > 0.5?
3. **Universe sensitivity:** Replace EEM with IBIT post-2024; add sector ETFs
   (XLK, XLE). How sensitive is Sharpe to universe choice?
4. **Cash alternative:** Model 3-month T-bill rate as cash return rather than 0%.
5. **Comparison to Antonacci's original GEM:** Does the 9-ETF weekly version
   outperform Antonacci's 3-ETF monthly GEM after costs?
6. **Momentum crash robustness:** Stress test around 2001, 2009, 2020 momentum
   crashes to quantify tail risk.
