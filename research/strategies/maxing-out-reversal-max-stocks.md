# Maxing Out Short-Term Reversals in Weekly Stock Returns

## Overview

A weekly cross-sectional short-term reversal strategy filtered by the MAX metric (highest
daily return in the week before last). The core finding: short-term reversals are approximately
2.5× stronger in high-MAX (lottery-like) stocks than in low-MAX stocks. Mechanism: lottery-
seeking retail investors overreact to news, creating larger price dislocations that reverse the
following week. Published in Journal of Empirical Finance Vol. 82, 2025.

**Authors:** Chen Chen, Andrew Cohen, Qiqi Liang, Licheng Sun
**Affiliations:** All — Old Dominion University
**SSRN:** 4622831 (last updated March 6, 2025)
**Journal:** Journal of Empirical Finance, Vol. 82, 2025 (peer-reviewed)
**DOI:** S0927539825000301

---

## Asset Class / Timeframes

- **Asset class:** US Equities (common stocks, NYSE + AMEX + NASDAQ)
- **Data source:** CRSP (monthly and daily files, commercial subscription required)
- **Timeframe:** Weekly rebalancing
- **Sample period:** July 1963 to December 2022 (59 years)
- **Universe:** All domestic common stocks on NYSE, AMEX, NASDAQ; no explicit size filter
  stated in accessible sources; the strategy skews toward small-cap/illiquid stocks by construction
  (high-MAX stocks are predominantly lottery/small-cap names)

---

## Core Logic

**Three-step construction (weekly, repeated every week):**

1. **Compute MAX:** For each stock, calculate the maximum daily return during the *week before
   last* (week t−2). This identifies stocks with recent extreme positive daily moves.

2. **Sort by MAX quintile:** Each week, sort all stocks into five groups (Q1 = lowest MAX,
   Q5 = highest MAX). High-MAX (Q5) stocks are the "lottery stocks" — they experienced the
   most extreme single-day return two weeks ago.

3. **Sort by prior-week return within quintile:** Within each MAX quintile, sort stocks by
   their last-week return (week t−1). Implement a standard short-term reversal:
   - **Long:** high-MAX past-week losers (Q5 MAX × bottom return quintile)
   - **Short:** high-MAX past-week winners (Q5 MAX × top return quintile)

The strategy is the **difference in reversal profits across MAX quintiles**: high-MAX reversal
(1.66%/week) vs. low-MAX reversal (0.65%/week). The edge claim is that the MAX filter amplifies
the underlying reversal phenomenon.

**Conditional overlay:** The effect is strongest (and, critically, reportedly only significant)
when retail order imbalance is in the highest quintile. Implementing the condition requires
real-time retail order imbalance data — a significant practical barrier.

**MAX window robustness:** Results replicated across 4-week, 13-week, and 26-week MAX windows;
2-week (week-before-last) is the main specification but not uniquely superior.

---

## Economic Rationale

**Primary mechanism:** Lottery-seeking investors (mostly retail) have pent-up demand for
high-MAX stocks. When these stocks experience a down week, retail investors buy the dip in
excess of fundamental value (overreaction), driving prices above fair value. The following
week, prices revert as the temporary demand pressure dissipates.

**Asymmetry by MAX:** Low-MAX stocks do not have concentrated retail lottery-demand, so their
reversal is smaller (~0.65%/week). High-MAX stocks attract disproportionate retail attention
(recency bias → lottery preference), amplifying overreaction and thus the subsequent reversal.

**When the edge should disappear (falsifiability test — passed):**
- When retail order imbalance is *low* (paper explicitly shows this; edge documented only in
  highest OIB quintile)
- In large-cap, liquid stocks (where bid-ask friction is low and institutional correction is
  fast)
- After transaction costs, because high-MAX = small-cap lottery stocks with wide spreads
- If retail lottery-seeking behavior is arbitraged away by institutional short-sellers (though
  short-selling constraints on lottery stocks limit this)
- Post-publication, if quants target this exact 2-week MAX / 1-week return double-sort

The mechanism is grounded in behavioral finance (overreaction / lottery demand) and ties to
the well-documented MAX effect (Bali/Cakici/Whitelaw, JFE 2011). The paper explicitly names
the conditions under which the edge weakens, satisfying the falsifiability requirement.

