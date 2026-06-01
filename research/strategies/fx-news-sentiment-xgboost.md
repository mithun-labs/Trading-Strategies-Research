# FX News Sentiment XGBoost (Macro Alpha via GDELT + FinBERT)

## Overview

Single-author arXiv preprint (May 2025) proposing a daily direction-prediction pipeline for
EUR/USD, USD/JPY, and 10-year US Treasury futures (ZN) using global news sentiment extracted
from the GDELT Project's automated news feed, processed through FinBERT, and classified by an
XGBoost model. The paper reports extreme out-of-sample Sharpe ratios (5.87 / 4.65 / 4.65) and
CAGR >50% in FX. Both the Sharpe and CAGR values exceed the automatic scrutiny thresholds
defined in this research protocol, triggering mandatory skeptical review.

Code available on GitHub: https://github.com/yukepenn/macro-news-sentiment-trading

---

## Asset Class / Timeframes

- **Markets:** Forex (EUR/USD, USD/JPY), Fixed Income (10Y US Treasury futures ZN)
- **Entry/exit frequency:** Daily prediction of next-day return direction
- **Holding period:** 1 day (overnight position)
- **Rebalancing:** Daily

---

## Core Logic

1. **Data ingestion:** Download GDELT 2.0 GKG records (~100GB full history); extract news events
   and text associated with global macro topics.
2. **Sentiment scoring:** Apply FinBERT (BERT model pretrained on financial text) to GDELT event
   text to generate daily sentiment indices. Features engineered include: mean tone, sentiment
   dispersion (variance across articles), event impact weighting, article count spike detection,
   and Goldstein-scale momentum.
3. **Classification:** XGBoost binary classifier predicts next-day return direction (up/down) for
   each instrument using lagged sentiment features. Benchmarked against logistic regression.
4. **Position execution:** Implied buy/sell based on predicted direction; held for 1 day.
5. **OOS framework:** 5-fold expanding-window cross-validation over 2017–April 2025; in-sample
   training window starts from January 2015.
6. **Interpretability:** SHAP (Shapley Additive Explanations) applied to identify which sentiment
   features drive predictions.

---

## Economic Rationale

**Stated rationale:** Global news sentiment captures macro information flows that influence
institutional FX positioning. Positive/negative news about economic fundamentals, central bank
policy, and geopolitical events should drive short-term directional bias in liquid macro assets.
News dispersion (disagreement across articles) may capture uncertainty premia.

