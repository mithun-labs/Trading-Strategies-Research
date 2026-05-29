# Statistical Arbitrage via Graph Clustering Multi-Pair Trading

## Overview

A multi-pair statistical arbitrage strategy applied to S&P 500 historical components, using
graph clustering algorithms to form pairs from structurally similar stocks, an ensemble of
machine learning classifiers to filter and rank trade signals, Kelly criterion for position
sizing, and time-variant stop-loss/take-profit functions. Backtested OOS from March 2006 to
December 2022 (~17 years) on survivorship-bias-corrected data. Information Ratio
statistically significantly exceeds the S&P 500 benchmark at the 1% significance level.
Published as an arXiv preprint and Warsaw University working paper; not yet peer-reviewed.

- **Authors:** Adam Korniejczuk and Robert Ślepaczuk (Warsaw School of Economics / WNE)
- **Sources:** arXiv:2406.10695 (June 15, 2024); SSRN:4849095; WNE Working Paper 2024-09
- **Status:** arXiv preprint / institutional working paper — NOT peer-reviewed in a journal

## Asset Class / Timeframes

- **Market:** US Equities — S&P 500 historical components (survivorship-bias-corrected)
- **Timeframe:** Daily; rebalancing every 10 trading days
- **Style:** Market-neutral statistical arbitrage (long-short pairs)

## Core Logic

**Step 1 — Universe construction (survivorship-bias-corrected):**
All historical S&P 500 constituents from Jan 2000 onward are included. A stock must have
been *in the index at the time of trading* to be eligible. This explicitly avoids survivorship
bias (a critical improvement over many pairs trading studies).

**Step 2 — Graph clustering for pair formation:**
A correlation matrix (or correlation-derived distance matrix) is computed over a rolling
window. A graph is constructed where nodes are stocks and edges represent correlation
strength. Graph clustering algorithms (specific algorithm: NOT REPORTED in accessible
sources — likely Louvain, spectral clustering, or similar) partition stocks into clusters of
highly correlated equities. Pairs are formed from stocks within the same cluster.

**Step 3 — Statistical arbitrage signal:**
Within each cluster, pairs are evaluated for cointegration or spread mean reversion.
Specific test (ADF, Johansen, KPSS): NOT REPORTED. Entry signal triggered when the
normalized spread exceeds a threshold (exact z-score threshold: NOT REPORTED).

**Step 4 — ML classifier filter:**
An ensemble of standard ML classifiers (specific models: NOT REPORTED — likely
Random Forest, Gradient Boosting, Logistic Regression, or similar) ranks all open
long-short pair candidates. Only the **top 10%** of candidate positions are executed.
This filters to the highest-confidence signals and explicitly reduces transaction costs by
limiting turnover.

**Step 5 — Kelly criterion sizing:**
Position size for each pair is determined by the Kelly criterion, parameterized by the
estimated probability of success and win/loss ratio from the classifier output.

**Step 6 — Time-variant stop-loss and take-profit:**
Novel: stop-loss and take-profit levels are dynamically adjusted over time (e.g., as a
function of recent volatility or holding duration). Exact implementation: NOT REPORTED.

**Step 7 — Portfolio rebalancing every 10 days:**
All open positions are reassessed, the ML classifier reruns, and the portfolio is
rebalanced to the current top-10% candidates.

## Economic Rationale

**Pairs trading rationale:** Stocks in the same graph cluster share fundamental business
similarities (sector, supply chain linkages, shared macro sensitivities). When two
fundamentally similar stocks diverge in price, market friction (information delay,
liquidity differences, fund flow imbalance) creates a temporary pricing gap that tends
to revert as arbitrageurs and the broader market re-equilibrate.

**Why graph clustering improves pair selection:** Traditional distance or cointegration
methods test all $\binom{N}{2}$ pairs, producing many spurious pairings between superficially
similar but fundamentally different companies. Graph clustering identifies *structural*
similarity in the return correlation network, selecting pairs that share economic drivers
rather than just statistical correlation.

**Why the ML filter works:** Not all statistically cointegrated pairs diverge predictably.
An ML classifier trained on historical divergence/convergence patterns can discriminate
between pairs that are "diverged and likely to revert" vs. "diverging permanently." Trading
only the top 10% concentrates the portfolio in the highest-confidence mean-reversion candidates.

**Falsifiability:** The edge should disappear when: (a) pairs are permanently decoupled
(structural breaks in correlation — sector disruption, M&A, regulatory change); (b) the
statistical arbitrage space is over-crowded (too many funds running similar strategies reduce
the available spread); (c) transaction costs exceed the spread earned (particularly at low
spread levels during low-volatility regimes); (d) the ML classifier's training patterns stop
generalizing (regime change after 2022). **The paper was submitted in 2024 but OOS ends
December 2022 — the 2023–2024 regime is entirely out-of-sample even relative to the paper's
own OOS window.** This is a realistic falsifiability boundary.

