# Catching Crypto Trends — Donchian Channel Ensemble (CCT-DCE)

## Overview

A long-only trend-following strategy applied to Bitcoin and liquid altcoins, using an ensemble
of Donchian channel models calibrated at multiple lookback periods, combined with
volatility-based position sizing. Presented in an academic paper by Zarattini, Pagani, and
Barbon (2025), Swiss Finance Institute Research Paper No. 25-80.

## Asset Class / Timeframes

- **Asset class:** Cryptocurrency (Bitcoin, altcoins)
- **Timeframe:** Daily
- **Universe:** Bitcoin; extended to top-20 most liquid coins (median daily volume ≥ $2M over
  30 days, listed ≥ 1 year since 2015)
- **Holding period:** Multi-day to multi-month (trend-following)

## Core Logic

1. For each asset in the universe, compute Donchian channels at **nine lookback periods:**
   5, 10, 20, 30, 60, 90, 150, 250, and 360 days.
2. Each individual Donchian model generates a binary trend signal (+1 long / 0 flat) based on
   whether price is above (long) or at/below (exit) the channel boundary.
3. The individual signals are **aggregated into a single "Combo" signal** (ensemble approach).
4. **Volatility-based position sizing:** position size is inversely proportional to recent
   volatility (larger positions in low-volatility regimes, smaller in high-volatility).
5. For the altcoin portfolio: monthly rotation among top-20 by liquidity, allocating based
   on the Combo signal.

## Economic Rationale

**FACTS (from sources):** Zarattini et al. apply a methodology "successfully deployed for
decades in traditional asset classes" (trend following / CTA) to cryptocurrencies, citing the
pronounced momentum effects and regime-dependent volatility in crypto markets.

**ANALYSIS:** Trend-following has documented economic rationale across asset classes:
(a) behavioral momentum — investors underreact then overreact to information; (b) risk premium
for bearing drawdown risk during momentum crashes; (c) supply/demand imbalances. In crypto
specifically, structural factors amplify trends: retail-dominated order flow, narrative-driven
cycles, limited short-selling historically, halving cycles, and risk-on/risk-off macro dynamics.
The ensemble approach (9 lookbacks) is designed to reduce sensitivity to any single lookback
choice, though 9 parameters still raises mild overfitting concern.

## Entry Conditions

- For each lookback N in {5, 10, 20, 30, 60, 90, 150, 250, 360}:
  - **Enter long** when closing price > N-day Donchian channel high (breakout above N-day high)
  - **Exit long** when closing price ≤ N-day Donchian channel mid or falls below N-day low
  (exact exit rule not fully specified in sources)
- Individual signals aggregated: a positive Combo score triggers a long position

## Exit Conditions

- Signal reversal or weakening below ensemble threshold (NOT FULLY SPECIFIED in sources retrieved)
- Stop loss mechanism: NOT REPORTED beyond volatility-based sizing
- No short-selling (long-only strategy)

## Risk Management

- **Volatility-based position sizing:** position size inversely scaled to recent realized volatility
  (exact formula NOT REPORTED from sources retrieved)
- **Long-only:** maximum loss per asset is 100% of position, but sizing limits this in practice
- **Rotation discipline:** monthly rebalancing of altcoin portfolio to top-20 by liquidity
- No fixed stop loss reported

## Indicators Used

- **Donchian Channels** (9 lookbacks: 5, 10, 20, 30, 60, 90, 150, 250, 360 days)
- **Realized Volatility** (lookback period NOT REPORTED): position sizing
- **Relative liquidity rank** (30-day median daily volume): universe selection

## Backtest Evidence

**CLAIMED, UNVERIFIED:** Zarattini, Pagani & Barbon (2025), Swiss Finance Institute Research
Paper No. 25-80 (SSRN 5209907). Backtest period: January 2015 – March 2025.

No independent replication found in sources retrieved.

## Forward-Test Evidence

NOT REPORTED.

