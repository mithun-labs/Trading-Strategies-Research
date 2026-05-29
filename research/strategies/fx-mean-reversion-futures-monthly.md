# FX Futures Mean Reversion — Monthly Rebalancing

## Overview

A monthly mean reversion strategy applied to FX futures. Currencies that have deviated
furthest below their historical mean are bought (long undervalued); currencies that have
deviated furthest above are sold (short overvalued). Two position-sizing variants: linear
(proportional to deviation) and exponential (amplified). FX futures are used rather than
spot to naturally capture interest rate differentials (carry component) in the roll. Tested
2007–2024. Working paper on SSRN (October 2024). Authors: Soňa Beluská and Radovan Vojtko
(Quantpedia Research). **Key finding: Linear variant has Sharpe 0.12 — essentially no edge.**
Exponential variant performs better but at higher risk; specific numbers not accessible.

---

## Asset Class / Timeframes

- **Markets:** FX futures (G10 currencies, specific set NOT REPORTED)
- **Timeframe:** Monthly rebalancing
- **Style:** Systematic macro mean reversion
- **In scope:** Forex ✓

---

## Core Logic

1. **Universe:** Continuous G10 FX futures (exact set not specified in accessible sources).

2. **Mean reversion signal:** For each currency, compute the deviation of the current FX
   futures price from its historical mean (rolling lookback period NOT REPORTED). Rank
   currencies by signed deviation.

3. **Direction:** Long currencies furthest below their mean (undervalued vs. historical
   average). Short currencies furthest above their mean (overvalued).

4. **Position sizing — Linear variant:** Weight proportional to the deviation from mean.
   Mild overshoot → small weight; large overshoot → large weight.

5. **Position sizing — Exponential variant:** Weight increases exponentially with deviation.
   Same direction but amplified bets on the most extreme deviations.

6. **Rebalancing:** Monthly. Positions updated at calendar month-end.

7. **Carry capture:** Using FX futures instead of spot automatically incorporates the
   interest rate differential (carry) into the roll return, making the strategy implicitly
   long currencies with positive carry when they are below their mean, and short
   high-carry currencies when they are above their mean.

---

## Economic Rationale

**Why an edge could exist:** Long-run exchange rate theory supports mean reversion:
- **Purchasing Power Parity (PPP):** Over long horizons, exchange rates should revert to
  fundamentally justified levels based on relative price levels.
- **Uncovered Interest Parity (UIP) violations:** Carry trade profits (the "carry anomaly")
  exist because high-interest currencies do not depreciate as fast as UIP predicts — implying
  temporary overvaluation of high-yield currencies.
- **Policy intervention:** Central banks often lean against currency extremes, creating
  artificial mean-reversion pressure.

**When the edge should disappear (falsifiability):**
- When exchange rates are driven by persistent flows (e.g., sustained current account
  surpluses/deficits) rather than fundamentals — the "mean" itself may shift.
- During carry regime (trend-following conditions): high-yield currencies can remain
  "overvalued" for years, making mean reversion a chronically losing position.
- During major regime breaks (e.g., Brexit, COVID, US dollar reserve currency shifts) where
  the historical mean becomes irrelevant.
- When the reversion horizon is longer than the holding period — the strategy may capture
  the wrong frequency of mean reversion.

---

## Entry Conditions

- Long FX futures contracts for currencies most below their historical mean
- Short FX futures contracts for currencies most above their historical mean
- Rebalance monthly (calendar month-end)
- Size proportional to deviation (linear) or amplified by deviation (exponential)

---

## Exit Conditions

- Monthly rebalance forces exit/partial exit as deviations normalize
- No intra-month stop-loss defined in accessible sources

---

## Risk Management

**Defined:**
- Long-short structure provides partial hedging vs. directional USD risk
- Monthly rebalancing limits excessive concentration in extreme bets
- Linear variant naturally caps position size at proportional to deviation

**NOT defined:**
- No explicit stop-loss
- No maximum position size limit per currency
- No drawdown circuit breaker
- Exponential variant has undefined risk profile (amplifies extreme bets)

---

## Indicators Used

- FX futures continuous price series
- Historical mean of each FX futures series (rolling lookback NOT REPORTED)
- Deviation from mean as the signal (signed distance from rolling average)

---

## Transaction Costs & Capacity

- **Treatment:** NOT explicitly stated in accessible sources. Monthly rebalancing implies low
  turnover and minimal transaction costs vs. daily strategies.
- **Capacity:** G10 FX futures (CME) are highly liquid. Strategy should scale well to
  institutional size without slippage concerns.
- **Carry cost:** Using futures implicitly rolls the carry — adds carry return but also
  introduces roll cost on the short side in high-yield environments.

---

## Backtest Evidence

- **Status:** CLAIMED, UNVERIFIED — SSRN working paper; not peer-reviewed; no independent
  replication.
- **Period:** 2007–2024 (17 years).
- **Method:** Full-period simulation; monthly rebalancing.
- **OOS:** No walk-forward or hold-out OOS described in accessible sources.

