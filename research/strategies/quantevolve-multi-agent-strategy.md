# QuantEvolve: Multi-Agent Evolutionary Strategy Discovery Framework

## Overview

QuantEvolve is a meta-framework — not a specific trading strategy — that uses quality-diversity (QD)
optimization combined with a hypothesis-driven multi-agent LLM system to automatically discover and
evolve quantitative trading strategies. Published as a pre-print in October 2025 and accepted to the
AI4F Workshop at ACM ICAIF 2025 (Singapore). Authors: Junhyeog Yun, Hyoun Jun Lee, Insu Jeon (AI
Tech Lab, Qraft Technologies).

**IMPORTANT NOTE:** This entry covers a methodology framework, not a specific deployable strategy.
The confidence score reflects both the framework's conceptual merit AND the absence of auditable
evidence that strategies discovered by it generalize out-of-sample.

## Asset Class / Timeframes

- **Evaluated on:** 6 equities + 2 futures (asset names and markets NOT REPORTED in accessible sources)
- **Related community fork:** BTC/USDT 1-hour on Binance (tarsyang/quantevolve — independent adaptation)
- **Timeframe:** NOT REPORTED (specific instruments and periods not disclosed in accessible summaries)

## Core Logic

QuantEvolve operates as a three-layer system:

1. **Quality-Diversity (QD) Feature Map:** Maintains a multi-dimensional grid of candidate strategies
   where each cell is defined by strategy attributes: Sharpe Ratio, Trading Frequency, Maximum
   Drawdown, Sortino Ratio, Total Return, and Strategy Category. New strategies only survive if
   they occupy an empty or underperforming cell — forcing the population to remain diverse rather
   than converging to a single "best" strategy type.

2. **Hypothesis-Driven Multi-Agent LLM System:** LLM agents generate new strategy code by
   analyzing "parent" and "cousin" strategies from the QD archive and forming explicit hypotheses
   about why a proposed strategy should work. This is intended to guide exploration toward
   theoretically motivated patterns rather than pure random mutation.

3. **Iterative Evaluation Loop:** Each candidate strategy is backtested on historical data. Metrics
   are computed and the strategy is placed in the QD map. Underperforming occupants are replaced.
   Each evolutionary cycle requires 5–10 LLM inferences.

## Economic Rationale

The framework's implicit claim is that quality-diversity optimization combined with LLM hypothesis
generation will discover strategies that (a) are diverse enough to avoid correlated failures and
(b) are theoretically grounded enough to generalize OOS.

**Falsifiability verdict: FAILS.** The framework cannot state conditions under which LLM-discovered
strategies should fail to generalize. The hypothesis-generation mechanism is explicitly acknowledged
by the authors as potentially producing post-hoc rationalizations rather than genuine economic insights:
"We have not formally validated whether generated hypotheses reflect established market theories."
Any market pattern can receive a plausible-sounding LLM explanation. A real economic rationale
must predict when the edge should disappear — this framework cannot do that for the strategies
it discovers, because the discovery process is hypothesis-agnostic data mining.

**Rationale score: pattern-only.** The QD diversity mechanism is a genuine conceptual contribution,
but the framework is fundamentally a systematic data-mining engine applied to a small historical
dataset. This is not an economic rationale for any specific strategy it discovers.

## Entry Conditions

NOT REPORTED — the framework generates strategy code, but the specific entry/exit rules of
strategies it produced in the evaluation are not publicly available from accessible sources.

## Exit Conditions

NOT REPORTED — same caveat as Entry Conditions.

## Risk Management

- The QD feature map includes MDD and Sortino Ratio as selection dimensions — strategies with
  poor risk profiles are systematically deprioritized.
- Specific risk management rules (stop-losses, position sizing, maximum loss limits, regime
  exits) for the strategies produced are NOT REPORTED.
- There is no described mechanism preventing discovered strategies from having hidden tail risk
  that is not captured by the selected metrics on limited historical data.

## Indicators Used

NOT REPORTED — the framework generates diverse strategies using any indicators LLMs choose.
The community fork uses candlestick OHLCV data for BTC/USDT.

