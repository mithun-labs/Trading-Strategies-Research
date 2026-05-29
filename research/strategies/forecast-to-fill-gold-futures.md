# Forecast-to-Fill: Trend-Momentum Gold Futures

## Overview

This entry covers arxiv preprint 2511.08571 (November 2025) by Mainak Singha, Jose
Aguilera-Toste, and Vinayak Lahiri. The paper tests whether trend and momentum signals can
generate durable out-of-sample alpha in gold futures (GC), using a rigorous walk-forward
design. The reported metrics are **exceptional to the point of implausibility** — Sharpe 2.88,
max drawdown 0.52%, CAGR 43% over 10 years at 15% volatility target — and are flagged as
LOW CONFIDENCE. These figures exceed typical per-market trend-following Sharpe (industry:
0.15–0.20) by 14–19×, and the 0.52% max drawdown is statistically inconsistent with a 15%
annualized volatility target over a 10-year period.

**Classification: LOW CONFIDENCE / INSUFFICIENT VERIFICATION**

---

## Asset Class / Timeframes

- **Market:** Gold Futures (GC), referenced against spot XAUUSD
- **Data:** Daily OHLCV (implied from walk-forward description)
- **Backtest period:** 2015–2025 (2,793 trading days, ~10 years)
- **Walk-forward:** 10-year rolling training window; 6-month test window; monthly roll

---

## Core Logic

A two-component signal → position-sizing pipeline:

1. **Signal construction:** A "smoothed trend-momentum regime signal" — the paper combines
   trend direction (long-horizon price direction indicator) and momentum (medium-horizon
   momentum) into a single regime signal. The exact formulas and lookback windows are NOT
   REPORTED in accessible sources (all arxiv fetches returned HTTP 403).

2. **Position sizing (Forecast-to-Fill):** The signal is passed through:
   - **Volatility targeting:** Scale position to maintain 15% annualized volatility
   - **Fractional Kelly sizing:** λ = 0.40 (40% of full Kelly, to reduce sensitivity to estimation errors)
   - **Market impact adjustment:** Square-root impact model with γ = 0.02
   - **ATR-based exits:** Average True Range used for stop placement (parameters NOT REPORTED)
   - **Transaction cost model:** 0.7 basis points linear cost + square-root impact term

The "Forecast-to-Fill" framework is the signal-to-execution engineering layer — connecting
a transparent signal to an executable trade with explicit friction control.

---

## Economic Rationale

Gold trend-following has a well-established economic rationale grounded in decades of CTA
literature:
- Gold acts as a safe-haven asset: demand shocks (macro risk events) create persistent price
  trends due to delayed institutional repositioning
- Momentum in gold reflects gradual information diffusion and herding behavior among
  non-professional market participants
- CTA/managed futures programs in gold have historically generated positive (if modest) Sharpe
  ratios across long periods

**This rationale is sound.** The issue is not the economic logic but the claimed magnitude of
returns, which is not consistent with the established literature on per-market CTA performance.

---

## Entry Conditions

NOT FULLY REPORTED. Signal enters long (or short) when the smoothed trend-momentum regime
signal crosses a threshold. Exact construction of the trend and momentum components, their
lookback windows, smoothing method, and threshold values are NOT REPORTED in accessible sources.

---

## Exit Conditions

- **ATR-based trailing stop** (parameters NOT REPORTED)
- **Regime signal reversal** (implied by regime structure)
- **Volatility-driven position reduction:** When realized volatility exceeds target, position
  is scaled down automatically

---

## Risk Management

- **Volatility targeting:** 15% annualized target; positions scaled continuously
- **Fractional Kelly:** λ = 0.40 — limits max position to 40% of full Kelly
- **Market impact model:** Square-root impact adjustment prevents size that would move the market
- **ATR exits:** Adaptive stop placement based on recent realized range
- **Transaction costs:** 0.7 bps linear cost per trade

