# Cointegration-Based Pairs Trading (ETF Application; FX Candidate)

## Overview

Chen and Alexiou (Journal of Asset Management, August 2025) examine cointegration-based
pairs trading on 30 ETF pairs over 2000–2024. Their dynamic approach monthly reassesses each
pair's cointegration validity using the trailing 12 months, extends the trading window when
cointegration holds, and exits when it breaks. The paper is notable for its intellectual
honesty: the authors explicitly state that overall profitability across 30 pairs is "limited"
and that short cointegration windows restrict long-term performance. The portfolio Sharpe
ratio is ~0.28 (at z-score threshold 2), representing a near-null result.

The strategy is noted here as a potential **FX candidate** because the cointegration
framework translates directly to currency pairs (EURUSD/GBPUSD share USD exposure;
AUDUSD/NZDUSD share commodity-currency exposure), though the paper itself does not
test FX crosses.

**Source paper**: "Cointegration-based pairs trading: identifying and exploiting similar
exchange-traded funds", Kezhong Chen and Constantinos Alexiou, *Journal of Asset Management*,
Volume 26, Issue 5, pages 464–488, August 2025. **Peer-reviewed.**

---

## Asset Class / Timeframes

- **Primary market (paper)**: US Equities — Exchange-Traded Funds (30 ETF pairs; specific
  list not fully disclosed in accessible sources; includes SPY-IVV among others)
- **Potential FX application (ANALYSIS, not from paper)**: Forex — cointegrated currency
  cross pairs (EURUSD/GBPUSD, AUDUSD/NZDUSD, etc.)
- **Data period**: 2000–2024 (24 years)
- **Timeframe**: Daily execution; monthly reassessment of pair validity

---

## Core Logic

**Step 1 — Pair selection**:
Select pairs of ETFs that track economically related assets (e.g., similar sector ETFs,
two S&P 500 trackers, correlated bond ETFs). The paper's 30 pairs include SPY-IVV as
one example.