---

## Entry Conditions

1. At end of week t−1:
   - Calculate MAX for each stock over the prior week (week t−2) — max of 5 daily returns
   - Sort stocks into MAX quintiles
   - Sort stocks by prior-week return (week t−1) within each quintile
2. Enter long positions: stocks in MAX Q5 (highest MAX) and return bottom quintile (largest losers)
3. Enter short positions: stocks in MAX Q5 (highest MAX) and return top quintile (largest winners)
4. Condition (from paper): Confirm retail order imbalance is in the highest quintile (if data
   available)

---

## Exit Conditions

- Close all positions at end of the following week (week t)
- Weekly rebalancing — all positions are 1-week holds
- No stop-loss or trailing exit rules described in accessible sources

---

## Risk Management

**As described in accessible sources:** None beyond the inherent long-short hedging of the
cross-sectional construction. No stop-loss, no position limits, no regime exit, no max-loss
rules stated.

**Implied:** Long-short construction provides beta neutrality (approximately). The paired
MAX-quintile sort means longs and shorts are in similar-MAX-profile stocks, which may reduce
some factor exposures.

**NOT specified:** Individual position size, portfolio-level volatility target, leverage
constraint, or tail-risk management.

---

## Indicators Used

- **MAX(t−2):** Maximum single daily return during week t−2 (week before last)
- **Return(t−1):** Total stock return during last week (week t−1)
- **Retail order imbalance:** Retail OIB quintile (conditioning variable; exact measure not
  specified in accessible sources — likely BJZZ or similar signed-volume measure)

No technical indicators, moving averages, or price chart analysis.

---

## Transaction Costs & Capacity

**Transaction cost modeling:** NOT REPORTED in accessible sources. The paper states the
strategy is "robust to market microstructure biases," which means the authors corrected for
bid-ask bounce in *return measurement* (using mid-quote adjustments, Dimson betas, etc.) —
this is categorically different from subtracting actual trading costs.

**Critical cost concern (ANALYSIS):**
High-MAX stocks are lottery stocks — predominantly small-cap, high-idiosyncratic-volatility,
thinly traded names with wide bid-ask spreads. For a weekly-rebalancing strategy targeting
these stocks:
- Typical NYSE small-cap bid-ask spread: 30–100bps per side; round-trip 60–200bps/week
- For micro-cap/lottery stocks: effective round-trip cost could reach 300–500bps/week
- The gross edge (1.66%/week high-MAX reversal) must exceed round-trip costs to be viable

At 100bps round-trip (very conservative for lottery stocks), the net edge falls from 1.66%
to 0.66%/week. At 200bps, net edge is −0.34%/week — negative. The 0.65%/week gross return
for low-MAX stocks would be negative at virtually any realistic cost level.

**The Hou/Xue/Zhang (RFS 2020) problem:** Their "Replicating Anomalies" study found that
the baseline short-term reversal strategy earns only −0.26%/month (vs. −1.99% in Jegadeesh
1990) when using VW returns with NYSE breakpoints. This is a crucial precedent: the reversal
anomaly shrinks dramatically with proper portfolio construction. The MAX filter in this paper
targets the exact segment (small-cap, illiquid, high-attention stocks) that is *most* affected
by the Hou/Xue/Zhang replication concerns. The paper does test value-weighted and equal-weighted
variants, but specific VW return numbers are not available in accessible sources.

**Capacity:** Minimal. The strategy concentrates in illiquid lottery stocks where position
sizes above very small amounts would move markets and trigger execution costs that destroy the
edge. Not scalable beyond a tiny research portfolio.

**Short-selling constraints:** High-MAX (lottery) stocks have well-documented short-sale
constraints. Fee-to-borrow for lottery stocks can be 50–500%+ annualized. Short-leg profits
may be entirely consumed by lending fees. The paper does not report whether short-leg alpha
survives borrowing costs.

---

## Backtest Evidence

**Source:** Journal of Empirical Finance, Vol. 82, 2025 (peer-reviewed)

**Status: CLAIMED, UNVERIFIED** — despite peer review, the specific performance numbers come
from a backtest on CRSP data; the methodology is documented and reviewed, but results are not
independently auditable without CRSP data access and full paper access. The full paper is
paywalled.