## Entry Conditions

When a pair's normalized spread exceeds the entry threshold (exact threshold: NOT REPORTED):
- Enter long the underperformer and short the outperformer within the pair.
- Execute only if the ML classifier ranks this pair in the top 10% of candidates.
- Size position using Kelly criterion based on classifier output probabilities.

## Exit Conditions

- **Target exit:** Spread reverts to mean (exact exit z-score: NOT REPORTED).
- **Time-variant take-profit:** Dynamic TP level adjusted by holding duration or recent volatility.
- **Stop-loss:** Dynamic SL level (time-variant); prevents unlimited loss if pairs diverge permanently.
- **Mandatory rebalance:** All positions reassessed every 10 days.

## Risk Management

- **Top-10% ML filter:** Concentrates capital in high-confidence signals, reducing unnecessary turnover and exposure.
- **Kelly criterion:** Position size reflects estimated edge magnitude; automatic deleveraging when edge is low.
- **Time-variant stop-loss:** Prevents runaway losses from permanent pair divergence.
- **Market-neutral:** Long-short pairs structure provides inherent market-factor hedge.
- **No explicit max portfolio drawdown circuit breaker:** NOT REPORTED.

## Indicators Used

- Pairwise return correlations (rolling window; exact window: NOT REPORTED)
- Graph clustering partition (algorithm: NOT REPORTED)
- Spread z-score (exact normalization: NOT REPORTED)
- Ensemble ML classifier (models: NOT REPORTED; features: NOT REPORTED)
- Kelly criterion output (win probability + expected payout from classifier)

## Transaction Costs & Capacity

**Design philosophy:** The ML filter (top 10% of candidates only) and 10-day rebalancing
are explicitly designed to reduce transaction costs. The paper claims the strategy has
"increased immunity to transaction costs" vs. naive pairs trading.

**Costs present but specifics NOT REPORTED:** The exact bid-ask spread, commission, and
borrow cost assumptions used in the backtest are NOT REPORTED in accessible sources. Daily
rebalancing is typical for statistical arbitrage, but 10-day rebalancing is intentionally
lower frequency.

**Capacity:** S&P 500 components are highly liquid. A long-short pairs strategy on these
names has good capacity up to moderate AUM (~$100–500M). Beyond that, impact costs become
material.

**Assessment:** Explicit cost immunity was a design goal; this is a genuine strength
relative to naive pairs trading. However, the specific cost assumptions are not
independently verifiable from accessible sources.

## Backtest Evidence

**CLAIMED (UNVERIFIED):**
- Training/in-sample period: January 1, 2000 – ~March 2006 (implied)
- Out-of-sample test: **March 2006 – December 2022 (~17 years)**
- Daily frequency; 10-day portfolio rebalancing
- Source: arXiv preprint + SSRN + WNE Working Paper; NOT peer-reviewed

The 17-year OOS window is a genuine strength — it includes the 2008 financial crisis,
the 2010–2011 European debt crisis, the 2015 volatility spike, COVID March 2020, and
the 2022 bear market. This is one of the longer OOS tests in the current database.

Survivorship-bias correction is confirmed (only stocks historically present in the index
at the time of trading are eligible).

## Forward-Test Evidence

NOT REPORTED. The paper's OOS ends December 2022; the 2023–2024 period is unevaluated.

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

**Benchmark comparison (CLAIMED, UNVERIFIED):** Information Ratio of the strategy is
statistically significantly greater than SPY ETF at 1% significance level (probabilistic
t-test; exact IR values NOT REPORTED).

**Note:** This is the most metric-sparse entry in the database. The paper is described as
33 pages with 18 tables and 16 figures, suggesting extensive results are present in the
full document — but all primary sources returned HTTP 403.

## Community Sentiment

No community discussion found in accessible sources. The paper was presented at the QFRG
and DSLab Seminar at Warsaw University's Faculty of Economic Sciences (WNE). No forum
discussions, independent replications, or critical reviews found as of 2026-05-29. The
arXiv/SSRN preprint status means no journal peer review has occurred.

## Why This Might Be Nothing

**1. Specific metric values not accessible — "all approaches outperformed" may obscure variance:**
The claim that "all tested approaches outperformed appropriate benchmarks" is encouraging but
also potentially cherry-picked. With 18 tables and 16 figures across multiple combinations of
clustering algorithms, classifier ensembles, Kelly parameterizations, and TP/SL functions,
the paper is testing many configurations. Reporting that "all" outperform the benchmark is
unusual — typically some configurations fail. This suggests either (a) the statement is
overly broad and refers only to a subset of final configurations, or (b) the benchmark
comparison is set up to be easy to beat (e.g., using a buy-and-hold benchmark for a market-
neutral strategy is not a fair comparison).

