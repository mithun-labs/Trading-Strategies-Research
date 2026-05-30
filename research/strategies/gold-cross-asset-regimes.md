# Gold Cross-Asset Regimes — Dual Momentum Filter (GLD + IEF)

## Overview

A simple monthly tactical allocation strategy that holds gold only when both gold (GLD) and
10-year US Treasuries (IEF) show positive 12-month total returns; otherwise stays in cash.
The core insight: gold's forward returns are systematically conditioned by the joint momentum
of gold and long-duration Treasury bonds. When both are in uptrend, the underlying macro
regime (falling real rates, risk-off, monetary easing) is persistently favorable for gold.

Author: Cyril Dujava (Quantpedia Research). Published as a Quantpedia blog post, December
2025. Extended historical dataset: December 31, 1969 – September 30, 2025 (~56 years).
Independently replicated by Allocate Smartly (backtest: November 30, 1992 – April 2, 2026).

No dedicated SSRN paper for this specific strategy found in accessible sources. Related
research by Dujava and Vojtko: "Using Inflation Data for Systematic Gold and Treasury
Investment Strategies" (SSRN 5151557, Vojtko/Dujava, Feb 2025).

**Key tradeoff:** Strategy CAGR (8.5%) underperforms B&H gold (~10.5%) but with
materially better risk-adjusted outcomes and lower drawdowns.

---

## Asset Class / Timeframes

- **Asset:** Gold (proxied by GLD ETF post-2004; spot gold or proxy pre-2004)
- **Cross-asset signal:** 10-year US Treasuries (proxied by IEF)
- **Cash proxy:** Federal Funds Rate / short-term bills (when not in GLD)
- **Rebalancing:** Monthly (end of month)
- **Historical coverage:** 1969–2025 (Dujava); 1992–2026 (Allocate Smartly)
- **Timeframe:** Monthly signal, monthly hold
- **Markets in scope:** Gold/XAUUSD ✓, US Treasuries (cross-asset signal only)

---

## Core Logic

### Signal Computation (end of last trading day of each month)

1. Compute 12-month total return of GLD (or gold proxy)
2. Compute 12-month total return of IEF (or 10-year Treasury total return proxy)

### Entry Rule

If **both** 12-month returns are **positive**: Go long GLD at month-end close.

### Exit to Cash

If **either** 12-month return is **zero or negative**: Move to cash (Federal Funds rate
or short-term bill proxy).

### Hold

Maintain position through the following month. Re-evaluate at each month-end.

### Exposure Statistics

Cross-asset requirement excluded 77 out of 405 months (19%) from the strategy. The
remaining 81% of months were invested in GLD; the excluded months represent the subset
where either gold or treasuries had negative 12-month momentum.

---

## Economic Rationale

**Mechanism:** Gold and long-duration US Treasuries are both defensive assets. They
move together during falling-real-rate, risk-off, monetary-easing macro regimes.

- When **both GLD and IEF 12-month momentum are positive**: The macro regime is
  characterized by falling real yields, rising recession risk (or post-recession recovery),
  and de facto or anticipated monetary easing. This configuration historically produces
  persistently positive gold excess returns.

- When **GLD+ / IEF−** (gold up, bonds down): Rising nominal yields may be outpacing
  inflation expectations — a mixed signal for gold. Research shows this state produces
  weak forward gold returns.

- When **GLD− / IEF+** (gold down, bonds up): Deflationary or credit-stress environment;
  gold is not the primary safe-haven choice. Weak forward gold returns.

- When **both GLD and IEF 12-month momentum are negative**: Rising real yields, tightening
  financial conditions, rotation into pro-cyclical assets — the most adverse environment
  for gold.

**Falsifiability:** The edge should disappear when: (1) the gold-treasury correlation breaks
down permanently (e.g., prolonged stagflation where gold rises but bonds fall, making the
"both positive" gate exclude genuine bull moves for gold); (2) the 12-month lookback window
is arbitrary — a different lookback (6-month, 18-month) might produce different results;
(3) central banks alter their gold accumulation patterns, decoupling gold from US monetary
policy signals.

**Assessment:** The mechanism is intuitive and grounded in well-understood macro finance.
The "both positive" rule is essentially "hold gold during goldilocks/risk-off macro." The
12-month lookback is a design choice with limited justification.

---

## Entry Conditions

1. At end of last trading day of month t
2. 12-month total return of GLD (or gold proxy) > 0
3. 12-month total return of IEF (or 10yr Treasury total return proxy) > 0
4. Enter long GLD at month-end close

---

## Exit Conditions

1. At end of last trading day of month t
2. Either 12-month return of GLD ≤ 0 OR 12-month return of IEF ≤ 0
3. Exit GLD at month-end close → move to cash/bills