---

## Forward-Test Evidence

NOT REPORTED.

---

## Reported Metrics

| Metric | Value | Variant | Status |
|--------|-------|---------|--------|
| Sharpe ratio | **0.12** | Linear | CLAIMED, UNVERIFIED — **null result** |
| Calmar ratio | 0.05 | Linear | CLAIMED, UNVERIFIED — **null result** |
| Portfolio growth | ~Flat (±1.1× over 10 years) | Linear | CLAIMED, UNVERIFIED |
| Sharpe ratio | NOT REPORTED (higher than linear) | Exponential | CLAIMED, UNVERIFIED |
| Max drawdown | NOT REPORTED (deeper than linear) | Exponential | CLAIMED, UNVERIFIED |
| CAGR | NOT REPORTED | Both | NOT REPORTED |

**Critical finding:** The linear variant, which is the most transparent and reproducible version,
achieved a Sharpe ratio of 0.12 — essentially zero alpha. The exponential variant improves
on this, but specific numbers are not accessible and selection between two approaches
introduces lookback bias.

---

## Community Sentiment

- Quantpedia is a credible quantitative research firm (Radovan Vojtko is well-known in the
  quant community).
- Quantocracy referenced the paper — positive signal.
- No critical community discussion or independent replication found.
- The strategy is published as a working paper / blog article, not a peer-reviewed journal.

---

## Why This Might Be Nothing

### 1. Most likely benign explanation: FX mean reversion is a known weak signal
The linear variant's Sharpe ratio of 0.12 is the most informative result: it directly
demonstrates that simple FX mean reversion (long undervalued, short overvalued) does NOT
generate meaningful alpha. This is consistent with the literature — FX "value" strategies
typically underperform in short/medium-term windows and PPP mean reversion operates at
multi-year horizons.

### 2. Exponential variant = selection / data-mining bias
Comparing two variants and reporting the better-performing one inflates expected returns.
If the linear variant achieves Sharpe 0.12, the exponential variant's apparent improvement
may reflect fitting the specific volatility structure of 2007-2024 (which includes
exceptional carry dynamics in 2007-2015 and currency wars in 2012-2016).

### 3. Regime dependence
FX mean reversion strategies historically work poorly when:
- USD strengthens persistently (2014-2016, 2022)
- Major carry crises occur (2008: unwinding yen carry; 2015: EM currency crises)
- Geopolitical/macro shocks cause persistent currency realignments

### 4. Sample / overfitting
Monthly rebalancing over 17 years gives 204 data points. With two sizing variants and
an unspecified rolling lookback for the mean, the degrees of freedom are limited.

### 5. What would change my mind
An independent replication showing the exponential variant's specific Sharpe (e.g., 0.5+)
with walk-forward analysis, or a peer-reviewed paper using the same methodology on a
different currency universe and period, would improve confidence.

---

## Strengths / Weaknesses

**Strengths:**
- Covers FX asset class (underrepresented in database)
- 17-year backtest with clear evidence of both positive (exponential) and negative (linear) results
- FX futures automatically capture carry component
- Monthly rebalancing = low transaction costs
- PPP/UIP theory provides genuine economic rationale
- Authors honestly report the null result for linear variant

**Weaknesses:**
- Linear variant's Sharpe 0.12 is a null result — the most accessible variant has no edge
- No peer review
- Specific exponential performance numbers not accessible
- No walk-forward analysis
- Exponential sizing introduces undefined risk profile
- Monthly FX futures is institutional-scale (requires futures access; not accessible to retail)

---

## Confidence Score

### Per-dimension scoring

| Dimension | Wt | Score | Contribution | Rationale |
|---|---|---|---|---|
| Logical / economic soundness (falsifiable) | 3 | 4 | 12.0 | FX mean reversion theory solid (PPP/UIP); linear variant Sharpe 0.12 confirms weak empirical basis; failure conditions well-stated |
| Transparency & reproducibility | 2 | 6 | 12.0 | Methodology clearly described; two variants; FX futures standard instrument |
| Evidence auditability | 2 | 4 | 8.0 | SSRN working paper; not peer-reviewed; linear Sharpe confirmed from search summaries |
| Risk management quality | 2 | 4 | 8.0 | Long-short structure; monthly rebalancing; no explicit stops or drawdown limits |
| Cost & capacity realism | 2 | 5 | 10.0 | Monthly FX futures = low turnover; costs not explicitly modeled but low implied |
| Honest treatment of drawdowns / failure | 1.5 | 6 | 9.0 | Linear null result honestly reported; exponential drawdowns acknowledged |
| Robustness evidence (OOS / walk-forward) | 1.5 | 4 | 6.0 | 17 years; but no walk-forward; single study; linear variant essentially fails |
| Survived independent scrutiny | 1 | 4 | 4.0 | Quantocracy reference; Quantpedia credible; no peer review |

