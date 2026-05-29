# Statistical Jump Model Regime-Switching Equity Strategy

## Overview

A binary market-timing overlay applied to major equity indices. A Statistical Jump Model (JM)
classifies each trading day into one of two regimes (bull/bear) using features derived solely
from the index's own return series. The portfolio holds 100% equity exposure in bull regimes
and 0% (cash) in bear regimes. Published in Journal of Asset Management (peer-reviewed, 2024)
by Yizhan Shu, Chenyu Yu, and John M. Mulvey (Princeton). Open-source Python implementation
available.

---

## Asset Class / Timeframes

- **Markets:** Major equity indices — US (S&P 500, Nasdaq-100), Germany (DAX), Japan (Nikkei/TOPIX)
- **Timeframe:** Daily regime identification; position changes occur at close with 1-day delay
- **Style:** Position (regime holds days to months; not intraday)
- **In scope:** NAS100, US30, major indices ✓

---

## Core Logic

1. Compute three features from the equity index's daily excess return series: return-based
   and risk-based measures (exact formula set not accessible; described as "return and risk
   measures derived from the return series").
2. Fit a Statistical Jump Model (JM) on a historical window. The JM is an unsupervised
   clustering algorithm that assigns each day to a bull or bear regime. Unlike a hidden Markov
   model, the JM imposes a jump penalty λ on state transitions, which discourages high-frequency
   regime flipping and enforces realistic persistence in regime assignments.
3. Select λ via time-series cross-validation that directly optimizes the downstream portfolio
   Sharpe ratio (not held-out likelihood).
4. Apply the regime signal with a mandatory 1-day trading delay to prevent look-ahead bias.
5. 0/1 portfolio allocation: full long position in bull regime; flat (cash) in bear regime.
   No leverage, no short.
6. Transaction costs are included in performance evaluation.

---

## Economic Rationale

**Why an edge could exist:** Equity markets exhibit macro-driven persistent regimes tied to
business cycles, credit conditions, central bank policy, and risk appetite. Bull regimes
feature sustained demand from institutional accumulation, positive earnings revisions, and
low credit spreads. Bear regimes feature systematic selling, deteriorating fundamentals, and
elevated correlation to risky assets. If these regime transitions are detectable with a lead
sufficient to outweigh round-trip costs, a binary timing strategy captures most of the
equity risk premium while avoiding large drawdowns.

**When the edge should disappear (falsifiability):**
- During V-shaped recoveries (e.g., March 2020 COVID bounce): model is slow to re-enter after
  a bear signal, missing the initial leg of the recovery.
- In QE-dominated "buy-the-dip" environments where bear regimes are repeatedly short-lived and
  rapidly reversed by central bank intervention — transition costs compound and returns are lost.
- When volatility and return features become decorrelated from macro state (low-volatility,
  directionless markets with no regime separation).
- When regime transitions are more frequent than the jump penalty accommodates, producing
  excessive false signals and round-trip costs.

The paper explicitly focuses on regime *persistence* enhancement (the jump penalty), which
implicitly acknowledges this is the core failure mode of simpler HMM approaches.

---

## Entry Conditions

- Open (or maintain) 100% long equity index position when JM predicts bull regime
- Apply signal with 1-day delay (trade at next day's close)
- No intraday entry; signal evaluated at close

---

## Exit Conditions

- Close (or reduce to 0%) long position when JM predicts bear regime
- Apply signal with 1-day delay
- No stop-loss or drawdown-based forced exit outside the regime signal

---

## Risk Management

**Defined:** Binary 0/1 allocation; 1-day execution delay to reduce whipsaw risk.

**NOT defined:** No explicit stop-loss, no intraday risk management, no max-drawdown circuit
breaker, no position sizing gradation (partial exposures not used), no leverage limit beyond
the 1× implicit cap.

**Weakness:** Gap risk on the day of regime transition is unprotected — if a bear regime
triggers on a day when the index gaps down 5%, the full intraday loss is incurred. The
strategy exits at end-of-day, not at the signal time.

**Assessment:** Risk management is structurally minimal — only the regime filter itself reduces
risk. No explicit protective mechanisms beyond "go to cash when regime is bad."

---

## Indicators Used

- Equity index daily return series (OHLCV daily data from public sources, e.g., Yahoo Finance)
- 3 derived features: return-based + risk-based measures (exact formulas NOT REPORTED in
  accessible sources; available in the full paper and inferred from open code)
- Jump penalty λ (hyperparameter, tuned via time-series CV)

---

## Transaction Costs & Capacity

- **Included:** Yes — the paper explicitly states performance is evaluated "in the presence of
  transaction costs and trading delays."
- **Specific cost assumptions:** NOT REPORTED in accessible sources.
- **Trading delay:** 1-day (reduces costs and prevents look-ahead bias).
- **Capacity:** Equity indices are highly liquid. An ETF overlay (SPY, QQQ, EWG, EWJ) would
  incur negligible market impact at retail or small-institutional scale. No capacity concerns
  at typical managed account sizes.
- **Assessment:** Cost treatment is methodologically sound. The 1-day delay and sparse trading
  (regime changes are infrequent) keep round-trip costs low. Passes cost realism check.

---

## Backtest Evidence

- **Status:** CLAIMED — published in peer-reviewed Journal of Asset Management (paywalled);
  open-source code available for independent replication.
- **Period:** OOS evaluation from 1990 to 2023 (US, Germany, Japan).
- **Method:** Online prediction (rolling historical fit, predict forward, apply with 1-day delay).
- **OOS discipline:** Yes — online regime prediction is structurally OOS; λ selected via
  time-series CV (not look-ahead).
- **Note:** Specific numerical results (Sharpe, CAGR, max drawdown by market) are NOT
  REPORTED in accessible sources. All performance claims below are from paper abstract/search
  summaries only.

---

## Forward-Test Evidence

NOT REPORTED. No live trading results available in accessible sources.

---

## Reported Metrics

| Metric | Value | Source | Status |
|--------|-------|---------|--------|
| Sharpe ratio (JM vs buy-and-hold) | Higher (margin NOT REPORTED) | Abstract/summary | CLAIMED, UNVERIFIED |
| Maximum drawdown | Reduced vs buy-and-hold (magnitude NOT REPORTED) | Abstract/summary | CLAIMED, UNVERIFIED |
| Volatility | Reduced vs buy-and-hold (magnitude NOT REPORTED) | Abstract/summary | CLAIMED, UNVERIFIED |
| CAGR | NOT REPORTED | — | NOT REPORTED |
| vs HMM strategy | Consistent outperformance | Abstract/summary | CLAIMED, UNVERIFIED |

---

## Community Sentiment

- The paper is published in Journal of Asset Management (Springer, Q2 Finance).
- The open-source library `jumpmodels` on PyPI has active maintenance.
- A follow-up paper (arxiv 2410.14841 "Dynamic Factor Allocation Leveraging Regime-Switching
  Signals") builds on this framework, extending it to factor portfolios — indicating peer
  acceptance and methodological trust.
- A separate MDPI Mathematics paper (2025) extends the framework with Model Predictive Control.
- No critical community reviews found in accessible sources (absence may reflect paywall on
  full paper rather than lack of engagement).

---

## Why This Might Be Nothing

### 1. Most likely benign explanation: survivorship/hindsight in regime selection
The 1990–2023 OOS period covers two known catastrophic bear markets (2001-2003, 2008-2009)
that *any* regime model trained on volatility features will identify. The jump penalty λ is
tuned to maximize Sharpe — if this tuning was sensitive to these specific historical episodes,
the results are inflated. A model optimized on a period containing the dot-com crash and the
GFC is not reliably predictive of future, differently-structured crises.

### 2. The cost case
Market timing costs are real but low here — the strategy trades infrequently (regime
persistence is enforced) and equity index ETFs have tight spreads. This is less likely to be
the fatal flaw than for high-frequency strategies.

### 3. Regime dependence
The strategy is implicitly *short volatility bursts* in the sense that rapid recoveries (V-shapes)
hurt it. The 2020 COVID crash and recovery is the canonical failure case: a model that exits in
March 2020 and re-enters in May 2020 captures the full drawdown and misses most of the bounce.
The strategy has a structural lag of at least 1 day (by design) plus the regime change latency.

### 4. Sample / overfitting
The jump penalty λ is cross-validated on the full OOS period (1990-2023). The "time-series CV"
method avoids data leakage within each CV fold, but the final λ chosen was tested on data that
includes 2008-2009 and 2001-2003. This is a mild but real overfitting risk. A model trained
exclusively on data from before 2001 and tested forward would provide cleaner evidence.

### 5. What would change my mind
An independent replication on a held-out period (e.g., 2024-present) or on markets outside
the three tested (emerging markets, sector indices) with the same λ would substantially
increase confidence. The paper's own code enables this — that is the key advantage of
open-source methodology.

---

## Strengths / Weaknesses

**Strengths:**
- Peer-reviewed in a reputable journal (not pre-print only)
- Open-source code (Python/GitHub/PyPI) enables independent replication
- OOS evaluation spanning 33 years and three markets
- Transaction costs and execution delay explicitly modeled
- Statistical Jump Model is a genuine methodological improvement over HMM (better regime
  persistence; penalty selected by CV)
- Highly liquid markets (equity indices, ETF implementable)

**Weaknesses:**
- Binary 0/1 allocation is naive risk management — no intermediate hedging, stop-loss, or
  partial exposure
- Specific performance numbers NOT accessible (paywalled)
- No forward test or live trading validation
- λ cross-validation on the full OOS set creates mild overfitting risk
- V-shaped recovery failure mode is significant and acknowledged implicitly
- Strategy catches large drawdowns (2008, 2020) but likely underperforms in choppy or
  QE-recovery environments (2012-2015, 2020-2021)

---

## Confidence Score

### Per-dimension scoring

| Dimension | Wt | Score | Contribution | Rationale |
|---|---|---|---|---|
| Logical / economic soundness (falsifiable) | 3 | 7 | 21.0 | Macro regime rationale is plausible; failure conditions well-specified (V-shapes, rapid reversals, QE-driven recoveries) |
| Transparency & reproducibility | 2 | 9 | 18.0 | Open-source GitHub + PyPI + example notebook for Nasdaq-100; scikit-learn API |
| Evidence auditability | 2 | 8 | 16.0 | Peer-reviewed publication (Journal of Asset Management, 2024); open code enables replication |
| Risk management quality | 2 | 4 | 8.0 | Binary 0/1 only; no stop-loss, no partial sizing, no drawdown circuit breaker |
| Cost & capacity realism | 2 | 7 | 14.0 | Costs explicitly included; 1-day delay; liquid equity index ETFs; minimal capacity concern |
| Honest treatment of drawdowns / failure | 1.5 | 6 | 9.0 | Paper explicitly targets downside risk reduction; V-shape failure acknowledged implicitly |
| Robustness evidence (OOS / walk-forward / multi-market) | 1.5 | 7 | 10.5 | Three markets; 33-year OOS period; multi-asset extension to 12 assets in companion paper |
| Survived independent scrutiny | 1 | 6 | 6.0 | Peer-reviewed; follow-up papers build on the framework; active open-source library |

**Weighted sum:** 21.0 + 18.0 + 16.0 + 8.0 + 14.0 + 9.0 + 10.5 + 6.0 = **102.5**
**Max possible:** 150
**latent_score:** round(102.5 / 150 × 100) = **68**

**Verification cap check:**
- Evidence auditability = 8 → no cap applies (peer-reviewed + open code)
- **confidence = 68** (Worth Researching, 60–74)

*latent 68 (no cap; peer-reviewed + open code; specific result numbers behind paywall)*

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Low-moderate. The JM algorithm is well-implemented in `jumpmodels` (PyPI).
  Features are derived from daily index prices (public data). Online prediction is
  structurally simple.
- **Scalability:** High. Works on any liquid equity index. Can be extended to a multi-asset
  portfolio (companion paper demonstrates this).
- **Automation:** Feasible. Daily data update → compute features → run `JumpModel.predict_online()`
  → execute 0/1 allocation at close. Straightforward cron job + broker API integration.

---

## MQL5 / Python / Pine Feasibility

- **Python:** HIGH. `jumpmodels` library is directly usable. `pip install jumpmodels`.
  Example notebook for Nasdaq-100 already exists. Requires `pandas`, `numpy`, `scikit-learn`
  ecosystem. Full implementation feasible in <200 lines.
- **MQL5:** LOW. The JM algorithm involves matrix operations and iterative optimization
  not natively available in MQL5. Could port the regime *signal* (pre-computed offline)
  to MT5 as a lookup, but the model must be trained and updated externally.
- **Pine Script:** NOT FEASIBLE. Pine Script cannot implement iterative optimization
  algorithms required for jump penalty tuning.

---

## Similar Strategies

- `intraday-momentum-spy` (score 50) — also US equity, also in-scope for NAS100/SPY
  but very different approach (intraday momentum vs daily regime detection)
- `orb-stocks-in-play` (score 78) — US equities, different style (intraday, single stock vs
  index-level position)
- Related paper: arxiv 2410.14841 "Dynamic Factor Allocation Leveraging Regime-Switching
  Signals" — factor-level extension of the same JM framework

---

## Red Flags

- Specific performance numbers NOT accessible (paywall); strategy rests on peer review trust
- Binary 0/1 is structurally naive risk management for a claimed "downside risk reduction" strategy
- λ hyperparameter was optimized on the same OOS period used for performance evaluation —
  mild but real circularity
- V-shaped recovery failure mode (2020 is a test case where the strategy likely fared poorly
  in the initial recovery leg)
- No live trading or forward-test evidence

---

## Source Links

| URL | Accessed | Notes |
|-----|----------|-------|
| https://arxiv.org/abs/2402.05272 | 2026-05-29 | Pre-print (HTTP 403) |
| https://link.springer.com/article/10.1057/s41260-024-00376-x | 2026-05-29 | Journal of Asset Management (HTTP 403 — paywalled) |
| https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4719989 | 2026-05-29 | SSRN working paper (HTTP 403) |
| https://github.com/Yizhan-Oliver-Shu/jump-models | 2026-05-29 | Open-source Python library — ACCESSIBLE |
| https://pypi.org/project/jumpmodels/ | 2026-05-29 | PyPI package — ACCESSIBLE |
| https://ideas.repec.org/p/arx/papers/2402.05272.html | 2026-05-29 | IDEAS.repec (HTTP 403) |

All primary sources (arxiv, SSRN, Springer) returned HTTP 403. Evidence gathered from:
search engine snippets, GitHub README (accessible), PyPI page (accessible).

---

## Analyst Notes

**FACTS (from accessible sources):**
- Published in Journal of Asset Management, Vol 25, No. 5, 2024, pp. 493–507
  (doi: 10.1057/s41260-024-00376-x)
- Authors: Yizhan Shu, Chenyu Yu, John M. Mulvey (Princeton University)
- OOS evaluation on US, Germany, Japan equity indices from 1990 to 2023
- Transaction costs and 1-day trading delay included
- Open-source code: GitHub `Yizhan-Oliver-Shu/jump-models`, PyPI `jumpmodels`
- Nasdaq-100 example notebook exists at `examples/Nasdaq/example.ipynb`

**ANALYSIS (my reasoning):**
- Peer-review in a reasonable journal is meaningful evidence that methodology is sound.
- The jump penalty improving on HMM is plausible: HMMs flip regimes too frequently in
  noisy markets, which is exactly the problem the JM addresses.
- The 0/1 binary allocation is a deliberate simplification to isolate regime detection from
  portfolio construction — a reasonable choice for a methodology paper, but naive for live trading.
- The open code substantially increases the research value: one could run the example
  notebook on recent data (2024+) and observe whether the regime signals still work.

**ASSUMPTIONS (not confirmed):**
- ASSUMED the peer-reviewed paper contains the full methodology; specific feature formulas
  not verified by me.
- ASSUMED the OOS evaluation uses the rolling/online approach implied by the abstract
  language; exact cross-validation structure not verified.
- ASSUMED the example notebook uses the same methodology as the published paper.

---

## Future Research Needed

1. **Run the open-source code:** Execute `examples/Nasdaq/example.ipynb` on recent 2023-2026
   data to verify the regime signals still function and assess post-publication performance.
2. **Access the full paper:** Retrieve specific Sharpe ratio, CAGR, and max drawdown numbers
   for each market tested — these are behind the Springer paywall.
3. **Partial exposure extension:** Evaluate a continuous (0–100%) allocation based on regime
   probability (from `.predict_proba()`) instead of binary 0/1 — the code supports this.
4. **Apply to NAS100 / US30:** The example is Nasdaq-100 which directly maps to NAS100. Test
   the strategy on the full 2000–2026 period to capture the dot-com crash and 2020 recovery.
5. **Independent replication:** Run the exact strategy on the 2024+ out-of-sample period (post
   the paper's evaluation window) to assess live-time performance.