**Available performance claim:**
- High-MAX reversal: 1.66%/week
- Low-MAX reversal: 0.65%/week
- Both are GROSS returns

**Not available from accessible sources:**
- Annualized Sharpe ratio
- Maximum drawdown
- CAGR
- Number of trades or turnover
- Net-of-cost returns
- Value-weighted vs. equal-weighted performance differential

---

## Forward-Test Evidence

NOT REPORTED. No live or paper trading results known.

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | NOT REPORTED | NOT REPORTED | — |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | — |
| CAGR | NOT REPORTED | NOT REPORTED | — |
| Annual Return (high-MAX reversal) | ~86%/yr (gross) | CLAIMED, UNVERIFIED | Chen et al. JEF 2025 (1.66%/wk × ~52 wks; ANALYSIS — caution: weekly compounding assumption; paper cites weekly average return, not CAGR) |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | 1 week | CLAIMED, UNVERIFIED | Chen et al. JEF 2025 |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | ~100% long + 100% short | NOT REPORTED | Inferred from long-short construction |
| High-MAX weekly reversal | 1.66% | CLAIMED, UNVERIFIED | Chen et al. JEF 2025 (SSRN 4622831) |
| Low-MAX weekly reversal | 0.65% | CLAIMED, UNVERIFIED | Chen et al. JEF 2025 (SSRN 4622831) |

**ANALYSIS note on implied annual return:** 1.66%/week × 52 weeks ≈ 86%/year gross — but this
is a *portfolio return* (long-short), not a CAGR. It is not compounded and is definitely
not achievable after costs. This number is shown for illustration only and is NOT entered
in the CAGR row.

---

## Community Sentiment

**Academic:** Published in Journal of Empirical Finance (peer-reviewed, Elsevier; ranked
B+/A− in empirical finance). JEF has a reputation for solid empirical work.

