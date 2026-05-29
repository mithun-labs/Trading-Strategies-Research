# Attention Factors for Statistical Arbitrage

## Overview

A machine-learning statistical arbitrage strategy that jointly learns a characteristics-driven factor
structure and a mean-reversion trading policy on its residuals, optimizing both simultaneously to
maximize net-of-cost performance. Accepted as a full paper at ACM ICAIF 2025 (6th International
Conference on AI in Finance, Singapore, November 2025). Authors: Elliot L. Epstein, Rose Wang,
Jaewon Choi, Markus Pelger (Stanford University).

**Lineage:** Direct successor to "Deep Learning Statistical Arbitrage" (Guijarro-Ordonez, Pelger,
Zanotti; SSRN 3862004; published Management Science; Crowell Memorial Second Prize 2022). The
Attention Factors paper achieves an 84% improvement in net Sharpe over that prior state-of-the-art.

## Asset Class / Timeframes

- **Markets:** 500 largest and most liquid US equities (daily data)
- **Timeframe:** Daily (position changes daily)
- **OOS period:** January 1998 to December 2021 (24 years)
- **Training window:** 8-year rolling window, retrained annually

## Core Logic

The strategy operates in two jointly-optimized stages:

**Stage 1 — Attention Factor Construction:**
Replace PCA's fixed eigenvectors with a learned factor structure. A cross-attention mechanism
groups stocks into K latent factors based on time-varying firm characteristics (39 characteristics
across 6 categories — size, value, momentum, profitability, investment, and liquidity-related
features). For each stock at each point in time, the attention mechanism computes a weighted
similarity to other stocks based on their current characteristics, producing conditional latent
factors that adapt to the changing cross-sectional structure of the market. Unlike PCA factors
(which are fixed in the cross-section), Attention Factors incorporate economic information about
why stocks should be similar.

**Stage 2 — Residual Signal Extraction:**
After constructing factor-neutral portfolios (idiosyncratic residuals), a LongConv (long convolution)
sequence model extracts mean-reversion signals from the residual return time series. LongConv uses
32 different convolutions with a 30-day lookback window of residual returns.

**Key innovation — joint optimization:**
The two-step approach (prior art: estimate PCA factors, then separately estimate residual signals)
is suboptimal because factors have no incentive to reduce trading costs. Jointly estimating factors
and the trading strategy allows the factor structure to adapt in a way that produces lower-turnover,
lower-cost residuals, directly improving net returns. This joint training is identified as the
crucial driver of the 84% net Sharpe improvement over the two-step baseline.

**Optimal parameters:** 30 attention factors. Gross Sharpe maximized at 8 factors; adding to 30
improves net performance through reduced turnover.

## Economic Rationale

**Mechanism:** Stocks with similar economic characteristics (size, value, profitability, momentum)
share common return drivers. Temporary deviations of a stock's return from the factor-implied return
of its characteristic peers represent mispricings that are expected to revert. The attention
mechanism identifies economically similar peers dynamically (a growth stock in 2000 has different
peers than in 2008), improving the quality of the residual signal.

**Falsifiability test: PASSES.** The paper explicitly names failure conditions:
1. Crowding: as institutional statistical arbitrage capital increases, spreads compress and the edge
   decays — the authors acknowledge "the later part of the sample where arbitrage trading is more
   challenging" as the mechanism at work.
2. Distribution shifts: the 2020 COVID period is explicitly identified as a "short-lived
   distribution shift relative to the preceding training windows" — the rolling window model
   temporarily underperforms when cross-sectional return dynamics break from the training-window
   pattern.
3. Momentum dependence: removing past return information from the attention factors drops net Sharpe
   from 2.28 to 0.59 — the strategy relies heavily on short-horizon momentum signals in the
   residuals; if the momentum premium is arbitraged away, the strategy degrades.
4. Characteristics stability: if firm characteristic data quality deteriorates or reporting changes,
   the factor structure becomes less reliable.

These are specific, testable conditions — not generic disclaimers.

## Entry Conditions

- Compute 39-firm-characteristic-conditioned Attention Factors (K=30) for all 500 stocks at each
  daily rebalancing.
