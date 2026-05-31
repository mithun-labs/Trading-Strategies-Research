# Mining Gold for Regimes — PCA/K-Means Regime-Conditional Gold Allocation

## Overview

A regime identification framework for gold that uses unsupervised machine learning (PCA +
k-means clustering) to identify three latent macro regimes driving gold's return dynamics.
Within each regime, gold plays a different portfolio role (stable currency, real asset
inflation hedge, or low-rate safe haven). A tactical asset allocation (TAA) rule based
on the identified regime is shown to outperform several benchmarks on performance, risk,
and diversification.

**Authors:** Colin Suvak, Matthew J. Johnson, Vineer Bhansali, Linda Chang, Jeremie Holdom
(LongTail Alpha).

**Published:** Journal of Alternative Investments, Summer 2025, Volume 28, Issue 1, pp. 58–74.
This is a **peer-reviewed** publication in a recognized alternatives-focused journal.

**SSRN:** 5247471 (posted April 28, 2025).

**Critical limitation:** The full methodology, specific input features, and tactical
allocation rules are behind a paywall. Evidence in this file comes entirely from search
engine summaries of the pre-print white papers and SSRN abstract. Specific performance
metrics (Sharpe, CAGR, max drawdown) are NOT REPORTED in accessible sources.

---

## Asset Class / Timeframes

- **Asset:** Gold (physical, futures, or GLD ETF proxy)
- **Regime features:** NOT FULLY REPORTED — macro features related to real yields,
  inflation expectations, and possibly equity/bond returns (inferred from context);
  specific PCA input variables paywalled
- **Rebalancing:** Monthly (inferred from regime persistence statistics)
- **Historical coverage:** Long history (gold has data going back centuries; specific
  backtest window NOT REPORTED in accessible sources)
- **Markets in scope:** Gold/XAUUSD ✓

---

## Core Logic

### Regime Identification (PCA + K-Means)

1. **Dimensionality reduction:** Apply PCA to a set of macro/financial features relevant
   to gold (specific features NOT REPORTED in accessible sources).

2. **Clustering:** Apply k-means clustering on the PCA-reduced features, fixing k=3 (three
   regimes). Run 1,000 iterations with different starting points to address k-means
   sensitivity to initialization. Select the best-fit solution.

3. **Three regimes identified:**
   - **Regime 1 (pre-GFC baseline):** The "normal" regime; gold acts as a **stable currency**
     / store of value. Most common regime historically.
   - **Regime 2 (intermediate):** Gold functions as a **real asset inflation hedge**. Details
     on entry/exit conditions NOT REPORTED.
   - **Regime 3 (post-GFC low-rate):** First appeared December 2008 after the GFC and Great
     Recession (the "low-rate / unconventional monetary policy" regime). Only briefly reverted
     to Regime 1 for March–May 2021 (COVID fiscal expansion peak). Characterized by:
     initially positive for gold (falling real yields), but regime 3's persistence through
     2022 means rising real yields within this regime damaged gold returns unexpectedly.

### Regime Persistence

Monthly Markov transition probabilities of remaining in the same regime:
- Regime 1: 93.8% stay probability
- Regime 3: 98.9% stay probability (extremely persistent once established)

This high persistence means regime switches are rare and the signal is slow-moving —
more useful for strategic or medium-term positioning than short-term tactical trades.

### Tactical Allocation

Based on regime identification, a TAA rule is presented that outperforms several benchmarks
on multiple measures of performance, risk, and diversification. Specific TAA rules are
**NOT ACCESSIBLE** (paywalled). The general principle: increase gold allocation in regimes
where gold historically performed well (stable currency regime, entering inflation-hedge
regime); reduce or hedge gold in adverse regimes.

---

## Economic Rationale

**Mechanism:** Gold serves different economic functions in different macro regimes:
- **Stable currency regime:** Gold is an alternative store of value when fiat currency
  trust is uncertain (geopolitical/fiscal stress, currency debasement concerns).
- **Inflation hedge regime:** Gold correlates positively with inflation expectations and
  rises as real yields fall; inflation erodes the value of nominal bonds and currency.
- **Post-GFC low-rate regime:** Ultra-low real rates reduce the opportunity cost of holding
  gold, making it attractive relative to cash and bonds.

**Key empirical insight:** The 2022-2023 period revealed that gold's real yield sensitivity
collapsed. The historical (2005-2021) correlation between gold and real yields was 84%;
in 2022-2023 this dropped to 3%, and post-2024 is only 7%. This regime shift — where gold
maintained or rose despite rising real yields — suggests gold may have entered a new
"stable currency / debasement hedge" regime driven by central bank buying and fiscal
sustainability concerns.

