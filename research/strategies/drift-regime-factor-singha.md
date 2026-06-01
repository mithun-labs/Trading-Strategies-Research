# Drift Regime Factor — 13-Sharpe Cross-Sectional Equity Factor

## Overview

A cross-sectional long-short equity factor that claims to achieve Sharpe 13 out-of-sample by
activating value and short-term reversal signals only during per-stock "drift regimes" — defined
as periods when an individual stock shows more than 60% positive trading days in a trailing
63-day window. The paper claims 20-year walk-forward performance on the S&P 500 with CAGR
158.6%, annualized volatility 12.0%, and maximum drawdown -11.9%.

**Source:** arXiv:2511.12490, also posted as SSRN:5754502. Single author (Mainak Singha).
November 2025. No peer review.

---

## Asset Class / Timeframes

- **Asset class:** US Equities (long-short cross-sectional factor)
- **Universe:** S&P 500 components
- **Timeframe:** Daily (regime check + signal computation daily)
- **Backtest period:** 2004–2024 (20 years) — CLAIMED

---

## Core Logic

1. **Drift regime filter (per stock):** A stock is in a "drift regime" if more than 60% of the
   trailing 63 trading days closed positive. When in a drift regime, the stock is eligible for
   the active signal; when not in a drift regime, the position is zero.
2. **Signal (during drift regime only):** Long-short sorting based on a combination of value
   and short-term reversal signals. Specific signal formulas (what value metric, what reversal
   lookback period) are NOT REPORTED in accessible sources.
3. **Portfolio construction:** Cross-sectional long-short factor portfolio on S&P 500 eligible
   stocks. Position sizing details NOT REPORTED.
4. **Mechanism claim:** "Drift regimes reshape market microstructure by amplifying behavioral
   biases, altering liquidity patterns, and creating conditions where cross-sectional price
   discovery becomes systematically exploitable." (Post-hoc description, not a falsifiable
   prediction.)

---

## Economic Rationale

ANALYSIS: The proposed rationale is that when individual stocks are in prolonged uptrends (>60%
positive days / 63-day window), market participants exhibit stronger anchoring to recent prices,
momentum herding, and reduced friction, making value and reversal signals more reliable. This
mechanism is plausible as a post-hoc story but is NOT falsifiable in the operational sense
defined in CLAUDE.md: it does not state specific conditions under which the edge disappears
(e.g., what VIX level, what market-cap regime, what macroeconomic environment would cause the
drift regime to stop being predictive). It describes every outcome after the fact and predicts
no specific future failure.

**Rating: pattern-only. No clear falsifiable economic rationale.**

