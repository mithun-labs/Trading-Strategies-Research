# Bitcoin Multi-Timeframe Trend Strategy (D1H1 STOP)

## Overview

A progressive multi-timeframe trend-following strategy on Bitcoin, building from a naive
hourly MACD crossover baseline toward a daily-filtered, trailing-stop-equipped final model.
Developed by Mesíček and Vojtko (Quantpedia), SSRN 5748642 (Nov 2025). The core idea is
Alexander Elder's "Trade with the Tide" principle: use the daily timeframe to determine trend
direction and execute entries only on the hourly timeframe in that direction. The third
version adds a trailing stop to improve exits.

The paper evaluates three sequential model generations, each adding one layer:

1. **MACD Pure** — hourly MACD crossover on H1, no filter
2. **D1H1 Filter** — H1 MACD entry, filtered to only longs when D1 trend is bullish
3. **D1H1 STOP** — D1H1 Filter + trailing stop exit

---

## Asset Class / Timeframes

- **Asset:** Bitcoin (BTC/USD)
- **Exchange:** Gemini Exchange (data source)
- **Entry timeframe:** H1 (1-hour MACD crossover)
- **Trend filter timeframe:** D1 (daily MACD direction)
- **Backtest window:** December 2018 – approximately late 2025 (~7 years)

---

## Core Logic

### D1 Trend Filter
Determine the dominant daily trend using D1 MACD. When D1 MACD is bullish (positive
momentum), only long positions are taken. Short signals from H1 MACD are ignored entirely.
This is the application of Elder's "Triple Screen" / multi-timeframe hierarchy principle.

### H1 Entry Signal
Within the D1 trend direction, enter when H1 MACD crosses upward (bullish crossover). Exit
when H1 MACD crosses downward or trailing stop is hit.

### Trailing Stop (D1H1 STOP version)
An unspecified trailing stop is added to the D1H1 Filter. Exact parameters (ATR multiple,
percentage, or fixed pip) are **NOT REPORTED** in accessible sources.

### Direction
Long-only (strategy description explicitly refers to ignoring short signals). Whether the
strategy has a short component is NOT CONFIRMED in accessible sources.

---

## Economic Rationale

**Stated mechanism:** Higher-timeframe trend confirmation reduces noise from short-term
reversals. When daily momentum is positive, hourly long entries align with institutional
order flow and trend continuation. The trailing stop lets profits run during strong trend
phases while protecting gains.

**Falsifiability:** The edge should disappear in — and this is stated directly by the
mechanism — sideways/range-bound Bitcoin markets, where D1 MACD oscillates around zero,
generating false signals, and hourly entries achieve no momentum continuation. Also, in
prolonged bear markets, the long-only design produces zero entries, leaving capital idle.

**Assessment:** The mechanism is coherent. MACD trend-filtering has genuine theoretical
grounding (following institutional flow, reducing noise). However, the fact that the strategy
is effectively long-only during a 2018-2025 period where BTC appreciated dramatically (from
~$3,000 to ~$90,000+) raises the question of whether the edge is momentum or simply reduced
exposure to sharp drawdowns within a sustained uptrend. The trailing stop improvement may be
data-mined rather than mechanistically driven.

---

## Entry Conditions

1. D1 MACD is bullish (positive, or crossed above signal line — exact definition NOT REPORTED)
2. H1 MACD crosses upward (bullish crossover — exact parameters NOT REPORTED)
3. Open long position

---

## Exit Conditions

**D1H1 Filter:** Exit when H1 MACD crosses downward.

**D1H1 STOP:** Exit when H1 MACD crosses downward OR trailing stop is triggered. Trailing
stop specification NOT REPORTED in accessible sources.

---

## Risk Management

- **Stop mechanism:** Trailing stop (D1H1 STOP version only; parameters NOT REPORTED)
- **Position sizing:** NOT REPORTED
- **Maximum position loss:** NOT REPORTED
- **Regime exit / max drawdown limit:** NOT REPORTED
- **Leverage:** NOT REPORTED (assumed 1× from context, but not confirmed)

Assessment: Risk management is minimal — a trailing stop is present but no sizing rules,
no portfolio-level max-loss discipline, and no explicit regime exit rule is documented.

---

## Indicators Used

- **MACD** — D1 and H1 timeframes (exact parameters, e.g. 12/26/9, NOT REPORTED in accessible sources)
- Inspired by Alexander Elder's multi-timeframe hierarchy / "Triple Screen" methodology

---

## Transaction Costs & Capacity

