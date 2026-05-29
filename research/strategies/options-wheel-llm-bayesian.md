# Options Wheel Strategy with LLM-Generated Bayesian Networks

## Overview

A hybrid decision-support architecture applied to the standard options "wheel" strategy. The
core wheel cycles between selling cash-secured puts and, after assignment, selling covered
calls. The paper's contribution is replacing static rules with a dynamic Bayesian network
constructed at each decision point by a large language model (LLM), which interprets
current market conditions and selects "analogous" historical scenarios to populate conditional
probability tables. Authors claim this eliminates assignment through strategic rolling and
achieves dramatically lower drawdowns than a static wheel or the market.

- **Authors:** Xiaoting Kuang, Boken Lin
- **Source:** arXiv:2512.01123 (November 30, 2025, preprint — not peer-reviewed)
- **Data period:** January 2007 – September 2025 (18.75 years, 8,919 total trades)
- **Claimed OOS period:** January 2020 – September 2025

## Asset Class / Timeframes

- **Market:** US Equities options (underlying assets NOT REPORTED in available sources)
- **Timeframe:** Options-expiry driven (weekly or monthly assumed; exact expiry selection NOT REPORTED)
- **Style:** Income / premium collection

## Core Logic

The standard options wheel:
1. Sell a cash-secured put at a selected strike and expiry.
2. If the put expires out-of-the-money: collect premium, repeat from Step 1.
3. If put goes in-the-money near expiry: roll it out (and possibly down) to avoid assignment.
4. If assigned (stock purchased): immediately sell a covered call against the position.
5. If the call is exercised: stock is called away; return to Step 1.

The hybrid innovation: before each trade decision, an LLM (model NOT REPORTED) builds a
context-specific Bayesian network by:
- Interpreting current market conditions (prices, volatility, trends, news).
- Selecting relevant historical analogues from the full 18.75-year trade dataset.
- Populating the network's conditional probability tables with those analogues.
- Running probabilistic inference to output position sizing and strike selection guidance.
- Incorporating a feedback loop where outcomes refine subsequent network structures.

The result, per the authors: a "0% assignment rate" maintained over 18.75 years via strategic
rolling, and dramatically reduced drawdowns vs. the market.

## Economic Rationale

The base wheel strategy harvests the **volatility risk premium (VRP)**: implied volatility
systematically exceeds realized volatility for equity index and single-stock options, so short
options have positive expected premium collection over time. This is one of the
better-documented cross-asset return premia (Carr & Wu 2009; Coval & Shumway 2001).

However, the VRP is not a free lunch: it is essentially short-crash-insurance. Short puts lose
catastrophically in tail events. The wheel's economic rationale **depends critically on when
the edge disappears**: extreme realized vol (VIX > 50), gap-down events (earnings, macro
shocks), liquidity crises, and long bear markets all stress the strategy severely. A 0%
assignment rate through 2008 and March 2020 would be extraordinary if it were purely
economic (not just an artifact of rolling accounting).

The LLM-Bayesian enhancement has **no stated falsifiable mechanism** — the paper cannot
articulate conditions under which the LLM layer would *fail* to add value. This is a
fabrication-risk flag: the improvement could be data-mining over the in-sample historical
analogues the LLM selects.

**Falsifiability verdict:** The base VRP mechanism is partially falsifiable (fails in
crash/high-vol regimes). The LLM enhancement is **not falsifiable** as described — it is a
learned pattern-matcher with no clear failure condition stated.

## Entry Conditions

NOT REPORTED in available sources for the LLM-Bayesian layer. The base wheel would enter by
selling a put when:
- Cash balance is sufficient to cover full assignment
- Implied vol environment is favorable (specific VIX or IV threshold: NOT REPORTED)
- Strike selection: guided by the Bayesian network (specific delta target: NOT REPORTED)
- Expiry selection: NOT REPORTED

## Exit Conditions

- **Primary exit:** Option expires worthless → premium collected.
- **Rolling exit:** When option threatens to go in-the-money, roll out (extend expiry) and
  possibly down (lower strike), paying a rolling debit or collecting a credit.
- Rolling conditions (delta threshold, DTE trigger): NOT REPORTED.
- Assignment avoidance claim: 0% assignment rate over 18.75 years — maintained entirely
  through rolling (see Why This Might Be Nothing).
- If assigned: sell covered call (strike and expiry selection: NOT REPORTED).

## Risk Management

- **Assignment avoidance:** Described as "strategic option rolling" — specific conditions,
  roll-down limits, and maximum loss per position: NOT REPORTED.