---

## Risk Management

- **Stop-loss:** None — binary on/off monthly rule only
- **Position sizing:** 100% in GLD or 100% in cash; no partial allocations stated
- **Maximum drawdown limit:** Not specified
- **Leverage:** None (1× implied for long-only cash/GLD strategy)
- **Implicit protection:** Going to cash avoids gold bear markets; max DD (-33.7%) still
  significant, suggesting the filter does not fully protect against sharp gold declines

---

## Indicators Used

- 12-month trailing total return of GLD (gold ETF)
- 12-month trailing total return of IEF (iShares 7-10 Year Treasury Bond ETF)
- No other technical indicators

---

## Transaction Costs & Capacity

**Costs modeled:** NOT EXPLICITLY MODELED in accessible sources, but effectively negligible.

**Why costs are minimal:**
- 12 transactions/year (one round-trip per month) — very low turnover
- GLD and IEF are among the most liquid ETFs on US exchanges
- Bid-ask spread for GLD and IEF: ~1-2 bps each side
- Annual cost burden: ~2 × 0.01% × 12 = ~0.24% — negligible against 8.5% CAGR
- ETF management fees: GLD 0.40% p.a., IEF 0.15% p.a. — already embedded in total return
- No slippage concern at any retail or moderate institutional scale

**Capacity:** No formal capacity analysis found; GLD and IEF are multi-billion-dollar ETFs
with effectively unlimited retail capacity.

---

## Backtest Evidence

**Status:** CLAIMED, UNVERIFIED

- Quantpedia blog post (Dec 2025, Dujava); Quantpedia Research, not peer-reviewed
- No dedicated SSRN paper for this strategy (related: SSRN 5151557, different focus)
- Independently replicated by Allocate Smartly (external backtest, not auditable but
  represents a separate implementation) — partial corroboration
- All primary sources returned HTTP 403; evidence from search engine summaries

---

## Forward-Test Evidence

**Status:** NOT REPORTED — no forward test or live trading results found.

However, the strategy has been active as a known rule since at least December 2025 (the
publication date), and the backtest extends to April 2026. A short forward test is implied
by the recent backtesting period but not formally separated.

---

## Performance Metrics

**Primary source: Allocate Smartly implementation (1992-11-30 to 2026-04-02)**

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | 0.71 | CLAIMED, UNVERIFIED | Allocate Smartly / search summaries |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | −33.7% | CLAIMED, UNVERIFIED | Allocate Smartly / search summaries |
| CAGR | 8.5% | CLAIMED, UNVERIFIED | Allocate Smartly / search summaries |
| Annual Return | ~6% (alternate estimate) | CLAIMED, UNVERIFIED | QuantifiedStrategies.com / search summaries |
| Number of Trades | ~12/year × 33 years ≈ 400 (but only ~325 positions) | NOT REPORTED | — |
| Average Trade Duration | ~1 month (monthly hold) | CLAIMED, UNVERIFIED | from strategy rules |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | ~81% (328/405 months invested) | CLAIMED, UNVERIFIED | search summaries |

**Buy-and-hold gold comparison (CLAIMED, UNVERIFIED):**
- B&H gold CAGR: ~10.5% (per QuantifiedStrategies.com search summary, same period)
- Note on contradictory data: one source reports strategy CAGR 8.5%, another "almost 6% per year."
  These may refer to different backtest periods. The Allocate Smartly figure (8.5%) appears
  to be the more complete and recent figure; the ~6% figure may be from a shorter window or
  a different implementation variant.

**Sharpe 0.71 context (ANALYSIS):** This is a monthly-rebalanced gold allocation strategy.
A Sharpe of 0.71 for a single-asset tactical strategy over 33 years is reasonable. The
comparison to B&H gold Sharpe (implied ~0.5-0.6 given higher returns but much larger DD)
suggests a genuine risk-adjusted improvement. A Sharpe of 0.71 is below the automatic
scrutiny trigger of 3.0 — no mandatory scrutiny required.

---

## Community Sentiment

- **Allocate Smartly:** Independently implemented and published the backtest — this is a
  meaningful neutral third-party corroboration of the strategy's rules and approximate results.
  They track the strategy actively as a TAA (tactical asset allocation) rule.
- No forum-level discussion (r/algotrading, ForexFactory, etc.) found in accessible sources.
- The strategy is conceptually related to the broader literature on gold-treasury comovement,
  which has academic support (e.g., Erb and Harvey, Bhansali et al.).

---

## Why This Might Be Nothing

