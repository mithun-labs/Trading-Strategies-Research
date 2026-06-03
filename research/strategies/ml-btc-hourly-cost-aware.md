# ML-Based Bitcoin Trading Under Transaction Costs (Cost-Aware Walk-Forward)

## Overview

University of Warsaw researchers (Bysik/Ślepaczuk) evaluate three ML models — XGBoost,
LSTM, and iTransformer — for hourly BTC-USDT return forecasting, using a rigorous 27-fold
walk-forward protocol over ~70,000 hourly observations (2018–2026). The central finding is
negative: **naive sign-based strategies fail once 10bps transaction costs are imposed.** A
cost-aware execution filter — trade only when the model's forecast magnitude exceeds the
cost threshold — restores profitability in selected configurations, with the best long-only
XGBoost variant producing annualized returns > 65% and Sharpe > 1.

---

## Asset Class / Timeframes

- **Asset:** Bitcoin (BTC-USDT)
- **Timeframe:** Hourly (H1) — entry/exit at each hourly bar
- **Universe:** Single asset

---

## Core Logic

1. **Feature engineering:** Technical indicators computed on BTC hourly OHLCV data.
   EGARCH-derived features are also tested but found not to provide uniformly robust
   improvement.
2. **Forecast:** An ML model (XGBoost in the best configuration) predicts the sign and
   magnitude of the next-bar BTC-USDT return.
3. **Cost-aware execution filter:** A trade is entered only if `|predicted_return| > c`,
   where `c` = transaction cost threshold (10bps in the paper). This prevents trading when
   the predicted edge is smaller than the round-trip cost.
4. **Position:** Long-only in the best reported configuration. Presumably flat when no
   signal fires or when predicted return is negative.
5. **Exit:** Implied to be close-and-reverse on signal flip, or close on zero/negative signal.
6. **Walk-forward validation:** 27-fold rolling-window walk-forward; training window expands
   or rolls; no look-ahead. This is the methodologically strongest aspect.

---

## Economic Rationale

Plausible mechanism: BTC hourly prices exhibit persistent non-linear patterns (microstructure
noise, momentum, order-flow imbalances, retail sentiment cycles) that a regularized ML model
can exploit. The cost-aware filter operationalizes the insight that most apparent ML signal
is too small to survive execution costs — only large-magnitude predictions carry a genuine
edge.

**When the edge should disappear:** (1) Bitcoin market efficiency increases at hourly
resolution (more algorithmic participation arbitrages the pattern away); (2) transaction
costs rise above the filter threshold, eliminating the tradeable signals; (3) the specific
indicator set that worked in 2018–2026 stops generating predictive signal in a future
regime (e.g., post-maturation of BTC market microstructure). The falsifiability is partial
— the cost-aware filter mechanism is well-grounded, but we do not know which specific
indicators drive the performance.

---

## Entry Conditions

- Enter long when `XGBoost_predicted_return > 10bps` (cost threshold)
- Remain flat otherwise
- Specific feature set: Technical indicators (set not fully specified in accessible summary);
  EGARCH features tested but not uniformly beneficial — exact feature engineering is NOT
  REPORTED in accessible summaries

---

## Exit Conditions

- Exit when signal flips or falls below cost threshold (implied; NOT REPORTED explicitly)
- No explicit stop-loss reported
- Holding period: varies with signal; determined by model output each hour

---

## Risk Management

- Long-only constraint limits catastrophic short-side losses
- Cost-aware filter reduces overtrading (high turnover is the primary enemy of hourly crypto strategies)
- No explicit stop-loss or maximum position loss threshold reported
- No position sizing rules reported beyond implied 100%-long when in position
- **Assessment:** Minimal. The cost filter is the primary risk control; explicit
  downside management is absent from accessible descriptions.

---

## Indicators Used

- **Explicitly confirmed:** XGBoost model on technical indicators (specific list NOT REPORTED)
- **Tested but not uniformly beneficial:** EGARCH-derived volatility features
- **Not reported:** Lookback lengths, specific feature transformations, hyperparameters,
  regularization settings, retraining frequency

---

