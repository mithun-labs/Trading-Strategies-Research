# Commodity Momentum-Reversal Flow Decomposition

## Overview

Zhao, Ding, Yu, and Kang (2026) document the coexistence of short-term return momentum and
trading-based reversal in commodity futures markets, reconciling these seemingly contradictory
effects by decomposing returns into two orthogonal components using unique investors' position
data. The flow component (Q), tied to speculators' net buying/selling, negatively predicts
next-week returns (reversal). The non-flow residual (R_nonQ) positively predicts next-week
returns (momentum). Short-term R_nonQ signals can be aggregated to enhance the traditional
intermediate-term momentum strategy, reportedly lifting annual returns from 5.2% to 9.9%
(CLAIMED, from secondary search summaries).

**Source paper**: "Momentum and Reversal on the Short-Term Horizon: Evidence from Commodity
Markets", Shen Zhao, Yiyi Ding, Jianfeng Yu, Wenjin Kang, SSRN 6425598, March 16, 2026.
*Not peer-reviewed as of June 2026.*

**Key author credentials**: Wenjin Kang (University of Macau) co-authored "A Tale of Two
Premiums: The Role of Hedgers and Speculators in Commodity Futures Markets" (JF 2020,
Kang/Rouwenhorst/Tang). Jianfeng Yu has multiple JFE publications. Both are credible
researchers in commodity futures microstructure.

---

## Asset Class / Timeframes

- **Market**: Commodity futures — specific exchange NOT REPORTED in accessible sources;
  the "unique investors' position data" strongly suggests Chinese commodity exchanges
  (SHFE, DCE, CZCE) which publish daily position data by trader type, unlike US CFTC
  which only reports weekly. The prior JF (2020) paper by Kang et al. used CFTC COT data
  (US commodity futures), so US markets cannot be ruled out.
- **Timeframe**: Weekly (formation and holding period)
- **Style**: Cross-sectional long-short within commodity futures universe

---

## Core Logic

**Step 1 — Decompose weekly commodity futures returns**:

At each week end, for each commodity futures contract, decompose the week's return into:
- **Q (flow component)**: return component attributable to speculators' net trading during
  the week. Computed as a linear function of the speculator net position change (exact
  formula NOT REPORTED in accessible sources; likely a scaling of Δ(speculator net position)
  by price sensitivity or dollar position weight)
- **R_nonQ (non-flow residual)**: orthogonal residual component = total return − Q;
  captures price moves NOT explained by speculator flow

**Step 2 — Weekly signal construction**:
- Sort commodity futures by R_nonQ: long top quintile/decile (high non-flow return = momentum)
  and short bottom quintile/decile (low non-flow return = momentum)
- Alternatively: sort by Q and go **short** top (high speculator buying = near-term reversal)
  and **long** bottom (high speculator selling = near-term reversal)
- The combined strategy mixes both signals

**Step 3 — Enhance intermediate-term momentum**:
The short-term R_nonQ signals are aggregated over multiple weeks to produce an enhanced
formation signal for traditional 1–12 month momentum strategies. This reportedly increases
annual momentum returns from 5.2% to 9.9% (CLAIMED from secondary summaries; exact
methodology NOT REPORTED).

**Additional conditional finding**: Short-term momentum (R_nonQ component) is stronger
when volatility/uncertainty is LOW and when the expected profitability of momentum is HIGH.
This suggests a conditional signal: trade the R_nonQ momentum only in low-volatility regimes.

---

## Economic Rationale

**Stated mechanism**: Two effects operate simultaneously in commodity futures prices:

1. **Speculator flow → reversal (Q component)**: When speculators net-buy (trend-chase),
   they push prices temporarily above fundamental value. As speculator position reverses
   or other traders absorb the order flow, the inflated price partially reverts next week.
   This is consistent with the classic price pressure / liquidity provision model (Kyle
   1985; Grossman-Miller 1988). The flow component Q measures this pressure.