- **Position sizing:** Guided by the Bayesian network — specific rules: NOT REPORTED.
- **Maximum loss per trade / portfolio stop:** NOT REPORTED.
- **Regime exit conditions:** NOT REPORTED.

## Indicators Used

- Current market prices, implied volatility, trend indicators, and news (exact features NOT REPORTED)
- Bayesian network node structure (variables, conditional dependencies): NOT REPORTED
- LLM model and prompt: NOT REPORTED

## Transaction Costs & Capacity

**Costs:** Options strategies carry:
- Bid-ask spread on options: typically 5–20% of premium for short-dated liquid options
- Commission: typically $0.65–$1.00/contract (~retail) to sub-$0.10 (institutional)
- Slippage on rolling: mid-to-market execution rarely achieved on wide-spread options

The paper does NOT explicitly state whether bid-ask spreads, slippage, or commissions were
modeled in the 8,919-trade backtest. If costs were excluded, the 15.3% CAGR and Sharpe 1.08
are **presumptively overstated** for this strategy type.

**Rolling costs:** Each roll involves simultaneously closing one position and opening another.
The net debit (or credit) of each roll is a real cost. If rolling is always modeled at
mid-price with no slippage, assignment-avoidance comes at an understated cost.

**Capacity:** A cash-secured put wheel requires full cash collateral (100 shares × strike ×
number of contracts). At meaningful capital size, this strategy does not face capacity issues
for index ETF options (SPY, QQQ), but liquidity degrades sharply for individual stocks
especially in stressed markets.

## Backtest Evidence

**Claimed (UNVERIFIED):** 18.75 years of data (Jan 2007 – Sep 2025), 8,919 trades.
- Authors claim OOS test period: January 2020 – September 2025 (5.75 years)
- Single in/out-of-sample split — no rolling walk-forward
- Source: arXiv preprint, not peer-reviewed