1. **Gold-treasury correlation is not stable:** The most recent counterexample is 2022, where
   both gold and treasuries fell together. While the strategy would correctly exit gold in this
   environment (IEF was deeply negative), the mechanism implies that the filter is just
   "don't hold gold when rates are rising aggressively" — a rate-regime timing rule, not an
   independent two-signal model. The two signals may be more correlated than they appear.

2. **Absolute return sacrifice:** The strategy CAGR of 8.5% underperforms B&H gold (~10.5%)
   by ~200bps per year over 33+ years. An investor who just held physical gold (or rolled
   futures) would have generated higher absolute wealth. The risk-adjusted improvement is real,
   but only investors with specific drawdown constraints benefit.

3. **Single-parameter lookback choice:** The 12-month lookback was almost certainly tested
   against alternatives (6-month, 9-month, 18-month). The Quantpedia blog post does not
   report parameter sensitivity analysis in accessible sources. A single in-sample-optimized
   lookback over 56 years is actually reasonable (long enough to reduce overfitting risk), but
   no formal OOS split is documented.

4. **Extended data quality (pre-1970):** Using historical gold data before the USD-gold peg
   was broken (1971 Nixon Shock) introduces structural break risk. The price of gold was
   legally fixed to $35/oz under Bretton Woods; momentum signals on that data are not
   economically meaningful. The "December 1969 – September 2025" claim includes data
   where gold had no free-float.

5. **Regime luck:** The 1992-2026 period includes the greatest bond bull market in history
   (1982-2020). A strategy that holds gold during bond uptrends has implicitly been long
   during the longest Treasury tailwind on record. Post-2020, the correlation has shifted.
   Whether the rule survives a structurally higher-rate environment (2020s) is the open question.

6. **What would change my mind:** A formal OOS test where 1992-2010 is in-sample and
   2010-2026 is OOS, showing the Sharpe improvement persists. I do not have this.

---

## Strengths / Weaknesses

### Strengths
- **Extremely simple rule** (2 conditions, monthly, no parameters beyond 12-month lookback)
- **Very long backtest** (33-56 years covering multiple rate cycles and macro regimes)
- **Independently replicated** by Allocate Smartly — adds credibility beyond author's own backtest
- **Negligible transaction costs** at monthly rebalancing with liquid ETFs
- **Clear falsifiability** (mechanism tied to rate regime; fails in stagflation or regime breaks)
- Gold is an in-scope market (XAUUSD / GLD)

### Weaknesses
- **Not peer-reviewed** — Quantpedia blog post only
- **Underperforms B&H gold** on absolute CAGR (~8.5% vs ~10.5%)
- **Pre-Bretton Woods data contamination** in the extended 1969 sample
- **Max DD -33.7%** is still substantial for a "protected" strategy
- **No OOS test explicitly documented**
- **Contradictory CAGR figures** across two sources (8.5% vs ~6%) — period/variant unclear

---

## Confidence Score

**Per-dimension breakdown:**

| Dimension | Weight | Score | Contribution |
|-----------|--------|-------|-------------|
| Logical / economic soundness (falsifiable) | 3 | 6 | 18 |
| Transparency & reproducibility | 2 | 8 | 16 |
| Evidence auditability | 2 | 2 | 4 |
| Risk management quality | 2 | 3 | 6 |
| Cost & capacity realism | 2 | 7 | 14 |
| Honest treatment of drawdowns / failure | 1.5 | 5 | 7.5 |
| Robustness evidence (OOS / walk-forward / multi-market) | 1.5 | 5 | 7.5 |
| Survived independent scrutiny | 1 | 4 | 4 |

**Weighted sum:** 77 / 150 max

**latent 51 (capped to 51 — Evidence auditability = 2 (≤4), cap = 59; latent already below cap)**

**Confidence: 51 — EXPERIMENTAL**

*Notes on key score drivers:*
- **Transparency = 8:** The rule is completely specified (two signals, one lookback, monthly rebalance). Any practitioner can reproduce it exactly.
- **Cost/capacity = 7:** Monthly rebalancing of liquid ETFs — costs are effectively zero. This is the best score available without explicit modeling.
- **Evidence auditability = 2:** Quantpedia practitioner blog post. Allocate Smartly implementation is a partial corroboration but is still a practitioner replication, not peer-reviewed methodology.
- **Robustness = 5:** 33+ year backtest (strong), but no OOS split documented and the 12-month lookback is a single parameter choice.

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Very low — two lookback calculations, binary on/off signal, monthly check
- **Scalability:** GLD and IEF are multi-billion-dollar ETFs; unlimited retail capacity
- **Automation:** Trivial — one script, monthly cron job, two ETF price lookups
- **Replication risk:** Extremely low — no proprietary data needed; Yahoo Finance or similar

---

## MQL5 / Python / Pine Feasibility

