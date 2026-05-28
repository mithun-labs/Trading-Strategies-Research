# AdaptiveTrend — Systematic Trend-Following with Adaptive Portfolio Construction

## Overview

Multi-component systematic trend-following framework for cryptocurrency markets. Combines
6-hour momentum signals with volatility-calibrated trailing stops, monthly rolling-Sharpe
asset selection, and an asymmetric 70/30 long/short capital allocation. Published as arXiv
preprint 2602.11708 on February 12, 2026, by Duc Bui and Thanh Nguyen (Talyxion Research,
Hanoi, Vietnam).

## Asset Class / Timeframes

- **Asset class:** Cryptocurrency (Binance Futures perpetual swap pairs)
- **Universe:** 150+ cryptocurrency pairs; filtered by market cap and rolling Sharpe
- **Execution timeframe:** 6-hour (H6) candlesticks
- **Portfolio rebalance:** Monthly

## Core Logic

1. **Signal generation:** Momentum-based entry signal on H6 candlesticks (exact formula NOT
   RETRIEVED from paper text — arxiv.org blocked during this research pass).
2. **Exit / Stop:** Dynamic trailing stop calibrated to intra-day volatility regime (exact
   formula NOT RETRIEVED).
3. **Asset selection:** At each monthly rebalance, filter universe by market cap threshold
   and rank surviving assets by rolling Sharpe ratio. Select top-N for the long leg and
   bottom-N for the short leg (exact N and lookback NOT RETRIEVED).
4. **Capital allocation:** 70% of capital allocated to long positions; 30% to short positions.
   Equal-weight within each leg.

## Economic Rationale

Cryptocurrency markets exhibit pronounced momentum effects (cross-sectional and time-series),
documented in multiple independent studies. Trend following is a risk premium with a behavioral
foundation: under-reaction to news followed by delayed correction. The asymmetric 70/30
long/short split is justified by the empirical positive long-run drift of crypto markets —
ANALYSIS: this is a stated rationale, not a derived necessity; the 70/30 ratio is itself a
free parameter that was presumably optimized.

## Entry Conditions

NOT RETRIEVED in detail. Known at a high level: momentum-based trigger on the H6 candle.
Exact indicator (price vs. moving average, breakout level, or return threshold) was not
accessible from search summaries. Full paper body could not be fetched.

## Exit Conditions

Dynamic trailing stop calibrated to the prevailing intra-day volatility regime. Regime
detection method NOT RETRIEVED. Stop tightens in high-volatility regimes and widens in
low-volatility regimes (INFERRED from description; not directly stated in accessible text).

## Risk Management

- Volatility-calibrated trailing stops (positive; stops adapt to regime)
- Transaction cost modeling included in backtests
- Equal-weight diversification within each portfolio leg
- Capacity constraint: short leg limited by liquidity of smaller-cap assets (stated by authors)
- **Not stated:** Maximum position sizing, leverage limits, portfolio-level stop, drawdown
  circuit breakers

## Indicators Used

- Rolling Sharpe ratio (lookback period NOT RETRIEVED)
- Market capitalization filter (threshold NOT RETRIEVED)
- Intra-day volatility estimate (calculation method NOT RETRIEVED)
- 6-hour momentum signal (formula NOT RETRIEVED)

## Backtest Evidence

CLAIMED, UNVERIFIED. Authors' own backtesting — no independently auditable source.

- **OOS period:** 36 months (2022–2024)
- **Universe:** 150+ cryptocurrency pairs
- **Method:** In-sample / out-of-sample split defined by authors; exact IS period NOT RETRIEVED
- **Benchmarks compared:** TSMOM (time-series momentum), equal-weight buy-and-hold
- Robustness analyses reported: parameter sensitivity, transaction cost modeling, regime-
  conditional performance decomposition (details NOT RETRIEVED)
- No walk-forward or expanding-window test described in accessible summaries

## Forward-Test Evidence

NOT REPORTED. The related Talyxion framework (arxiv 2511.13239, from the same research group)
reported a 30-day live Binance Futures result of ROI +16.68% and Sharpe 5.72. However: (1)
30 days is statistically trivial; (2) this is a different paper; (3) no independent auditor.
Tagged: CLAIMED, UNVERIFIED, NOT ATTRIBUTABLE TO THIS SPECIFIC STRATEGY.

## Reported Metrics

