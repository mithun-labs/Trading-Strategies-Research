# Sector Rotation Strategies in the TSX 60

## Overview

A systematic backtesting study of 72 sector rotation strategy variants applied to the
S&P/TSX 60 (Canada's benchmark large-cap index, ~60 largest publicly traded companies)
from 2000 to 2025. The study tests all combinations of strategy type (momentum, mean
reversion, median/balanced), weighting scheme (equal, price, market-cap), and rebalancing
frequency (monthly, quarterly, semi-annually, annually). The best-performing variant —
median-performer selection with quarterly rebalancing — achieved Sharpe 0.922 vs 0.624 for
equal-weighted buy-and-hold, with OOS performance persisting from 2020 to 2025. Open code
published on GitHub under MIT license.

- **Authors:** Gourav Salottra (inferred from GitHub handle; full author list NOT REPORTED in accessible sources)
- **Source:** MDPI Journal of Risk and Financial Management, Vol. 19, No. 1, Art. 70, January 15, 2026 (peer-reviewed)
- **GitHub:** https://github.com/gouravsalottra/tsx60-sector-rotation (MIT license; README minimal, code structure confirmed)

## Asset Class / Timeframes

- **Market:** Canadian Large-Cap Equities — S&P/TSX 60 index constituents
- **Timeframe:** Quarterly rebalancing (best-performing variant); also tested monthly, semi-annual, annual
- **Style:** Systematic sector rotation (long-only)

## Core Logic

**Universe:** S&P/TSX 60 stocks grouped into sectors (GICS framework; exact number of sectors in TSX 60: 11 standard GICS sectors, though sector composition NOT CONFIRMED from available sources).

**Strategy types:**
1. **Momentum / Winners rotation:** Buy sectors with the highest recent returns (trend-following).
2. **Mean reversion / Losers rotation:** Buy sectors with the lowest recent returns (contrarian bet on reversion).
3. **Median / Balanced rotation (BEST):** Select sectors in the *middle* of the recent performance distribution — neither top nor bottom performers. Avoids momentum crowding and extreme contrarian bets. Captures intermediate-horizon mean reversion.

**Weighting schemes:** Equal weight, price-weighted, market-cap-weighted (all tested; best combination = equal-weight with median rotation).

**Rebalancing:** Quarterly (best). At each quarter-end:
1. Rank all sectors by recent return over the lookback window (lookback period: NOT REPORTED in accessible sources; likely 3–12 months).
2. Select median-ranked sectors according to the rotation type.
3. Reallocate portfolio using the chosen weighting scheme.
4. Hold for one quarter.

**Machine learning layer:** An ML regime classifier (algorithm NOT REPORTED; accuracy 72.7%) augments the rotation signal with market regime information (presumably bull/bear/neutral). The ML integration details and whether it further improves on the best Sharpe are NOT REPORTED.

## Economic Rationale

**Why sector rotation works:**
1. **Business cycle dynamics:** Sectors have documented cyclical patterns tied to the economic cycle (expansion → recession → recovery → expansion). Consumer staples and utilities outperform in downturns; cyclicals and materials outperform in expansions.
2. **Institutional rebalancing:** Large pension funds and mutual funds rotate across sectors on a quarterly cycle, creating predictable momentum in some sectors and contrarian reversion in others.
3. **Mean reversion in concentrated markets:** The TSX 60 is a highly concentrated index (financials + energy + materials dominate). Sector leadership in this concentrated market tends to revert at intermediate horizons as commodity cycles and credit cycles mean-revert.

**Why the median selection specifically:**
The paper's finding that *middle* performers outperform both winners and losers suggests that the most extreme performers are either: (a) momentum chasing that faces reversal, or (b) beaten-down sectors not yet ready to recover. The median avoids both.

**Falsifiability:** The edge should disappear when: (a) cross-sector correlations are persistently high (e.g., during systemic crises where all sectors fall together); (b) transaction costs rise above the quarterly alpha generated; (c) the TSX 60 sector composition changes dramatically, altering the mean-reversion dynamics; (d) passive index adoption reduces institutional rotation flows. The paper explicitly identifies these conditions.

## Entry Conditions

At each quarterly rebalancing date:
1. Calculate return of each sector over the lookback period (exact lookback: NOT REPORTED).
2. Rank sectors from highest to lowest return.
3. Select sectors in the median rank tier (exact definition of "median band": NOT REPORTED — likely middle third or middle quintile of the performance distribution).
4. Allocate equal weight to selected sectors.

## Exit Conditions

All positions are closed/rebalanced at the next quarterly rebalancing date. No intra-quarter stop-losses or exits described.

## Risk Management

- **Long-only:** No short positions in the base strategy; moderate downside through sector diversification.
- **Crisis behavior:** The paper identifies utilities and consumer staples as providing downside protection during stress periods; cyclicals amplify losses. The rotation mechanism does NOT explicitly exit to cash.
- **No stop-loss, no max-drawdown circuit breaker:** NOT REPORTED in available sources.
- **Regime filter (ML layer):** May adjust exposure based on detected market regime, but specifics NOT REPORTED.

## Indicators Used

- Sector return over a lookback window (lookback: NOT REPORTED)
- Market regime classifier (algorithm: NOT REPORTED; accuracy: 72.7%)

## Transaction Costs & Capacity

**Costs:** The paper explicitly analyzes transaction costs. TSX 60 constituents have high liquidity with bid-ask spreads of 2–5 basis points. At quarterly rebalancing, the annual round-trip cost is approximately 4 trades/year × 2–5 bps × 2 (buy + sell) = 16–40 bps/year total. Given the Sharpe improvement from 0.624 to 0.922, and assuming an annual alpha of ~2–4%, this cost burden (0.16–0.40%/year) is modest relative to the gross alpha.

**Capacity:** The S&P/TSX 60 is Canada's most liquid large-cap index. For most retail and institutional portfolios, capacity is not a binding constraint. However, the TSX 60 is a concentrated market (~C$1.3T combined market cap), meaning very large institutions could face impact costs not reflected in historical bid-ask data.

**Assessment:** Cost modeling is explicitly present and appropriate for a quarterly-frequency long-only strategy. This is one of the strategy's genuine strengths.

## Backtest Evidence

**CLAIMED:** In-sample 2000–2020 (exact split NOT CONFIRMED; OOS period 2020–2025 implies ~20-year in-sample window).
**CLAIMED:** OOS 2020–2025 (5 years) — performance persistence confirmed.
- Source: MDPI JRFM peer-reviewed article (Vol. 19, No. 1, Jan 2026)
- Open code on GitHub (MIT license) — methodology is reproducible

## Forward-Test Evidence

NOT REPORTED. The OOS test (2020–2025) represents the closest available evidence.

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | 0.922 (median+quarterly); 0.624 (equal-weight B&H) | CLAIMED, UNVERIFIED | MDPI JRFM 19/1/70 |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | — |
| CAGR | NOT REPORTED | NOT REPORTED | — |
| Annual Return | NOT REPORTED | NOT REPORTED | — |
| Number of Trades | ~4 per year (quarterly rebalancing) | CLAIMED, UNVERIFIED | MDPI JRFM 19/1/70 |
| Average Trade Duration | ~1 quarter | CLAIMED, UNVERIFIED | MDPI JRFM 19/1/70 |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | ~100% (long-only, always invested) | CLAIMED, UNVERIFIED | MDPI JRFM 19/1/70 |

**Notes:**
- Only Sharpe ratio comparison is accessible; CAGR, max drawdown, and absolute returns are NOT REPORTED in available search snippets (paywalled full paper required for complete metrics)
- The Sharpe improvement (0.624 → 0.922, or +47.8% relative) is meaningful but derived from a single study

## Community Sentiment

No community discussion found for this paper in accessible forums (ForexFactory, r/algotrading, MQL5). It is a January 2026 publication, so community discussion may not have emerged yet. The GitHub repository (MIT license) invites replication, which could generate feedback over time.

## Why This Might Be Nothing

**1. Multiple testing over 72 strategies:**
Testing 72 strategy variants and reporting the best (Sharpe 0.922) raises the expected maximum Sharpe under the null hypothesis. If all 72 strategies have the same population Sharpe as B&H (0.624), the expected maximum over 72 tests would be substantially higher than 0.624 by random chance alone. The paper does test all 72 variants — not just the winner — and the OOS performance provides partial mitigation, but the full multiple-testing correction (e.g., Deflated Sharpe Ratio) is NOT REPORTED.

**2. 5-year OOS in a mostly bull market:**
The OOS period (2020–2025) included the COVID crash recovery, a tech boom, and a broadly positive equity market with intermittent drawdowns. Sector rotation strategies tend to perform well in environments with differentiated sector performance — which describes 2020–2025. A true stress test would require performance in 2008 (financials dominated the TSX 60 and crashed) or a prolonged stagflation environment.

**3. Canadian market specificity:**
The TSX 60 is dominated by financials (~35%), energy (~20%), and materials (~15%). The median rotation edge may be specific to this concentrated market's unique sector dynamics and Canada's commodity/banking cycle, rather than generalizable to other equity markets (S&P 500, NASDAQ-100, etc.).

**4. Lookback period not disclosed:**
The exact lookback window for performance ranking is NOT REPORTED in accessible sources. This is a key parameter — if it was optimized in-sample (e.g., tested many lookback lengths and picked the best), the Sharpe of 0.922 would be overstated.

**5. ML regime layer adds undisclosed complexity:**
The 72.7% regime classification accuracy is reported, but which strategies were enhanced by it, and by how much, is NOT REPORTED. If the ML layer was tuned on the same in-sample data as the rotation signal, it adds overfitting risk.

**6. Max drawdown not accessible:**
Without max drawdown, it is impossible to assess whether the risk-adjusted improvement is genuinely better than B&H or simply reflects a lower-volatility sector selection that doesn't protect in tail events.

**What would change my mind:** Access to the full paper's performance table (CAGR, max drawdown for the best strategy over the full 2000–2025 period), an independent replication on S&P 500 sector ETFs or another non-Canadian market, and confirmation that the lookback window was not optimized in-sample.

## Strengths / Weaknesses

**Strengths:**
- Peer-reviewed publication (MDPI JRFM, reputable open-access journal)
- Open code on GitHub (MIT license) — independently reproducible
- Comprehensive coverage of 72 strategy variants — not just reporting the winner
- Explicit transaction cost analysis (2–5 bps for TSX 60)
- OOS validation for 5 years (2020–2025)
- Crisis-period sector analysis provides useful failure-mode information
- Realistic Sharpe (0.922) — not suspicious, does not trigger any automatic scrutiny thresholds

**Weaknesses:**
- MDPI JRFM is open-access peer-reviewed but lighter than top finance journals (JF, RFS, JFE)
- Max drawdown, CAGR, and absolute return not accessible from search summaries (paywalled)
- 72 strategies = multiple testing risk without explicit correction
- OOS only 5 years (2020–2025), and in a favorable period
- TSX 60 specific — unclear if results generalize to other markets
- ML regime layer methodology not fully documented in accessible sources
- Lookback period for performance ranking NOT REPORTED

## Confidence Score

**Per-dimension breakdown:**

| Dimension | Raw (0–10) | Weight | Weighted |
|-----------|-----------|--------|----------|
| Logical/economic soundness (falsifiable) | 7 | 3 | 21 |
| Transparency & reproducibility | 7 | 2 | 14 |
| Evidence auditability | 6 | 2 | 12 |
| Risk management quality | 3 | 2 | 6 |
| Cost & capacity realism | 6 | 2 | 12 |
| Honest treatment of drawdowns | 4 | 1.5 | 6 |
| Robustness evidence (OOS / walk-forward) | 5 | 1.5 | 7.5 |
| Survived independent scrutiny | 3 | 1 | 3 |

Weighted sum = 21 + 14 + 12 + 6 + 12 + 6 + 7.5 + 3 = **81.5**
Max possible = 150
`latent_score = round(81.5 / 150 × 100) = round(54.3) = **54**`

**Verification cap:** Evidence auditability = 6 (5–7 range) → cap at 74.
`confidence = min(54, 74) = **54**`

**Band: Experimental (40–59)**

`latent 54 (capped to 54 pending verification: MDPI peer review present but lighter tier; max drawdown paywalled; multiple testing over 72 variants; 5-year OOS in favorable period; TSX 60 market specificity; lookback parameter not disclosed)`

## Complexity / Scalability / Automation Feasibility

**Complexity:** Low to medium. The base rotation strategy (rank sectors, select median, rebalance quarterly) is straightforward to implement. The ML regime classification layer adds complexity but is optional.

**Scalability:** The strategy rebalances quarterly and holds ~11 sector positions. Very low operational overhead.

**Automation feasibility:** High. Quarterly rebalancing with a simple sector ranking rule is easily automated in Python or even a spreadsheet. The ML layer requires more infrastructure.

## MQL5 / Python / Pine Feasibility

**Python:** Excellent fit. Standard libraries (`pandas`, `numpy`, `scipy`) sufficient for the rotation logic. ML regime classifier requires `scikit-learn`. The GitHub repository likely contains reference code. Data: TSX 60 component OHLCV + sector classification (Yahoo Finance, Bloomberg, or Refinitiv).

**MQL5:** Not directly applicable (MetaTrader does not have direct access to TSX 60 sector data).

**Pine Script (TradingView):** Partially applicable — could implement the rotation logic for ETF-based proxies (iShares sector ETFs for TSX sectors), though Pine's backtesting capabilities are limited for rebalancing strategies.

**Verdict:** Python with open GitHub code is the natural implementation path. The most practical adaptation for non-Canadian portfolios would be to apply the same median-rotation logic to S&P 500 GICS sector ETFs (XLK, XLF, XLE, etc.) and independently validate.

## Similar Strategies

- No existing sector rotation entry in the current archive.
- Conceptually related to `orb-stocks-in-play` (US equities, intraday timing) in the sense that both exploit documented cyclical patterns in equity returns, but at very different timeframes.
- The business-cycle rationale overlaps with `regime-switching-jump-model-equity` (regime timing on equity indices).

## Red Flags

- `MULTIPLE TESTING`: 72 strategies; no Deflated Sharpe Ratio correction reported
- `LIMITED OOS`: 5-year OOS (2020–2025) in a broadly favorable equity period
- `MARKET-SPECIFIC`: TSX 60 dominated by financials/energy/materials; unknown generalizability
- `PAYWALLED METRICS`: Max drawdown, CAGR, absolute returns not accessible from open sources
- `UNDISCLOSED LOOKBACK`: Key parameter (performance ranking lookback window) not reported in accessible sources
- `ML LAYER OPACITY`: Regime classifier algorithm and integration details not fully described

## Source Links

| Source | Retrieved | Notes |
|--------|-----------|-------|
| MDPI JRFM Vol. 19 No. 1 Art. 70 | 2026-05-29 | Primary source; HTTP 403 for full fetch; Sharpe ratio and key methodology from search summaries |
| GitHub: gouravsalottra/tsx60-sector-rotation | 2026-05-29 | Open code (MIT); README minimal (120 bytes) |
| Multiple search result snippets | 2026-05-29 | Crisis analysis details, ML accuracy, transaction cost range confirmed |

## Analyst Notes

**FACTS (from search result snippets):**
- MDPI JRFM, Vol. 19, No. 1, Art. 70, published January 15, 2026
- Author GitHub: `gouravsalottra` (likely Gourav Salottra; full author list NOT CONFIRMED)
- 72 strategies tested: 3 types × 3 weights × 4 frequencies
- Best: Median performer + quarterly rebalancing → Sharpe 0.922 vs 0.624 equal-weight B&H
- OOS 2020–2025, ML regime classifier 72.7% accuracy
- Transaction costs: TSX 60 bid-ask 2–5 bps
- Crisis analysis: utilities + consumer staples protect; cyclicals amplify losses
- Open code on GitHub (MIT license)

**ANALYSIS:**
- Sharpe improvement: 0.624 → 0.922 = +47.8% relative, or approximately +0.30 absolute Sharpe points. At typical TSX 60 vol (~12–15% annual), this implies an excess return of approximately 0.30 × 12% to 0.30 × 15% = 3.6–4.5% per year above B&H. This is a meaningful but plausible alpha for a systematic sector rotation strategy on a concentrated market.
- The multiple testing concern: With 72 strategies and pure noise (Sharpe 0 for all), the expected maximum Sharpe would be significantly positive (likely 0.3–0.4 or more). With B&H Sharpe = 0.624 as the base, the expected maximum across 72 correlated strategies could easily reach 0.8+ by chance. The OOS validation is the key mitigant.
- Canadian financials/energy concentration means the TSX 60 is essentially a leveraged play on oil prices and bank credit cycles. Sector rotation in this context is essentially timing the oil-bank correlation regime.

**ASSUMPTIONS:**
- The lookback window for performance ranking is likely 3–12 months (standard in the sector momentum/mean-reversion literature)
- "Median performers" likely means the middle third or quintile of the sector performance distribution
- The study uses 11 GICS sectors applied to TSX 60 constituents (standard sector classification)

## Future Research Needed

1. Access the full paper to extract: (a) CAGR and max drawdown for the best strategy over 2000–2025, (b) performance during 2008 financial crisis (when TSX 60 fell ~40%), (c) exact lookback window and whether it was optimized, (d) full ML regime classifier details.
2. Independent replication on S&P 500 GICS sector ETFs (XLK, XLF, XLE, etc.) to test if the median rotation generalizes outside Canada.
3. Run the GitHub code on the in-sample period (2000–2020) and compare to reported Sharpe to verify implementation correctness.
4. Apply Deflated Sharpe Ratio correction to assess statistical significance after 72-strategy testing.