The framework description is methodologically coherent. However, the claimed 0.52% max drawdown
is inconsistent with a 15% volatility target: at 15% annual vol (≈0.945% daily), the expected
maximum drawdown over 2,793 trading days would be substantially above 0.52% under any standard
statistical model. This raises the possibility that "max drawdown" in this paper is measured
differently (perhaps per-position ATR stop depth, not portfolio equity peak-to-trough).

---

## Indicators Used

- Trend indicator (construction NOT REPORTED)
- Momentum indicator (construction NOT REPORTED)
- ATR (lookback period NOT REPORTED)
- Realized volatility (lookback period NOT REPORTED)

---

## Backtest Evidence

- **Type:** Walk-forward (rolling 10-year train / 6-month test), monthly roll
- **Period:** 2015–2025 (reported as 2,793 out-of-sample trading days)
- **Market:** Gold Futures (GC/COMEX or equivalent)
- **Transaction costs:** 0.7 bps linear + square-root impact (γ = 0.02)
- **Auditable?** No — unreviewed arXiv preprint; no public code; all fetches 403'd

**Statistical concern on walk-forward setup:** A 10-year training window rolled monthly from
2015 to 2025 yields approximately 20 non-overlapping 6-month test periods. With 2 free
parameters (trend lookback, momentum lookback) and 20 test periods, standard multiple-testing
concerns apply. The paper does not report p-values or multiple-testing corrections in accessible
summaries.

---

## Forward-Test Evidence

NOT REPORTED. Authors describe themselves as "seeking feedback on live deployment" — suggesting
no live trading record exists.

---

## Reported Metrics

All metrics CLAIMED, UNVERIFIED (unreviewed arXiv preprint; no independent replication; all
primary sources 403'd):

| Metric | Value | Tag | Plausibility |
|--------|-------|-----|-------------|
| Sharpe ratio (OOS) | 2.88 | CLAIMED, UNVERIFIED | IMPLAUSIBLE — 14–19× industry per-market norm |
| Max drawdown | 0.52% | CLAIMED, UNVERIFIED | IMPLAUSIBLE — inconsistent with 15% vol over 10 years |
| CAGR | ~43% | CLAIMED, UNVERIFIED | IMPLAUSIBLE for gold trend-following |
| Alpha (vs spot gold) | ~37% annualized | CLAIMED, UNVERIFIED | IMPLAUSIBLE |
| Information Ratio | 2.09 | CLAIMED, UNVERIFIED | IMPLAUSIBLE — no independently audited IR this high in commodity trend-following |
| Beta (vs spot gold) | ~0.03 | CLAIMED, UNVERIFIED | Plausible for a long/short strategy |
| Volatility target | 15% | CLAIMED, UNVERIFIED | Plausible and standard |
| Kelly fraction (λ) | 0.40 | CLAIMED, UNVERIFIED | Standard and plausible |
| Cost model | 0.7 bps linear + γ=0.02 square-root | CLAIMED, UNVERIFIED | Reasonable |

**Critical inconsistency:** At 15% annual volatility with Sharpe 2.88, CAGR = Sharpe × vol + rf ≈
2.88 × 15% + 3% ≈ 46%. This is mathematically consistent with the 43% CAGR claim. However, a
43% CAGR in gold over 10 years starting from 2015 would have grown $100 to approximately
$3,700 — transforming a gold futures position into a venture-capital-like return profile. This
is extraordinary and would represent one of the best 10-year track records in any commodity
fund globally.

---

## Community Sentiment

**No community discussion found** on Reddit r/algotrading, ForexFactory, MQL5, TradingView,
Quantopian, or any practitioner forum.

The paper appears to have had minimal external visibility since its November 2025 publication.
No independent replication, critique, or review was found. Authors are "seeking feedback on live
deployment" — suggesting they have not yet deployed it live and are aware of the gap between
claimed and demonstrated performance.

---

## Strengths

1. **Sound economic rationale:** Gold trend-following is well-grounded in the CTA literature
2. **Methodologically structured:** Walk-forward, volatility targeting, fractional Kelly, realistic
   cost model — the framework components are individually sound
3. **Market capacity claim ($1B):** Plausible for a single-asset futures strategy using scaled
   position sizing
4. **Benchmark-neutral design (beta ≈ 0.03):** Long/short structure properly isolates alpha

## Weaknesses

1. **Metrics are implausibly good:** Sharpe 2.88 is 14–19× typical per-market CTA Sharpe;
   max drawdown 0.52% at 15% vol over 10 years violates basic statistical expectations
2. **Signal construction not accessible:** Exact trend/momentum formulas not retrievable
3. **No live trading record:** Authors acknowledge they are seeking feedback for live deployment
4. **No peer review:** Unreviewed preprint only
5. **No independent replication found**
6. **Possible max drawdown measurement error:** 0.52% at 15% vol is inconsistent with portfolio
   equity drawdown; may refer to per-position stop depth, not portfolio peak-to-trough
7. **Training window concern:** 10-year training window for trend/momentum parameters risks
   curve-fitting to gold's specific 2005–2025 price regime (bull run followed by consolidation
   followed by 2020+ bull)