| Metric | Value | Tag |
|--------|-------|-----|
| Annualized Sharpe ratio (OOS) | 2.41 | CLAIMED, UNVERIFIED |
| Maximum drawdown (OOS) | -12.7% | CLAIMED, UNVERIFIED |
| Calmar ratio | 3.18 | CLAIMED, UNVERIFIED |
| Cumulative returns (OOS 2022–2024) | ~140% | CLAIMED, UNVERIFIED |
| Win rate | NOT REPORTED | — |
| CAGR | NOT REPORTED | — |
| Average trade PnL | NOT REPORTED | — |

## Community Sentiment

No community discussion found. Paper published February 2026; too new to have accumulated
meaningful independent commentary. No Reddit (r/algotrading, r/CryptoCurrency), ForexFactory,
or MQL5 discussion found. Authors promote via LinkedIn (Talyxion page).

## Strengths / Weaknesses

**Strengths:**
- Builds on well-supported crypto momentum literature
- Volatility-calibrated trailing stop is conceptually sound
- Monthly dynamic asset selection avoids stale portfolio allocation
- Transaction costs included in backtest
- OOS period includes a full crypto bear market (2022) — not just cherry-picked bull run

**Weaknesses:**
- Core entry/exit rules not accessible; reproducibility currently impossible
- No public code repository found
- No peer review (arXiv preprint only)
- No independent replication
- Multiple free parameters (trailing stop calibration, rolling Sharpe lookback, 70/30 ratio,
  market cap threshold) create high curve-fitting risk
- "OOS" is defined within authors' own framework — no external audit
- Sharpe 2.41 on a 36-month window is extremely high by traditional asset standards; warrants
  exceptional skepticism without auditable confirmation
- Requires Binance Futures access; subject to regulatory restrictions

## Confidence Score

| Dimension | Weight | Score | Weighted |
|-----------|--------|-------|---------|
| Logical / economic soundness | 3 | 6 | 18.0 |
| Transparency & reproducibility | 2 | 3 | 6.0 |
| Evidence quality (auditable > claimed > none) | 2 | 3 | 6.0 |
| Risk management quality | 2 | 6 | 12.0 |
| Honest treatment of drawdowns / failure regimes | 1.5 | 5 | 7.5 |
| Robustness evidence (OOS, walk-forward, multi-market) | 1.5 | 3 | 4.5 |
| Simplicity / low overfitting risk | 1 | 2 | 2.0 |
| Survived independent scrutiny | 1 | 1 | 1.0 |
| **TOTAL** | **14** | — | **57.0** |

**Score: (57.0 / 140) × 100 = 40.7 → 41 — EXPERIMENTAL**

Scoring notes:
- Momentum rationale is sound (6/10 logical soundness), but attribution across three
  interacting "innovations" is opaque.
- Reproducibility (3/10): entry, stop, and selection formulas unavailable.
- Evidence (3/10): authors' own OOS, no independent audit.
- Risk management (6/10): trailing stops and vol-calibration are positives; position sizing
  details absent.
- Drawdown honesty (5/10): max DD reported; regime decomposition mentioned but not verified.
- Robustness (3/10): parameter sensitivity mentioned; no walk-forward or multi-market test.
- Simplicity (2/10): three optimization layers, multiple unlisted hyperparameters.
- Scrutiny (1/10): new preprint, no community engagement found.

## Complexity / Scalability / Automation Feasibility

- **Complexity:** HIGH. Four interacting components, all with unspecified parameters.
- **Scalability:** LIMITED on short side; smaller-cap crypto pairs have thin order books.
- **Automation:** Technically feasible (Binance API + Python), but requires parameter
  discovery from the full paper before implementation can begin.

## MQL5 / Python / Pine Script Feasibility