**2. ML classifier is a black box in available descriptions:**
The ensemble ML classifier that filters to the top 10% of candidates is described only at a
high level. What features it uses, what its out-of-sample accuracy is, and whether it was
fit on a separate period from the cointegration test are NOT REPORTED in accessible sources.
If the classifier was trained on the same period as the cointegration signal, it adds
overfitting risk rather than genuine filtering value.

**3. Large parameter space:**
The strategy involves: (a) graph clustering algorithm and parameters, (b) rolling window
for correlation matrix, (c) entry/exit z-score thresholds, (d) ML classifier type and
hyperparameters, (e) Kelly fraction, (f) time-variant TP/SL function shape. With 18 tables
tested, this is a wide search over a large parameter space. The "all outperformed" claim
should be read in this context.

**4. OOS ends December 2022 — 2023-2025 regime unevaluated:**
The post-2022 environment (Fed tightening cycle, large tech concentration, factor
crowding/uncrowding in 2023-2024) represents a genuinely different regime from 2006-2022.
Mean reversion in stat arb was notably stressed in 2022-2023.

**5. Not peer-reviewed:**
The paper is a working paper / arXiv preprint. The rigorous review process of a top finance
journal (which would scrutinize the methodology for multiple testing, parameter search, and
overfitting) has not occurred.

**What would change my mind:** Full access to the performance table (Sharpe, max drawdown,
CAGR for the best configuration and the full distribution of results), confirmation that the
ML classifier was trained on a strictly separated holdout, an independent replication on a
different equity market, and publication in a peer-reviewed journal.

## Strengths / Weaknesses

**Strengths:**
- 17-year OOS period (March 2006 – December 2022) is one of the longest in the database — includes 2008 crisis, COVID, 2022 bear market
- Survivorship-bias correction confirmed (critical for S&P 500 backtests)
- Explicit transaction cost immunity as a design goal (ML filter + 10-day rebalancing)
- Kelly criterion sizing is more principled than fixed position sizing
- Time-variant TP/SL is novel and limits permanent pair divergence losses
- Statistically significant IR improvement at 1% vs. SPY benchmark

**Weaknesses:**
- No peer review
- No specific metric values accessible (Sharpe, max DD, CAGR all NOT REPORTED)
- ML classifier mechanism opaque (features, training, accuracy not reported)
- "All configurations outperformed" claim is atypical — may reflect selection bias in reporting
- Large parameter space relative to 18 tested variants
- No open code confirmed
- OOS ends 2022; 2023-2025 unevaluated
- Graph clustering algorithm not specified

## Confidence Score

**Per-dimension breakdown:**

| Dimension | Raw (0–10) | Weight | Weighted |
|-----------|-----------|--------|----------|
| Logical/economic soundness (falsifiable) | 6 | 3 | 18 |
| Transparency & reproducibility | 4 | 2 | 8 |
| Evidence auditability | 5 | 2 | 10 |
| Risk management quality | 5 | 2 | 10 |
| Cost & capacity realism | 5 | 2 | 10 |
| Honest treatment of drawdowns | 3 | 1.5 | 4.5 |
| Robustness evidence (OOS / walk-forward) | 7 | 1.5 | 10.5 |
| Survived independent scrutiny | 2 | 1 | 2 |

Weighted sum = 18 + 8 + 10 + 10 + 10 + 4.5 + 10.5 + 2 = **73**
Max possible = 150
`latent_score = round(73 / 150 × 100) = round(48.7) = **49**`

**Verification cap:** Evidence auditability = 5 (5–7 range) → cap at 74.
`confidence = min(49, 74) = **49**`

**Band: Experimental (40–59)**

`latent 49 (capped to 49 pending verification: arXiv preprint, no peer review, specific metrics inaccessible, ML classifier opaque, large parameter space, OOS ends 2022)`

**Note on latent vs. capped score:** The high robustness score (7/10 for a 17-year OOS with
survivorship bias correction) drives the latent score up. If access to the full paper confirms
reasonable metrics (e.g., Sharpe in the 0.5–1.0 range, max DD < 20%), the confidence would
remain 49 (capped by evidence tier) but the strategy would be a strong candidate for deeper
investigation and potential implementation once peer review occurs.

## Complexity / Scalability / Automation Feasibility

**Complexity:** Medium-high. Graph clustering + cointegration + ML ensemble + Kelly +
dynamic TP/SL requires a non-trivial implementation, but all components are standard Python
libraries.

**Scalability:** Good — S&P 500 names are liquid; 10-day rebalancing is manageable. The
ML filter concentrates capital in a small subset of positions.