---

## Confidence Score

| Dimension | Score (0–10) | Weight | Weighted |
|-----------|-------------|--------|----------|
| Logical / economic soundness | 5 | ×3 | 15.0 |
| Transparency & reproducibility | 3 | ×2 | 6.0 |
| Evidence quality | 2 | ×2 | 4.0 |
| Risk management quality | 6 | ×2 | 12.0 |
| Honest treatment of drawdowns / failure | 2 | ×1.5 | 3.0 |
| Robustness evidence (OOS, walk-forward) | 4 | ×1.5 | 6.0 |
| Simplicity / low overfitting risk | 5 | ×1 | 5.0 |
| Survived independent scrutiny | 1 | ×1 | 1.0 |
| **Total** | | | **52.0** |

**Score = (52 / 140) × 100 = 37** — LOW CONFIDENCE

**Dimension rationale:**
- *Logical soundness 5/10:* Gold trend-following has genuine economic basis (CTA literature,
  persistent safe-haven demand shocks). Penalized because claimed metrics are implausibly far
  from any known benchmark.
- *Transparency 3/10:* Walk-forward setup is described. Signal construction formulas are NOT
  retrievable. No code published.
- *Evidence quality 2/10:* Claimed metrics are inconsistent with known gold market dynamics
  and industry benchmarks. The "evidence" as reported cannot be trusted without independent
  verification. Possible calculation error in drawdown figure.
- *Risk management 6/10:* The framework components (Kelly fraction, vol targeting, ATR exits)
  are individually sound and well-described.
- *Honest drawdowns 2/10:* 0.52% max drawdown claim is implausible for a 15% vol strategy
  over 10 years. Either a calculation methodology issue or an error — not reported honestly.
- *Robustness 4/10:* Walk-forward structure is described correctly. But 2015–2025 is a
  specific gold bull-then-consolidation-then-bull regime that may not generalize.
- *Simplicity 5/10:* Two conceptually simple signals (trend + momentum). But exact parameters
  are unknown. Vol targeting + Kelly + impact adjustment adds complexity.
- *Scrutiny 1/10:* No community discussion; no independent replication; no peer review.

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Medium — trend + momentum signal construction is standard; Kelly + vol
  targeting requires real-time vol estimates; ATR exits are straightforward
- **Scalability:** Authors claim $1B capacity for single-asset gold futures — plausible
- **Automation:** Feasible in Python with market data + futures broker API

## MQL5 / Python / Pine Feasibility

- **Python:** YES — all components implementable (pandas/numpy for signals; backtesting
  libraries for walk-forward; position sizing module)
- **Pine Script:** Partial — can implement signal and basic sizing but not impact-adjusted
  Kelly or robust walk-forward validation
- **MQL5:** YES — all components implementable for MetaTrader futures access

---

## Similar Strategies

- `catching-crypto-trends-donchian-ensemble` — also trend-following with vol-based sizing,
  different market (crypto) and signal type (Donchian channel ensemble)