## Transaction Costs & Capacity

- Transaction costs modeled: NOT REPORTED in accessible sources.
- The evaluation universe (6 equities + 2 futures) is far too small to make any claim about
  capacity or robustness.
- Given the framework discovers high-frequency or high-turnover strategies via optimization,
  transaction costs are a significant concern — strategies selected for high Sharpe may be
  implicitly selecting for high turnover that becomes negative after costs.
- **Assessment: costs LIKELY NOT rigorously modeled**, given the acknowledged limitation that
  "baselines are simple and do not represent state-of-the-art quantitative approaches." A
  paper using state-of-the-art cost models would say so.

## Backtest Evidence

- **Status: CLAIMED, UNVERIFIED**
- Tested on 6 equities + 2 futures over "limited date ranges" (specific dates NOT REPORTED).
- Authors state the framework "significantly outperforms conventional baselines."
- Baselines are described by the authors themselves as "simple" and "not state-of-the-art."
- Specific Sharpe, MDD, and return numbers for the final strategies NOT REPORTED in accessible
  search summaries.
- Primary paper (arxiv 2510.18569) returned HTTP 403 — all evidence from search summaries.

## Forward-Test Evidence

- NOT REPORTED. No live trading evidence.

## Reported Metrics

- Framework outperforms conventional baselines: CLAIMED, UNVERIFIED
- QD map includes: Sharpe Ratio, Trading Frequency, MDD, Sortino Ratio, Total Return, Strategy Category — CONFIRMED (framework design)
- Specific performance numbers for discovered strategies: NOT REPORTED
- Date ranges: NOT REPORTED

## Community Sentiment

- Selected for AI4F Workshop at ACM ICAIF 2025 (Singapore, November 2025) — workshop-level acceptance
- Qraft Technologies is an established AI-focused quantitative asset management firm in Korea — adds credibility to the authors' practical context
- No independent replication found in accessible sources
- Community fork (tarsyang/quantevolve on GitHub) adapts the concept for BTC/USDT but is an independent implementation, not a validation of the paper's specific results
- Field-wide criticism of LLM strategy discovery applies: LLMs can generate plausible-sounding hypotheses for any pattern, creating the illusion of theoretical grounding for data-mined results. See: Quantbeckman (2025), "Are LLMs the Ultimate Alpha Generator?" — finds LLM agents' returns largely explained by passive market exposure.

## Why This Might Be Nothing

### 1. Most likely benign explanation
This framework is a systematic data-mining engine applied to a tiny dataset (6 equities + 2 futures, limited date range). Running thousands of LLM iterations, each generating and testing a new strategy variant, is multiple-testing at an unprecedented scale. The QD feature map is sophisticated, but selecting the "best" strategy from thousands of in-sample candidates is textbook p-hacking. The authors acknowledge this risk when they note that generated hypotheses may be post-hoc rationalization rather than genuine market theory.

### 2. The cost case
Transaction costs are NOT REPORTED. The evolutionary process optimizes for risk-adjusted performance, but if costs are not accurately modeled, the selected strategies may achieve high Sharpe by being high-turnover — which looks good in backtests but erodes to zero or negative after slippage, commission, and market impact. This is a standard failure mode for machine-generated strategies.

### 3. Regime dependence
The evaluation period and regime are NOT DISCLOSED. A strategy discovered to work in 2020–2023 (COVID volatility → bull → Fed tightening) may be implicitly long a specific vol regime. The framework has no described mechanism to ensure regime diversity across the test period.

### 4. Sample / overfitting concerns
6 equities + 2 futures is a dangerously small universe for an evolutionary algorithm that explores thousands of strategy variants. The probability of finding a strategy that looks good in-sample by chance approaches 1 as the number of tested candidates grows. Walk-forward validation is not described in accessible sources. The authors acknowledge the evaluation "uses relatively simple baselines" and needs to be expanded.

### 5. What would change the verdict
An independent, cost-inclusive walk-forward test of a strategy discovered by QuantEvolve, applied to a held-out asset universe of ≥50 instruments over ≥10 years, showing statistically significant OOS Sharpe after proper multiple-testing correction. The authors themselves identify this as future work. We do not have it.