**Falsifiability:** The edge should disappear when: (1) the three-regime structure is
not stable out-of-sample (k-means may identify different regimes in a new period);
(2) gold's regime drivers shift completely (e.g., if central banks stop accumulating gold,
removing the debasement regime); (3) the PCA features lose predictive power for gold as
the gold market structure changes.

**Assessment:** The mechanism is genuinely grounded in economic theory and has peer-reviewed
academic backing. The three-regime framework is consistent with the broader gold literature.
The 2022-2023 observation (real yield correlation breakdown) is honest and significant.

---

## Entry Conditions

**NOT FULLY SPECIFIED** — tactical allocation rules are paywalled.

**Inferred general principle:** Increase gold allocation when the model identifies being in
or transitioning toward the "stable currency" or "inflation hedge" regime. Reduce/exit gold
when in the adverse real-yield-rising regime.

---

## Exit Conditions

**NOT SPECIFIED** in accessible sources — depends on regime transition detection.

---

## Risk Management

- **Stop-loss:** NOT REPORTED
- **Position sizing:** NOT REPORTED
- **Maximum drawdown limit:** NOT REPORTED
- **Regime transition timing:** High persistence (93-98%) means rare switches; monthly
  rebalancing likely adequate; low transaction cost implication

Assessment: No explicit risk management rules accessible. The regime filter itself
is an implicit risk control (reducing exposure in adverse regimes), but without specific
rules this dimension cannot be scored well.

---

## Indicators Used

- PCA-reduced macro/financial features (specific variables NOT REPORTED in accessible sources)
- K-means cluster assignment (3 regimes)
- Inferred features from context: real yields (TIPS), inflation expectations, possibly
  USD index, equity volatility, central bank gold purchases — but this is ASSUMED

---

## Transaction Costs & Capacity

**Costs modeled:** NOT REPORTED in accessible sources.

**Implied costs:** Given regime persistence of 93-99%, regime switches occur on average
every 16 months (Regime 1) to 83 months (Regime 3). Low turnover strategy. Monthly
rebalancing of a gold position has minimal cost (GLD bid-ask spread ~1-2 bps). Cost
impact is likely negligible.

**Capacity:** Gold futures and GLD ETF are highly liquid; capacity unlimited at retail.

---

## Backtest Evidence

**Status:** CLAIMED, UNVERIFIED (specific metrics); CLAIMED AUDITABLE (publication status)

- Published in Journal of Alternative Investments (Vol 28, No 1, Summer 2025, pp 58-74)
  — **peer-reviewed** in a recognized alternatives journal
- Pre-print available as LongTail Alpha white papers (Nov 2024 and July 2024 versions)
- SSRN 5247471
- Full methodology and performance results paywalled; pre-print PDFs also HTTP 403
- No open code found
- Evidence gathered from SSRN abstract + search engine summaries of white paper + publication notice

**Note on auditability:** The peer-review process provides external methodology validation,
but because I cannot access and read the full paper, the evidence auditability score is
capped at 6 — the peer review is confirmed (published in a known journal), but I cannot
independently verify the methodology line-by-line.

---

## Forward-Test Evidence

**Status:** NOT REPORTED — no explicit forward test found.

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | NOT REPORTED | NOT REPORTED | — |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | — |
| CAGR | NOT REPORTED | NOT REPORTED | — |
| Annual Return | NOT REPORTED | NOT REPORTED | — |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | NOT REPORTED | NOT REPORTED | — |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | NOT REPORTED | NOT REPORTED | — |

**Qualitative performance claim (CLAIMED, UNVERIFIED):** The tactical allocation based on
regime identification outperforms "several benchmarks on multiple measures of performance,
risk, and diversification." No numerical specifics accessible.

---

## Community Sentiment

- Published in Journal of Alternative Investments — implies peer community acceptance
- LongTail Alpha is a recognized quantitative research firm (Vineer Bhansali is a well-known
  quantitative finance practitioner, formerly of PIMCO)
- The research direction (regime-conditional gold) is consistent with the broader gold
  regime literature (see also Dujava/Quantpedia cross-asset regimes, which independently
  arrives at a similar structural conclusion)
- No forum-level criticism or independent replication found in accessible sources

---

## Why This Might Be Nothing

1. **Tactical allocation rules not accessible:** Without knowing the specific TAA rules,
   the "outperforms benchmarks" claim cannot be scrutinized. If the rules were as simple as
   "be long gold in Regime 1 and 2, cash in Regime 3," the result could be trivially driven
   by the 2022 avoidance — a single regime shift that happened once in the backtest.