- Extract idiosyncratic residual returns from the factor structure (30-day rolling window).
- LongConv model generates a signal (direction and magnitude) for each residual portfolio.
- Enter long/short positions in proportion to the LongConv signal output, subject to turnover
  constraint from joint optimization.

## Exit Conditions

- Daily rebalancing: positions are updated at each close as new residuals and signals are computed.
- No explicit stop-loss described in accessible sources — the joint optimization implicitly manages
  turnover and risk through the net Sharpe objective.

## Risk Management

- **Market-neutral construction:** Beta ≈ 0.05 (confirmed from accessible summaries) — the
  factor structure absorbs systematic risk.
- **Transaction cost control:** Joint optimization explicitly reduces turnover vs. two-step approach.
  Transaction costs and shorting costs modeled and subtracted in reported net Sharpe.
- **Explicit cost modeling:** Shorting costs included in optimization objective — not retrofitted.
- **No explicit stop-loss, drawdown limit, or regime exit described** in accessible sources.
- The rolling 8-year training window provides implicit regime adaptation (models don't extrapolate
  from stale distributions indefinitely).

## Indicators Used

- **39 firm-specific characteristics** in 6 categories (size, value, momentum, profitability,
  investment, liquidity/market microstructure — exact list NOT REPORTED in accessible summaries)
- **Past 30 days of idiosyncratic residual returns** (critical: removing these drops net Sharpe
  to 0.59)
- **LongConv (32 convolutions):** Sequence model over residual return time series

## Transaction Costs & Capacity

- **Costs modeled:** YES — transaction costs AND shorting costs are explicitly included in the
  optimization objective and in net Sharpe calculation. The one-step joint optimization is
  specifically designed to reduce turnover vs. PCA-based two-step approaches.
- **Gross vs. net Sharpe:** Gross >4.0 → Net 2.28 after costs. Cost drag ≈ 1.72 Sharpe units,
  which is substantial — the strategy has meaningful turnover even with cost-aware optimization.
- **Annual return:** 16% (net figure implied from Sharpe 2.28; volatility ≈ 7% implied if return
  is 16% / 2.28)
- **Capacity:** NOT REPORTED. The 500 largest US equities are liquid, and the strategy distributes
  across many stocks (helping capacity). However, stat-arb at scale faces market impact on daily
  rebalancing — a known capacity constraint not addressed in accessible sources.
- **Assessment:** Costs are taken seriously and the architecture is designed to minimize them.
  This is one of the most cost-aware designs in the database. Specific capacity limits unknown.

## Backtest Evidence