## Transaction Costs & Capacity

- **Costs modeled:** YES — explicitly 10bps round-trip, which is the paper's central focus
- **Cost-aware filter:** Trades only execute when |predicted return| > 10bps; this is the
  key mechanism for surviving costs
- **Capacity:** BTC/USDT spot/perpetual markets are highly liquid; 10bps is a reasonable
  but not conservative estimate for institutional-scale execution. Retail-scale trading on
  Binance can achieve near-zero maker fees (~2–4bps), making 10bps slightly conservative
  for small sizes. At larger sizes, slippage and market impact will exceed 10bps, potentially
  eroding the edge.
- **Turnover:** The cost-aware filter sharply reduces turnover relative to naive sign-based
  strategies; no specific trade count or holding period reported.

---

## Backtest Evidence

- Walk-forward: 27-fold rolling-window walk-forward (2018–2026); this is `CLAIMED, UNVERIFIED`
  but the methodology is well-specified
- Dataset: ~70,000 hourly observations (2018–2026)
- All metrics below are `CLAIMED, UNVERIFIED` — arXiv preprint; no peer review; no open code

---

## Forward-Test Evidence

`NOT REPORTED` — paper does not describe any live or paper-trading validation.

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | > 1 (exact value NOT REPORTED) | CLAIMED, UNVERIFIED | arXiv 2606.00060 (Bysik/Ślepaczuk 2026) |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | — |
| CAGR | > 65% annualized (long-only XGBoost best config) | CLAIMED, UNVERIFIED | arXiv 2606.00060 |
| Annual Return | NOT REPORTED (period-specific) | NOT REPORTED | — |
| Number of Trades | NOT REPORTED (described as "sharply reduced" by filter vs naive) | NOT REPORTED | — |
| Average Trade Duration | NOT REPORTED | NOT REPORTED | — |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | NOT REPORTED | NOT REPORTED | — |

**Automatic scrutiny trigger:** CAGR > 65% exceeds the 50% mandatory scrutiny threshold.
See `## Why This Might Be Nothing` below.

---

## Community Sentiment

No community discussion found as of 2026-06-03. Paper was submitted ~May 2026 and is a
new preprint (~2 weeks old at time of research). Ślepaczuk's group at University of Warsaw
has a productive track record publishing on ML-based trading strategies (multiple MDPI Sensors
and SSRN papers on BTC/equity ML); this is not a first-time author with an implausible result.
No independent replication, replication critique, or forum thread found.

---

## Why This Might Be Nothing

### 1. Most likely benign explanation: Bitcoin bull market + model selection

The 2018–2026 period contains two major Bitcoin bull markets (2020–2021, 2023–2025). A
long-only strategy in any asset with a structural uptrend will produce high absolute returns.
The 65%+ CAGR reported is almost certainly **not** a generic ML edge — it is substantially
driven by BTC's secular appreciation during the sample. A buy-and-hold BTC position over
2018–2026 would itself produce very high CAGR; the relevant comparison is **Sharpe relative
to B&H**, not absolute return. The Sharpe > 1 is more informative, but the absolute CAGR
is not meaningful without knowing the buy-and-hold benchmark.

### 2. The cost case

10bps is modeled, which is positive. However:
- 10bps is the **stated** round-trip cost but may understate total execution costs:
  taker fees on major exchanges run 5–7bps per side (10–14bps round-trip), and any market
  impact adds further cost. At 10–14bps round-trip, the filter threshold of 10bps is near
  the breakeven line.
- More critically: the filter parameter `c = 10bps` was presumably chosen to work with 10bps
  costs. If costs were 15bps, a different `c` would be needed. This is a circular calibration
  risk: the filter was tuned to the cost assumption.

### 3. Multiple-testing / model selection bias

Three models × multiple configurations × technical indicator sets × possibly multiple
lookback periods. The "best long-only XGBoost strategy" was selected from many candidates.
With 27-fold walk-forward, the in-fold model selection is rigorous, but the cross-fold model
selection (choosing XGBoost over LSTM/iTransformer) uses the same evaluation period.
Bootstrap evidence does not show statistical dominance — meaning the Sharpe > 1 for
XGBoost may be indistinguishable from the other models' noise, and from randomness.