**Costs modeled:** NO — transaction costs are explicitly NOT modeled in accessible sources.
This is a significant gap.

**Cost impact analysis (ANALYSIS):**
~1,000 trades over 7 years = ~143 trades/year. At Gemini BTC spot fees of 0.10–0.40%
per side (maker/taker), a round-trip cost of 0.20–0.80% per trade implies annual cost
burden of 143 × 0.5% (midpoint) ≈ **71.5% per year**. Even at the best-case 0.20% per
round trip: 143 × 0.20% = **28.6% per year** in fees alone. Annual return for D1H1 STOP
is NOT REPORTED, but even if comparable to D1H1 Filter (~6–10%), costs likely exceed
gross returns. **This is a presumptive invalidation flag for the gross edge.**

Note: Gemini fees are historically higher than Binance (which uses 0.05-0.10% taker).
Even at 0.10% round trip: 143 × 0.10% = 14.3%/year. Against implied CAGR (ANALYSIS: if
Calmar 0.87 × assumed max DD ~12%: CAGR ≈ 10.4%), costs may still erase the gross edge.

**Capacity:** Bitcoin on Gemini is highly liquid; capacity is not a binding constraint for
retail-scale trading. For institutional size, slippage becomes a factor but is not the
primary concern here.

---

## Backtest Evidence

**Status:** CLAIMED, UNVERIFIED

- SSRN 5748642 working paper (Mesíček and Vojtko, Nov 2025)
- Not peer-reviewed
- No open code found
- No independent replication found
- All primary sources returned HTTP 403; evidence from search engine summaries only

---

## Forward-Test Evidence

**Status:** NOT REPORTED — no forward test or live trading results found in accessible sources.

---

## Performance Metrics

The paper presents three model versions. Values tagged below are for the progression:

| Metric | Value (MACD Pure) | Value (D1H1 Filter) | Value (D1H1 STOP) | Status | Source |
|--------|------------------|--------------------|--------------------|--------|--------|
| Annual Return | 4.6% | 6.6% | NOT REPORTED | CLAIMED, UNVERIFIED | SSRN 5748642 / search summaries |
| Sharpe Ratio | 0.33 | 0.80 | 1.07 | CLAIMED, UNVERIFIED | SSRN 5748642 / search summaries |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | 0.87 | CLAIMED, UNVERIFIED | SSRN 5748642 / search summaries |
| Maximum Drawdown | −23.9% | −12.4% | NOT REPORTED | CLAIMED, UNVERIFIED | SSRN 5748642 / search summaries |
| Number of Trades | ~2,200 | ~1,000 | ~1,000 | CLAIMED, UNVERIFIED | SSRN 5748642 / search summaries |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | NOT REPORTED | NOT REPORTED | — |
| CAGR | NOT REPORTED | NOT REPORTED | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | NOT REPORTED | NOT REPORTED | NOT REPORTED | NOT REPORTED | — |
| Recovery Factor | NOT REPORTED | NOT REPORTED | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | NOT REPORTED | NOT REPORTED | NOT REPORTED | NOT REPORTED | — |

**BTC Buy-and-Hold benchmark (reported in paper):** Annual return >60%, max drawdown ~−80%.

**Sharpe > 3 trigger:** Does NOT apply (max Sharpe = 1.07).

**ANALYSIS — Calmar implied CAGR:**
If Calmar (D1H1 STOP) = 0.87 and Max DD (D1H1 Filter, assumed similar) ≈ 12.4%, then implied
CAGR ≈ 0.87 × 12.4% ≈ 10.8%. This is a rough ANALYSIS estimate only — NOT a table entry.

---

## Community Sentiment

No community discussion of SSRN 5748642 specifically found in accessible sources (no
forum threads, no r/algotrading posts, no independent blogger review retrieved). This is
a very recent paper (Nov 2025) from a niche Quantpedia research blog. Absence of criticism
is not a positive signal here — it reflects limited community awareness, not resilience
to scrutiny.

Quantpedia's work generally attracts limited independent verification; their papers are
working papers by their research team, not externally reviewed.

---

## Why This Might Be Nothing

1. **Long-only bull market beta:** The 2018-2025 period includes Bitcoin's most dramatic
   appreciation (from ~$3,000 to $90,000+). A long-only strategy on this asset will
   mechanically look good in a period dominated by uptrends. The Sharpe improvement from
   0.33 to 1.07 may reflect nothing more than the D1H1 Filter switching off during bear
   phases, reducing equity drawdown — which is a genuine but trivial result: any "hold less
   BTC during downtrends" rule would improve Sharpe in a period with multiple violent
   drawdowns.