Potential contamination: The LLM populates Bayesian network conditional probability tables
by selecting from "an 18.75-year, 8,919-trade dataset." If this full dataset is available at
every OOS decision point (including the test period's own trades), this constitutes
**lookahead bias**: the model is selecting analogues that include outcomes it should not have
access to at that decision time. This is not conclusively established from available sources
but is a structural concern given the description.

## Forward-Test Evidence

NOT REPORTED. No live trading or paper-trading results cited.

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | 1.08 | CLAIMED, UNVERIFIED | arXiv:2512.01123 |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | -8.2% | CLAIMED, UNVERIFIED | arXiv:2512.01123 |
| CAGR | 15.3% | CLAIMED, UNVERIFIED | arXiv:2512.01123 |
| Annual Return | NOT REPORTED | NOT REPORTED | — |
| Number of Trades | 8,919 over 18.75 years | CLAIMED, UNVERIFIED | arXiv:2512.01123 |
| Average Trade Duration | NOT REPORTED | NOT REPORTED | — |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | NOT REPORTED | NOT REPORTED | — |

**Comparison baselines (CLAIMED, UNVERIFIED):**
- Pure LLM: CAGR 8.7%, Sharpe 0.45
- Static Bayesian network: CAGR 11.2%, Sharpe 0.67
- Market benchmark: Sharpe ~0.62 (implied), max DD ~-60%

## Community Sentiment

No independent community discussion found (this is a November 2025 preprint). No forum
discussions, replications, or criticism threads identified. The paper has not survived
scrutiny from a knowledgeable community.

## Why This Might Be Nothing

**1. Lookahead bias in LLM data selection (primary concern):**
The paper describes the LLM selecting "relevant historical data from an 18.75-year, 8,919-trade
dataset." If this full dataset — including the claimed OOS period (2020–2025) — is available
to the LLM at every decision point in the OOS test, the model has access to future outcomes
when selecting "analogous" scenarios. This would systematically inflate performance in the OOS
period. The description is ambiguous enough that this cannot be ruled out from available
sources. This is the single most important unresolved question about this paper.

**2. 0% assignment rate is implausible:**
8,919 trades over 18.75 years with zero assignments — including through the 2008 financial
crisis (VIX > 80, market -57%), the March 2020 COVID crash (-35% in 33 days), and the
2022 bear market (-25%). A 0% assignment rate through these events through "strategic rolling"
means every threatened position was successfully rolled at a net debit/credit that still
produced a positive total outcome. This is extraordinary. If rolling costs were understated
or if "assignment" is defined narrowly (excluding rolls that are effectively forced assignments
in disguise), this metric misleads about true strategy performance.

**3. Maximum drawdown -8.2% over 18.75 years:**
This triggers the automatic scrutiny rule (< 5% threshold not triggered, but -8.2% over
18.75 years including 2008 and 2020 is statistically implausible for a short-volatility
strategy). The actual realized max DD of a short-put strategy without hedging through 2008
would routinely exceed 30–50%. The "rolling avoidance" mechanism effectively converts
realized losses into accumulated deferred rolling costs — if rolling debits are excluded
from the DD calculation, -8.2% is an understatement of true economic drawdown.

**4. Transaction costs almost certainly not fully modeled:**
8,919 options trades over 18.75 years at retail spreads (5–15% of premium) would
meaningfully erode returns. If costs were not fully modeled, the 15.3% CAGR overstates
net performance. Options market-impact and bid-ask spreads are especially wide in stressed
markets — precisely when the rolling mechanism is most needed.

**5. Opaque LLM-Bayesian mechanism, no open code:**
The specific LLM, prompt design, Bayesian network node structure, and analogue-selection
algorithm are not publicly documented. This strategy cannot be independently replicated or
audited.

**6. Short OOS period in a low-volatility bull market:**
The claimed OOS (2020–2025) does include the COVID crash but spans a period of generally
recovering and trending equity markets with compressed vol post-2020 — favorable for premium
selling. A true stress test would require the 2008 period, which is in-sample.

**What would change my mind:** An independent replication with clearly verified walk-forward
OOS methodology (no future data accessible to LLM), explicit bid-ask spread and commission
modeling, open publication of the Bayesian network structures, and assignment tracking that
counts rolled positions at their full economic cost.

## Strengths / Weaknesses

**Strengths:**
- The base wheel strategy has genuine economic rationale (VRP is well-documented)
- Hybrid architecture (LLM as model-builder, not decision-maker) is a creative design
- 18.75-year dataset is long by academic standards
- Comparison across 3 architectures (pure LLM, static BN, hybrid) is methodologically sound
  in design

**Weaknesses:**
- Lookahead bias risk in LLM data selection is not addressed
- 0% assignment rate is implausible and likely reflects either rolling-cost accounting
  gaps or definitional choices
- Max DD -8.2% through 2008 + 2020 = almost certainly understated
- Transaction costs not explicitly modeled
- No open code; not peer-reviewed; no community scrutiny
- Underlying assets not specified; exact entry/exit rules not documented
- Single OOS split only; no walk-forward; no second market test

## Confidence Score

**Per-dimension breakdown:**

| Dimension | Raw (0–10) | Weight | Weighted |
|-----------|-----------|--------|----------|
| Logical/economic soundness (falsifiable) | 4 | 3 | 12 |
| Transparency & reproducibility | 2 | 2 | 4 |
| Evidence auditability | 2 | 2 | 4 |
| Risk management quality | 2 | 2 | 4 |
| Cost & capacity realism | 2 | 2 | 4 |
| Honest treatment of drawdowns | 3 | 1.5 | 4.5 |
| Robustness evidence (OOS / walk-forward) | 2 | 1.5 | 3 |
| Survived independent scrutiny | 1 | 1 | 1 |

Weighted sum = 12 + 4 + 4 + 4 + 4 + 4.5 + 3 + 1 = **36.5**
Max possible = 150
`latent_score = round(36.5 / 150 × 100) = round(24.3) = **24**`

**Verification cap:** Evidence auditability = 2 (≤ 4) → cap at 59.
`confidence = min(24, 59) = **24**`

**Band: Low Confidence (0–39)**

`latent 24 (capped to 24 pending verification: arXiv preprint, no peer review, no open code, probable lookahead bias in LLM data selection, 0% assignment rate implausible, -8.2% max DD through 2008/2020 understated)`

## Complexity / Scalability / Automation Feasibility

**Complexity:** Very high. Requires an LLM integration layer, dynamic Bayesian network
construction, live options data feed, and real-time market condition parsing. Far more
complex than a rule-based system.

**Scalability:** The paper's LLM-BN approach requires an LLM API call per trade decision,
making it computationally tractable but operationally dependent on an LLM provider.

**Automation feasibility:** Medium-low. The LLM + BN pipeline is described at a high level
but cannot be reproduced without the specific LLM, prompt templates, BN structure, and
analogue-selection algorithm — none of which are published.

## MQL5 / Python / Pine Feasibility

**Python:** The base wheel strategy is straightforward to implement in Python with
`ibapi` (Interactive Brokers) or `alpaca-trade-api` for options execution. The LLM-BN
enhancement requires OpenAI/Anthropic API integration and a Bayesian network library
(e.g., `pgmpy`, `pymc`). The full hybrid cannot be implemented without the unpublished
methodology details.

**MQL5:** Not directly applicable (MetaTrader does not natively support options trading).

**Pine Script:** Not applicable (TradingView does not support options execution).

**Verdict:** The BASE wheel strategy is implementable in Python with standard tools. The
LLM-BN enhancement is NOT implementable from the published paper alone.

## Similar Strategies

- None in current archive with options/volatility premium focus. The economic mechanism
  (VRP harvest) is conceptually related to any short-volatility strategy.

## Red Flags

- `PROBABLE LOOKAHEAD BIAS`: LLM populates Bayesian network from "18.75-year dataset" —
  if full dataset is accessible at OOS decision points, future data contaminated the model
- `IMPLAUSIBLE METRIC`: 0% assignment rate over 18.75 years including 2008 crisis and
  March 2020 crash via rolling is extraordinarily unlikely
- `PROBABLE COST UNDERSTATEMENT`: Options bid-ask spread and rolling costs not explicitly
  modeled; 8,919 trades impacted by real-world execution costs
- `UNDERSTATED DRAWDOWN`: -8.2% max DD through 2008 + 2020 for a short-put strategy is
  structurally implausible unless rolling costs are excluded
- `OPAQUE MECHANISM`: LLM model, prompt, BN structure, analogue-selection algorithm all
  unpublished; no open code
- `NO PEER REVIEW`: arXiv preprint only

## Source Links

| Source | Retrieved | Notes |
|--------|-----------|-------|
| arXiv:2512.01123 abstract | 2026-05-29 | Primary source; not directly fetchable due to HTTP 403 |
| Search snippet: arXiv:2512.01123 | 2026-05-29 | Key metrics extracted from search result descriptions |
| harbourfronttechnologies.wordpress.com (HTTP 403) | 2026-05-29 | Blog post about paper; inaccessible |
| harbourfronts.com (HTTP 403) | 2026-05-29 | Company blog; inaccessible |

## Analyst Notes

**FACTS (from search result snippets describing the paper):**
- Authors: Xiaoting Kuang, Boken Lin
- arXiv:2512.01123, submitted November 30, 2025
- 18.75-year dataset (2007–Sep 2025), 8,919 total trades
- Claimed OOS test: January 2020 – September 2025
- Hybrid: CAGR 15.3%, Sharpe 1.08, max DD -8.2%
- Pure LLM baseline: CAGR 8.7%, Sharpe 0.45
- Static BN baseline: CAGR 11.2%, Sharpe 0.67
- Market benchmark: max DD -60% (implied ~S&P 500)
- 0% assignment rate maintained "through strategic option rolling"
- Underlying assets: NOT REPORTED in accessible sources

**ANALYSIS:**
- CAGR/vol implied Sharpe check: 15.3% CAGR with Sharpe 1.08 implies vol ≈ 15.3/1.08 ≈ 14.2%.
  A short-put options portfolio with -8.2% max DD and 14.2% annualized vol is internally
  somewhat consistent, but the max DD still seems low relative to the vol (typical rule of
  thumb: max DD ≈ 2–3× annual vol for diversified strategies → would imply max DD
  of ~28–43%, vs. reported 8.2%).
- An options wheel on SPY with zero assignment through 2008 would require rolling puts down
  from, say, 1500 to 700 (a 53% decline) at each roll, paying significant debits. The total
  rolling debit during the crisis could amount to the full loss avoided. If this debit is
  not in the P&L, the -8.2% max DD is fictitious.

**ASSUMPTIONS:**
- The strategy is applied to US equity options (assumed based on context; underlying assets
  not confirmed in available sources)
- "18.75-year dataset" likely includes all trades from training + OOS periods combined

## Future Research Needed

1. Full paper access to determine: (a) exact OOS methodology and whether LLM had access only
   to historical-at-time-of-decision data, (b) how rolling costs were accounted for, (c)
   what underlying assets were used, (d) whether bid-ask spreads were modeled.
2. Independent replication of the rolling accounting — is a position that was rolled 10 times
   eventually at a large debit counted as "no assignment"?
3. Community peer review — this paper has received no critical analysis as of 2026-05-29.
4. Out-of-sample period extension: how does performance look in a flat-to-bearish market
   with elevated realized vol (2022-type conditions)?