## Strengths / Weaknesses

**Strengths:**
- Quality-diversity concept is genuinely novel: forces the evolutionary process to maintain strategic diversity rather than converging to one over-fitted solution
- Framework design is transparent and coherent
- Qraft Technologies has practical market expertise, unlike purely academic authors
- The multi-dimensional feature map (Sharpe, MDD, Sortino, turnover, category) is a more nuanced selection mechanism than simple Sharpe optimization

**Weaknesses:**
- Framework paper: no specific deployable strategy produced
- Evaluation universe is far too small (6+2) to support any generalization claim
- Date ranges and assets not disclosed — impossible to assess regime risk
- LLM hypothesis generation can manufacture plausible explanations for any pattern, undermining the "hypothesis-driven" claim
- 5–10 LLM inferences per cycle = resource-intensive; scales poorly to large universes
- Baselines acknowledged as simple and non-state-of-the-art
- No open code from original authors
- No walk-forward or independent replication

## Confidence Score

| Dimension | Wt | Raw (0-10) | Weighted |
|---|---|---|---|
| Logical/economic soundness (falsifiable) | 3 | 2 | 6.0 |
| Transparency & reproducibility | 2 | 4 | 8.0 |
| Evidence auditability | 2 | 3 | 6.0 |
| Risk management quality | 2 | 2 | 4.0 |
| Cost & capacity realism | 2 | 1 | 2.0 |
| Honest treatment of drawdowns/failure | 1.5 | 4 | 6.0 |
| Robustness evidence (OOS/walk-forward) | 1.5 | 1 | 1.5 |
| Survived independent scrutiny | 1 | 3 | 3.0 |
| **Total** | **15** | | **36.5** |

`latent_score = round(36.5 / 150 × 100) = 24`

**Verification cap:** Evidence auditability = 3 (≤4) → cap at 59
`confidence = min(24, 59) = 24` → **LOW CONFIDENCE**

`latent 24 (capped to 24 pending verification: workshop paper only; no open code; no independent replication of discovered strategies; tiny evaluation universe)`

## Complexity / Scalability / Automation Feasibility

- **Complexity:** High — requires LLM inference (5–10 calls per evolutionary cycle), backtesting infrastructure, and QD archive management
- **Scalability:** Poor as described — 5–10 LLM calls per cycle makes large-scale evaluation expensive; authors flag this as a limitation
- **Automation feasibility:** The framework is designed for automation but requires significant compute infrastructure and LLM API costs

## MQL5 / Python / Pine Feasibility

- The framework itself would require Python implementation
- Community fork (tarsyang/quantevolve) provides a Python starting point for BTC/USDT
- The strategies it discovers could theoretically be implemented in any language, but without knowing what strategies are produced, implementation feasibility cannot be assessed
- Pine Script: limited to indicator-based strategies; RL or complex ML output incompatible

## Similar Strategies

- `interpretable-hypothesis-driven-microstructure` — also a methodology paper testing strategy patterns via a rigorous framework (but with honest null results)
- Related work: "Automate Strategy Finding with LLM in Quant Investment" (arxiv 2409.06289, EMNLP 2025) — similar LLM-based strategy discovery for Chinese A-shares

## Red Flags

- **METHODOLOGY PAPER:** No specific deployable strategy. The framework may produce good or bad strategies — we cannot evaluate without seeing what it produces on a realistic universe.
- **Data mining by design:** The evolutionary search explicitly optimizes over strategy space on historical data. This is structurally equivalent to running thousands of backtests and keeping the best result.
- **LLM rationalization risk:** Generated hypotheses may be post-hoc stories about data-mined patterns. Authors acknowledge this.
- **Tiny evaluation universe:** 6 equities + 2 futures is insufficient to validate any strategic claim.
- **Costs NOT REPORTED:** High risk that cost-unaware strategies are being selected.
- **Conflict of interest:** Qraft Technologies is a commercial AI asset management firm. The framework may be designed to showcase their AI capabilities rather than produce independently replicable results.
- **Simple baselines:** Authors explicitly acknowledge that baselines "do not represent state-of-the-art quantitative approaches."