**Practitioner commentary:**
- CXO Advisory ran a summary (title: "Amplifying Short-term Reversal via Stocks with High
  Recent Returns") but the full analysis is behind a paywall ($17.99/month). No free summary
  excerpt of their verdict is available.
- No discussion found on r/algotrading, Quantpedia community, or other practitioner forums.
- SSRN page has been accessed but full content is paywalled at ScienceDirect.

**Contextual criticism (related literature):**
- Hou/Xue/Zhang (RFS 2020, "Replicating Anomalies") showed the baseline short-term reversal
  anomaly earns −0.26%/month value-weighted with NYSE breakpoints — a dramatic shrinkage from
  Jegadeesh (1990). This paper's MAX filter helps (focuses on illiquid segment), but the
  segment it targets is exactly where replication concerns are most acute.
- Nagel (JFE 2012, "Evaporating Liquidity") established that short-term reversal returns are
  compensation for liquidity provision, not alpha — arguing market-makers and not trend-chasers
  are best positioned to profit.
- Groot/Huij/Zhou literature: reversal strategies in the 100 largest stocks can generate
  30–50bps/week net after costs. High-MAX lottery stocks are the opposite end of the
  liquidity spectrum — the 1.66%/week gross is entirely consistent with a very high liquidity
  risk premium that disappears after costs.

---

## Why This Might Be Nothing

**1. Most likely benign explanation: illiquidity premium masquerading as alpha**

The standard interpretation of short-term reversal profits is that they are *compensation for
liquidity provision* (Nagel 2012), not behavioral alpha. Market-makers earn the bid-ask spread
and temporarily hold imbalanced inventories; their return is the reversal profit. For a strategy
that requires *taking* the other side, the round-trip bid-ask cost wipes out the gross return.
High-MAX lottery stocks have the widest bid-ask spreads of any market segment. The 1.66%/week
gross return is almost certainly dominated by illiquidity compensation — it is the spread the
liquidity provider earns, not tradable alpha for a strategy investor.

**2. The cost case — probably fatal at realistic spreads**

Round-trip trading costs for small-cap lottery stocks commonly run 200–500bps. A weekly
strategy that requires entering AND exiting positions in this universe every 5 days would
generate annual turnover of ~100×. At 200bps round-trip: 200bps × 100 turns = 20,000bps =
200%/year in costs. The gross return of 1.66%/week × 52 = ~86%/year gross. The strategy is
deeply negative net of costs at any realistic cost assumption for the target segment.

If we apply only a 50bps round-trip (implying liquid large-cap stocks), net return falls to
~60%/year gross equivalent — but then the MAX filter no longer applies (large-caps don't have
wide bid-ask reversal premium). The strategy cannot be migrated to liquid stocks without
losing the effect.

**3. Hou/Xue/Zhang replication concern for the underlying effect**

The baseline reversal anomaly earns −0.26%/month VW (Hou/Xue/Zhang RFS 2020), essentially
zero. This paper's MAX filter concentrates the portfolio in small-cap/micro-cap stocks —
exactly the segment the Hou/Xue/Zhang critique identifies as driving the gross anomaly through
the equal-weighting of tiny, illiquid names. Adding a MAX filter on top of a broken baseline
does not resurrect the baseline.

**4. Short-leg may be impossible to implement**

Short-selling high-MAX lottery stocks is notoriously difficult. These stocks have very high
shares-on-loan and days-to-cover ratios, resulting in high stock-borrowing fees (50–500%+
annualized). The long leg (buying past-week losers) generates most academic profit — but the
paper's reported alpha includes both legs. Net of short-borrowing fees, the short leg likely
destroys rather than adds value.

**5. Retail order imbalance condition creates a data-availability barrier**

The strategy only "works" in the highest retail OIB quintile. Real-time retail order imbalance
data (signed retail trades) is difficult to obtain. Institutional platforms may proxy this with
odd-lot order flow, but the exact measure from the paper (likely the BJZZ measure or Boehmer/
Jones/Zhang/Zhang retail flow proxy) requires processing TAQ or similar microstructure data.
Without this conditioning signal, the raw MAX reversal strategy underperforms the 1.66%/week
headline by an unknown but significant amount.

**6. Publication effect (forward-looking concern)**

Published in March 2025, the double-sort is now publicly known. The universe of high-MAX
lottery stocks is small and illiquid — any institutional capital targeting this pattern would
immediately exhaust the capacity and front-run the effect.

**7. What single piece of evidence would most change the assessment:**
A cost-inclusive net-of-spread implementation study on large-cap and mid-cap stocks (NYSE
top-2-quartile by market cap) showing that the MAX filter generates positive net returns after
round-trip costs of 30bps. Without that, the prior remains "illiquidity premium, not alpha."

---

## Strengths / Weaknesses

**Strengths:**
- Peer-reviewed JEF paper — genuine academic scrutiny
- Very long sample (59 years, 1963–2022) — covers multiple regimes
- Clear behavioral mechanism (lottery demand → overreaction → reversal)
- Well-grounded in prior MAX literature (Bali/Cakici/Whitelaw 2011)
- Falsifiable: effect explicitly tied to retail OIB and disappears in low-OIB quintile
- Multiple MAX windows (4-wk, 13-wk, 26-wk) all work → robust to MAX window length
- Controls for microstructure biases (bid-ask bounce correction in returns)
- Both EW and VW portfolios tested

**Weaknesses:**
- Gross returns only — no net-of-cost performance shown (likely fatal)
- High-MAX stocks are small-cap/illiquid — worst-case segment for transaction costs
- Short-leg practically infeasible: short-borrowing fees on lottery stocks are extremely high
- Retail order imbalance conditioning data unavailable in real-time for most traders
- No explicit risk management, drawdown, Sharpe, or CAGR reported in accessible sources
- Hou/Xue/Zhang (2020) showed baseline reversal essentially disappears VW — MAX filter
  concentrates in the exact illiquid segment driving the spurious equal-weighted alpha
- No international replication, no OOS split, no walk-forward

---

## Confidence Score

| Dimension | Score | Weight | Weighted |
|-----------|-------|--------|---------|
| Logical/economic soundness (falsifiable) | 6 | 3 | 18 |
| Transparency & reproducibility | 7 | 2 | 14 |
| Evidence auditability | 6 | 2 | 12 |
| Risk management quality | 2 | 2 | 4 |
| Cost & capacity realism | 1 | 2 | 2 |
| Honest treatment of drawdowns/failure | 3 | 1.5 | 4.5 |
| Robustness evidence (OOS/walk-forward/multi-market) | 5 | 1.5 | 7.5 |
| Survived independent scrutiny | 4 | 1 | 4 |
| **Total** | | **15** | **66** |

`latent_score = round(66 / 150 × 100) = 44`

**Evidence auditability = 6 → cap = min(latent, 74) → confidence = min(44, 74) = 44**

**`latent 44 (capped to 44; evidence auditability 6/10: peer-reviewed JEF, methodology
documented but full paper paywalled; evidence cap at 74 not binding since latent 44 < 74;
primary cap concern is transaction costs and cost data not accessible)`**

**Confidence: 44 — Experimental**

Score breakdown rationale:
- Logical soundness (6): solid behavioral mechanism with named failure conditions; partial
  circularity (high OIB explains both MAX effect and reversal) prevents higher score
- Transparency (7): JEF peer-review; three-step procedure clearly described; no open code
- Evidence auditability (6): peer-reviewed methodology; not independently verified without
  full paper and CRSP access; closer to 5 than 8 (no open code, no reproducible pipeline)
- Risk management (2): no explicit RM; long-short hedging only
- Cost & capacity (1): zero cost modeling shown in accessible sources; high-MAX = maximum
  illiquidity environment; presumptively invalid for most practitioners
- Drawdowns/failure (3): no drawdown reported; no failure regime analysis in accessible material
- Robustness (5): multiple MAX windows and EW+VW tested; no OOS split or international test
- Independent scrutiny (4): JEF peer review meaningful; no independent replications found;
  Hou/Xue/Zhang precedent is structurally adverse

---

## Complexity / Scalability / Automation Feasibility

**Complexity:** Moderate. Three-step weekly sort is mechanical. Requires CRSP subscription
for clean daily return data. Retail order imbalance measure requires TAQ or an institutional
microstructure data provider — adds significant complexity.

**Scalability:** Very low. Strategy concentrates in illiquid lottery stocks. AUM above
$1–5M would move markets. Not scalable to institutional size.

**Automation:** Partially feasible for the sort itself (CRSP + Python); the retail OIB
condition requires specialized infrastructure that is effectively unavailable to retail traders
and most small firms.

---

## MQL5 / Python / Pine Feasibility

**Python:** Feasible for academic replication with CRSP data (pandas/numpy; weekly sort
pipeline straightforward). Retail OIB conditioning requires BJZZ or TAQ data access.

**Pine Script / TradingView:** Not feasible. No individual-stock cross-sectional sort
capability in Pine; no CRSP data access.

**MQL5:** Not feasible. MQL5 lacks cross-sectional rank logic and CRSP data access.

**Realistic implementation:** Institutional Python or R with CRSP subscription + TAQ for OIB
conditioning. Execution requires direct market access for small-cap stocks with minimal
slippage. Effectively out of reach for retail traders.

---

## Similar Strategies

- `intraday-momentum-spy` (score 50) — reversal/momentum on SPY intraday; same reversal
  direction but single-instrument, intraday vs. cross-sectional
- `fast-alphas-intraday-overlay` (score 55) — streak-based mean reversion overlay on ATR
  trend; short-term reversal in single instrument context
- `currency-momentum-factor-menkhoff` (score 59) — cross-sectional momentum in FX; similar
  double-sort methodology in a different asset class
- `cmm-daily-return-weighted-momentum` (score 54) — also uses daily return structure
  (EA-day weighting) to enhance monthly momentum; complementary approach

The base short-term reversal strategy on which this builds is well-known (Jegadeesh 1990;
Quantpedia strategy #31). The MAX filter is the novel contribution.

---

## Red Flags

- `COSTS OMITTED ON HIGH-ILLIQUIDITY STRATEGY`: Weekly trading in lottery/small-cap stocks
  without explicit cost modeling is presumptively invalid. The gross 1.66%/week is almost
  certainly illiquidity compensation, not tradable alpha.
- `INSUFFICIENT VERIFICATION`: No Sharpe, CAGR, or drawdown available from accessible sources.
- `SHORT-SELLING CONSTRAINTS`: High-MAX stocks have well-documented short-borrowing difficulty.
  Short-leg alpha is likely inaccessible.
- `DATA BARRIER`: Retail OIB condition requires institutional microstructure data unavailable
  to most practitioners.
- `HOU/XUE/ZHANG CONCERN`: Baseline reversal essentially null VW; MAX filter concentrates in
  the problematic micro-cap universe.

---

## Source Links

| Source | URL | Retrieved |
|--------|-----|-----------|
| SSRN 4622831 (primary paper) | https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4622831 | 2026-06-18 |
| ScienceDirect (JEF article) | https://www.sciencedirect.com/science/article/abs/pii/S0927539825000301 | 2026-06-18 (HTTP 403) |
| IDEAS/RePEC listing | https://ideas.repec.org/a/eee/empfin/v82y2025ics0927539825000301.html | 2026-06-18 |
| CXO Advisory summary | https://www.cxoadvisory.com/technical-trading/amplifying-short-term-reversal-via-stocks-with-high-recent-returns/ | 2026-06-18 (paywalled) |
| Bali/Cakici/Whitelaw 2011 (MAX origin) | https://pages.stern.nyu.edu/~rwhitela/papers/max%20jfe.pdf | 2026-06-18 |
| Hou/Xue/Zhang RFS 2020 (replication context) | https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3275496 | 2026-06-18 |
| Quantpedia reversal strategy page | https://quantpedia.com/strategies/short-term-reversal-in-stocks | 2026-06-18 |

---

## Analyst Notes

**FACTS (from sources):**
- High-MAX reversal generates 1.66%/week gross; low-MAX generates 0.65%/week gross
- Strategy requires retail order imbalance in highest quintile for significance
- Sample: July 1963 to December 2022 (59 years), NYSE/AMEX/NASDAQ CRSP
- Peer-reviewed in Journal of Empirical Finance, Vol. 82, 2025
- All four authors at Old Dominion University
- MAX defined as max daily return during the week before last (week t−2)
- Results robust across 4-week, 13-week, and 26-week MAX windows
- Paper "controls for microstructure biases" — meaning mid-quote return corrections, NOT cost deduction
- Hou/Xue/Zhang (RFS 2020) found baseline VW reversal = −0.26%/month (essentially zero)
- Nagel (JFE 2012): short-term reversal profits = compensation for liquidity provision

**ANALYSIS (my reasoning):**
- A 1.66%/week gross return in illiquid lottery stocks is almost certainly dominated by
  bid-ask spread and market impact — the classic "liquidity provider's rent"
- Weekly rebalancing into high-illiquidity stocks implies annual turnover ≈ 100× —
  devastating after any realistic cost assumption
- The short leg faces additional barrier: lottery-stock borrowing fees of 50–500%+ annualized
  can consume all short-leg alpha
- The Hou/Xue/Zhang precedent and the conditional-on-high-OIB finding together suggest the
  strategy is concentrated in a tiny, illiquid, hard-to-trade universe where gross returns
  are highest but implementation is effectively infeasible

**ASSUMPTIONS:**
- Assumed MAX window is 5 daily returns (5-day week), consistent with paper's "week before
  last" specification
- Assumed retail OIB measure is a BJZZ-style signed-volume proxy; exact definition NOT
  REPORTED in accessible sources

---

## Future Research Needed

1. **Net-of-cost analysis:** The most important missing piece. Access the full JEF paper to
   find whether the authors model explicit transaction costs (bid-ask spread, market impact,
   short-borrowing fees) and show net returns.

2. **Large-cap subsample:** Does the MAX filter generate positive net returns in the top 2
   NYSE size quartiles? If yes, the strategy is salvageable for institutional implementation
   with limited capacity.

3. **International replication:** Does the MAX-reversal effect hold in developed markets
   outside the US (e.g., LSE, Euronext, TSE)? Chen et al. do not report international tests.

4. **Post-2022 OOS:** The sample ends December 2022 — a period of unprecedented market
   volatility (2022 rate shock). Forward performance since publication (March 2025) is unknown.

5. **Short-borrowing cost study:** Are the short-leg returns positive after fee-to-borrow?
   A study pairing this with securities lending data would be highly informative.

6. **Retail OIB proxy availability:** Identify real-time retail OIB proxies available to
   systematic traders (e.g., FINRA OTC data, signed odd-lot flow) and test whether they
   replicate the paper's conditioning signal.