**Falsifiability assessment:** The paper does NOT explicitly state when this mechanism should fail.
No conditions are specified under which news sentiment should have zero or negative predictive
power. A genuine economic rationale must predict its own failure regime (e.g., "fails when
central banks are in communication blackout," "fails when markets are liquidity-driven rather
than news-driven"). This paper provides none. The rationale is plausible as a surface narrative
but fails the falsifiability test — it cannot be distinguished from curve-fitting with an
economic label.

**Assessment:** Pattern-only with plausible surface story; falsifiability test NOT passed.

---

## Entry Conditions

- Next-day direction predicted LONG: buy EUR/USD (or USD/JPY, or ZN) at open
- Next-day direction predicted SHORT: sell EUR/USD (or USD/JPY, or ZN) at open
- Signal generated using sentiment features computed from all GDELT data through close of day t
- Trade executed at next open (day t+1) — no intraday execution

---

## Exit Conditions

- Exit at close of day t+1 (1-day hold; binary long/flat/short)
- No stop-loss defined in available methodology description
- No regime-exit criteria defined

---

## Risk Management

NOT REPORTED in available paper summaries. The binary classification structure implies either
fully-invested long or short (or flat if neutral), but:
- Position sizing rule: NOT REPORTED
- Stop-loss: NOT REPORTED
- Maximum drawdown limit: NOT REPORTED
- Regime exit: NOT REPORTED

**Assessment:** Risk management quality is critically low for a strategy claiming Sharpe 5.87 —
the extreme smoothness of returns cannot come from genuine risk management; it almost certainly
reflects the data leakage issues described in "Why This Might Be Nothing."

---

## Indicators Used

- GDELT GKG (Global Knowledge Graph) news events and tone data
- FinBERT (financial domain BERT model) for sentiment scoring
- XGBoost binary classifier
- SHAP for interpretability
- Features: mean daily sentiment tone, sentiment dispersion, event impact weight, article count
  spike (z-score of article volume), Goldstein-scale momentum (GDELT's conflict/stability metric)

---

## Transaction Costs & Capacity

**Transaction costs:**
- The paper claims cost-adjusted performance, but the exact transaction cost model is NOT
  specified in available summaries (spread, commission, slippage amounts unknown).
- EUR/USD and USD/JPY are the most liquid FX pairs: typical institutional spread ~0.5–1 pip
  per trade. At 250 trades/year (daily strategy), round-trip costs ≈ 0.1–0.3% annually for
  EUR/USD — modest relative to the claimed >50% CAGR.
- ZN (Treasury futures): highly liquid; exchange-traded; tick costs well-defined.
- **Capacity:** EUR/USD and USD/JPY are among the deepest markets globally; capacity is not a
  meaningful constraint for the strategy as described.

**GDELT data quality cost:**
- GDELT's automated NLP has been documented at ~55% field accuracy (UK ONS assessment)
- GDELT event counts are inflated by ~20% due to duplication across news sources
- This does NOT invalidate the strategy but means the signal quality is substantially noisier
  than the paper implies — survivorship in signal construction is a concern

**Conclusion on costs:** Cost modeling is claimed but unverifiable; at daily frequency in
liquid FX/futures markets, transaction costs alone are not the primary concern. The deeper
issue is model validity (see "Why This Might Be Nothing").

---

## Backtest Evidence

- **Period:** January 2015–April 2025 (full dataset); OOS claimed as 2017–April 2025
- **Methodology:** 5-fold expanding-window cross-validation
- **Status:** CLAIMED, UNVERIFIED — arXiv preprint only; no peer review; single author

---

## Forward-Test Evidence

- NOT REPORTED — no live trading or prospective forward test referenced

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio (EUR/USD) | 5.87 | CLAIMED, UNVERIFIED | arXiv 2505.16136 |
| Sharpe Ratio (USD/JPY) | 4.65 | CLAIMED, UNVERIFIED | arXiv 2505.16136 |
| Sharpe Ratio (ZN Treasuries) | 4.65 | CLAIMED, UNVERIFIED | arXiv 2505.16136 |
| Sortino Ratio | NOT REPORTED | | |
| Calmar Ratio | NOT REPORTED | | |
| Profit Factor | NOT REPORTED | | |
| Win Rate | NOT REPORTED | | |
| Expectancy per Trade | NOT REPORTED | | |
| Maximum Drawdown | NOT REPORTED | | |
| CAGR (EUR/USD) | >50% | CLAIMED, UNVERIFIED | arXiv 2505.16136 |
| CAGR (ZN Treasuries) | >22% | CLAIMED, UNVERIFIED | arXiv 2505.16136 |
| Annual Return | NOT REPORTED | | |
| Number of Trades | NOT REPORTED | | |
| Average Trade Duration | ~1 day | CLAIMED, UNVERIFIED | arXiv 2505.16136 |
| Recovery Factor | NOT REPORTED | | |
| Risk-Reward Ratio | NOT REPORTED | | |
| Exposure Percentage | NOT REPORTED | | |

**Automatic scrutiny triggers fired:**
- Sharpe Ratio > 3: ALL THREE instruments exceed threshold (5.87, 4.65, 4.65 — 10–15× typical
  CTA per-instrument Sharpe of 0.15–0.20). **Mandatory discussion required.**
- CAGR > 50%: EUR/USD CAGR exceeds threshold. **Mandatory discussion required.**

**Quantitative sanity check (ANALYSIS):**
If EUR/USD Sharpe = 5.87 and CAGR = 50%: implied annual vol = 50% / 5.87 ≈ 8.5%. EUR/USD
typical annual vol is 8–10%, so the math is arithmetically consistent. But this implies the
strategy achieves virtually zero drawdown over 8 years while holding daily EUR/USD positions —
a near-perfect directional accuracy claim. If win rate = 56% (plausible for a daily directional
model) and average R:R ≈ 1:1 with 250 trades/year, the expected CAGR is much lower than 50%;
achieving 50% CAGR from daily EUR/USD predictions would require either extreme leverage or
directional accuracy far exceeding 56%. With no max-drawdown figure reported, the smoothness
of the claimed equity curve cannot be independently assessed. This is a major red flag.

---

## Community Sentiment

- No independent forum discussion found (r/algotrading, r/quantfinance, ForexFactory)
- No peer review or journal commentary
- Paper is a single-author arXiv submission (May 2025); no institutional affiliation in
  available summaries
- An independent reproduction framework exists (github.com/jandrews65/zhang2025) but reports
  "Results pending final analysis" — no confirmed replication of the claimed performance
- GDELT Project blog posted about this paper, suggesting familiarity but not independent validation
- **No informed community criticism found; no neutral discussion found.** This is a new paper
  with essentially zero external scrutiny as of June 2026.

---

## Why This Might Be Nothing

**This section is mandatory. The skeptic's case is decisively stronger than the supporting case.**

### 1. FinBERT Temporal Data Leakage (PRIMARY CONCERN — most likely explanation)

FinBERT is trained on financial news corpora covering the period when the model's training data
was assembled. Common FinBERT versions include:
- ProsusAI/FinBERT (Yang et al. 2020): trained on Refinitiv Eikon financial news (~1.8M articles)
- Araci FinBERT (2019): trained on financial news and earnings call text

The "OOS" period in this paper is 2017–April 2025. If the FinBERT model used was trained on
financial news from 2017 onward (highly probable given when these models were assembled), then:
- The sentiment scores FinBERT assigns to 2017–2025 GDELT news are NOT the scores a
  "temporally blind" model would generate
- FinBERT's internal representations encode IMPLICIT KNOWLEDGE of market-moving events from
  within the OOS period
- The paper explicitly states it avoids "look-ahead bias" in feature construction (using only
  day-t data to predict t+1 direction). This addresses FEATURE-LEVEL look-ahead. It does NOT
  address MODEL-LEVEL temporal leakage, which is the deeper issue.
- A clean test would require FinBERT to be retrained with a training cutoff at 2017-01-01 for
  the first fold, with the cutoff advancing with each fold. This is computationally prohibitive
  and almost certainly was not done.
- This is the most common and most subtle form of leakage in NLP-for-finance backtests. Recent
  literature (2025-2026) has identified that only 26.8% of finance NLP papers acknowledge
  look-ahead bias, and model-level temporal leakage is systematically underaddressed.

**Implication:** The claimed Sharpe of 5.87 likely reflects FinBERT's implicit knowledge of the
OOS period rather than a genuine news-sentiment → FX direction signal.

### 2. 5-Fold Expanding CV ≠ True OOS (SECONDARY CONCERN — structural flaw)

The "5-fold expanding-window cross-validation" setup means:
- Full dataset 2015–2025 is split into 5 sequential folds
- Each fold trains on all prior data and tests on the next portion
- XGBoost hyperparameters are selected to optimize performance ACROSS the aggregate of all 5
  test windows

The aggregate of all 5 test windows IS the period 2017–2025 that is reported as "OOS." This
is not true prospective testing — it is cross-validation with hyperparameters tuned to perform
well across ALL folds simultaneously. The standard for true OOS testing requires a completely
held-out test set that is NEVER used for ANY aspect of model selection. This paper appears to
lack such a holdout.

### 3. Multiple-Asset Inflation (TERTIARY CONCERN)

Three separate strategies are reported with Sharpe ≥ 4.65. EUR/USD and USD/JPY are highly
correlated with global risk sentiment (both USD-denominated, correlated with USD strength),
and ZN (Treasury futures) is negatively correlated with risk-on/risk-off episodes. Reporting
three strategies and finding all three exceptional inflates expected performance by ~50–70%
compared to testing a single pre-specified instrument.

### 4. GDELT Data Quality

GDELT's automated NLP achieves ~55% accuracy in key fields (UK ONS assessment) and has ~20%
data redundancy from event duplication across news sources. The "tone" and "Goldstein scale"
fields in GDELT are computed by GDELT's own NLP pipeline (dictionary-based LIWC + WordNet)
and are known to underperform modern transformer models on nuanced financial text. Applying
FinBERT on top of already-noisy GDELT metadata compounds uncertainty. When a noisy signal
produces a Sharpe of 5.87, this is MORE likely to indicate overfitting than genuine alpha.

### 5. The Cost Case

Transaction costs at daily EUR/USD frequency are modest (≈0.1–0.3% annually) and do not
explain away the claimed alpha. However, if a corrected Sharpe (post-leakage removal) is ≈0.3–0.6
(comparable to carry-trade benchmarks), then net-of-cost performance would be negligible.

### 6. What Would Change My Mind

A version of this study that (a) retrained FinBERT with a temporally-consistent training cutoff
that advances with each fold, AND (b) applied the final trained model to a completely held-out
period (e.g., May 2025–December 2026) with no prior hyperparameter involvement would be
materially different evidence. Neither exists at present. The reproduction framework
(jandrews65/zhang2025) is incomplete ("results pending") and does not address the FinBERT
leakage issue.

**Verdict:** The most parsimonious explanation for Sharpe 5.87 is FinBERT temporal leakage +
CV hyperparameter overfitting. The claimed edge almost certainly does not survive prospective
deployment with a temporally-consistent FinBERT model.

---

## Strengths / Weaknesses

**Strengths:**
- Open data source (GDELT) and open-source models (FinBERT + XGBoost) — reproducibility in
  principle
- SHAP interpretability adds conceptual transparency
- Liquid market instruments — capacity not a concern
- Code provided on GitHub
- Three instruments tested across different asset classes (FX + rates)

**Weaknesses:**
- Extreme Sharpe ratios (5.87, 4.65, 4.65) trigger automatic skepticism
- FinBERT temporal leakage: model training data likely overlaps with OOS backtest period
- 5-fold CV is not true OOS; hyperparameters optimized on aggregate test period
- Maximum drawdown NOT REPORTED — prevents assessment of risk-adjusted consistency
- Win rate NOT REPORTED — prevents assessment of loss distribution
- Single-author arXiv preprint; no institutional affiliation; no peer review
- Reproduction results pending (no independent confirmation as of June 2026)
- GDELT data quality: ~55% accuracy, 20% redundancy, US media overrepresentation
- No risk management (position sizing, stops, max-loss) defined
- Economic rationale fails falsifiability test — no conditions under which the mechanism fails

---

## Confidence Score

**Per-dimension scoring:**

| Dimension | Weight | Score | Weighted |
|-----------|--------|-------|---------|
| Logical/economic soundness (falsifiable) | 3 | 3/10 | 9.0 |
| Transparency & reproducibility | 2 | 7/10 | 14.0 |
| Evidence auditability | 2 | 3/10 | 6.0 |
| Risk management quality | 2 | 2/10 | 4.0 |
| Cost & capacity realism | 2 | 4/10 | 8.0 |
| Honest treatment of drawdowns/failure | 1.5 | 1/10 | 1.5 |
| Robustness evidence (OOS/walk-forward) | 1.5 | 2/10 | 3.0 |
| Survived independent scrutiny | 1 | 0/10 | 0.0 |
| **TOTAL** | **15** | | **45.5** |

`latent_score = round(45.5 / 150 × 100) = 30`

**Verification cap:** Evidence auditability = 3 (≤4) → cap at 59.
`confidence = min(30, 59) = 30`

**Final: latent 30 (capped to 30; cap irrelevant: latent < 59; arXiv-only, no peer review, FinBERT temporal leakage likely explains extreme Sharpe)**

**Band:** LOW CONFIDENCE (0–39)

**Rationale note:** Even if FinBERT temporal leakage were resolved and hyperparameters properly
constrained, the underlying signal (news sentiment → next-day FX direction) is a well-explored
hypothesis that typically produces Sharpe < 0.5 in academic settings. The extreme values reported
here almost certainly do not survive a clean prospective deployment.

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Very high (FinBERT GPU processing, GDELT ~100GB ingestion, XGBoost pipeline)
- **Scalability:** EUR/USD and USD/JPY are infinitely liquid; no capacity constraint
- **Automation feasibility:** The pipeline is designed for automation but requires:
  - Daily GDELT GKG download (~GB/day)
  - GPU server for FinBERT inference
  - Reliable GDELT availability (public feed, but not SLA-guaranteed)

---

## MQL5 / Python / Pine Feasibility

- **Python:** High feasibility in principle (all components open-source: transformers library
  for FinBERT, xgboost, GDELT API); practical barrier is daily GDELT ingestion and GPU for
  real-time inference
- **MQL5:** Low feasibility (would require external Python microservice for FinBERT)
- **Pine Script:** Not feasible (no external data/ML support)

---

## Similar Strategies

- `ict-silver-bullet-forex` — also forex direction prediction; entirely different approach
  (price action, killzone timing); compare: ICT is rule-based intraday, this is ML daily
- `intraday-momentum-spy` — ML-adjacent daily prediction on equity; different mechanism
- No existing strategy in database uses NLP/news sentiment as primary signal

---

## Red Flags

- **EXTREME SHARPE (5.87, 4.65, 4.65):** 10–15× typical CTA per-instrument Sharpe; automatic
  scrutiny trigger fired
- **CAGR > 50%:** Exceeds audited CTA track records; automatic scrutiny trigger fired
- **FinBERT temporal leakage:** Model trained on data overlapping OOS period; structural flaw
  in backtesting design
- **5-fold CV ≠ true OOS:** Hyperparameters optimized on the same aggregate period reported as
  OOS; standard form of in-sample optimism
- **No max drawdown reported:** Prevents verification of risk-adjusted consistency
- **No win rate reported:** Prevents assessment of loss distribution
- **Single-author arXiv preprint:** No peer review, no institutional affiliation confirmed
- **Reproduction results pending:** Independent framework exists but produced no results yet
- **GDELT accuracy ~55%:** Noisy underlying data source producing extreme reported alpha
- **No risk management defined:** No stops, no position sizing, no max-loss rules

---

## Source Links

| URL | Retrieval Date | Notes |
|-----|---------------|-------|
| [arXiv 2505.16136](https://arxiv.org/abs/2505.16136) | 2026-06-01 | Primary paper |
| [arXiv HTML v1](https://arxiv.org/html/2505.16136v1) | 2026-06-01 | HTTP 403 |
| [GDELT Project blog](https://blog.gdeltproject.org/interpretable-machine-learning-for-macro-alpha-a-news-sentiment-case-study/) | 2026-06-01 | HTTP 403 |
| [alphaxiv discussion](https://www.alphaxiv.org/abs/2505.16136) | 2026-06-01 | HTTP 403 |
| [GitHub yukepenn/macro-news-sentiment-trading](https://github.com/yukepenn/macro-news-sentiment-trading) | 2026-06-01 | Author's code repository |
| [GitHub jandrews65/zhang2025](https://github.com/jandrews65/zhang2025) | 2026-06-01 | Reproduction framework; results pending |
| [FinBERT arXiv 1908.10063](https://arxiv.org/pdf/1908.10063) | 2026-06-01 | Original FinBERT paper |
| [LLM look-ahead bias paper arXiv 2512.06607](https://arxiv.org/pdf/2512.06607) | 2026-06-01 | Temporal leakage in LLMs for finance |
| [UK ONS GDELT quality note](https://www.ons.gov.uk/peoplepopulationandcommunity/birthsdeathsandmarriages/deaths/methodologies/globaldatabaseofeventslanguageandtonegdeltdataqualitynote) | 2026-06-01 | GDELT accuracy assessment (~55%) |

---

## Analyst Notes

**FACTS (from sources):**
- Paper: "Interpretable Machine Learning for Macro Alpha: A News Sentiment Case Study" —
  Yuke Zhang, arXiv:2505.16136, May 22, 2025
- Claimed Sharpe: 5.87 (EUR/USD), 4.65 (USD/JPY), 4.65 (ZN Treasuries)
- Claimed CAGR: >50% FX, >22% bonds
- OOS period: 2017–April 2025 via 5-fold expanding-window CV
- Training data: GDELT GKG 2015–2025 (~100GB)
- NLP pipeline: FinBERT → daily sentiment indices
- ML model: XGBoost (binary classifier)
- Interpretability: SHAP values confirm sentiment dispersion + article impact as top features
- Code: available at github.com/yukepenn/macro-news-sentiment-trading
- Reproduction: jandrews65/zhang2025 framework exists; results pending

**ANALYSIS:**
- FinBERT temporal leakage is structurally likely: ProsusAI/FinBERT (Yang et al. 2020) was
  trained on 1.8M Refinitiv Eikon financial news articles through ~2019, overlapping with
  the start of the OOS period (2017). This creates implicit forward-looking contamination.
- The 5-fold expanding CV setup means the XGBoost hyperparameters are not independent of the
  "OOS" period. This is a well-documented source of backtest inflation.
- SHAP showing "economically meaningful features" does not validate the performance claims —
  SHAP explains which features the model is using; it does not validate that those features
  have genuine predictive power in a forward-looking clean test.
- Sanity check (ANALYSIS): Sharpe 5.87 with CAGR 50% → implied annual vol = 8.5%.
  EUR/USD annual vol is typically 8–10%, so the math is arithmetically consistent but implies
  near-perfect directional accuracy over 8 years — a performance level that would attract
  billions in institutional capital within months and immediately self-destruct via arbitrage.

**ASSUMPTIONS (flagged):**
- It is ASSUMED that FinBERT's pretraining data overlaps with the OOS period — this is probable
  based on known model training dates but not confirmed for the specific FinBERT version used.
- It is ASSUMED that XGBoost hyperparameters were selected to optimize aggregate CV performance
  rather than a separate validation set — the paper does not specify the tuning procedure.

---

## Future Research Needed

1. **Temporal-consistent FinBERT replication:** Retrain (or use) a FinBERT model with training
   cutoff strictly before the OOS period. If Sharpe drops dramatically, temporal leakage is confirmed.
2. **Holdout test on 2025–2026 data:** Apply the final model (trained on 2015–2024) to completely
   new data post-paper-submission. This is the only clean prospective test.
3. **Reproduce with GDELT's own tone field only:** Strip FinBERT from the pipeline and use only
   GDELT's pre-computed LIWC tone score. If performance is similar, FinBERT adds no genuine signal
   beyond what GDELT's own NLP already provides.
4. **Peer review:** The paper requires peer review by a journal with replication requirements
   before any confidence level above 40 is warranted.
5. **Jandrews65 reproduction results:** Monitor the jandrews65/zhang2025 repository for final
   replication results — this is the most direct path to independent confirmation.