2. **Information diffusion → momentum (R_nonQ component)**: The non-flow component captures
   genuine information (supply shocks, demand shifts, inventory news) that is incorporated
   gradually into prices. This slow diffusion generates persistent returns — the same
   mechanism as classic momentum literature (Hong/Stein 1999 gradual diffusion).

**Why separating these two effects improves signals**: By orthogonalizing the momentum
signal from the flow-driven reversal, R_nonQ is a cleaner measure of information-based
price trends, free from the reversal-inducing flow noise.

**When this edge should fail (falsifiability)**:
1. If position data becomes unavailable, the Q/R_nonQ decomposition cannot be computed
   (the signal literally disappears)
2. If speculators become less trend-chasing (e.g., algorithmic HFT replaces discretionary
   traders), the flow-reversal pattern weakens
3. If information diffusion speeds up (faster price discovery via better data infrastructure),
   the R_nonQ momentum component shortens in duration or disappears
4. If the commodity market becomes more dominated by index investors (non-directional flow),
   the flow component Q loses its predictive reversal signal

---

## Entry Conditions

**Short-term R_nonQ momentum (weekly)**:
1. At week-end, obtain speculator net position data for each commodity
2. Compute Q (speculator flow component) and R_nonQ (residual)
3. Rank commodities by R_nonQ
4. Long top quintile (high non-flow return = positive information momentum)
5. Short bottom quintile (low non-flow return = negative information momentum)
6. Hold for 1 week

**Enhanced intermediate-term momentum**:
1. Aggregate rolling weekly R_nonQ scores into a multi-week formation signal
2. Use this signal instead of raw trailing return for standard 1–12 month momentum
3. Exact aggregation method NOT REPORTED

**Conditional application (conditional on regime)**:
- Apply enhanced signal primarily when realized volatility / uncertainty is LOW
- Scale position proportionally to estimated momentum profitability metric
- Exact regime measure NOT REPORTED

---

## Exit Conditions

- Weekly rebalancing (hard close and re-open for the short-term strategy)
- Intermediate-term version: monthly rebalancing (implied, NOT CONFIRMED)

---

## Risk Management

NOT REPORTED in accessible summaries. Academic long-short equal-weighted portfolio assumed
as default. No explicit stop-loss, leverage limits, or drawdown controls described.

---

## Indicators Used

- Speculator net position data (weekly or daily; from CFTC COT reports or Chinese exchange
  position data — specific source NOT REPORTED)
- Q: flow component derived from Δ(speculator net position) × some price scaling
  (exact formula NOT REPORTED)
- R_nonQ: weekly return minus Q
- Volatility/uncertainty measure for conditioning (NOT REPORTED — possibly realized vol,
  VIX equivalent, or dispersion measure)
- Expected momentum profitability proxy (NOT REPORTED)

---

## Transaction Costs & Capacity

- **Spread/slippage/commission/swap modeled?** NOT REPORTED in accessible summaries
- **Weekly turnover concern**: Weekly rebalancing of a commodity futures portfolio generates
  substantial turnover compared to monthly strategies. Futures bid-ask spreads for liquid
  contracts (crude oil, gold, copper) are narrow, but less liquid contracts (agricultural,
  some energy) can have material round-trip costs.
- **ANALYSIS**: If the strategy trades ~20 commodity futures weekly, expected annual turnover
  is ~52× notional. Even at 2–3 bps round-trip per trade, this implies ~100–150 bps of
  annual cost. The improvement from 5.2% to 9.9% (claimed ~470 bps/yr gross improvement)
  needs to survive ~100–150 bps in costs for the incremental component to be worthwhile.
  This is plausible but untested.
- **Capacity**: Chinese commodity futures markets are generally deep for major contracts
  (rebar, copper, crude oil) but capacity is limited for smaller contracts. If the strategy
  trades Chinese commodities, non-resident access restrictions may apply.
- **Data access barrier**: The "unique position data" is the most significant constraint.
  If it requires Chinese exchange membership or proprietary data vendors, retail replication
  is impossible; institutional replication requires verifying that the data is indeed
  publicly available.