### 4. Sample period concerns

- 2018–2026 is a single regime for BTC: high-volatility, high-directional-bias, retail
  participation, structurally growing market. The strategy has not been tested in a
  fully matured or declining BTC market.
- All 27 walk-forward windows are still within the same 2018–2026 BTC environment.
  Robustness across regimes (e.g., 2014–2018 BTC bear market) is NOT tested.

### 5. No maximum drawdown reported

The maximum drawdown is NOT REPORTED in accessible paper summaries. This is a significant
gap. A long-only BTC hourly strategy during 2018 (−80% bear market), 2019–2020 (March 2020
COVID crash), or 2022 (−75% bear) could easily have drawn down 50%+ even with a cost-aware
filter, since the filter only prevents overtrading — it does not exit long positions when
BTC trends strongly downward.

### 6. The one piece of evidence that would most change my mind

An independently replicated walk-forward test on data AFTER 2024 (out-of-the-paper-sample)
with explicit comparison to buy-and-hold Sharpe, AND a reported maximum drawdown. The
current evidence establishes that a cost-aware filter is a useful design pattern — but
whether the specific XGBoost model + feature set has a durable, non-bull-market edge
remains unverified.

---

## Strengths / Weaknesses

**Strengths:**
- Walk-forward methodology (27 folds) is rigorous; not in-sample optimization
- Transaction costs explicitly modeled; the paper honestly shows naive strategies fail
- Honest about EGARCH features not uniformly adding value
- XGBoost is interpretable relative to deep learning alternatives
- The cost-aware filter concept is a genuinely useful design pattern for practitioners
- From an established research group (Ślepaczuk, University of Warsaw) with track record

**Weaknesses:**
- arXiv preprint only; not peer-reviewed
- CAGR > 65% driven substantially by BTC bull market
- Maximum drawdown not reported (critical gap)
- Specific feature set not disclosed; not reproducible
- No open code
- Single-asset, single-period test
- Model selection may inflate best-configuration results
- Cost-aware filter parameter likely tuned to stated cost assumption (circularity risk)

---

## Confidence Score

### Per-Dimension Breakdown

| Dimension | Raw Score | Weight | Contribution |
|-----------|-----------|--------|--------------|
| Logical / economic soundness (falsifiable) | 5 | 3 | 15 |
| Transparency & reproducibility | 4 | 2 | 8 |
| Evidence auditability | 5 | 2 | 10 |
| Risk management quality | 3 | 2 | 6 |
| Cost & capacity realism | 7 | 2 | 14 |
| Honest treatment of drawdowns / failure | 5 | 1.5 | 7.5 |
| Robustness evidence (OOS / walk-forward) | 5 | 1.5 | 7.5 |
| Survived independent scrutiny | 1 | 1 | 1 |

**Weighted sum:** 69 / 150 → **latent 46**

**Evidence auditability = 5 → cap at 74**

**`confidence = min(46, 74) = 46`** (Experimental)

*latent 46 (capped to 46 pending verification: arXiv preprint only; no peer review; no
open code; CAGR > 65% driven by BTC bull market; max drawdown not reported)*

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Moderate. Requires hourly OHLCV data pipeline, feature engineering,
  XGBoost model retraining on rolling window, cost-aware trade filter execution.
- **Scalability:** Limited — single-asset, likely degraded by market impact beyond small
  size. Hourly frequency means ~6,500 potential signals per year; filter reduces this
  substantially.
- **Automation:** Feasible but non-trivial. Requires reliable hourly data feed, model
  retraining schedule (not specified), and exchange connectivity for BTC execution.

---

## MQL5 / Python / Pine Feasibility

- **Python:** PRIMARY — XGBoost, LSTM, and iTransformer are all Python libraries (sklearn,
  PyTorch/Keras); pandas + ccxt for BTC data; walk-forward backtesting framework needed
  (e.g., vectorbt, backtesting.py with custom WF).
- **MQL5:** Very difficult — XGBoost not natively supported in MQL5; would require external
  Python service or ONNX model export.
