# REIT Factor Momentum

## Overview

Monthly rotation strategy applied within the US REIT market: rank four established REIT factor portfolios (value, size, momentum, profitability) by their recent performance, go long the top-two performers and short the bottom-two, rebalancing monthly. The central finding is that while traditional single-factor strategies are unstable and atypical in the REIT market compared to broader equities, the *momentum of those factors* delivers significant positive alpha — particularly during bear markets.

The paper extends the factor momentum literature (Ehsani/Linnainmaa JF 2021; Arnott et al. SSRN 2018) from equity markets into the distinct institutional REIT universe.

## Asset Class / Timeframes

- **Asset class:** US REITs (Real Estate Investment Trusts)
- **Universe:** All US-listed REITs (estimated ~150–200 instruments)
- **Rebalancing:** Monthly
- **Holding period:** Monthly (each month hold the top-2/bottom-2 factor portfolio)

## Core Logic

Within the REIT market, four standard factor portfolios are constructed each month:
1. **Value** — REITs with high book-to-price ratio
2. **Size** — small REITs by market capitalization
3. **Momentum** — REITs with best past 11-month return (standard 12-1 formation)
4. **Profitability** — REITs with high operating profitability

Each month, these four factor portfolios are ranked by their recent performance (formation period NOT REPORTED in accessible sources). The strategy goes:
- **Long:** the two factor portfolios with the best recent performance
- **Short:** the two factor portfolios with the worst recent performance

Rebalancing is monthly. The long and short legs are (presumably) equal-weighted, resulting in a dollar-neutral long-short portfolio.

The paper's key diagnostic: traditional static REIT factor strategies (buying high-value / small / high-momentum / high-profitability REITs always) show "unstable and atypical behavior" relative to equity factor returns. But ranking the factors by their own recent performance and rotating into current winners creates significant alpha.

## Economic Rationale

The rationale draws on the general factor momentum literature:
- Factor returns are autocorrelated over intermediate horizons due to slow capital flows: investors and fund managers allocate based on recent factor performance, creating short-term persistence.
- In the REIT market specifically, REIT-specific factors (e.g., cap rate vs. NAV discount for value; operational cash flow for profitability) may have additional persistence due to the illiquid underlying assets, which adjust more slowly than equity prices.
- Factor leadership shifts as macroeconomic regimes change (interest rates, credit cycles, property sector demand). A factor momentum strategy captures these shifting regimes without needing to identify the regime explicitly.

**Conditions under which the edge should disappear:**
- When the REIT market becomes more efficient and factor autocorrelation decays
- When short-selling of underperforming REIT factor portfolios becomes costlier or more constrained
- During a period when all four REIT factors move in the same direction (high correlation), reducing the discriminating power of the ranking
- If the result reflects data-mining over a small number of factors (only 4, long-2/short-2 = effectively binary)

ANALYST NOTE: The 4-factor universe is notably small. Rotating "top-2 vs bottom-2" from a pool of 4 is essentially a binary bet: factor A vs factor B on the long side, and factor C vs factor D on the short side. With only 4 choices, sampling variability is high and the distinction between "genuine factor autocorrelation" and "luck in a small sample" may be impossible to resolve.

## Entry Conditions

- At month start: rank all four REIT factor portfolios by their recent return (formation period NOT REPORTED)
- Enter LONG positions in the two highest-ranked factor portfolios
- Enter SHORT positions in the two lowest-ranked factor portfolios
- Equal weighting within each leg (assumed — not confirmed from accessible sources)

## Exit Conditions

- Exit all positions at month end
- Roll into the new ranking for the following month

## Risk Management

NOT REPORTED from accessible sources. No stop loss, drawdown limit, or position sizing protocol described. Implied equal-weight allocation across factor portfolios.

ANALYST NOTE: Equal dollar exposure to long and short legs creates a market-neutral structure within the REIT universe, but leaves the portfolio exposed to REIT sector-level moves (interest rate sensitivity in particular). No explicit risk management framework is described.

## Indicators Used

- Monthly returns of four REIT factor portfolios: Value, Size, Momentum, Profitability
- Ranking of those returns over the formation period (formation window NOT REPORTED)

## Transaction Costs & Capacity

**Modeled costs:** NOT CONFIRMED from accessible sources. No explicit statement about spread, slippage, commission, or REIT-specific borrowing costs in secondary summaries.

**Capacity concerns:**
- The REIT market is smaller than the equity market; the universe of ~150-200 US REITs has many thinly traded names, especially in the size and value factors.
- Short-selling small REITs carries stock-borrowing costs that can be substantial (5–15% annualized for the hardest-to-borrow names).
- Monthly rebalancing limits turnover relative to weekly or daily strategies, partially mitigating costs.
- The 5.7% annual gross return is modest — a 2–3% round-trip cost (bid-ask + borrow on the short leg) would consume a significant fraction of the gross edge.

**CRITICAL CONCERN:** A pre-cost Sharpe of 1.35 on an unconstrained long-short REIT strategy, without explicit cost modeling, provides no reliable signal about net-of-cost performance. Specifically, the short leg (short the worst-performing REIT factor portfolio each month) may incur prohibitive borrow costs and short-squeeze risks if it concentrates in small, illiquid REITs.