- `adaptivetrend-crypto` — also multi-component trend-following with rolling Sharpe selection

---

## Red Flags

- **Sharpe 2.88 is 14–19× the typical per-market trend-following Sharpe** (CRITICAL)
- **Max drawdown 0.52% inconsistent with 15% annual vol over 10 years** (CRITICAL)
- **CAGR 43% in gold futures over 10 years exceeds all known audited CTA track records in gold**
- **No live trading record — authors "seeking feedback on live deployment"**
- **No peer review — unreviewed preprint only**
- **No independent replication**
- **Signal construction formulas not publicly available**
- **2015–2025 gold had a strong 2019–2025 bull run — in-sample favorable regime possible**

---

## Source Links

| URL | Retrieved |
|-----|-----------|
| https://arxiv.org/abs/2511.08571 | 2026-05-29 (HTTP 403 — search summary only) |
| https://arxiv.org/html/2511.08571v1 | 2026-05-29 (HTTP 403) |
| https://arxiv.org/pdf/2511.08571 | 2026-05-29 (HTTP 403) |
| https://ideas.repec.org/p/arx/papers/2511.08571.html | 2026-05-29 (HTTP 403) |
| https://www.researchgate.net/publication/397522459 | 2026-05-29 (HTTP 403) |

All primary source fetches failed (HTTP 403). All data gathered via search engine summaries.

---

## Analyst Notes

**FACTS (from search engine summaries of arxiv 2511.08571):**
- Paper by Mainak Singha, Jose Aguilera-Toste, Vinayak Lahiri; arXiv November 2025
- Uses trend + momentum signals on gold futures with 10-year rolling walk-forward
- Volatility targeting at 15%, fractional Kelly λ=0.40, ATR exits, 0.7 bps cost + γ=0.02 impact
- Claimed OOS Sharpe 2.88, max drawdown 0.52%, CAGR ~43%, IR 2.09, beta 0.03
- Authors are "seeking feedback on live deployment" (no live trading results)

**ANALYSIS:**
The core economic idea (trend + momentum in gold) is sound and well-established. The execution
framework (vol targeting + Kelly + impact model) is methodologically coherent. The problem is
the claimed performance metrics.

Benchmark context: The best long-running single-market CTA programs in gold historically achieve
Sharpe ratios of 0.5–0.9 in favorable trend regimes. Sharpe 2.88 represents an outlier claim
that exceeds even the most celebrated systematic trading programs (e.g., Renaissance Medallion
Fund's ~2.0 Sharpe was achieved across hundreds of instruments, not a single commodity).

The 0.52% max drawdown is the most suspicious figure. At 15% annual vol (≈0.945% daily), over
2,793 days, a standard Brownian motion model predicts an expected maximum drawdown of
approximately 8–12% for a strategy of this type. The 0.52% figure suggests either:
(a) The drawdown is measured per-position (ATR stop depth), not portfolio equity
(b) There is a calculation error in the paper
(c) The strategy has exceptionally clean equity curve due to overfitting to the training period

**ASSUMPTIONS:**
- The walk-forward training windows did not selectively use favorable parameter combinations
  across the gold bull run of 2019–2025
- "Maximum drawdown" is measured as portfolio equity peak-to-trough (not confirmed)
- The 0.7 bps transaction cost assumption is realistic for institutional gold futures execution
  (plausible but not confirmed)

---

## Future Research Needed

1. Access the full paper text to verify signal construction formulas and drawdown measurement
   methodology
2. Independently replicate the strategy with published parameter ranges
3. Test on gold futures data outside the 2015–2025 window (2000–2015 would be a true OOS test)
4. Compare against benchmarks: AQR, Man AHL, and other published CTA programs in gold
5. Clarify whether max drawdown = portfolio equity peak-to-trough vs. per-position ATR stop
6. Check whether the walk-forward training windows were evaluated on multiple parameter sets
   (multiple-testing correction needed)