- **Pine Script (TradingView):** NOT FEASIBLE — ML model execution not supported in Pine Script.
- **Latent score (46) is below implementation candidate threshold (latent ≥ 60); will not be added to implementation_candidates.**

---

## Similar Strategies

- `bitcoin-multitf-trend` — MACD D1/H1 multi-timeframe, different signal (technical rule vs. ML forecast); both are BTC hourly/daily strategies
- `bitcoin-max-min-trendrev` — Rolling MAX/MIN price breakout, daily, different mechanism
- `crypto-ts-xs-momentum-realistic` — 28-day TSMOM on 471 coins, daily, different mechanism
- `adaptivetrend-crypto` — Multi-component trend following on 150+ crypto pairs, 6H

---

## Red Flags

- **CAGR > 65%:** Exceeds 50% automatic scrutiny threshold. Substantially explained by BTC
  bull market (2020–2021, 2023–2025). Not a model-only result.
- **Maximum drawdown not reported:** Critical omission for a long-only BTC strategy spanning
  2018–2026 (which includes −80%, −55%, −75% BTC bear markets).
- **No open code:** Cannot independently verify methodology or replicate results.
- **Single-period test:** 2018–2026 is one regime for BTC; no pre-2018 or OOS test.
- **Model selection from multiple configurations:** "Best" XGBoost chosen post-evaluation;
  no multiple-testing correction.

---

## Source Links

| URL | Retrieved | Notes |
|-----|-----------|-------|
| https://arxiv.org/abs/2606.00060 | 2026-06-03 | Primary source — arXiv abstract |
| https://arxiv.org/html/2606.00060 | 2026-06-03 | arXiv HTML — HTTP 403, not accessible |

---

## Analyst Notes

**FACTS (from sources):**
- Authors: Andrei Bysik and Robert Ślepaczuk, University of Warsaw, Quantitative Finance
  Research Group
- arXiv: 2606.00060 (q-fin.TR)
- Dataset: ~70,000 hourly BTC-USDT observations, 2018–2026
- Models: XGBoost, LSTM, iTransformer
- Walk-forward: 27-fold protocol
- Transaction costs: 10bps round-trip explicitly modeled
- Finding 1: Naive sign-based strategies fail after 10bps costs
- Finding 2: Cost-aware filter (trade when |forecast| > 10bps) restores profitability
  in selected configurations
- Finding 3: Technical indicators improve in selected cases; EGARCH features not uniformly
  beneficial
- Finding 4: XGBoost descriptively stronger than LSTM/iTransformer, not statistically dominant
- Best result: Long-only XGBoost → annualized return > 65%, Sharpe > 1

**ANALYSIS:**
- The 65%+ CAGR is not a clean ML edge — BTC buy-and-hold over 2018–2026 also produced
  very high CAGR (~25–30% annualized depending on exact dates). The relevant excess return
  is not reported.
- The cost-aware filter concept (trade when |signal| > cost) is the paper's most valuable
  contribution. It is a general design principle applicable beyond BTC.
- 27-fold walk-forward is methodologically sound but does not eliminate model selection bias
  across configurations.
- The absence of max drawdown data makes it impossible to evaluate risk-adjusted performance
  beyond Sharpe.

**ASSUMPTIONS (flagged):**
- ASSUMED: The feature set is standard technical indicators (moving averages, RSI, etc.)
  based on the Ślepaczuk group's prior published work on BTC ML strategies.
- ASSUMED: Exit is at signal flip or threshold reversion; specific exit rules not confirmed.

---

## Future Research Needed

1. Access full paper PDF for specific: max drawdown, exact feature list, Sharpe values
   by model, trade counts, holding periods.
2. Compare XGBoost Sharpe and drawdown against BTC buy-and-hold benchmark (critical missing context).
3. Test the cost-aware filter framework on broader crypto universe or other liquid markets.
4. Independent replication with post-2024 data (genuine OOS).
5. Verify whether 10bps cost assumption is the right calibration for current market
   conditions (vs. taker fee structure on major exchanges).