## Backtest Evidence

- Source: Dobrynskaya, Tomtosov, Rechmedina — SSRN 5631072 (October 2025)
- Backtest period: NOT REPORTED in accessible sources
- Auditable: NO — SSRN preprint; full methodology not verifiable from accessible summaries
- Status: CLAIMED, UNVERIFIED

## Forward-Test Evidence

NOT REPORTED.

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | 1.35 | CLAIMED, UNVERIFIED | QuantSeeker weekly recap (secondary summary) |
| Sortino Ratio | "high" — numerical value NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | — |
| CAGR | NOT REPORTED | NOT REPORTED | — |
| Annual Return | 5.7% | CLAIMED, UNVERIFIED | QuantSeeker weekly recap (secondary summary) |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | ~1 month (inferred from monthly rebalancing) | NOT REPORTED | — |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | ~100% long + ~100% short (inferred) | NOT REPORTED | — |

**Alpha reported:** 5.9% per annum CLAIMED, UNVERIFIED (vs. REIT benchmark, benchmark identity NOT REPORTED)

**Note on Sharpe 1.35:** No automatic scrutiny threshold is triggered (Sharpe > 3 threshold not met). However, note that 1.35 pre-cost Sharpe is high for a long-short REIT strategy and cost drag could reduce it materially.

## Community Sentiment

Limited community engagement found. The paper was summarized in the QuantSeeker Weekly Research Recap (June 2026). No substantive critical discussion or independent replication found as of 2026-06-22. Victoria Dobrynskaya is a known researcher (HSE University, Russia) with prior peer-reviewed work on momentum and carry trades; this specific REIT paper does not yet appear on her faculty publications page.

## Why This Might Be Nothing

**1. Most likely benign explanation — tiny universe and binary rotation:**
The core finding is that of 4 REIT factors, the top-2 outperform the bottom-2. But with only 4 factors, the long-2/short-2 design is essentially a binary bet. Any two factors will outperform any other two in any given period, so the "momentum" signal in a 4-factor universe has far lower statistical power than factor momentum in equity markets (where dozens of factors are available). The 5.7% annual gross return could reflect one sustained regime period (e.g., quality/profitability dominated for several years in REIT cycles tied to cap rate compression) rather than a persistent, general rotation mechanism.

**2. Cost case:**
Gross annual return of 5.7%, pre-costs. The short leg requires borrowing REIT stocks, which includes many small, illiquid names. Borrow cost on the worst-performing REIT factor portfolio (e.g., small-cap REITs when size factor is underperforming) can be 5–15%+ annualized. If borrow costs average 3–5% on the short leg, and trading costs (bid-ask + commission) add another 1–2% round-trip annually for a monthly-rebalanced strategy, net-of-cost performance could be negative despite a 5.7% gross. **The absence of confirmed cost modeling means this is the most critical unresolved concern.**

**3. Regime dependence:**
REITs are uniquely interest rate sensitive. In a rising-rate regime (e.g., 2022–2023), all four REIT factors likely declined together, potentially reducing the spread between "top-2" and "bottom-2" factors to noise. The claim of "superior returns in adverse market conditions" refers to equity bear markets; the paper may not have experienced a sustained rate-rising bear market for REITs within the sample.

**4. Sample and overfitting:**
Formation period NOT REPORTED: with the ability to choose 1, 3, 6, 9, or 12-month formation periods over 4 factors, there are ~20 variants tested even in a simple search. If the reported result is the best of these, the Sharpe of 1.35 is upward-biased. Backtest dates NOT REPORTED: we cannot assess whether this is primarily a post-GFC result or a full-cycle result.

**5. What would change my mind:**
A peer-reviewed paper with explicit transaction cost modeling (including borrow costs) showing net Sharpe > 0.5 across multiple formation period choices, covering at least one full REIT bear market cycle (e.g., 2022–2023), with international REIT out-of-sample validation. None of these are available.

## Strengths / Weaknesses

**Strengths:**
- Grounded in the documented equity-market factor momentum literature (Ehsani/Linnainmaa 2021)
- Monthly rebalancing reduces turnover relative to shorter-horizon strategies
- Positive bear-market attribution (if accurate) makes it diversifying vs equity-only strategies
- Alpha vs REIT benchmark (5.9%) suggests outperformance vs passive REIT allocation

**Weaknesses:**
- Only 4 factors: extremely small universe for a rotation strategy
- Key parameters (formation period, backtest dates) NOT REPORTED from accessible sources
- No cost modeling confirmed; REIT short-selling costs are material
- Not peer-reviewed
- No international REIT replication
- Factor construction for REITs may be non-standard (e.g., what is "profitability" for a REIT that must distribute 90%+ of taxable income?)

## Confidence Score