2. **Transaction cost presumptive invalidation:** ~143 trades/year × ~0.5% round-trip
   cost = ~71.5% annual cost burden at typical Gemini rates. This very likely erases the
   gross edge entirely (see Transaction Costs section for full ANALYSIS). Gross annual
   return for D1H1 STOP is NOT REPORTED, but even generous assumptions suggest costs could
   consume the edge.

3. **In-sample selection over 3 versions:** The paper evaluates three progressively complex
   versions on the same dataset. The "best" (D1H1 STOP) was selected post-hoc. No
   walk-forward or OOS validation is mentioned in accessible sources — the improvement
   from MACD Pure (Sharpe 0.33) to D1H1 STOP (Sharpe 1.07) may substantially reflect
   in-sample fitting of the trailing stop parameter.

4. **MACD is a lagging indicator:** MACD crossover strategies are consistently negative
   after costs in equity markets due to whipsaw. The improvement here may reflect not
   that MACD works, but that combining daily and hourly filters reduces trade frequency
   enough to reduce the cost drag — which is not alpha, just less beta at lower transaction
   cost.

5. **Same research group, correlated evidence:** This is the third consecutive Bitcoin
   strategy from Vojtko/Quantpedia/Beluška in the research database. The group's work is
   methodologically consistent, but reviewing a single group's papers creates correlated
   evidence — they share the same dataset biases and optimism about Bitcoin trend strategies.

6. **The single piece of evidence that would change my mind:** A cost-inclusive walk-forward
   test covering at least the 2022 bear market (BTC −70%) and 2023-2024 recovery as OOS
   periods, with explicit fee modeling using Gemini's actual trading fees and realistic
   slippage. Without this, the gross Sharpe improvement is not meaningful.

---

## Strengths / Weaknesses

### Strengths
- Clear, reproducible logic (Elder D1H1 is public domain and well-documented)
- Progressive methodology: builds complexity logically, each addition motivated
- Substantial drawdown improvement vs. B&H (max DD −12.4% vs. −80% for B&H)
- Reduced trade count (2,200 → 1,000) is a cost-awareness positive signal
- Authors acknowledge comparison to B&H benchmark honestly

### Weaknesses
- No transaction costs modeled — renders absolute performance figures meaningless
- No OOS / walk-forward period; single in-sample window
- Long-only during a massive structural bull market — survivorship regime bias
- D1H1 STOP performance is incomplete (annual return NOT REPORTED)
- Parameters not fully specified in accessible sources
- Same authors as bitcoin-max-min-trendrev — correlated evidence base
- No independent replication

---

## Confidence Score

**Per-dimension breakdown:**

| Dimension | Weight | Score | Contribution |
|-----------|--------|-------|-------------|
| Logical / economic soundness (falsifiable) | 3 | 6 | 18 |
| Transparency & reproducibility | 2 | 5 | 10 |
| Evidence auditability | 2 | 2 | 4 |
| Risk management quality | 2 | 3 | 6 |
| Cost & capacity realism | 2 | 1 | 2 |
| Honest treatment of drawdowns / failure | 1.5 | 4 | 6 |
| Robustness evidence (OOS / walk-forward / multi-market) | 1.5 | 2 | 3 |
| Survived independent scrutiny | 1 | 1 | 1 |

**Weighted sum:** 50 / 150 max

**latent 33 (capped to 33 — Evidence auditability ≤ 4, cap = 59; latent already below cap)**

**Confidence: 33 — LOW CONFIDENCE**

*Notes on key drags:*
- Cost/capacity = 1: costs entirely absent on a 143-trade/year strategy
- Evidence auditability = 2: SSRN working paper, no peer review, no open code, no replication
- Robustness = 2: no OOS or walk-forward, single 7-year in-sample window
- Scrutiny = 1: no independent community scrutiny found

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Low — MACD crossover on two timeframes with a trailing stop
- **Scalability:** Bitcoin is highly liquid; retail scale poses no capacity constraint
- **Automation:** Straightforward — two MACD calculations on H1/D1 + trailing stop logic
- **Implementation risk:** Main risk is parameter sensitivity and cost overhead at scale

---

## MQL5 / Python / Pine Feasibility

- **MQL5:** Straightforward — native multi-timeframe MACD available via `iMACD()` on
  multiple timeframes; trailing stop is standard