## Reported Metrics

All from Zarattini et al. (2025) — CLAIMED, UNVERIFIED:

| Metric | Value | Portfolio | Tag |
|--------|-------|-----------|-----|
| CAGR | 30% | Combo model (Bitcoin) | CLAIMED, UNVERIFIED |
| Sharpe ratio | 1.58 | Combo model (Bitcoin) | CLAIMED, UNVERIFIED |
| Sortino ratio | 2.03 | Combo model (Bitcoin) | CLAIMED, UNVERIFIED |
| Annualized alpha vs. Bitcoin | 14% | Combo model (Bitcoin) | CLAIMED, UNVERIFIED |
| Maximum drawdown | 19% | Combo model (Bitcoin) | CLAIMED, UNVERIFIED |
| Bitcoin passive buy-and-hold max DD | >80% | — | CLAIMED, UNVERIFIED |
| Sharpe ratio (altcoin rotation) | >1.5 | Top-20 liquid altcoins | CLAIMED, UNVERIFIED |
| Annualized alpha vs. Bitcoin (altcoin) | 10.8% | Top-20 liquid altcoins | CLAIMED, UNVERIFIED |
| Win rate | NOT REPORTED | — | — |

## Community Sentiment

**Positive:**
- Paper authored by Zarattini and Barbon — same team behind the well-regarded ORB paper
  (SSRN 4729284), lending some credibility.
- Swiss Finance Institute affiliation is a credibility signal.
- QuantifiedStrategies.com covered the Bitcoin version positively.

**Skeptical/Critical:**
- Published April 2025; limited independent scrutiny at time of research.
- Trend-following in crypto is well-known to practitioners; the claimed edge may represent
  beta to a well-known factor rather than alpha.
- Nine Donchian lookbacks raises mild data-mining concern, even with ensemble aggregation.
- Long-only in a historically bull-biased market inflates CAGR vs. a neutral strategy.
- No explicit out-of-sample or walk-forward test found in sources retrieved.
- 10-year backtest (2015–2025) covers Bitcoin's entire institutional history — earlier periods
  may not reflect current market structure (lower liquidity, different participants).

## Strengths / Weaknesses

**Strengths:**
- Academic paper from credible authors and institution
- Clear, reproducible methodology (Donchian channels are standard and well-defined)
- Long 10-year backtest across full Bitcoin market cycle
- Volatility-based sizing is principled risk management
- Significant drawdown reduction vs. passive Bitcoin (19% vs. 80%+)
- Extended to altcoin universe with similar results (reduces single-asset bias)

**Weaknesses:**
- Long-only — misses significant shorting opportunities in bear markets
- Exact exit rule and volatility scaling formula NOT REPORTED in sources
- No independent replication found
- 9 lookback parameters mild overfitting risk
- Transaction costs (especially for altcoin rotation) not fully specified
- Crypto-specific risks (exchange failure, regulation, liquidity crises) not addressed
- Recency: limited live-trading validation post-publication

## Confidence Score

| Dimension | Raw (0–10) | Weight | Weighted |
|-----------|-----------|--------|----------|
| Logical/economic soundness | 7 | 3 | 21 |
| Transparency & reproducibility | 7 | 2 | 14 |
| Evidence quality | 7 | 2 | 14 |
| Risk management quality | 6 | 2 | 12 |
| Honest treatment of drawdowns/failure | 7 | 1.5 | 10.5 |
| Robustness (OOS, walk-forward, multi-market) | 5 | 1.5 | 7.5 |
| Simplicity / low overfitting risk | 6 | 1 | 6 |
| Survived independent scrutiny | 4 | 1 | 4 |
| **Total** | | **14** | **89** |

**Score: 89 / 140 × 100 = 64 → WORTH RESEARCHING**