- **Status: CLAIMED — peer-reviewed conference paper methodology (ICAIF 2025); specific results
  not independently reproducible (no open code yet; primary sources 403'd)**
- 8-year rolling window, annual retraining, OOS Jan 1998 – Dec 2021 (24 years)
- Gross Sharpe ratio: **>4.0** (CLAIMED, UNVERIFIED)
- Net Sharpe ratio: **2.28** (CLAIMED, UNVERIFIED)
- Annual return: **~16%** (CLAIMED, UNVERIFIED; derived from net Sharpe + implied vol)
- Market beta: **~0.05** (CLAIMED, UNVERIFIED)
- 84% improvement over prior state-of-the-art (two-step model): CLAIMED, UNVERIFIED
- Performance held in COVID 2020 with "short-lived distribution shift" acknowledgment
- Momentum dependence confirmed: removing past returns drops net Sharpe to 0.59

## Forward-Test Evidence

- NOT REPORTED. Paper submitted October 2025; OOS period ends December 2021 (4-year gap to
  submission suggests the 2022–2025 period is NOT in the evaluation). No live trading evidence.

## Reported Metrics

- Gross Sharpe ratio: >4.0 — CLAIMED, UNVERIFIED
- Net Sharpe ratio: 2.28 — CLAIMED, UNVERIFIED
- Annual return (net): ~16% — CLAIMED, UNVERIFIED (derived figure)
- Market beta: ~0.05 — CLAIMED, UNVERIFIED
- 84% net Sharpe improvement over two-step baseline — CLAIMED, UNVERIFIED
- Net Sharpe without past return information: 0.59 — CLAIMED, UNVERIFIED
- Maximum drawdown: NOT REPORTED in accessible sources
- Calmar ratio: NOT REPORTED

## Community Sentiment

- Accepted as a **full paper** at ACM ICAIF 2025 — the leading AI in Finance conference. This is
  full peer review, not a workshop submission.
- The research group (Pelger lab at Stanford) has a strong track record: the predecessor paper
  (DLSA) was published in Management Science (top journal) and won the **Crowell Memorial Second
  Prize 2022** (awarded by PanAgora Asset Management for best paper in the field of quantitative
  investing). This sets a high quality bar for the group's work.
- No independent replication found — paper is October 2025, too new for replications.
- Community commentary from BSIC (Bocconi Students Investment Club) confirms the key results but
  does not constitute independent validation (primary source 403'd).
- The stat-arb field at large is known for rapid decay as strategies become crowded — the paper
  itself acknowledges this as a failure mechanism.

## Why This Might Be Nothing

### 1. Most likely benign explanation
The strongest skeptical case is **momentum risk premium masquerading as arbitrage return**: the model's dependence on past return information is explicit (net Sharpe drops from 2.28 to 0.59 without it). Short-horizon momentum in idiosyncratic returns has been documented in the academic literature (e.g., Lehmann 1990, Lo-MacKinlay 1990). If the strategy is primarily capturing cross-sectional reversal of intraday/short-window returns, it may be a well-known risk premium rather than a genuine pricing error. The Attention Factor machinery may simply be a more efficient way to harvest the cross-sectional reversal premium — real but already well-known and potentially increasingly crowded.

### 2. The cost case
Gross Sharpe >4 → Net Sharpe 2.28 means trading costs consume roughly 43% of gross performance. The strategy has high turnover on 500 large-cap stocks. At institutional scale (>$1B), market impact would further erode this — an unaddressed concern. The paper's cost model likely uses historical bid-ask spreads and borrow rates, which may understate actual execution costs for large positions.

### 3. Regime dependence
The OOS period ends December 2021 — missing the 2022 Fed tightening cycle (the worst cross-sectional momentum crash since 2009), the 2022 NASDAQ correction, and the 2023-2025 market structure. The paper explicitly notes the later part of the 2021 sample was already more challenging. Post-2021 performance is unknown. The 8-year rolling window would include the 2014–2021 low-vol, low-interest-rate environment — a favorable regime for stat-arb that ended sharply in 2022.

### 4. Sample / overfitting concerns
39 characteristics + 30 attention factors + 32 LongConv kernels + hyperparameter tuning = high-dimensional model. 8-year training windows with annual retraining provide some structural protection, but the number of tunable parameters is large relative to the signal-to-noise ratio of daily stock returns. The 84% improvement over the prior state-of-the-art is remarkable — remarkable improvements in a competitive field sometimes reflect overfitting to the evaluation period.

### 5. What would change the verdict
An independent, cost-inclusive forward test on 2022–2025 data using the published methodology (rolling window, 39 characteristics, 30 factors, LongConv). The Pelger group's release of code (consistent with their DLSA precedent) would enable this. We do not have it. Alternatively, a journal publication (Management Science or RFS) with a mandatory replication package would significantly raise the evidence bar.

## Strengths / Weaknesses

**Strengths:**
- Strong economic rationale (characteristics-based peer grouping) with named failure conditions
- Joint optimization is a genuine methodological advance over two-step approaches
- 24-year OOS with rolling window — one of the longest OOS evaluations in the database
- Market-neutral construction (beta ≈ 0.05) — not a disguised equity factor bet
- Transaction costs AND shorting costs explicitly modeled in optimization objective
- Research group track record: DLSA won Crowell Memorial Prize, published in Management Science
- Full peer review at leading AI/Finance conference

**Weaknesses:**
- OOS period ends Dec 2021 — 2022–2025 regime not evaluated
- Momentum dependence: removing past returns collapses performance — possible risk premium harvest, not pure arbitrage
- Drawdown not reported in accessible sources
- Capacity estimate missing — potentially problematic at scale
- No open code yet for this specific paper
- 39 characteristics + 30 factors + 32 LongConv kernels = high model complexity; overfitting risk
- One-dimensional evidence: US equities only, no multi-asset or international test

## Confidence Score

| Dimension | Wt | Raw (0-10) | Weighted |
|---|---|---|---|
| Logical/economic soundness (falsifiable) | 3 | 8 | 24.0 |
| Transparency & reproducibility | 2 | 6 | 12.0 |
| Evidence auditability | 2 | 7 | 14.0 |
| Risk management quality | 2 | 5 | 10.0 |
| Cost & capacity realism | 2 | 7 | 14.0 |
| Honest treatment of drawdowns/failure | 1.5 | 5 | 7.5 |
| Robustness evidence (OOS/walk-forward) | 1.5 | 7 | 10.5 |
| Survived independent scrutiny | 1 | 7 | 7.0 |
| **Total** | **15** | | **99.0** |

`latent_score = round(99.0 / 150 × 100) = 66`

**Verification cap:** Evidence auditability = 7 (5–7 range) → cap at 74
`confidence = min(66, 74) = 66` → **WORTH RESEARCHING**

`latent 66 (capped to 66 pending verification: conference paper; open code not yet confirmed; no independent replication; OOS ends Dec 2021; max drawdown not reported)`

## Complexity / Scalability / Automation Feasibility

- **Complexity:** High — requires cross-attention architecture (transformer-based), LongConv sequence model, rolling retraining pipeline, 39-characteristic data pipeline
- **Scalability:** Designed for 500 large-caps; capacity at institutional scale uncharacterized
- **Automation feasibility:** High — daily rebalancing with systematic rules; no discretionary judgment required once trained; retraining pipeline is the primary operational challenge

## MQL5 / Python / Pine Feasibility

- **Python:** YES — the architecture (cross-attention, LongConv) is standard PyTorch/TensorFlow. The prior DLSA paper has open code on Pelger's Stanford website (mpelger.people.stanford.edu/data-and-code), which provides a starting point. Characteristic data available from WRDS/Compustat/CRSP (institutional access required). Implementation complexity: HIGH.
- **Pine Script:** NO — cross-attention and LongConv are not implementable in Pine.
- **MQL5:** NO — neural network architectures require Python; would need external signal generation.
- **Data requirements:** Daily OHLCV + 39 firm characteristics (size, B/M, momentum, profitability, investment, liquidity) for 500 large-cap US stocks — WRDS/Compustat/CRSP access required.

## Similar Strategies

- `interpretable-hypothesis-driven-microstructure` — also uses idiosyncratic residual signals on US equities, but simpler methodology, honest null result (Sharpe 0.33)
- Predecessor: Deep Learning Statistical Arbitrage (Guijarro-Ordonez, Pelger, Zanotti; SSRN 3862004; Management Science 2022) — same architecture family, this paper improves on it
- `omega-model-intraday-nasdaq` — intraday mean-reversion theme; much simpler; different frequency

## Red Flags

- **OOS ends Dec 2021:** 2022–2025 regime (Fed tightening, momentum crash, new vol regime) NOT evaluated. This is the highest-information gap.
- **Momentum dependence:** Net Sharpe collapses from 2.28 to 0.59 when past returns removed — the strategy may be primarily a cross-sectional reversal/momentum harvester, not a pure pricing-error arbitrage.
- **No drawdown reported:** Critical risk metric absent from accessible sources.
- **Capacity gap:** No estimate of strategy capacity at institutional scale.
- **4-year gap:** Paper submitted Oct 2025; OOS ends Dec 2021 — where is 2022–2025 in the evaluation?
- **High model complexity:** 39 characteristics + 30 factors + 32 LongConv kernels. Ablation studies reduce performance substantially but the base model complexity is high.

## Source Links

- [arxiv 2510.11616 abstract](https://arxiv.org/abs/2510.11616) — HTTP 403, retrieved 2026-05-29
- [arxiv 2510.11616 HTML](https://arxiv.org/html/2510.11616v1) — HTTP 403, retrieved 2026-05-29
- [arxiv 2510.11616 PDF](https://arxiv.org/pdf/2510.11616) — HTTP 403, retrieved 2026-05-29
- [SSRN 5588830](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5588830) — not fetched
- [ACM ICAIF 2025 proceedings](https://dl.acm.org/doi/10.1145/3768292.3770398) — not fetched
- [alphaXiv overview](https://www.alphaxiv.org/overview/2510.11616v1) — HTTP 403, retrieved 2026-05-29
- [Moonlight literature review](https://www.themoonlight.io/en/review/attention-factors-for-statistical-arbitrage) — HTTP 403, retrieved 2026-05-29
- [BSIC review](https://bsic.it/attention-factors/) — HTTP 403, retrieved 2026-05-29
- [Markus Pelger Stanford research page](https://mpelger.people.stanford.edu/research) — not fetched

## Analyst Notes

**FACTS (from accessible sources via search summaries):**
- Authors: Epstein, Wang, Choi, Pelger (Stanford)
- Full paper at ACM ICAIF 2025 (not a workshop)
- Builds on Deep Learning Statistical Arbitrage (Management Science 2022, Crowell Prize 2022)
- 39 firm characteristics in 6 categories
- 30 attention factors (optimal), 32 LongConv convolutions, 30-day lookback
- 8-year rolling training window, annual retraining
- OOS Jan 1998 – Dec 2021 (24 years)
- Gross Sharpe >4, Net Sharpe 2.28
- Annual return ~16% (net)
- Market beta ~0.05
- 84% net Sharpe improvement over prior two-step state-of-the-art
- Without past return info: Net Sharpe drops to 0.59
- COVID 2020 acknowledged as short-lived distribution shift

**ANALYSIS (my reasoning):**
- The joint optimization insight is genuine: if you don't account for costs when learning factors, the factors will be cost-inefficient. This is a real methodological contribution.
- The 24-year OOS period is exceptional — longer than most academic backtests. The rolling window prevents look-ahead bias.
- However, the OOS ending in 2021 is concerning. The 2022-2025 period is the most relevant recent test — post-QE, high-rate environment, different volatility structure. This is the most important missing evidence.
- The momentum dependence is a real structural concern. "Statistical arbitrage" in the marketing sense implies exploiting pricing errors; "harvest cross-sectional momentum/reversal premium" is a better description of what the strategy actually does.
- Net Sharpe 2.28 over 24 years on 500 liquid large-caps with costs modeled is, IF TRUE, one of the strongest results in the academic literature. The Pelger group's track record makes this more credible than a typical claim of this magnitude. Still CLAIMED, UNVERIFIED for our purposes.

**ASSUMPTIONS (flagged):**
- Assuming "500 largest" refers to Russell 1000 top 500 or similar; exact universe not confirmed
- Assuming "24-year OOS" refers to Jan 1998 – Dec 2021 (the figure reported in search summaries); actual evaluation window not directly confirmed
- Assuming the 16% annual return figure refers to net returns (as reported in one search summary)

## Future Research Needed

1. **PRIMARY:** Check Markus Pelger's Stanford website (mpelger.people.stanford.edu/data-and-code) for code release — if code is released, this becomes a tier-1 implementation candidate and the evidence score rises to 8/10+.
2. Run the strategy on 2022–2025 OOS data (Jan 2022 – present) when code is available — this is the highest-value test.
3. Confirm the exact OOS period (Jan 1998 – Dec 2021 vs. some other window) and whether 2022–2025 was held out intentionally or simply not evaluated.
4. Investigate capacity: at what AUM does market impact erode the 16% net return?
5. Cross-check with the predecessor DLSA paper — compare OOS periods and verify that results are consistent rather than the new paper cherry-picking a favorable comparison period.
6. Identify the 39 firm characteristics used — standard academic characteristic lists (Green-Hand-Zhang 2017; Hou-Xue-Zhang 2020) are the likely source.