- **Python:** Simple with `pandas`, `ta-lib`, or manual MACD calculation + ccxt for data
- **Pine Script (TradingView):** Fully supported; multi-timeframe MACD via `request.security()`
- **Feasibility note:** Implementation is easy; the challenge is modeling realistic costs
  before any performance conclusion is possible

---

## Similar Strategies

- `bitcoin-max-min-trendrev` — same authors (Vojtko/Quantpedia), same asset; rolling
  MAX/MIN price signal vs. MACD multi-TF; related research stream
- `catching-crypto-trends-donchian-ensemble` — Donchian channel trend following on crypto;
  different mechanism but same broad theme; has OOS evidence

---

## Red Flags

- **COSTS OMITTED on a ~143-trade/year strategy** — high-severity flag; presumptive
  invalidation until cost-inclusive test is shown
- **Long-only in a structural bull market** — results may reflect secular Bitcoin trend,
  not alpha generation
- **No OOS or walk-forward** — in-sample parameter fitting risk for trailing stop
- **Same research group dominance** — third Vojtko/Quantpedia Bitcoin paper in database
- **D1H1 STOP performance incomplete** — annual return NOT REPORTED; hard to assess magnitude

---

## Source Links

| URL | Retrieved |
|-----|-----------|
| [SSRN 5748642](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5748642) | 2026-05-30 |
| [Quantpedia blog post](https://quantpedia.com/how-to-design-a-simple-multi-timeframe-trend-strategy-on-bitcoin/) | 2026-05-30 (HTTP 403) |
| [ainvest.com summary](https://www.ainvest.com/news/crypto-quant-trading-trend-models-enhancing-bitcoin-performance-multi-timeframe-strategies-elder-d1h1-filters-2512/) | 2026-05-30 (HTTP 403) |

*Note: All primary sources returned HTTP 403. Evidence sourced entirely from search engine
summaries and snippets, which extracted text from the Quantpedia blog post and the SSRN
abstract.*

---

## Analyst Notes

**FACTS (from sources):**
- Authors: David Mesíček and Radovan Vojtko (Quantpedia)
- Data: BTC/USD, Gemini Exchange, Dec 2018 – ~2025
- Three strategy versions evaluated sequentially
- MACD Pure: annual return 4.6%, Sharpe 0.33, max DD −23.9%, ~2,200 trades
- D1H1 Filter: annual return 6.6%, Sharpe 0.80, max DD −12.4%, ~1,000 trades
- D1H1 STOP: Sharpe 1.07, Calmar 0.87; annual return and max DD NOT REPORTED
- BTC B&H: annual return >60%, max DD ~−80%
- No transaction costs modeled
- No OOS or walk-forward reported

**ANALYSIS (my reasoning):**
- The trade reduction from 2,200 to 1,000 is directionally positive but the absolute
  cost burden at typical crypto exchange fees likely remains excessive vs. gross returns
- Calmar 0.87 implies CAGR/max_DD ≈ 0.87; if max DD similar to D1H1 Filter (~12%),
  implied CAGR ≈ 10–11%. This is low for Bitcoin and plausibly sub-cost
- The D1H1 approach genuinely reduces drawdown (from −80% B&H to −12%), which has value
  as a risk-management tool, but that benefit exists independent of whether the strategy
  generates alpha after costs
- The research is methodologically transparent for a Quantpedia working paper; the
  incremental build (add one element, test again) is a reasonable approach, but evaluating
  multiple versions on the same dataset inflates the probability of selecting an
  over-optimized version by chance

**ASSUMPTIONS (flagged):**
- ASSUMED long-only: description explicitly mentions "ignoring short signals," but whether
  this is the only direction or whether short entries exist is not definitively confirmed
  in accessible summaries
- ASSUMED max DD for D1H1 STOP ≈ similar to D1H1 Filter (~12%) for ANALYSIS calculations;
  actual value NOT REPORTED

---

## Future Research Needed

1. **Cost-inclusive backtest:** Add realistic Gemini exchange fees and rerun; the strategy
   conclusion may reverse entirely
2. **OOS period:** Carve out 2022 (BTC −70%) as OOS; test whether D1H1 STOP survives the
   bear market regime (long-only, so likely minimal entries)
3. **Short-side extension:** Test whether adding shorts when D1 is bearish improves
   performance in bear markets; changes the nature of the strategy but addresses the
   long-only bias
4. **Independent replication:** The Quantpedia working paper needs independent replication
   with open code before any confidence score increase is warranted
5. **Full parameter specification:** Exact MACD parameters and trailing stop specification
   needed to confirm reproducibility