**Automation feasibility:** High in principle, but requires: (a) S&P 500 historical
constituent database (to avoid survivorship bias), (b) a live or historical price feed,
(c) replication of the ML classifier (features unknown), (d) borrow cost tracking for
the short side.

## MQL5 / Python / Pine Feasibility

**Python:** Best fit. Required libraries: `networkx` or `igraph` (graph clustering),
`statsmodels` (cointegration tests), `scikit-learn` (ML classifiers), `scipy` (Kelly
criterion). S&P 500 historical constituent data: available via commercial databases
(Compustat) or partially reconstructed from ETF constituent history. Implementation
is substantial but tractable.

**MQL5:** Not directly applicable (MetaTrader lacks built-in graph algorithms and the
multi-symbol OMS needed for pairs).

**Pine Script:** Not applicable for pairs trading.

**Verdict:** Python is the only viable implementation environment. The specific classifier
features are the key unknown that prevents immediate implementation from this paper alone.

## Similar Strategies

- `attention-factors-statistical-arbitrage` (score 66) — also US equities stat arb, but
  factor-based (not pairs-based); joint factor learning; daily; 500 large-caps.
  Both are in the "machine-learning statistical arbitrage" bucket with complementary approaches.
- Cross-link both strategies as related concepts.

## Red Flags

- `METRICS NOT REPORTED`: Sharpe, max DD, CAGR all inaccessible — critical gap for evaluation
- `ML OPACITY`: Classifier features, training methodology, OOS accuracy not documented
- `ATYPICAL CLAIM`: "All configurations outperformed benchmark" — unusual; may reflect narrow comparison set
- `LARGE PARAMETER SPACE`: 18 result tables across multiple strategy variants = potential data-mining
- `NO PEER REVIEW`: Working paper / arXiv preprint only

## Source Links

| Source | Retrieved | Notes |
|--------|-----------|-------|
| arXiv:2406.10695 | 2026-05-29 | HTTP 403 for all access attempts; key details from search summaries |
| SSRN:4849095 | 2026-05-29 | HTTP 403 |
| WNE Working Paper 2024-09 (ideas.repec.org) | 2026-05-29 | HTTP 403 |
| NASA/ADS: 2024arXiv240610695K | 2026-05-29 | HTTP 403 |

## Analyst Notes

**FACTS (from search result snippets):**
- Authors: Adam Korniejczuk and Robert Ślepaczuk (Warsaw University, WNE/Faculty of Economic Sciences)
- arXiv:2406.10695, June 15, 2024; SSRN:4849095; WNE Working Paper 2024-09
- Data: Daily S&P 500 historical constituents, Jan 2000 – Dec 2022 (Yahoo Finance, survivorship-bias-corrected)
- OOS test: March 2006 – December 2022 (~17 years)
- Strategy: Graph clustering + ensemble ML classifier (top 10%) + Kelly + time-variant TP/SL
- Rebalancing: Every 10 days
- Key result: IR statistically significantly > SPY at 1% significance level (probabilistic t-test)
- Paper: 33 pages, 18 tables, 16 figures
- Presented at QFRG and DSLab Seminar (Warsaw)

**ANALYSIS:**
- Robert Ślepaczuk has an active research program in quantitative finance at Warsaw. His
  prior work on systematic trading strategies has been published in peer-reviewed journals,
  which slightly increases credibility relative to a first-time arXiv submission.
- The 17-year OOS including 2008 is the second-longest OOS period in the database (after
  the `regime-switching-jump-model-equity` paper's 1990–2023 OOS, score 68). This is a
  genuine methodological strength.
- The "top 10% filter" design implies significant selectivity: if 500 stocks form ~12,500
  unique pairs, and each 10-day window has 1,000+ candidate signals, the top 10% represents
  ~100+ active pair positions. This is a diversified portfolio, not a concentrated bet.

**ASSUMPTIONS:**
- Graph clustering likely uses a Louvain, K-means on correlation-derived distance, or
  spectral clustering algorithm (standard choices for this type of problem)
- ML features likely include spread z-score, pair velocity, pair volatility, market regime
  indicators
- The "all outperformed" claim likely refers to the set of configurations presented in the
  paper's tables, not literally every configuration the authors tried

## Future Research Needed

1. Full paper access to extract Sharpe, max drawdown, CAGR, and the full distribution of
   results across the 18 tested configurations.
2. Identify what ML classifier features are used — this determines whether the filter is
   truly predictive or is fitting in-sample noise.
3. Independent replication on a non-US equity market (e.g., Eurostoxx 50, Nikkei 225) to
   test generalizability.
4. Test the strategy's performance in 2023–2024 (the paper's OOS ends December 2022).
5. Await journal peer review (Ślepaczuk's group typically submits to SSRN and publishes in
   journals like Quantitative Finance, Journal of Asset Management, or Finance Research Letters).