**Step 2 — Monthly cointegration test**:
At each month-end, test whether the pair remains cointegrated using the trailing 12-month
window (Engle-Granger test, implied by the paper's dynamic window description). If the
p-value exceeds the significance threshold, close the pair's position and stop trading.
If cointegration holds, continue.

**Step 3 — Spread construction and z-score**:
Form the spread: S_t = log(P1_t) - β × log(P2_t), where β is the cointegrating coefficient
estimated from the trailing window. Compute the z-score of the spread: z_t = (S_t - μ) / σ.

**Step 4 — Entry and exit**:
- Entry: go long the spread when z_t < −threshold (spread is cheap) or short when z_t > +threshold
  (spread is expensive). Equal-share-count positions (paper uses equal number of shares/units)
- Exit: close when z_t reverts to near 0 (e.g., |z_t| < 0.5 or similar exit band)
- Stop-cointegration exit: close immediately if monthly reassessment fails cointegration test

**Step 5 — Threshold sensitivity**:
Lower z-score thresholds generate more trades, higher gross profits and Sharpe ratios, but
also more volatility and drawdowns. Threshold choice is a key implementation parameter.

---

## Economic Rationale

**Stated mechanism**: Economically related assets share common fundamental drivers (e.g.,
two ETFs both tracking the S&P 500 face the same underlying earnings, rates, and risk appetite).
When temporary mispricing widens the spread between them (idiosyncratic flows, index
rebalancing friction, arbitrageur capacity limits), mean reversion occurs as arbitrageurs
close the gap. The cointegration framework formalizes this relationship: if two assets
are cointegrated, their spread is stationary and reverts to a long-run equilibrium.

**For FX application (ANALYSIS)**: Currency pairs that share a common driver (USD exposure,
commodity sensitivity, monetary policy regime) can be cointegrated. EURUSD and GBPUSD both
reflect EUR/GBP strength against the USD; when one overshoots relative to the other, mean
reversion toward the fundamental EURGBP equilibrium creates a pairs trade.

**When this edge should fail (falsifiability)**:
1. When the economic relationship between the pair breaks: index reconstitution, product
   closure, regulatory change, or structural economic shift (e.g., Brexit permanently
   severing EURUSD/GBPUSD correlation patterns)
2. When arbitrage capacity expands to the point of eliminating the spread (HFT arbitrage
   in near-identical ETFs, e.g., SPY-IVV-VOO)
3. During risk-off market crises: correlations collapse, cointegration breaks, stop exits
   multiply (systematic stop-out at exactly the worst time)
4. When the z-score threshold is not adaptive: a threshold appropriate in low-volatility
   regimes may generate too many false signals in high-volatility periods

---

## Entry Conditions

1. Monthly cointegration test passes (p-value below significance level, trailing 12 months)
2. Spread z-score exceeds entry threshold (default: |z| > 2; higher Sharpe at lower thresholds
   but higher volatility)
3. Go long asset with depressed spread component, short asset with elevated component
4. Equal share count positions (not dollar-neutral — equal shares of each leg)

---

## Exit Conditions

1. Z-score reverts toward zero (exit band: typically |z| < 0.5 or 0)
2. Monthly cointegration test fails → immediately close position (forced stop-loss)
3. Time-based stop: paper uses adaptive windows; trade terminates when cointegration fails

---

## Risk Management

- **Cointegration validity gate**: monthly reassessment is the primary risk control — positions
  are force-exited when statistical relationship breaks
- **Equal-share sizing**: implied from paper's equal-share portfolio construction
- **No explicit stop-loss beyond cointegration break**: NOT REPORTED in accessible sources
- **No leverage described**: implied 1× unleveraged long-short
- **No position limits, sector concentration limits, or max drawdown rules described**

---

## Indicators Used

- Engle-Granger cointegration test (p-value test, trailing 12-month window)
- Cointegrating coefficient β (hedge ratio, estimated from trailing window)
- Spread z-score: z_t = (S_t - μ) / σ (trailing mean and standard deviation)
- Z-score entry threshold (varied in paper; 1.5 and 2.0 explicitly mentioned)

---

## Transaction Costs & Capacity

- **Spread/slippage/commission modeled?**: NOT EXPLICITLY REPORTED in accessible summaries.
  ANALYSIS: The paper's conclusion that "overall profitability is limited" likely reflects
  implicit cost reduction in net results. ETF bid-ask spreads are narrow (1–2 bps for
  SPY/IVV; wider for sector ETFs). The strategy appears to have limited gross edge even
  pre-cost; post-cost it is close to break-even.
- **FX application costs**: Forex spreads for major crosses (EURUSD, GBPUSD) are 0.1–0.5
  pips, low enough that a pairs trade with sufficient edge could survive costs. Monthly
  rebalancing frequency limits cost drag.
- **Capacity**: High for ETF application (ETFs are deep). Medium for FX (spread between two
  currencies can be traded in large size via FX forwards/swaps at institutional level).
- **Key cost concern**: Equal-share (not dollar-neutral) positioning means the strategy
  may have significant net market exposure, reducing the mean-reversion benefit.

---

## Backtest Evidence

- **Period**: 2000–2024 (24 years)
- **Universe**: 30 ETF pairs
- **Status**: CLAIMED, UNVERIFIED from secondary sources for specific metrics;
  AUDITABLE methodology (peer-reviewed Journal of Asset Management, publicly verifiable
  ETF data, documented Engle-Granger methodology, 24-year period)
- Note: SPY-IVV is the best-performing pair but is effectively a near-zero-spread
  quasi-arbitrage (both track the S&P 500 from different providers)

---

## Forward-Test Evidence

NOT REPORTED.

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio (30-pair portfolio, z=2) | ~0.28 | CLAIMED, UNVERIFIED | Web search summary; primary paper HTTP 403 |
| Sharpe Ratio (lower threshold variant) | ~0.37–0.45 | CLAIMED, UNVERIFIED | Web search summary of JAM 2025 paper |
| Sharpe Ratio (SPY-IVV best pair) | ~1.5 | CLAIMED, UNVERIFIED | Web search summary; not representative |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | — |
| CAGR | NOT REPORTED | NOT REPORTED | — |
| Annual Return (30-pair portfolio) | NOT REPORTED | NOT REPORTED | — |
| Annual Return (SPY-IVV) | ~29% total over period? | CLAIMED, UNVERIFIED | Ambiguous from search snippet |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | NOT REPORTED | NOT REPORTED | — |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | NOT REPORTED | NOT REPORTED | — |

**Scrutiny note**: Portfolio Sharpe 0.28 triggers no automatic scrutiny thresholds, but
this is because it is so close to zero as to represent a near-null result. A Sharpe of
0.28 is below what most investors require as a minimum threshold for systematic strategy
deployment. The SPY-IVV Sharpe of 1.5 is an artifact of near-zero spread risk, not
informative about the general strategy.

---

## Community Sentiment

- Reviewed by HarbourfrontQuant blog (August 2025) — practitioner review but content
  not accessible (HTTP 403 during this research pass)
- No r/algotrading or ForexFactory discussion found
- The cointegration pairs trading framework is very well-studied in the academic literature
  (Gatev/Goetzmann/Rouwenhorst 2006 JFE; Vidyamurthy 2004 textbook; Perlin 2009; Do/Faff 2010)
  and the consensus is that profitability has declined substantially post-2006 as practitioners
  have moved into the space
- The paper's honest "limited profitability" conclusion aligns with the broader academic
  consensus about pairs trading decay

---

## Why This Might Be Nothing

**Most likely benign explanation**:

1. **The strategy is well-documented to have decayed post-2006**: Gatev, Goetzmann, and
   Rouwenhorst (JFE 2006) documented strong pairs trading profits in equities through 2002.
   Subsequent literature (Do/Faff 2010, Huck 2015) consistently shows declining profits
   as the strategy became crowded. A Sharpe of 0.28 on the 30-pair portfolio over 2000–2024
   is consistent with this decay trajectory — the early years of the sample likely dominate.

2. **SPY-IVV is a known degenerate case**: SPY (State Street S&P 500 ETF) and IVV
   (iShares S&P 500 ETF) track essentially the same index. Their spread is driven almost
   entirely by tracking error (< 1 bp) and intraday microstructure noise. Any reported
   29% return / Sharpe 1.5 on this pair likely reflects: (a) tight cointegration that
   generates a near-continuous signal, (b) spread entries that are effectively free of
   economic risk, but (c) transaction costs that consume the entire gross edge. In
   practice, the SPY-IVV spread is too small to trade after costs. Including this pair
   in the "best results" section overstates the strategy's viability.

3. **Equal-share (not dollar-neutral) creates hidden market exposure**: Long N shares of
   ETF1 + short N shares of ETF2 is dollar-neutral only if ETF1 and ETF2 have the same
   price. For pairs with different prices, this creates a persistent long or short dollar
   exposure that is not hedged. Net market exposure generates positive expected returns in
   a bull market, contaminating the measured pairs trading alpha.

4. **Short cointegration windows acknowledged by authors**: The paper explicitly states
   that short trading windows limit long-term profitability. This means the strategy
   generates returns mainly in brief periods of cointegration stability, with extended
   periods of no signal. This is consistent with a strategy that is disappearing as markets
   become more efficient.

5. **FX portability is speculative (ANALYSIS, not from paper)**: The paper does not test
   FX crosses. Applying cointegration pairs trading to EURUSD/GBPUSD requires that: (a)
   cointegration is detectable and stable in FX, (b) the spread mean-reverts fast enough to
   be profitable after FX transaction costs, and (c) regime changes (e.g., Brexit, central
   bank policy divergence) don't systematically destroy the cointegration. None of these
   are demonstrated in this paper.

6. **The 24-year result includes a pre-2006 sample dominated by early pairs trading profits**:
   The bulk of known pairs trading alpha was earned before practitioners discovered the
   anomaly. A Sharpe of 0.28 averaged over 2000–2024 likely masks a Sharpe of 0.5+ in
   2000–2005 and near-zero or negative in 2010–2024.

**The one piece of evidence that would most change my mind**:
A sub-period analysis showing that the strategy's Sharpe remains above 0.4 in 2010–2024
(out-of-sample relative to the pairs trading discovery literature), with explicit cost
modeling and without the SPY-IVV pair. If post-2010 performance is competitive with
passive B&H on a risk-adjusted basis, that would meaningfully change the assessment.

---

## Strengths / Weaknesses

**Strengths**:
- Peer-reviewed in a recognized journal (Journal of Asset Management, Springer)
- 24-year data period provides substantial statistical power
- Monthly rolling reassessment of cointegration is an adaptive (quasi-rolling-OOS) approach
- Honest about limitations: "overall profitability is limited" is a rare and credible statement
- Standard, reproducible methodology (Engle-Granger + z-score; well-documented in textbooks)
- ETF data is publicly available (Yahoo Finance, Bloomberg) — replication is feasible
- Low transaction costs for ETF application

**Weaknesses**:
- Portfolio Sharpe 0.28 is a near-null result — insufficient for standalone deployment
- SPY-IVV best-case result is not representative (degenerate near-arbitrage pair)
- No sub-period analysis available — likely decay from pre-2006 period dominates full-sample
- Equal-share (not dollar-neutral) positions create hidden market beta
- No transaction cost modeling in accessible summaries (implicit in "limited profitability")
- FX application is speculative (ANALYSIS) — paper does not test FX crosses
- No open code; methodology is reproducible but requires implementation

---

## Confidence Score

| Dimension | Score (0–10) | Weight | Weighted |
|-----------|-------------|--------|---------|
| Logical/economic soundness (falsifiable) | 6 | 3 | 18 |
| Transparency & reproducibility | 6 | 2 | 12 |
| Evidence auditability | 6 | 2 | 12 |
| Risk management quality | 4 | 2 | 8 |
| Cost & capacity realism | 5 | 2 | 10 |
| Honest treatment of drawdowns/failure | 8 | 1.5 | 12 |
| Robustness evidence (OOS/walk-forward/multi-market) | 6 | 1.5 | 9 |
| Survived independent scrutiny | 6 | 1 | 6 |
| **Total** | | **15** | **87** |

`latent_score = round(87 / 150 × 100) = 58`

**Evidence auditability = 6 (5–7) → cap at 74**

`confidence = min(58, 74) = 58`

**latent 58 (capped to 58 pending independent verification: peer-reviewed but primary paper
inaccessible; portfolio Sharpe 0.28 is near-null; no sub-period breakdown available;
FX application is ANALYSIS only; SPY-IVV best-pair result is degenerate)**

**Band**: Experimental (40–59)

---

## Complexity / Scalability / Automation Feasibility

- **Complexity**: Low-medium. Standard cointegration test (Engle-Granger or Johansen) +
  z-score computation. Monthly reassessment is straightforward. Well-documented in textbooks.
- **Scalability**: High for ETF application. Medium for FX (need to maintain cointegrated
  pair database and retest monthly).
- **Automation**: Highly automatable. Python with statsmodels/scipy handles all required
  statistics. Daily price data from free sources (Yahoo Finance). Monthly reassessment
  can be fully automated.

---

## MQL5 / Python / Pine Feasibility

- **Python**: HIGHLY FEASIBLE. statsmodels.tsa.stattools has `coint()` (Engle-Granger).
  Daily ETF or FX data from Yahoo Finance (yfinance library). Monthly rolling cointegration
  test + z-score computation is standard quantitative finance Python. Full implementation
  is possible with publicly available data.
- **Pine Script (TradingView)**: PARTIALLY FEASIBLE for a single pair. Manual computation
  of spread and z-score is possible; automated cointegration testing is not available.
- **MQL5**: FEASIBLE for a single FX pair if cointegration parameters are pre-computed
  externally. Real-time z-score execution is implementable; monthly parameter update
  requires external computation.

---

## Similar Strategies

- `graph-clustering-pairs-trading` (score 49) — ML-enhanced pairs trading on S&P 500
  stocks using graph clustering; same statistical arbitrage concept, different universe
  and selection method
- `fx-carry-conditional-dupuy` (score 54) — FX long-short based on carry + vol conditions;
  related in being a long-short FX strategy, different mechanism
- `fx-good-carry-bekaert-panayotov` (score 59) — FX G10 carry with quality filter;
  similar FX long-short but different signal

---

## Red Flags

- Portfolio Sharpe 0.28 across 30 pairs is a near-null result — below any meaningful
  deployment threshold
- SPY-IVV Sharpe 1.5 / high return is a degenerate pair (near-identical ETFs) and not
  a representative result — likely obscures that most pairs are unprofitable
- No sub-period analysis available — early-period pairs trading profits likely dominate
- Equal-share (not dollar-neutral) positioning creates unintentional market beta
- FX portability is ANALYST ASSUMPTION — the paper itself does not test FX crosses
- Cointegration pairs trading is a known decayed anomaly (Gatev et al. 2006 → Do/Faff 2010)
- Primary paper inaccessible (HTTP 403); specific metrics from secondary summaries

---

## Source Links

| Source | Retrieval Date | Notes |
|--------|---------------|-------|
| [JAM 2025 — Chen & Alexiou (DOI: 10.1057/s41260-025-00416-0)](https://link.springer.com/article/10.1057/s41260-025-00416-0) | 2026-06-10 | HTTP 403; abstract via IDEAS/RepEC |
| [IDEAS/RepEC entry](https://ideas.repec.org/a/pal/assmgt/v26y2025i5d10.1057_s41260-025-00416-0.html) | 2026-06-10 | HTTP 403; metadata only |
| [Harbourfrontquant blog review](https://harbourfronttechnologies.wordpress.com/2025/08/14/profitability-of-etf-pairs-trading/) | 2026-06-10 | HTTP 403; title and existence confirmed from search |

---

## Analyst Notes

**FACTS (from sources)**:
- Paper: Chen and Alexiou, Journal of Asset Management, Vol. 26 (5), pp. 464-488, Aug. 2025;
  peer-reviewed
- 30 ETF pairs; 2000–2024; daily data; monthly cointegration reassessment (trailing 12 months)
- Z-score threshold: 2 (baseline) and 1.5 (variant); lower threshold → higher trades and
  Sharpe but higher volatility
- Best pair: SPY-IVV (Sharpe ~1.5 / 29% return — secondary source summary)
- Portfolio Sharpe: ~0.28 (z-score = 2); ~0.37–0.45 (lower threshold)
- Authors' conclusion: "overall profitability is limited"; "short trading windows limit
  long-term profitability"; "adaptive strategies, better pairs selection, and strong risk
  management needed"

**ANALYSIS (my reasoning)**:
- Sharpe 0.28 on the 30-pair portfolio is a near-null result by any practical measure;
  the paper is effectively documenting that this strategy has limited live profitability
- SPY-IVV Sharpe 1.5 is a degenerate case (near-identical ETFs); its inclusion in the
  "best results" section likely overstates the strategy's representativeness
- The cointegration pairs trading anomaly has been progressively arbitraged since the
  Gatev et al. (JFE 2006) publication; the paper's 24-year sample likely shows strong
  early-period performance masking near-zero recent performance
- FX portability: EURUSD/GBPUSD and similar USD-correlated pairs do exhibit cointegration
  in certain regimes, but FX cointegration breaks frequently during divergent central bank
  policy cycles; the spread is also very small relative to even tight FX transaction costs
  (0.1–0.5 pip), requiring very tight z-score thresholds that generate excessive turnover

**ASSUMPTIONS (flagged)**:
- Assumed Engle-Granger cointegration test; NOT CONFIRMED from accessible sources
- Assumed monthly reassessment uses trailing 12 months; CONFIRMED from search summary
- Assumed FX portability is ANALYST ANALYSIS only; NOT from the paper
- The Sharpe 0.28 and 0.45 figures are from secondary sources; NOT confirmed from paper tables

---

## Future Research Needed

1. Access the full paper (DOI: 10.1057/s41260-025-00416-0) to verify: exact Sharpe by
   sub-period (pre/post 2010), specific ETF pair list, methodology (exact cointegration
   test, exit rules), maximum drawdown, and transaction cost modeling
2. Perform sub-period analysis: 2000–2009 vs. 2010–2024 — if post-2010 Sharpe is < 0.2,
   the strategy is effectively dead in the current era
3. Test FX application: EURUSD/GBPUSD with rolling 12-month cointegration test,
   z-score entry at |z| > 2, daily execution, realistic 0.3-pip spreads
4. Understand why the SPY-IVV pair reports such high results — if it's tracking error
   arbitrage, it's not executable at scale
5. Compare with distance method (Do/Faff 2010) and copula method to understand if
   cointegration is actually better than simpler approaches in modern markets