---

## Backtest Evidence

- **Period**: NOT REPORTED explicitly; likely spans at least 5–10 years based on convention
  in this literature
- **Status**: CLAIMED, UNVERIFIED — SSRN working paper; no peer review; primary paper
  inaccessible (HTTP 403); key performance metrics from secondary search summaries only

---

## Forward-Test Evidence

NOT REPORTED.

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
| Annual Return (baseline intermediate momentum) | ~5.2% | CLAIMED, UNVERIFIED | Web search summary of SSRN 6425598 |
| Annual Return (enhanced with R_nonQ signal) | ~9.9% | CLAIMED, UNVERIFIED | Web search summary of SSRN 6425598 |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | ~1 week (short-term) / ~1 month (intermediate) | CLAIMED, UNVERIFIED | Strategy design |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | NOT REPORTED | NOT REPORTED | — |

**Note on 5.2%→9.9% claim**: These figures are from a secondary search engine snippet and
could not be verified against the primary paper. They represent gross annual returns only;
no volatility, Sharpe, or cost data accompanies them. Without knowing volatility, the Sharpe
ratio improvement is unknown.

---

## Community Sentiment

- No independent forum discussion found (paper too recent — March 2026)
- Research group is credible:
  - Wenjin Kang (University of Macau): JF 2020 publication on commodity hedger/speculator
    risk premia; expert in commodity futures microstructure and trader position data
  - Jianfeng Yu: multiple JFE publications; established empirical asset pricing researcher
  - Shen Zhao: co-authored with Yu on investor sentiment and economic forces (SSRN 1991244)
- The JF 2020 paper (Kang/Rouwenhorst/Tang) demonstrated that hedgers and speculators earn
  different commodity risk premia — this 2026 paper extends that position-data framework to
  momentum/reversal decomposition
- No criticism threads found (too new for peer review)

---

## Why This Might Be Nothing

**Most likely benign explanation**:

1. **In-sample Q construction and reverse-engineering**: The flow component Q is derived
   from speculator position changes. If Q is computed in-sample using OLS or any calibrated
   formula, the orthogonal residual R_nonQ is essentially constructed to have maximum
   predictive power for the remaining returns. This is a form of in-sample look-back
   optimization. The OOS Sharpe of R_nonQ could be substantially lower than in-sample.

2. **5.2%→9.9% without Sharpe context is uninformative**: The improvement in annual return
   could come entirely from taking on more risk. If the enhanced strategy has twice the
   volatility, the Sharpe ratio is unchanged or worse. ANALYSIS: Without knowing the
   volatility of the enhanced strategy, this claim provides no real information about
   risk-adjusted improvement.

3. **Data availability as the binding constraint**: The "unique position data" is likely
   from Chinese commodity exchanges. These exchanges publish daily position data for the
   top 20 positions by member (not total market). If the top-20 breakdown is used to
   approximate speculator flow, the measurement error could be large and variable across
   time and contracts, making the decomposition unstable.

4. **The reversal (Q) component is well-known**: The finding that speculator net buying
   predicts reversal is not new — this was documented in Handler, de Raaij, and Rouwenhorst
   (2014) and by Kang/Rouwenhorst/Tang (JF 2020) for longer horizons. The novel part is
   the weekly horizon and the specific decomposition — which may or may not generalize OOS.

5. **Regime dependence on volatility**: The paper notes momentum is stronger in low-volatility
   regimes. This introduces a conditional filter that is chosen ex post to maximize in-sample
   performance. OOS performance in high-volatility periods (which are when you need risk
   management most) may be much worse.

6. **No cost analysis in available sources**: A weekly-rebalancing commodity strategy
   has material turnover costs that could erode a 5.2%→9.9% improvement substantially.
   If the incremental 4.7% per year comes at 2–3% additional annual turnover cost, the
   net improvement is only 1.7–2.7% — still positive, but much less exciting.