## Source Links

- [arxiv 2510.18569 abstract](https://arxiv.org/abs/2510.18569) — HTTP 403, retrieved 2026-05-29
- [arxiv 2510.18569 HTML](https://arxiv.org/html/2510.18569v1) — HTTP 403, retrieved 2026-05-29
- [arxiv 2510.18569 PDF](https://arxiv.org/pdf/2510.18569) — HTTP 403, retrieved 2026-05-29
- [alphaXiv summary](https://www.alphaxiv.org/resources/2510.18569v1) — HTTP 403, retrieved 2026-05-29
- [ResearchGate](https://www.researchgate.net/publication/396748086_QuantEvolve_Automating_Quantitative_Strategy_Discovery_through_Multi-Agent_Evolutionary_Framework) — HTTP 403, retrieved 2026-05-29
- [Moonlight literature review](https://www.themoonlight.io/en/review/quantevolve-automating-quantitative-strategy-discovery-through-multi-agent-evolutionary-framework) — HTTP 403, retrieved 2026-05-29
- [ACM ICAIF 2025 proceedings](https://dl.acm.org/doi/10.1145/3768292.3770398) — not attempted; evidence from search summaries
- [Welcome.AI summary](https://www.welcome.ai/content/quantevolve-advances-ai-driven-finance-with-enhanced-trading-strategies) — HTTP 403, retrieved 2026-05-29
- [GitHub community fork: tarsyang/quantevolve](https://github.com/tarsyang/quantevolve) — ACCESSIBLE, retrieved 2026-05-29

## Analyst Notes

**FACTS (from accessible sources):**
- Framework paper accepted to AI4F Workshop at ICAIF 2025 (workshop, not full conference paper)
- Authors from Qraft Technologies AI Tech Lab (commercial firm)
- Evaluated on 6 equities + 2 futures (exact identities NOT REPORTED)
- QD feature map dimensions confirmed: Sharpe, Trading Frequency, MDD, Sortino, Total Return, Strategy Category
- 5–10 LLM inferences per evolutionary cycle (authors' stated limitation)
- Authors explicitly acknowledge: baselines are simple/not state-of-the-art; needs larger universe; needs longer time horizons; LLM hypotheses may be post-hoc rationalization
- Community fork (tarsyang/quantevolve) targets BTC/USDT; is an independent adaptation, not a paper validation

**ANALYSIS (my reasoning):**
- The QD diversity mechanism is a genuine conceptual contribution to automated strategy discovery — preventing convergence to a single over-fitted solution is a real improvement over single-objective optimization
- However, the fundamental problem remains: systematic search over strategy space on historical data is data mining, and calling it "hypothesis-driven" because LLMs write stories about the found patterns does not change its statistical nature
- The Qraft Technologies connection is both a strength (practical expertise) and a concern (commercial motivation to showcase the framework in the best light)
- A framework paper is only as good as the strategies it produces in real OOS conditions — we have zero auditable evidence on this

**ASSUMPTIONS (flagged):**
- Assuming the 6 equities and 2 futures represent a reasonable starting point; if they were selected for their favorable results, the evaluation is even weaker than stated
- Assuming "outperforms conventional baselines" means statistically significant OOS improvement; the exact test methodology is unknown

## Future Research Needed

1. **Primary:** Read the full paper (when 403 resolves) to determine: exact test assets, date ranges, specific strategy examples discovered, cost modeling approach, and whether any walk-forward or multiple-testing corrections were applied.
2. Run the community fork (tarsyang/quantevolve) on BTC/USDT OOS data to test whether an independently implemented version of the concept produces profitable strategies in a different period.
3. Compare to related work: "Automate Strategy Finding with LLM in Quant Investment" (arxiv 2409.06289) — similar framework; check whether that paper's CSI300 results (192% annual return in H1 2023) are regime-specific or persistent.
4. Look for independent replication or follow-up papers citing QuantEvolve, particularly any that apply it to larger universes with explicit walk-forward + multiple-testing corrections.