2. **K-means regime labels are defined by data, not theory:** The three regimes are
   identified from historical data via k-means. K-means can identify "regimes" in noise.
   The regime interpretations (stable currency, inflation hedge) are applied post-hoc to
   the discovered clusters. This introduces circular reasoning: the clusters are labeled
   based on their historical gold returns, then used to predict gold returns.

3. **k-means sensitivity:** Despite 1,000 random restarts, k-means is sensitive to the
   choice of k=3. Why three regimes? The choice of k=3 appears motivated by parsimony
   rather than formal model selection. Testing k=2, 4, or 5 might yield different insights.

4. **Regime 3 is unique:** Regime 3 first appeared in December 2008 and has not ended (with
   one brief exception). This means the TAA rule has been evaluated on largely a single,
   15-year continuous period in Regime 3. The out-of-sample test for regime transitions is
   essentially: did the rule correctly identify the pre-GFC period? This is in-sample for
   the k-means fitting.

5. **Feature opacity:** The PCA input features are not accessible. If they include gold
   returns themselves or lagged gold performance, the regime identification could be
   partially look-ahead contaminated.

6. **What would change my mind:** Open code with documented PCA features, explicit TAA
   rules, and a walk-forward test where regimes are identified on t-5 years and tested on
   years t to t+1. I do not have this.

---

## Strengths / Weaknesses

### Strengths
- **Peer-reviewed in Journal of Alternative Investments** — provides external methodology validation
- **Credible authors** (Bhansali from LongTail Alpha, formerly PIMCO; recognized practitioner-researchers)
- **Grounded mechanism** — three-role gold theory is consistent with macro finance literature
- **Acknowledges failure mode** — the 2022-2023 real yield correlation breakdown is directly discussed
- **Long data history** — gold has multi-century price data; enough for meaningful regime analysis
- **Consistent with independent research** — Dujava (Quantpedia) independently reaches similar
  regime-conditional conclusion using a different (simpler) approach

### Weaknesses
- **Tactical allocation rules PAYWALLED** — cannot verify or implement the core strategy
- **No open code** — regime identification cannot be verified or reproduced by external parties
- **All performance metrics NOT REPORTED** in accessible sources
- **K-means circularity risk** — regimes defined by data, labeled by gold performance, used to predict gold
- **Regime 3 duration** — essentially a single persistent regime since 2008; limited OOS testing
- **Feature opacity** — PCA inputs not documented publicly

---

## Confidence Score

**Per-dimension breakdown:**

| Dimension | Weight | Score | Contribution |
|-----------|--------|-------|-------------|
| Logical / economic soundness (falsifiable) | 3 | 7 | 21 |
| Transparency & reproducibility | 2 | 3 | 6 |
| Evidence auditability | 2 | 6 | 12 |
| Risk management quality | 2 | 2 | 4 |
| Cost & capacity realism | 2 | 3 | 6 |
| Honest treatment of drawdowns / failure | 1.5 | 5 | 7.5 |
| Robustness evidence (OOS / walk-forward / multi-market) | 1.5 | 5 | 7.5 |
| Survived independent scrutiny | 1 | 5 | 5 |

**Weighted sum:** 69 / 150 max

**latent 46 (capped to 46 — Evidence auditability = 6 (5–7), cap = 74; latent already below cap)**

**Confidence: 46 — EXPERIMENTAL**

*Notes on key drivers:*
- **Logical soundness = 7:** Strongest dimension; mechanism clear, falsifiable (real yield
  correlation breakdown in 2022 acknowledged as failure mode)
- **Evidence auditability = 6:** Peer-reviewed publication (Journal of Alt Investments) is
  the key positive; capped at 6 (not 8) because methodology is paywalled and no open code
- **Transparency = 3:** Framework described (PCA + k-means, 3 regimes) but specific TAA rules
  and PCA features are paywalled; not reproducible from accessible information
- **Risk management = 2:** No explicit risk management rules accessible

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Medium-high — requires PCA + k-means implementation with monthly updates
  as new data arrives; regime assignment must be stable and robust to data revisions
- **Scalability:** Gold/GLD is highly liquid; capacity not a concern
- **Automation:** Feasible with Python (sklearn PCA + KMeans); requires access to the
  original PCA feature set (currently not publicly documented)
- **Implementation barrier:** High — without the specific feature set from the paper, the
  approach cannot be accurately replicated

---