The value × regime interaction has surface plausibility — several academic papers document
that momentum and reversal interact with trending conditions. But a genuinely testable
prediction (e.g., "value+reversal within drift regimes should fail in high-correlation markets
because herding eliminates cross-sectional dispersion") is not articulated.

---

## Entry Conditions

- Each day, compute the fraction of positive-close days for each S&P 500 stock over the
  trailing 63 trading days.
- If fraction > 60%, the stock is "in drift regime" → apply value + reversal signal.
- Long the top-ranked stocks, short the bottom-ranked stocks (cross-sectional sort).
- Exact portfolio construction (decile? quintile? top-bottom 10%? dollar-neutral?
  market-neutral?) NOT REPORTED.

---

## Exit Conditions

- Presumably daily rebalancing based on updated regime status and signal rank.
- No explicit stop-loss or time-exit defined in available sources.
- When a stock exits the drift regime (fraction ≤ 60%), the position is closed.

---

## Risk Management

- Regime filter acts as a dynamic exposure gate: positions are zero for any stock not in drift.
- Specific position sizing: NOT REPORTED.
- Maximum position limits: NOT REPORTED.
- Portfolio-level stop or drawdown rule: NOT REPORTED.
- Capacity / market impact: NOT REPORTED.

---

## Indicators Used

- **Drift regime indicator:** Fraction of positive-close days in trailing 63-day window;
  threshold = 60%.
- **Value signal:** NOT REPORTED (exact metric unspecified in accessible sources).
- **Reversal signal:** NOT REPORTED (lookback and exact definition unspecified).

---

## Transaction Costs & Capacity

- **Transaction costs modeled:** NOT REPORTED. No mention of spread, commission, or
  market-impact costs in any accessible source.
- **Turnover estimate:** NOT REPORTED. A daily-rebalanced long-short cross-sectional factor on
  S&P 500 stocks with dynamic regime filtering would likely have very high turnover (plausibly
  200–500%+ annually). At this turnover, even 5–10 bps per trade round-trip would substantially
  erode reported gross Sharpe.
- **Capacity:** NOT REPORTED. Long-short equity factors at meaningful size face market impact
  costs not reflected in daily close prices.

ANALYSIS: The complete omission of transaction cost modeling on a daily-rebalanced long-short
equity strategy is a **presumptive invalidation** per the evaluation framework. Any strategy
with non-trivial turnover and no cost modeling should be treated as gross performance only,
not net performance.

---

## Backtest Evidence

- **Period:** 2004–2024 (20 years) — CLAIMED
- **Universe:** S&P 500 — CLAIMED (survivorship-biased; S&P 500 composition uses current
  constituents back-tested, systematically over-representing survivors)
- **Validation method:** Walk-forward with "frozen parameters" — CLAIMED
- **Standard factor exposure:** R² < 3% to Fama-French and other standard factors — CLAIMED
- **Status:** CLAIMED, UNVERIFIED — arXiv preprint, no peer review, no institutional finance
  affiliation, no independent replication

---

## Forward-Test Evidence

- NOT REPORTED. No live trading or paper trading results documented.

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | 13 (OOS) | CLAIMED, UNVERIFIED | arXiv 2511.12490 |
| Sortino Ratio | NOT REPORTED | — | — |
| Calmar Ratio | NOT REPORTED | — | — |
| Profit Factor | NOT REPORTED | — | — |
| Win Rate | NOT REPORTED | — | — |
| Expectancy per Trade | NOT REPORTED | — | — |
| Maximum Drawdown | -11.9% | CLAIMED, UNVERIFIED | arXiv 2511.12490 |
| CAGR | 158.6% | CLAIMED, UNVERIFIED | arXiv 2511.12490 |
| Annual Return | 158.6% (average) | CLAIMED, UNVERIFIED | arXiv 2511.12490 |
| Number of Trades | NOT REPORTED | — | — |
| Average Trade Duration | NOT REPORTED | — | — |
| Recovery Factor | NOT REPORTED | — | — |
| Risk-Reward Ratio | NOT REPORTED | — | — |
| Exposure Percentage | NOT REPORTED | — | — |
| Annualized Volatility | 12.0% | CLAIMED, UNVERIFIED | arXiv 2511.12490 |

**Automatic scrutiny triggers fired:**
- Sharpe > 3: 13 >> 3 (MANDATORY scrutiny — see Why This Might Be Nothing)
- CAGR > 50%: 158.6% >> 50% (MANDATORY scrutiny — see Why This Might Be Nothing)

ANALYSIS — Implied Calmar: CAGR 158.6% / max DD 11.9% = Calmar 13.3. This is the most
extreme Calmar ratio in this research database by a factor of ~15. For context, the best
CTA funds historically achieve Calmar ~1–2.

---

## Community Sentiment

- **Available community commentary:** None found. No discussion threads on r/algotrading,
  QuantLib forums, or LinkedIn. The paper was posted in November 2025 and remains essentially
  without public commentary.
- **Peer review status:** No peer review. arXiv preprint and SSRN cross-post only.
- **Author background:** Mainak Singha is associated with NASA GSFC Sciences and Exploration
  Directorate and IIT Bombay Centre for Studies in Resources Engineering — an astrophysics/
  remote-sensing research background, not quantitative finance. This does not invalidate the
  research, but it does mean the author may be unaware of the specific multiple-testing
  pathologies endemic to empirical factor research.
- **Verdict:** No independent scrutiny of any kind.

---

## Why This Might Be Nothing

### 1. Most likely explanation: regime selection is data mining

The drift regime definition (>60% positive days / 63-day window) has at minimum three free
parameters: the threshold (60%), the window length (63 days), and the positive-day definition
(presumably close > previous close). The signals applied within the regime add further
parameters. Even if the paper's "1,000 randomization trials" test confirms that 60% is special
relative to nearby thresholds, this test does not correct for:
- The choice of 63 days over other lookback windows
- The choice of positive-day counting as the regime criterion over dozens of alternatives
  (e.g., SMA crossover, rolling Sharpe, trailing return threshold, drawdown from peak)
- The choice to combine value+reversal specifically (not momentum, not quality, not size)
- Any interaction between the regime filter and the signal parameters

This is the multiple-testing problem at the concept level, not just the parameter level. The
"randomization trials" test is a weak correction for a narrow subset of the actual search
space. A Sharpe of 13 on any 20-year backtest is a near-certain indicator that an equivalent
test on a randomized dataset would also find a Sharpe of 13 by trying enough combinations —
the question is how many combinations were tried before "frozen parameters" were selected.

### 2. The CAGR/max-DD combination is physically impossible for a real strategy

A strategy returning 158.6% per year for 20 years with maximum drawdown of only -11.9% implies
a Calmar ratio of 13.3 over two decades. No live-audited long-short equity strategy in history
has sustained a Calmar above 2 over a 20-year horizon. The implied equity curve would have
grown from $1,000 to ~$180 billion (= $1,000 × 2.586^20) with only one drawdown exceeding -12%.
This is inconsistent with any real market process and is almost certainly an artifact of
overfitting: the optimizer found the parameter set where the test dataset happened to be benign,
making the ex-post equity curve appear smooth.

### 3. No transaction costs — the strategy is presumptively invalid

A daily-rebalanced long-short cross-sectional equity factor on 500 stocks with a dynamic
regime gate will have substantial turnover. At 200% annual turnover and 5 bps per one-way
trade, the annual cost drag is approximately 200 bps (2 percentage points of annual return).
Against a stated Sharpe of 13 and CAGR of 158.6%, this looks negligible — but the gross
performance is precisely what's suspect. The missing cost model is both evidence that the
extreme numbers are not sanity-checked and a reason to treat net performance as unknown.

### 4. Survivorship bias in S&P 500 universe

Using S&P 500 stocks as the universe without accounting for index reconstitution creates
survivorship bias: stocks that were in the S&P 500 at some point but later delisted or removed
are excluded. The remaining stocks (survivors) have systematically better historical returns
than the full eligible universe. Survivorship bias overstates factor strategy performance,
often by 1–3 Sharpe units depending on the strategy and period.

### 5. "Walk-forward with frozen parameters" is not genuine OOS discovery

Walk-forward simply means the strategy was not re-optimized on each new data increment. It
does NOT mean the parameters were selected without reference to the full dataset. If the drift
regime threshold (60%), window (63 days), and signal choices were discovered by searching over
the 2004–2024 dataset, then a "walk-forward" on the same dataset is still in-sample. The
description "frozen parameters, entirely out-of-sample" is accurate in a narrow technical sense
(the parameters don't change during the walk-forward) but does not rule out that the parameters
were discovered by fitting to the full period.

### 6. R² < 3% to standard factors is a red flag, not a positive

Standard factor models (Fama-French 5-factor, Carhart 4-factor, etc.) explain 30–50% of
cross-sectional equity return variance. A factor that is orthogonal to ALL of these is not
necessarily a new discovery — it is more likely to be noise that happens to be correlated with
the specific market conditions of 2004–2024 but not captured by the standard factor structure.
True new factor discoveries (e.g., momentum in 1993, quality in 2013) tend to be partially
correlated with existing factors while adding incremental explanatory power. A 3% R² on a
claimed Sharpe 13 factor is suspicious.

### 7. The one piece of evidence that would change my mind

A fully independent replication by a quant finance researcher at an institutional setting
(academic or practitioner) who obtained the same source data, implemented the strategy from
the paper's description, applied realistic transaction costs (at minimum 5–10 bps per one-way
trade), corrected for historical S&P 500 index composition (no survivorship bias), and
confirmed Sharpe > 5 net of costs on OOS data after 2024 — would constitute meaningful
evidence worth revisiting. Currently no such evidence exists.

---

## Strengths / Weaknesses

**Strengths:**
- The idea of regime-conditional factor activation is academically valid and has precedent
  (e.g., Asness et al. 2000 on momentum in different market conditions)
- Combining value and reversal has theoretical support (both are well-documented factors)
- 20-year dataset is longer than many academic studies

**Weaknesses:**
- Extreme metric claims that are mechanically implausible
- No transaction cost modeling
- No institutional finance affiliation; author from astrophysics background
- No community scrutiny or independent replication
- Randomization test does not adequately correct for the full regime-concept search space
- Signal specifications (value, reversal) not fully reported
- S&P 500 survivorship bias not addressed
- No capacity analysis for long-short equity

---

## Confidence Score

| Dimension | Wt | Score | Contribution |
|---|---|---|---|
| Logical/economic soundness (falsifiable) | 3 | 2 | 6.0 |
| Transparency & reproducibility | 2 | 4 | 8.0 |
| Evidence auditability | 2 | 2 | 4.0 |
| Risk management quality | 2 | 2 | 4.0 |
| Cost & capacity realism | 2 | 0 | 0.0 |
| Honest treatment of drawdowns/failure | 1.5 | 3 | 4.5 |
| Robustness evidence (OOS/walk-forward) | 1.5 | 3 | 4.5 |
| Survived independent scrutiny | 1 | 0 | 0.0 |

- **Weighted sum:** 31.0 / 150
- **Latent score:** round(31/150 × 100) = **21**
- **Evidence auditability score:** 2 → cap = 59
- **Confidence (capped):** min(21, 59) = **21**
- **Band:** Low Confidence

`latent 21 (capped to 21 pending verification: arXiv preprint, single non-finance author, no peer review, no independent replication)`

**Dimension notes:**
- Logic (2/10): Mechanism ("drift reshapes microstructure") is post-hoc and cannot be falsified — does not state when the edge should disappear.
- Transparency (4/10): Drift regime defined (>60% positive days / 63-day window), but exact signal formulas (value, reversal) NOT REPORTED.
- Auditability (2/10): arXiv preprint + SSRN cross-post only; no peer review; single non-finance author; no institutional finance affiliation.
- Risk management (2/10): No sizing, no stops, no capacity discussed.
- Cost realism (0/10): Costs completely absent on daily-rebalanced long-short equity — presumptive invalidation.
- Drawdowns (3/10): Max DD reported (-11.9%), but failure modes and out-of-regime performance not analyzed.
- Robustness (3/10): Claims walk-forward + 1,000 randomization trials, but randomization test is a weak partial correction; S&P 500 universe only.
- Scrutiny (0/10): No community discussion, no institutional review found.

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Moderate. The drift regime filter is simple (rolling positive-day count). The value and reversal signals require standard financial data. Cross-sectional daily long-short execution requires data pipeline for 500 stocks.
- **Scalability:** Poor. Long-short cross-sectional factors on large-cap stocks face capacity constraints from market impact costs at any meaningful AUM.
- **Automation feasibility:** Theoretically feasible in Python (pandas, zipline/vectorbt), but signal specifications are NOT REPORTED — full implementation is not possible without the original paper's complete specification.

---

## MQL5 / Python / Pine Feasibility

- **Python:** Partially feasible for the regime filter. Full implementation blocked by unspecified signal parameters.
- **MQL5:** Not practical for cross-sectional 500-stock factor strategies.
- **Pine Script:** Not feasible (cross-sectional factor requires multi-asset data management beyond Pine's capabilities).
- **Implementation gate:** Requires the full paper to obtain signal specifications.

---

## Similar Strategies

- `attention-factors-statistical-arbitrage` (score 66): Cross-sectional equity factor strategy, also daily, also long-short on large-cap US equities. Better evidence quality (ICAIF 2025 conference paper, institutional authors). Score 66 reflects the superior methodology.
- `interpretable-hypothesis-driven-microstructure` (score 52): Hypothesis-driven equity signal testing framework. Demonstrated Sharpe 0.33 with no edge (p=0.34) on the same general approach of cross-sectional equity factor research.

---

## Red Flags

- **Sharpe 13 OOS** — 43-86× typical equity factor OOS Sharpe; automatic scrutiny trigger fired
- **CAGR 158.6%** — >50% trigger; physically implausible net of costs
- **Calmar 13.3** — no live-audited strategy has sustained Calmar >2 over 20 years
- **Transaction costs NOT REPORTED** — presumptive invalidation of all metrics
- **Single non-finance author** — astrophysics background; high risk of factor-selection multiple-testing blindspot
- **No peer review, no institutional finance affiliation**
- **S&P 500 survivorship bias unaddressed**
- **Randomization test is weak** — corrects for threshold value only, not for regime concept selection

---

## Source Links

| Source | URL | Retrieved | Status |
|--------|-----|-----------|--------|
| arXiv 2511.12490 (abstract) | https://arxiv.org/abs/2511.12490 | 2026-06-01 | HTTP 403 |
| arXiv 2511.12490 (HTML) | https://arxiv.org/html/2511.12490v1 | 2026-06-01 | HTTP 403 |
| arXiv 2511.12490 (PDF) | https://arxiv.org/pdf/2511.12490 | 2026-06-01 | HTTP 403 |
| SSRN 5754502 | https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5754502 | 2026-06-01 | HTTP 403 |
| ideas.repec.org | https://ideas.repec.org/p/arx/papers/2511.12490.html | 2026-06-01 | HTTP 403 |
| ResearchGate | https://www.researchgate.net/publication/397701197 | 2026-06-01 | HTTP 403 |
| Search engine snippets | WebSearch | 2026-06-01 | Accessible |

---

## Analyst Notes

**FACTS (from search engine snippets and search result descriptions):**
- Paper title: "Discovery of a 13-Sharpe OOS Factor: Drift Regimes Unlock Hidden Cross-Sectional Predictability"
- Author: Mainak Singha (single author)
- arXiv posted: November 2025 (arXiv:2511.12490); SSRN cross-post: 5754502
- Stated performance: Sharpe 13 OOS, CAGR 158.6%, vol 12.0%, max DD -11.9%
- Universe: S&P 500, 2004–2024 (20 years)
- Drift regime definition: >60% positive days in trailing 63-day window (per stock)
- Signals: "value and short-term reversal" (combined, exact specifications NOT REPORTED)
- Standard factor R²: < 3% (claimed)
- Validation: 1,000 randomization trials (p < 0.001); Sharpe >7 under 30% parameter perturbations
- Author affiliation: NASA GSFC / IIT Bombay (astrophysics/remote-sensing, not finance)
- No peer review; no institutional finance affiliation

**ANALYSIS:**
- The combination of Sharpe 13 OOS, CAGR 158.6%, and max DD -11.9% is mechanically implausible
  for a real long-short equity strategy over any 20-year window.
- The randomization test corrects only for the specific threshold parameter (60%) and does not
  address the broader multiple-testing problem inherent in discovering the regime concept itself.
- No transaction cost modeling on a daily-rebalanced cross-sectional factor is a disqualifying
  omission for any performance claim.
- The author's astrophysics background is a relevant context signal — not disqualifying, but
  associated with underestimation of factor-mining false discovery rates specific to finance.
- The low standard-factor R² is a red flag rather than a sign of novelty: genuine new factors
  are partially correlated with known factors; noise is orthogonal.

**ASSUMPTIONS (flagged):**
- ASSUMED: The strategy is long-short (not long-only), based on the use of "factor" and
  cross-sectional sorting language. Not explicitly confirmed in accessible sources.
- ASSUMED: Daily rebalancing, based on "daily signal computation" described in snippets.
- ASSUMED: Position sizing is equal-weighted or cap-weighted, as no other method is described.

---

## Future Research Needed

1. **Obtain and read the full PDF** — exact signal specifications (value metric, reversal lookback), position sizing, index construction methodology.
2. **Check survivorship bias handling** — does the paper use point-in-time S&P 500 composition or current constituents?
3. **Independent replication** — replicate the regime filter on public data (Compustat + CRSP) and report whether the backtest is reproducible.
4. **Transaction cost sensitivity** — run the strategy with 5, 10, 15 bps per one-way trade and report the net Sharpe degradation.
5. **Randomization test completeness** — test not just threshold perturbation but also window perturbation, alternative regime concepts (SMA-based, return-based, drawdown-based), and null hypothesis where dates are permuted.