**The one piece of evidence that would most change my mind**:
An OOS walk-forward showing the R_nonQ-enhanced strategy's Sharpe ratio (not just return)
with explicit transaction cost deduction, using data from a period not used in the Q formula
calibration. The enhancement from 5.2% to 9.9% needs to survive costs and out-of-sample
testing before this is worth implementing.

---

## Strengths / Weaknesses

**Strengths**:
- Credible research team (Kang and Yu have JF/JFE records)
- Well-grounded economic mechanism: liquidity-provision reversal (Q) vs. information
  diffusion momentum (R_nonQ)
- Addresses a genuine puzzle: simultaneous momentum and reversal at the same horizon
- Use of position data is unique — not available in standard return-only datasets
- Weekly holding period allows faster updating than monthly strategies
- The conditional finding (stronger in low-vol) provides risk-aware implementation guidance
- Builds on established literature (JF 2020 Kang et al.; Moskowitz 2012 TSMOM)

**Weaknesses**:
- SSRN working paper only; no peer review
- Primary paper inaccessible (HTTP 403); all figures from secondary snippets
- Q formula NOT REPORTED; strategy not implementable from public description
- No Sharpe ratio, drawdown, or volatility data available — 5.2%→9.9% return claim is
  meaningless without risk context
- Data access barrier: "unique position data" may require Chinese exchange membership
  or proprietary data vendors
- Weekly rebalancing costs not modeled in available sources
- In-sample Q calibration risk is structurally present in the decomposition
- Chinese commodity market access limitations for non-resident institutions

---

## Confidence Score

| Dimension | Score (0–10) | Weight | Weighted |
|-----------|-------------|--------|---------|
| Logical/economic soundness (falsifiable) | 7 | 3 | 21 |
| Transparency & reproducibility | 3 | 2 | 6 |
| Evidence auditability | 3 | 2 | 6 |
| Risk management quality | 2 | 2 | 4 |
| Cost & capacity realism | 2 | 2 | 4 |
| Honest treatment of drawdowns/failure | 2 | 1.5 | 3 |
| Robustness evidence (OOS/walk-forward/multi-market) | 3 | 1.5 | 4.5 |
| Survived independent scrutiny | 3 | 1 | 3 |
| **Total** | | **15** | **51.5** |

`latent_score = round(51.5 / 150 × 100) = 34`

**Evidence auditability = 3 (≤ 4) → cap at 59**

`confidence = min(34, 59) = 34`

**latent 34 (capped to 34 pending verification: SSRN working paper only; primary paper
inaccessible; Sharpe NOT REPORTED; 5.2%→9.9% claim without vol/risk context from
secondary source; Q formula NOT REPORTED; data access barriers)**

**Band**: Low Confidence (0–39)

---

## Complexity / Scalability / Automation Feasibility

- **Complexity**: High. Requires daily or weekly position data broken down by trader type
  (speculator vs. hedger), an orthogonal decomposition of returns into Q and R_nonQ, and
  the construction of the enhanced momentum signal. The conditional volatility filter adds
  further complexity.
- **Scalability**: Medium for liquid commodity contracts; limited by position data availability
  and Chinese market access for retail/international institutions.
- **Automation**: Implementable in principle if data is available. The decomposition formula
  (OLS regression of returns on speculator position changes) is standard Python/R.

---

## MQL5 / Python / Pine Feasibility

- **Python**: Feasible IF position data is available. The decomposition is standard OLS
  or orthogonalization. Pandas + statsmodels would suffice. However, obtaining weekly/daily
  speculator position data for a universe of commodity futures is non-trivial and potentially
  expensive.
- **Pine Script**: NOT FEASIBLE — position data not available on TradingView; multi-instrument
  sorting not supported.
- **MQL5**: NOT PRACTICAL — designed for single-symbol Forex/CFD, not commodity futures
  universe management.

---

## Similar Strategies

- `commodity-basis-reversal-futures` (score 54) — weekly M1-M2 spread reversal on 39
  commodity futures; overlapping market but different signal (basis rather than flow)
- `commodity-futures-curve-dynamics-ns` (score 62) — NS curve momentum on ~21 commodity
  futures; different signal construction but same weekly-to-monthly market