| Dimension | Score (0–10) | Weight | Weighted |
|-----------|-------------|--------|----------|
| Logical / economic soundness (falsifiable) | 6 | 3 | 18 |
| Transparency & reproducibility | 3 | 2 | 6 |
| Evidence auditability | 3 | 2 | 6 |
| Risk management quality | 2 | 2 | 4 |
| Cost & capacity realism | 2 | 2 | 4 |
| Honest treatment of drawdowns / failure | 2 | 1.5 | 3 |
| Robustness evidence (OOS / walk-forward) | 4 | 1.5 | 6 |
| Survived independent scrutiny | 2 | 1 | 2 |
| **Total** | — | 15 | **49** |

`latent_score = round(49 / 150 × 100) = 33`

**Verification cap:** Evidence auditability = 3/10 → cap at 59
**Confidence = min(33, 59) = 33** → **Low Confidence**

`latent 33 (capped to 33 — cap not binding at 59; held back by low raw score driven by missing parameters, unconfirmed costs, and binary 4-factor universe)`

## Complexity / Scalability / Automation Feasibility

- Complexity: Low — construct 4 equal-weighted factor portfolios from REIT universe monthly, rank by past returns, go long top-2 / short bottom-2
- Scalability: Limited — REIT market is small; meaningful capital would move prices in illiquid names
- Automation: Moderate — requires daily REIT price data, monthly rebalancing trigger, and short-selling access on the broker side

## MQL5 / Python / Pine Feasibility

- **Python:** Feasible in principle (REIT factor construction + monthly rotation), but requires commercial REIT data with point-in-time fundamentals (FactSet, Compustat, Bloomberg). Not implementable from free data.
- **Pine (TradingView):** Not feasible — cannot construct cross-sectional factor portfolios within TradingView.
- **MQL5:** Not applicable — no REIT instruments available in standard MT4/MT5 brokers.

## Similar Strategies

- [`dynamic-factor-allocation-regime-shu-mulvey`](dynamic-factor-allocation-regime-shu-mulvey.md) — same mechanism family (factor rotation) but uses regime-switching (SJM) to time US equity style factors rather than momentum-based rotation. Different universe (US equity factors vs REIT factors), different signal (regime vs momentum).
- [`sector-rotation-tsx60`](sector-rotation-tsx60.md) — return-ranked rotation within an equity index; analogous structure but sectors not factors.
- [`currency-momentum-factor-menkhoff`](currency-momentum-factor-menkhoff.md) — cross-sectional momentum applied to currency pairs; similar momentum-of-securities logic within a specific asset class.

## Red Flags

- Formation period NOT REPORTED (prevents independent replication)
- Backtest period NOT REPORTED (cannot assess cycle coverage)
- Transaction costs NOT CONFIRMED modeled — critical given REIT illiquidity and short-side borrow costs
- Universe of only 4 factors: binary rotation with minimal statistical power
- Paper not on author's published faculty page as of 2026-06-22
- Not peer-reviewed (SSRN preprint only)

## Source Links

| URL | Retrieved |
|-----|-----------|
| https://ssrn.com/abstract=5631072 | 2026-06-22 (HTTP 403 — abstract summary from secondary sources) |
| https://www.quantseeker.com/p/weekly-research-recap-97f | 2026-06-22 |

## Analyst Notes

**FACTS (from sources):**
- Strategy: monthly long top-2 / short bottom-2 REIT factor portfolios
- Factors: value, size, momentum, profitability (from secondary source description)
- Annual return: 5.7%; Alpha: 5.9%; Sharpe: 1.35 — all from QuantSeeker secondary summary
- Paper identifies REIT factors as "unstable and atypical" vs equity market
- Performance "particularly effective in adverse market conditions" (equity bear markets)

**ANALYSIS:**
- 4-factor universe means the "top-2" and "bottom-2" are each representing 50% of the factor set, limiting the strategy's ability to discriminate genuine factor momentum from noise
- Pre-cost Sharpe 1.35 on a monthly long-short REIT strategy is plausible in principle but could collapse by 50–80% after realistic REIT short-selling costs
- Factor momentum in equities (Ehsani/Linnainmaa 2021) is well-documented; whether this extends robustly to the REIT market is the testable claim here

**ASSUMPTIONS (flagged):**
- Assumed equal-weight within each leg (not confirmed)
- Assumed factors = value, size, momentum, profitability (inferred from standard literature; exact definitions not confirmed)
- Assumed backtest uses US REIT data (implied; international not confirmed)

## Future Research Needed

1. Full paper access (SSRN PDF): retrieve formation period, backtest dates, exact factor definitions, and drawdown data
2. Cost-inclusive replication: rebuild with point-in-time REIT fundamentals (Compustat REITs) + Sharadar data; add realistic 0.5% round-trip trade cost + 5% annual short borrow rate for the short leg
3. International REIT extension: apply to European or Asian REIT markets as OOS validation
4. Factor construction clarification: how are standard equity factors (especially profitability) adapted for REITs (which are required to distribute ≥90% of taxable income and have distinct accounting)?
5. Sub-period analysis: evaluate 1990s (rising rates) vs 2000–2007 (REIT bull) vs 2008–2009 (GFC) vs 2022–2023 (rate shock) separately