## MQL5 / Python / Pine Feasibility

- **Python:** Feasible with sklearn (PCA, KMeans); the framework is straightforward
  once features are known; ongoing regime updates require stable feature definitions
- **Pine Script:** Not practical for PCA/k-means — Pine is not designed for unsupervised ML
- **MQL5:** Possible but complex; k-means in MQL5 would require manual implementation
- **Best path:** Python + QuantConnect or backtrader framework; requires the original
  paper's feature specification

---

## Similar Strategies

- `gold-cross-asset-regimes` — Dujava's simpler cross-asset GLD/IEF momentum rule;
  different method but same regime-conditional gold concept; same market
- `forecast-to-fill-gold-futures` — gold futures daily strategy; different mechanism;
  overlapping market
- `regime-switching-jump-model-equity` — Statistical Jump Model for equity index
  regime timing; same broad family (ML regime detection for TAA); different asset

---

## Red Flags

- **Tactical allocation rules not reproducible** — core strategy component inaccessible
- **All performance metrics NOT REPORTED** — cannot assess quantitative claims
- **K-means circularity** — regimes defined post-hoc; circular feature selection risk
- **Regime 3 persistence** — one sustained regime since 2008; limited real OOS validation
- **Paywalled methodology** — cannot perform independent line-by-line audit

---

## Source Links

| URL | Retrieved |
|-----|-----------|
| [SSRN 5247471](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5247471) | 2026-05-30 (HTTP 403) |
| [Journal of Alternative Investments](https://www.pm-research.com/content/iijaltinv/28/1/58) | 2026-05-30 (HTTP 403; paywalled) |
| [LongTail Alpha white paper Nov 2024](https://www.longtailalpha.com/wp-content/uploads/2024/11/LongTail-Alpha-Mining-Gold-for-Regimes-November-14-2024.pdf) | 2026-05-30 (HTTP 403) |
| [LongTail Alpha white paper Jul 2024](https://www.longtailalpha.com/wp-content/uploads/2024/07/LongTail-Alpha-Mining-Gold-for-Regimes.pdf) | 2026-05-30 (HTTP 403) |

*Note: All primary sources returned HTTP 403. Evidence from search engine summaries,
SSRN abstract snippets, and publication notices only.*

---

## Analyst Notes

**FACTS (from sources):**
- Authors: Suvak, Johnson, Bhansali, Chang, Holdom (LongTail Alpha)
- Published: Journal of Alternative Investments Vol 28, No 1, Summer 2025, pp 58-74 (peer-reviewed)
- SSRN 5247471, April 28, 2025
- Methodology: PCA + k-means (k=3, 1,000 iterations)
- Three regimes: Regime 3 first appeared December 2008; persistence 93.8-98.9%
- Regime interpretations: stable currency / inflation hedge / real asset
- Tactical allocation outperforms benchmarks (qualitative claim, no metrics accessible)
- Real yield-gold correlation: 84% (2005-2021), 3% (2022-2023), 7% (post-2024)

**ANALYSIS (my reasoning):**
- The peer-reviewed status is the key distinguishing feature; most gold strategies in this
  database are practitioner working papers
- The three-regime structure is coherent and consistent with Dujava's independent
  cross-asset approach (convergent evidence from different methods)
- Regime 3's persistence since 2008 means the TAA is essentially claiming it would
  outperform in normal (pre-GFC) history AND in the single long post-GFC regime — but
  the regime boundary itself was identified in-sample
- The real yield correlation collapse post-2022 is a genuine regime shift concern that
  the authors honestly address

**ASSUMPTIONS (flagged):**
- ASSUMED the TAA rules are based on the current identified regime (logical but not confirmed)
- ASSUMED monthly rebalancing (from regime persistence statistics; frequency not stated)
- ASSUMED PCA features include real yields, inflation, and possibly FX given the paper's focus

---

## Future Research Needed

1. **Obtain full paper:** Access the Journal of Alternative Investments article to extract
   specific PCA features, TAA rules, and performance metrics — this is the single most
   important step
2. **Open code replication:** Implement the PCA + k-means framework with inferred features;
   check if identified regimes are robust to feature selection and k choice
3. **K sensitivity test:** Test k=2, 4, 5 to assess whether three regimes is optimal
4. **Combine with Dujava approach:** The simpler GLD/IEF two-signal rule and the PCA/k-means
   framework may be complementary; test whether combining signals improves performance
5. **Post-2022 performance:** Given the real yield correlation collapse, test whether the
   TAA added value after 2022 in the new "debasement" macro regime