**Weighted sum:** 12.0 + 12.0 + 8.0 + 8.0 + 10.0 + 9.0 + 6.0 + 4.0 = **69.0**
**Max possible:** 150
**latent_score:** round(69.0 / 150 × 100) = **46**

**Verification cap check:**
- Evidence auditability = 4 → cap at 59
- latent 46 < 59 → cap does not bind
- **confidence = 46** (Experimental, 40–59)

*latent 46 (capped to 59 pending verification: not peer-reviewed; linear variant Sharpe 0.12 confirms weak edge; exponential variant unverified)*

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Low. Simple deviation-from-mean ranking on monthly FX futures prices.
  Omega ratio not used here (unlike the intraday NASDAQ paper). Monthly cadence means
  minimal automation complexity.
- **Scalability:** High. G10 FX futures are the most liquid derivatives markets globally.
- **Automation:** Feasible as a monthly batch job. Yahoo Finance or broker API provides
  FX futures data.

---

## MQL5 / Python / Pine Feasibility

- **Python:** HIGH feasibility for the linear variant (~50 lines). Monthly rebalancing
  with `pandas` and standard deviation calculation. Exponential sizing is equally simple.
- **MQL5:** MEDIUM. MT5 supports FX futures through some brokers; monthly Expert Advisors
  are straightforward to implement.
- **Pine Script:** LOW for futures; no FX futures instrument access.

---

## Similar Strategies

- `london-breakout-forex` (score 47) — only other forex strategy in database; intraday
  vs. monthly here; completely different approach
- Carries related to: FX Carry Trade (long high-yield, short low-yield) — the inverse of
  this strategy when high-yield currencies are "above mean"

---

## Red Flags

- Linear variant Sharpe 0.12 = null result (most informative and concerning finding)
- Exponential variant performance not accessible — selection bias in reporting
- No peer review
- No walk-forward analysis
- Monthly FX futures requires institutional broker access
- Historical mean definition (rolling lookback) not specified in accessible sources

---

## Source Links

| URL | Accessed | Notes |
|-----|----------|-------|
| https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5002058 | 2026-05-29 | SSRN (HTTP 403) |
| https://quantpedia.com/how-to-build-mean-reversion-strategies-in-currencies/ | 2026-05-29 | Quantpedia (HTTP 403) |
| https://quantpedia.medium.com/how-to-build-mean-reversion-strategies-in-currencies-6774538952e8 | 2026-05-29 | Medium (HTTP 403) |
| https://x.com/Quantocracy/status/1850356253241065917 | 2026-05-29 | Quantocracy tweet (search snippet) |

All primary sources returned HTTP 403. Evidence from search engine summaries only.
**Key confirmed number:** Linear variant Sharpe ratio = 0.12 (from search result snippet).

---

## Analyst Notes

**FACTS (from accessible search summaries):**
- Authors: Soňa Beluská and Radovan Vojtko (Quantpedia Research)
- Published: SSRN 5002058, October 25, 2024
- Strategy: Long undervalued FX futures, short overvalued FX futures
- Rebalancing: Monthly
- Two variants: linear sizing and exponential sizing
- Data period: 2007-2024
- **Confirmed: Linear variant Sharpe = 0.12, Calmar = 0.05** (from search summary)
- Linear variant portfolio growth: ~flat (~1.1× initial value over 10 years)
- Exponential variant: better growth but higher risk and deeper drawdowns

**ANALYSIS (my reasoning):**
- The linear Sharpe of 0.12 is the most informative result. In a 17-year backtest, this is
  effectively a null result — the linear variant demonstrates the strategy has no meaningful
  edge in its simplest form.
- The exponential variant's apparent improvement is likely a combination of: (1) the
  natural amplification of any residual mean reversion effect, and (2) optimization/selection
  bias from choosing the better of two variants.
- FX mean reversion at monthly frequency is a difficult strategy because: (a) momentum
  dominates FX at 1-12 month horizons, (b) PPP operates at 3-10 year horizons, and (c)
  carry (which this captures through futures) is a separate alpha source not purely "mean reversion."

**ASSUMPTIONS (not confirmed):**
- ASSUMED the "historical mean" is a rolling window (length not confirmed).
- ASSUMED the currency universe is G10 (EUR, GBP, JPY, CHF, CAD, AUD, NZD + USD cross).
- ASSUMED specific cost assumptions were NOT modeled (monthly FX futures costs should be low).

---

## Future Research Needed

1. **Access the full SSRN paper:** Retrieve the exponential variant's specific Sharpe, CAGR,
   and max drawdown. This is the key missing number.
2. **Walk-forward analysis:** The paper lacks explicit walk-forward; apply it to test stability.
3. **Extend to regime filtering:** Does adding a VIX or trend filter improve the edge?
4. **Compare to FX carry trade:** The currency selection by mean deviation may overlap
   substantially with carry — decompose the alpha sources.
5. **Test at shorter timeframes:** Weekly mean reversion on FX may capture momentum reversals
   more effectively than monthly.