**Justification:** Academic paper with credible authors, clear methodology, and a plausible
economic rationale. Significant strengths (long backtest, alpha over passive, drawdown
reduction). Scored down for: no independent replication, long-only bias in bull market,
9 parameters without explicit walk-forward test, and incomplete rule specification in sources.

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Low-medium. Donchian channels are trivial to implement; the ensemble
  aggregation and volatility sizing add moderate complexity.
- **Scalability:** Moderate — top-20 liquid altcoins have sufficient volume; position sizing
  needs to account for market impact.
- **Automation:** High feasibility. Fully rule-based, no discretion required.

## MQL5 / Python / Pine Feasibility

- **Python:** High — Donchian channels and volatility scaling are standard; libraries like
  VectorBT or Backtrader can replicate with CCXT exchange data.
- **MQL5:** Medium — crypto CFDs available on some MT5 brokers; implementation is
  straightforward but liquidity/slippage on altcoins may differ from exchange data.
- **Pine Script:** High for signal generation; limited for multi-asset ensemble backtesting.

## Similar Strategies

- `orb-stocks-in-play.md` — breakout concept applied to equities (very different regime/market)
- AdaptiveTrend (arxiv 2602.11708) — more complex crypto trend system; see Future Research

## Red Flags

- Long-only bias inflates metrics in crypto's historically bull-biased environment.
- No walk-forward test confirmed in retrieved sources.

## Source Links

| Source | URL | Retrieved |
|--------|-----|-----------|
| Zarattini, Pagani, Barbon (2025) — SSRN 5209907 | https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5209907 | 2026-05-28 |
| Swiss Finance Institute Research Paper No. 25-80 | https://www.abarbon.com/papers/catching-crypto-trends | 2026-05-28 |
| QuantifiedStrategies.com coverage | https://www.quantifiedstrategies.com/crypto-trend-trading-strategy-for-bitcoin/ | 2026-05-28 |
| Semantic Scholar entry | https://www.researchgate.net/publication/391508704_Catching_Crypto_Trends_A_Tactical_Approach_for_Bitcoin_and_Altcoins | 2026-05-28 |

## Analyst Notes

**FACTS:** Paper published April 8, 2025, SSRN 5209907, SFI Research Paper 25-80. By same team
as ORB paper (Zarattini/Barbon). Daily timeframe, long-only. Nine Donchian lookbacks (5–360
days) aggregated. Universe: BTC + top-20 liquid coins (median daily vol ≥$2M, ≥1 year listed).
Backtest: Jan 2015 – Mar 2025. Combo model: CAGR 30%, Sharpe 1.58, Sortino 2.03, alpha 14%
vs BTC, max DD 19%. Altcoin rotation portfolio: Sharpe >1.5, alpha 10.8% vs BTC.

**ANALYSIS:** A 19% max drawdown vs. Bitcoin's 80%+ is the most compelling result here —
if robust, this represents genuine risk reduction. However, a 30% CAGR over 2015–2025 must
be evaluated knowing that passive Bitcoin exposure returned ~100%+ per year over the same
period, making 30% CAGR look modest in absolute terms (though the alpha is meaningful).
The Sharpe of 1.58 is plausible for a well-designed trend system but should be confirmed
with walk-forward analysis. The ensemble approach across 9 lookbacks is more robust than
any single Donchian model but is not immune to parameter fitting.

**ASSUMPTIONS (flagged):** Assuming the transaction cost model is realistic for exchange
trading (spot or perpetual futures). Assuming the altcoin liquidity filter ($2M/day) is
enforced consistently and not retroactively applied to surviving coins only (survivorship bias
in crypto is a significant concern).

## Future Research Needed

- Obtain full paper to verify exact exit rules and volatility scaling formula
- Examine whether explicit walk-forward testing was performed
- Assess survivorship bias methodology for altcoin universe
- Compare to AdaptiveTrend (arxiv 2602.11708) on same period/universe
- Build Python implementation to independently verify key metrics
- Evaluate performance in 2025–2026 (out-of-sample for the paper)