- **Python:** ~15 lines of code (pandas rolling return, boolean check, trade logic)
- **Pine Script:** Easily implemented; multi-asset via `request.security()`; GLDINV / IEF proxies needed
- **MQL5:** Requires external data for IEF; straightforward with MT5 multi-symbol capabilities
- **Best implementation path:** Python + Interactive Brokers / Alpaca for actual live trading;
  real-time signals not needed (monthly signal at month-end only)

---

## Similar Strategies

- `forecast-to-fill-gold-futures` — gold futures systematic strategy; daily vs monthly; different mechanism (trend + momentum signal + vol targeting); overlapping market
- `hmm-rl-regime-taa` — multi-asset regime-based TAA (SPY/TLT/GLD); same TAA concept, different assets and method
- `bitcoin-max-min-trendrev` — Bitcoin MAX/MIN price signals; same Vojtko/Quantpedia group; different asset

---

## Red Flags

- **Pre-Bretton Woods data (pre-1973):** Including 1969-1971 data when gold was price-fixed; Quantpedia's extended backtest may be using meaningless signals in this period
- **Underperforms B&H absolute returns** — the strategy is purely a risk-reduction tool, not a return enhancer
- **No OOS period documented** — 12-month lookback selected on full dataset
- **Practitioner blog post only** — no peer review or independent academic validation
- **Contradictory CAGR figures** in different sources (8.5% vs ~6%)

---

## Source Links

| URL | Retrieved |
|-----|-----------|
| [Quantpedia blog post (Dec 2025)](https://quantpedia.com/cross-asset-price-based-regimes-for-gold/) | 2026-05-30 (HTTP 403) |
| [Allocate Smartly implementation](https://allocatesmartly.com/gold-cross-asset-momentum/) | 2026-05-30 (HTTP 403) |
| [BestFolio strategy card](https://bestfolio.app/strategies/gold-cross-asset) | 2026-05-30 (HTTP 403) |
| [Related SSRN 5151557 (Vojtko/Dujava, Using Inflation Data)](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5151557) | 2026-05-30 (HTTP 403) |

*Note: All primary sources returned HTTP 403. Evidence from search engine summaries only.*

---

## Analyst Notes

**FACTS (from sources):**
- Author: Cyril Dujava, Quantpedia Research
- Published: Quantpedia blog, December 2025
- Strategy: Long GLD when both GLD 12M return > 0 AND IEF 12M return > 0; otherwise cash
- Historical data: Dec 1969 – Sep 2025 (Dujava); Nov 1992 – Apr 2026 (Allocate Smartly)
- Cross-asset filter excluded 77/405 months (19%) from exposure
- Allocate Smartly backtest: CAGR 8.5%, Sharpe 0.71, Max DD -33.7% (1992-2026)
- Alternate source: "almost 6% per year" vs B&H gold ~10.5% (period unclear)
- Independent Allocate Smartly implementation corroborates the strategy

**ANALYSIS (my reasoning):**
- The "both positive" rule is effectively a rate-regime timing rule for gold. GLD and IEF
  tend to comove positively during falling-rate regimes and diverge during stagflation or
  tightening cycles. The rule captures this comovement.
- The 8.5% vs 10.5% B&H comparison suggests the strategy gives up ~2% annual return for
  lower drawdown. Whether this is a good trade depends entirely on investor preferences.
- The pre-1973 data (1969-1971, Bretton Woods era) is problematic as gold was price-fixed.
  Any backtest starting from 1973 or later would be more defensible.
- Allocate Smartly is a paid subscription service that independently implements TAA
  strategies for subscribers — their backtest is independent but not academic validation.

**ASSUMPTIONS (flagged):**
- ASSUMED 12-month lookback is the primary/optimized parameter (not confirmed from summaries)
- ASSUMED cash earns the Federal Funds rate proxy (standard for TAA models)
- ASSUMED the Allocate Smartly 8.5% CAGR is net of ETF management fees

---

## Future Research Needed

1. **OOS test:** Define 1992-2005 as in-sample (lookback selection) and 2005-2026 as OOS;
   confirm Sharpe improvement persists
2. **Pre-Bretton Woods exclusion:** Re-run excluding pre-1973 data to eliminate fixed-price era
3. **Lookback sensitivity:** Test 6, 9, 12, 18, 24-month lookbacks; quantify stability of rule
4. **2022 stress test isolation:** 2022 was the worst year for both gold and treasuries since
   Bretton Woods. Measure the strategy's 2022 return vs B&H to confirm the filter worked
5. **Related paper:** Check "Mining Gold for Regimes" (SSRN 5247471, Bhansali et al.),
   which may provide peer-reviewed underpinning for the regime-conditional gold approach