- **MQL5:** Not applicable (MT5 doesn't natively support crypto perpetual futures from Binance)
- **Python:** Feasible once full rule set is retrieved. CCXT library for Binance Futures API.
  Rolling Sharpe selection and volatility-calibrated stops are standard Python operations.
- **Pine Script:** Not feasible for this strategy (Pine Script cannot execute multi-asset
  portfolio rebalancing or cross-instrument Sharpe calculations)

## Similar Strategies

- [`catching-crypto-trends-donchian-ensemble`](catching-crypto-trends-donchian-ensemble.md) —
  simpler daily Donchian ensemble, same market; lower complexity, slightly lower score (64)
- TSMOM (Time-Series Momentum, Moskowitz/Ooi/Pedersen) — cited as underperforming benchmark

## Red Flags

- **Sharpe 2.41, Calmar 3.18, Max DD -12.7%:** All three numbers are extraordinary for any
  strategy over a 36-month window. While crypto does produce high raw returns, these figures
  require extraordinary corroboration that is currently absent.
- **Multiple free parameters with no explicit optimization constraints:** High curve-fitting risk.
- **"OOS" defined by authors:** The train/test split integrity cannot be verified externally.
- **No independent replication found:** Zero external validation.
- **No public code:** Reproducibility cannot be confirmed.
- **Related Talyxion result (Sharpe 5.72 over 30 days):** Too short to be meaningful; an
  extreme reading warrants caution about optimism bias in the research group's reporting.

## Source Links

| Source | Retrieved | Notes |
|--------|-----------|-------|
| [arxiv 2602.11708 abstract](https://arxiv.org/abs/2602.11708) | 2026-05-28 | Primary source; body text blocked (HTTP 403) |
| [arxiv 2602.11708 HTML](https://arxiv.org/html/2602.11708v1) | 2026-05-28 | HTTP 403 |
| [arxiv 2602.11708 PDF](https://arxiv.org/pdf/2602.11708) | 2026-05-28 | HTTP 403 |
| [ResearchGate entry](https://www.researchgate.net/publication/400742265_Systematic_Trend-Following_with_Adaptive_Portfolio_Construction_Enhancing_Risk-Adjusted_Alpha_in_Cryptocurrency_Markets) | 2026-05-28 | HTTP 403 |
| [Medium write-up by Antonio Velazquez-Bustamante](https://antonio-velazquez-bustamante.medium.com/adaptivetrend-a-6-hour-trend-following-system-for-crypto-that-adapts-its-portfolio-every-month-eb114edcbcef) | 2026-05-28 | HTTP 403 |
| [Talyxion LinkedIn](https://www.linkedin.com/posts/talyxion_adaptive-trend-crypto-paper-activity-7426970999499448320-hKWI) | 2026-05-28 | Author promotion post |
| [Talyxion framework paper arxiv 2511.13239](https://arxiv.org/abs/2511.13239) | 2026-05-28 | Related prior paper by same group |

## Analyst Notes

**FACTS (from sources):**
- Published by Duc Bui and Thanh Nguyen (Talyxion Research, Hanoi, Vietnam), arXiv, February 12, 2026.
- Three stated innovations: volatility-calibrated trailing stop, rolling Sharpe asset selection, 70/30 asymmetric allocation.
- OOS period: 36 months, 2022–2024, 150+ cryptocurrency pairs on Binance Futures.
- Reported Sharpe 2.41, Max DD -12.7%, Calmar 3.18, cumulative returns ~140%.
- Benchmarks: TSMOM and equal-weight buy-and-hold (both outperformed per authors).
- Stated limitations: Binance dependency, regulatory risk, short-side liquidity capacity.
- Related Talyxion paper (arxiv 2511.13239) reported 30-day live result of Sharpe 5.72 for a different framework from the same group.

**ANALYSIS:**
- The three-component complexity makes it difficult to determine which element drives the alpha.
  If the trailing stop alone accounts for most performance, the other two components are noise.
  If the rolling Sharpe selection drives it, then the strategy may inadvertently use future
  information (assets that had high Sharpe in the selection window may be selected for the
  next period — this requires careful implementation to avoid look-ahead bias).
- The 70/30 long/short split is an unusual choice. Standard trend-following is often long/short
  symmetrically. The 70/30 choice assumes the bull drift persists — this assumption deserves
  testing across market regimes.
- The 2022–2024 window is useful because it includes a severe bear (2022). However, the bull
  regime of 2023–2024 likely dominates cumulative returns; the strategy's performance in
  extended sideways or range-bound crypto markets is unknown.

**ASSUMPTIONS:**
- ASSUMED: The "OOS" designation is genuine (authors did not tune hyperparameters on this
  period). This cannot be verified externally.
- ASSUMED: Transaction cost model is realistic. Crypto perpetual swaps carry funding rates
  in addition to taker fees; if funding costs are omitted, returns are overstated.

## Future Research Needed

1. Obtain full paper text (when arXiv access resumes) to extract: exact entry signal formula,
   trailing stop calculation, volatility regime definition, rolling Sharpe lookback period,
   market cap threshold, N assets per leg.
2. Search for any independent replication or code repository.
3. Compare against the Zarattini/Barbon crypto Donchian ensemble (SSRN 5209907) on the same
   period — both claim strong 2022–2024 OOS performance.
4. Verify whether funding rates (perpetual swap carry) are included in the transaction cost model.
5. Test the 70/30 allocation assumption: what happens to performance if symmetric 50/50 is used?