- `tsmom-china-commodity-futures` (score 60) — Chinese commodity futures time-series
  momentum; likely same market, different signal (raw return sign, not flow-decomposed)
- `commodity-intramarket-correlation-momentum` (score 45) — 4 sector ETFs; related idea
  of switching between momentum and reversal, but much smaller universe and different method

---

## Red Flags

- Primary paper inaccessible (HTTP 403); all strategy details from secondary summaries
- SSRN working paper only; no peer review
- Sharpe ratio NOT REPORTED — 5.2%→9.9% annual return claim is meaningless without
  volatility context; no risk-adjusted improvement confirmed
- Q formula NOT REPORTED — strategy is not implementable as described
- "Unique position data" may not be publicly accessible for non-Chinese institutions
- In-sample Q calibration risk embedded in the R_nonQ construction
- No cost analysis available; weekly rebalancing has non-trivial turnover costs
- Regime condition (low volatility) may mean the strategy is absent when most needed

---

## Source Links

| Source | Retrieval Date | Notes |
|--------|---------------|-------|
| [SSRN 6425598 — Commodity Momentum-Reversal Flow Decomposition](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=6425598) | 2026-06-10 | HTTP 403; abstract and key findings from search summaries |
| [Kang/Rouwenhorst/Tang JF 2020](https://onlinelibrary.wiley.com/doi/abs/10.1111/jofi.12845) | 2026-06-10 | Foundation paper on commodity hedger/speculator risk premia |

---

## Analyst Notes

**FACTS (from sources)**:
- Paper: Zhao/Ding/Yu/Kang (SSRN 6425598, March 16, 2026); not peer-reviewed
- Core finding: Q (flow component from speculator net trading) negatively predicts next-week
  commodity futures returns; R_nonQ (non-flow residual) positively predicts next-week returns
- Short-term momentum strengthens when volatility/uncertainty is low and expected momentum
  profitability is high
- Aggregating R_nonQ signals improves intermediate-term momentum from 5.2% to 9.9%/yr (annual
  return figure from secondary search snippets only)
- Wenjin Kang (University of Macau): JF 2020 paper on commodity hedger/speculator risk premia
- Jianfeng Yu: established JFE researcher

**ANALYSIS (my reasoning)**:
- The mechanism (liquidity-provision flow reversal vs. information-diffusion momentum) is
  economically coherent and grounded in decades of microstructure literature
- The 5.2%→9.9% improvement claim lacks Sharpe or volatility context and is therefore
  uninformative about risk-adjusted improvement
- The "unique position data" is likely Chinese exchange daily investor position data (SHFE,
  DCE, CZCE publish daily top-20 position breakdowns); this is the most significant
  implementation barrier
- The in-sample Q calibration risk is structural: R_nonQ is by construction the return
  component least explained by the flow measure, so it will show maximum non-flow momentum
  in-sample; OOS divergence is expected

**ASSUMPTIONS (flagged)**:
- Assumed Chinese commodity exchanges (SHFE/DCE/CZCE) for the "unique position data";
  NOT CONFIRMED — could be CFTC COT data for US markets
- Assumed weekly holding period; CONFIRMED for short-term strategy from abstract
- Assumed 20+ commodity universe; NOT CONFIRMED

---

## Future Research Needed

1. Access the full paper (SSRN 6425598) to verify: exact Q formula, data source (exchange),
   universe size, data period, Sharpe ratios, max drawdown, and OOS methodology
2. Verify whether the position data is CFTC COT (US, public) or Chinese exchange data
   (public but requiring Chinese data vendors)
3. Understand the exact orthogonalization procedure for Q and R_nonQ
4. Check whether Sharpe ratio improves alongside the 5.2%→9.9% return claim
5. If data is from Chinese exchanges, assess whether 2022+ data maintains the signal
   (COVID distortions, regulatory changes in Chinese commodity markets)
6. Test whether the volatility conditioning is a genuine improvement or in-sample fitting
