# Cryptocurrency Volume-Weighted Time Series Momentum

## Overview

A modification of the classical Time Series Momentum (TSMOM) strategy applied to
cryptocurrency markets, substituting volume-weighted market returns for equal-weighted
returns in the signal construction. The paper claims that volume-weighting identifies
"informed" trading more accurately than equal-weighting and that long-short portfolios
sorted on this signal generate returns that outperform standard TSMOM and are not
explained by common crypto factors or wash-trading volume.

**Authors:** Zih-Chun Huang, Ivan Sangiorgi, Andrew Urquhart (University of Birmingham /
University of Reading)
**Paper:** SSRN 4825389 (December 2024)
**Status:** SSRN working paper; not peer-reviewed as of 2026-06-12 (18 months on SSRN)

---

## Asset Class / Timeframes

- **Asset class:** Cryptocurrency
- **Markets:** Cryptocurrencies listed on Binance (implied from author group's prior work)
- **Timeframe:** Daily (inferred; paper follows TSMOM literature conventions)
- **Position holding:** NOT REPORTED (TSMOM literature typically 1-week to 1-month)

---

## Core Logic

The paper extends the classical TSMOM framework (Moskowitz, Ooi, Pedersen 2012) to
cryptocurrency markets by weighting returns by trading volume rather than computing
equal-weighted signals:

1. Compute a *volume-weighted* market return across the cross-section of cryptocurrencies
2. For each coin, compare its return against the volume-weighted benchmark
3. Coins whose returns are supported by above-average volume are classified as "winners";
   coins underperforming on volume are "losers"
4. Go long winner portfolio, short loser portfolio (zero-cost long-short)

The claimed innovation: volume-weighting filters out noise-driven returns and retains
returns driven by active, informed trading. Volume-weighted winner minus loser portfolios
outperform equal-weighted TSMOM strategies.

**Note:** The exact signal formula (how volume-weighting is applied, specific lookback
periods, hold periods, universe filters, and portfolio weighting) is NOT REPORTED in any
accessible secondary source as of 2026-06-12. This description is ANALYST RECONSTRUCTION
from secondary search summaries of the paper's abstract.

---

## Economic Rationale

**Proposed mechanism:** In cryptocurrency markets, trading volume reveals the intensity
of market participants' conviction. When a coin generates positive returns accompanied by
high volume, the momentum signal is interpreted as reflecting informed buying interest
rather than thin-market noise. Volume-weighted momentum therefore identifies "smart money"
following a trend.

**Falsifiability test:** The rationale is partially falsifiable. The edge should disappear
when:
- Cryptocurrency markets mature and wash trading is systematically eliminated (already
  partially addressed in the paper itself)
- Institutional market-making improves price discovery, reducing momentum persistence
- Exchange competition compresses spreads to near zero, making volume a less informative
  signal
- Token issuance creates supply-side disruptions that obscure volume signals

However, the rationale does not precisely specify the market conditions or institutional
frictions that generate the edge (beyond "volume = information"). ANALYSIS: This is
partially falsifiable — the wash-trading counter-argument is explicitly addressed, which
is a positive. But the broader mechanism ("volume reveals informed trading") is a general
story that does not uniquely predict when this specific signal form outperforms.

**Assessment:** Pattern-adjacent economic rationale. The mechanism is plausible but not
precisely specified enough to score at the top tier of the falsifiability scale.

---

## Entry Conditions

NOT REPORTED (exact signal formula, lookback period, and entry triggers are not accessible
from secondary sources).

Based on the TSMOM literature framework:
- ASSUMPTION: Signal computed over a lookback window (likely 1–12 months)
- ASSUMPTION: Volume-weighted market return computed as cross-sectional average of
  daily returns weighted by each coin's trading volume
- ASSUMPTION: Long coins whose volume-weighted signal is positive; short coins whose
  volume-weighted signal is negative

---

## Exit Conditions

NOT REPORTED.

ASSUMPTION: Fixed holding period (likely 1 week to 1 month, consistent with TSMOM
literature).

---

## Risk Management

NOT REPORTED. The paper follows academic portfolio construction conventions; no explicit
stop-losses, maximum position sizes, or regime exit rules are described in accessible
secondary sources.

---

## Indicators Used

- Daily returns per cryptocurrency
- Daily trading volume per cryptocurrency
- Volume-weighted market return (cross-sectional; construction NOT REPORTED)

---

## Transaction Costs & Capacity

**Transaction costs:** NOT REPORTED from accessible secondary sources. The paper does
not confirm explicit modeling of spread, slippage, commission, or funding rates in any
accessible secondary summary.

ANALYSIS: This is a critical gap. For daily-rebalanced long-short crypto portfolios,
realistic costs include:
- Exchange maker/taker fees (0.1% each side on Binance spot; higher on perpetuals)
- Slippage (particularly in smaller altcoins)
- Funding rate for perpetuals
- Bid-ask spread

A 0.94%/day portfolio return could easily be consumed by 0.2–0.4% round-trip cost
on any moderate turnover rate. If the strategy rebalances daily across many coins,
the cost profile would be substantial.

**Capacity:** NOT REPORTED. Academic long-short portfolios often show size-dependent
degradation; smaller coins with higher volume-weighting may not support institutional
size.

---

## Backtest Evidence

Source: SSRN 4825389 secondary summaries only. Primary paper HTTP 403 as of 2026-06-12.

**Evidence quality: CLAIMED, UNVERIFIED**

The paper reports an in-sample backtest over an unspecified data period. Full backtest
details (data period, coin universe, number of coins, transaction cost treatment) are
NOT ACCESSIBLE from secondary summaries.

---

## Forward-Test Evidence

NOT REPORTED.

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | 2.17 (annualized) | CLAIMED, UNVERIFIED | SSRN 4825389 secondary summaries |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | — |
| CAGR | NOT REPORTED | NOT REPORTED | — |
| Annual Return | 0.94%/day (raw portfolio return) | CLAIMED, UNVERIFIED | SSRN 4825389 secondary summaries |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | NOT REPORTED | NOT REPORTED | — |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | NOT REPORTED | NOT REPORTED | — |

### Metric Notes

- **0.94%/day:** This is a portfolio-level daily return figure for the zero-cost long-short
  winner-minus-loser portfolio. ANALYSIS: This implies annualized gross return of approximately
  (1.0094)^252 − 1 ≈ 970% for a zero-cost portfolio, or linearly ~237%/year. For an academic
  long-short portfolio in crypto, these numbers, while extreme, are not unknown in the
  literature — but they are gross of all transaction costs and reflect aggregate factor exposure,
  not deliverable investor returns.
- **Sharpe 2.17:** With daily return 0.94% and annualized Sharpe 2.17, implied daily volatility
  is approximately 0.94% / (2.17 / √252) ≈ 6.9% per day. ANALYSIS: This is elevated daily
  volatility (consistent with a long-short altcoin portfolio), but the Sharpe is below the 3.0
  scrutiny threshold so does not trigger mandatory extreme-scrutiny.
- **No transaction cost confirmation:** The Sharpe must be treated as gross. There is no
  confirmation from any accessible source that costs are modeled.

---

## Community Sentiment

No direct community scrutiny of SSRN 4825389 is available from accessible sources as of
2026-06-12. The paper has not appeared in major forum discussions or independent replication
attempts.

**Relevant general context:**
- "Cryptocurrency momentum has (not) its moments" (Springer FMPM 2025) documents that
  volatility-managed crypto momentum strategies show parameter instability and weaker
  OOS Sharpe ratios, challenging the robustness of crypto momentum returns in general.
- Han/Kang/Ryu (SSRN 4675565, 2023 — already in our database as `crypto-ts-xs-momentum-realistic`)
  show that cross-sectional crypto momentum fails after realistic 15bps costs, while TS
  momentum survives. This directly relevant context suggests costs are a decisive factor in
  crypto momentum.
- Wash trading on crypto exchanges remains a known structural concern, though the paper
  explicitly addresses this objection.

---

## Why This Might Be Nothing

### 1. Most Likely Benign Explanation: Transaction Costs

The 0.94%/day paper return almost certainly disappears after transaction costs. A daily-
rebalanced long-short portfolio on Binance incurs round-trip costs of at least 0.20% per
trade (assuming 0.10% maker fee each side, ignoring slippage). If each coin position is
turned over daily, costs at 0.20%/day would consume about 21% of the 0.94%/day gross.
With typical altcoin slippage and spread, the effective cost is likely 0.30–0.50%/trade
or more for smaller coins, potentially consuming all or most of the gross edge. Directly
parallel: Han/Kang/Ryu found that XS crypto momentum fails entirely after 15bps — and
the VWTS signal relies on volume-weighting of the same momentum signal.

### 2. No Transaction Cost Confirmation

No accessible secondary source confirms that the paper models transaction costs. The
absence of this disclosure, combined with a daily-rebalanced strategy on volatile crypto,
is the single most important red flag. A published Sharpe of 2.17 gross might be zero net.

### 3. Regime Dependence

The paper's data period is unknown, but December 2024 submission suggests data likely
through mid-2024. This covers the 2020-2021 bull market, 2022 crash, and 2023-2024
recovery. ANALYSIS: The 2020-2021 period produced enormous momentum returns in crypto.
If the signal construction was calibrated in-sample over this period, reported performance
may reflect crypto market regime luck more than a persistent edge.

### 4. Wash Trading Residuals

The paper argues results are "not the result of wash trading volume." However, detecting
wash trading volume is an open research problem. Even if major wash trading (on unregulated
exchanges) is excluded, residual self-trading and spoofing on Binance may contaminate the
volume signal in ways the paper's filter doesn't fully address.

### 5. 18 Months Without Journal Publication

The paper was submitted in December 2024 and has been on SSRN for 18 months without
confirmed peer review acceptance as of June 2026. Following the pattern in our database
(fractional momentum papers: 3+ years; factor timing paper: not peer-reviewed), SSRN
residence time is an inverse proxy for evidence quality. Persistent methodological
issues — often including transaction cost treatment — frequently delay publication.

### 6. What Would Change My Mind

An independent walk-forward replication with:
- Explicit cost modeling (spread + commission + slippage per coin, by coin size)
- Data separated into in-sample and out-of-sample (e.g., 2015-2020 in-sample, 2021-2024 OOS)
- Independent confirmation on a second exchange (not Binance)
- Open code with the exact signal formula

None of these are available.

---

## Strengths / Weaknesses

**Strengths:**
- Novel mechanism with plausible economic rationale (volume reveals information)
- From a credible research group (Urquhart has multiple peer-reviewed crypto finance papers)
- Explicitly addresses wash trading concern
- Factor robustness reported (not explained by market, size, momentum factors)

**Weaknesses:**
- No confirmed transaction cost modeling
- Maximum drawdown not reported
- Exact signal formula not accessible from secondary sources
- 18 months on SSRN without peer review
- Single-market test (Binance implied; no multi-exchange robustness)
- 0.94%/day gross return likely collapses after costs

---

## Confidence Score

| Dimension | Score (0–10) | Weight | Weighted |
|---|---|---|---|
| Logical / economic soundness (falsifiable) | 4 | 3 | 12 |
| Transparency & reproducibility | 4 | 2 | 8 |
| Evidence auditability | 2 | 2 | 4 |
| Risk management quality | 2 | 2 | 4 |
| Cost & capacity realism | 2 | 2 | 4 |
| Honest treatment of drawdowns / failure | 2 | 1.5 | 3 |
| Robustness evidence (OOS / walk-forward / multi-market) | 3 | 1.5 | 4.5 |
| Survived independent scrutiny | 2 | 1 | 2 |

**Weighted sum:** 41.5 / 150 maximum

**Latent score:** round(41.5 / 150 × 100) = **28**

**Verification cap:** Evidence auditability score = 2 ≤ 4 → cap at 59.
Latent 28 < 59, so cap does not bind.

**Confidence: 28 (Low Confidence)**

`latent 28 (capped to 28 — evidence auditability 2/10; no confirmed cost modeling; max DD NOT REPORTED; 18 months on SSRN without peer review; exact formula NOT ACCESSIBLE)`

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Moderate — requires cross-sectional coin universe, daily volume data, and
  signal computation. More complex than single-asset TSMOM; less complex than multi-factor models.
- **Scalability:** Constrained — smaller altcoins with higher volume-weighting may not support
  large position sizes. Trade capacity unknown.
- **Automation:** Feasible in Python with Binance API; CCXT library supports universe management.

---

## MQL5 / Python / Pine Feasibility

- **Python:** Feasible. Standard crypto data APIs (Binance API, CCXT) provide daily OHLCV.
  Signal construction requires the exact formula from the paper (NOT ACCESSIBLE from secondary
  sources). Not implementable without obtaining the full paper.
- **MQL5:** Not straightforward — MQL5 is optimized for single-instrument MT5 trading, not
  cross-sectional portfolio construction.
- **Pine Script:** Not feasible — Pine Script does not support cross-sectional universe operations.

---

## Similar Strategies

- `crypto-ts-xs-momentum-realistic` (score 55) — Han/Kang/Ryu (SSRN 4675565); TS vs XS
  momentum on ~471 Binance coins; 15bps costs explicitly modeled; TS momentum survives costs
  but XS does not. **Closest relative.** VWTS momentum is a variant on the TS framework.
- `catching-crypto-trends-donchian-ensemble` (score 64) — Zarattini/Pagani/Barbon; Donchian
  channel ensemble + vol sizing; applies to BTC + altcoins; implementation candidate.
- `bitcoin-max-min-trendrev` (score 45) — Bitcoin-specific rolling MAX/MIN price signals.
- `adaptivetrend-crypto` (score 41) — Multi-component crypto trend following with asset selection.

---

## Red Flags

- **No transaction cost confirmation** on a daily-rebalanced long-short strategy → presumptively
  invalid until costs are demonstrated not to erase the edge
- **0.94%/day gross is extreme**: implies ~237% annualized raw return; all of this could be
  consumed by costs
- **18 months on SSRN without peer review** — inverse quality proxy from our database
- **Single market test** (Binance only implied) — no multi-exchange robustness
- **Data period unknown** — may coincide with the 2020-2021 crypto bull market that produces
  inflated momentum returns across all strategies
- **Exact formula NOT REPORTED** — not implementable from secondary sources alone

---

## Source Links

| Source | Retrieval Date | Notes |
|--------|---------------|-------|
| [SSRN 4825389 — Huang/Sangiorgi/Urquhart (Dec 2024)](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4825389) | 2026-06-12 | Primary paper; HTTP 403 on direct access; abstract/metadata from secondary summaries |
| [Cryptocurrency momentum has (not) its moments — Springer FMPM 2025](https://link.springer.com/article/10.1007/s11408-025-00474-9) | 2026-06-12 | Community context; crypto momentum instability documented |
| [crypto-ts-xs-momentum-realistic strategy file](../strategies/crypto-ts-xs-momentum-realistic.md) | 2026-06-12 | Closest relative; Han/Kang/Ryu; costs modeled; XS fails; TS survives |

---

## Analyst Notes

**FACTS (from accessible secondary sources):**
- Paper authored by Huang, Sangiorgi, Urquhart; posted to SSRN December 4, 2024
- Claims 0.94% per day portfolio return and annualized Sharpe 2.17
- Uses volume-weighted market return in TSMOM framework
- Creates winner minus loser long-short portfolios
- Results not explained by market, size, momentum factors
- Results not attributed to wash trading volume

**ANALYSIS:**
- 0.94%/day for a zero-cost long-short portfolio implies ~237% annualized linear or ~970%
  compounded annualized — these are gross academic portfolio returns, not investable returns
- Sharpe 2.17 with 0.94%/day return implies ~6.9% daily portfolio volatility, consistent
  with a leveraged altcoin long-short portfolio
- Without transaction cost confirmation, this strategy is presumptively un-investable under
  realistic market conditions (cf. Han/Kang/Ryu's finding that XS momentum fails at 15bps)
- Urquhart's track record includes peer-reviewed crypto papers (e.g., Journal of Finance
  Letters, Finance Research Letters), lending credibility to the research group but not
  guaranteeing this particular paper's results

**ASSUMPTIONS:**
- Binance as primary exchange (inferred from author group's prior data usage)
- Daily rebalancing (inferred from "time series momentum" and Sharpe computation methodology)

---

## Future Research Needed

1. Obtain full paper to verify: (a) exact volume-weighting formula, (b) data period,
   (c) coin universe, (d) transaction cost treatment
2. Independent replication on alternative exchange data with explicit cost modeling
3. Subperiod analysis: pre-2020 vs 2020-2021 bull vs 2022 bear vs 2023-2024 recovery
4. Comparison with existing `crypto-ts-xs-momentum-realistic` (Han/Kang/Ryu) using
   matched data period to isolate volume-weighting contribution
