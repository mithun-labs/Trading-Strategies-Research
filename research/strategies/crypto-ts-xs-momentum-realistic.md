# Crypto Time-Series and Cross-Sectional Momentum (Realistic Costs)

## Overview

A comprehensive analysis of time-series (TS) and cross-sectional (XS) momentum strategies
in the cryptocurrency market, explicitly modeled with realistic transaction costs (15 bps per
trade). Authors: Chulwoo Han, Byeongguk Kang, Jehyeon Ryu — Sungkyunkwan University, South
Korea. SSRN 4675565 (December 2023). The key finding: **TS momentum survives realistic
costs** (Sharpe 1.51 vs market 0.84 at 28-day lookback / 5-day hold), while **XS momentum
does not** — many XS strategies with statistically significant gross returns yield insignificant
or negative profits after costs. The momentum effect is concentrated in large winners; losers
tend to mean-revert.

The TS momentum strategy is effectively a long-only market-timing rule: hold the aggregate
crypto market portfolio when 28-day past return is positive, otherwise stay flat. This is
similar to a simple trend filter applied to the market, not stock selection.

---

## Asset Class / Timeframes

- **Asset:** Cryptocurrencies — ~471 coins filtered by market cap + volume from Binance
- **Full sample:** January 26, 2014 – August 28, 2023
- **Primary analysis period:** January 1, 2017 – August 28, 2023 (~6.5 years)
- **Rebalancing:** Every 5 days (holding period)
- **Timeframe:** Daily (signal calculated on daily returns)

---

## Core Logic

### Time-Series (TS) Momentum — Long-Only Market Timing

**Formation:** Compute past return of the market portfolio over a J-day lookback period.

**Signal:** If market portfolio past J-day return is positive → long the market (or a
portfolio of coins). If negative → go flat (cash).

**Best parameters (reported):** J = 28 days lookback, H = 5 days holding period.

**Long-only design:** The TS strategy exploits continuation in the upside but avoids the
downside by going flat — it does not short the market.

### Cross-Sectional (XS) Momentum — Long-Short Coin Ranking

**Formation:** Rank all coins by past J-day return.

**Signal:** Long top quintile (winners), short bottom quintile (losers). Equal-weighted.

**Result:** XS momentum is weak after costs. Large winners continue, but losers
mean-revert and inflict losses on the short book. The "overreaction reversal" in losers
undermines XS performance.

### Coin Universe Filtering

Two filters applied: (1) minimum market capitalization, (2) minimum trading volume. In 2023,
the combined filter yielded ~471 coins (vs 1,332 coins from cap filter alone — volume is the
binding constraint). This ensures portfolio constituents are tradeable.

---

## Economic Rationale

**TS momentum mechanism:** Investor overreaction to news creates trending behavior in
cryptocurrency returns. Retail-dominated crypto markets are subject to herding, feedback
trading, and delayed price discovery, all of which generate short-term momentum. The effect
is strongest among large-cap winners (high attention, high liquidity, high retail participation).

**Falsifiability:** The TS edge should disappear when: (1) cryptocurrency markets mature
and attract institutional arbitrageurs who immediately price news; (2) the overreaction
mechanism is arbitraged away by momentum strategies themselves; (3) market structure shifts
(e.g., perpetual futures funding rates dominate price discovery, changing the signal-to-noise
ratio in spot returns). The paper itself acknowledges that "what drives overreaction is
unclear" — the mechanism is proposed but the specific driver is not nailed down.

**XS momentum failure after costs:** Consistent with academic crypto momentum literature —
the cross-sectional spread exists in gross returns but illiquidity and trading costs of the
loser book destroy the net edge. This is a clean, falsified result, not a speculation.

---

## Entry Conditions

**TS Momentum:**
1. At the close of day t, compute the J-day (28-day best) past return of the market portfolio
2. If positive → enter long position in the market portfolio (or equal-weighted coin basket)
3. Hold for H days (5-day best)

**XS Momentum:**
1. Rank all eligible coins by J-day past return
2. Long top quintile (20% of coins)
3. Short bottom quintile (20% of coins)
4. Equal-weighted within each quantile
5. Rebalance every H days

---

## Exit Conditions

- TS: Exit after H-day holding period; re-evaluate signal daily or at each rebalance
- TS: Go flat (exit long) when 28-day market momentum flips negative
- XS: Rebalance at each holding period; close shorts and longs, re-rank

---

## Risk Management

- **Stop-loss:** NOT REPORTED
- **Position sizing:** Equal-weighted within coin basket (TS uses market portfolio)
- **Maximum position / portfolio loss limit:** NOT REPORTED
- **Leverage:** NOT REPORTED (assumed 1× for TS long-only; XS may require margin for shorts)
- **Implicit downside protection (TS):** Going flat when momentum is negative avoids large
  drawdowns; this is the primary risk management mechanism

Assessment: TS has rudimentary risk management (going flat = exiting risk), which is better
than nothing. XS has no explicit risk management beyond equal weighting. Neither specifies
stops or max drawdown limits.

---

## Indicators Used

- **TS momentum:** J-day rolling return of the market portfolio (J = 28 best)
- **XS momentum:** J-day rolling return of individual coins, used for ranking
- No technical indicators — pure price-based momentum signals

---

## Transaction Costs & Capacity

**Costs modeled:** YES — 15 basis points (0.15%) per trade explicitly assumed.

**Round-trip cost:** 30 bps per rebalance cycle.

**Annual cost burden (ANALYSIS):** With a 5-day holding period, there are ~52 round trips
per year. 52 × 0.30% = **15.6% per year** in transaction costs. This is the key test the
paper is designed to perform: does the gross edge exceed this burden?

**For TS momentum:** Yes — Sharpe 1.51 after costs (vs market 0.84), suggesting a meaningful
net edge.

**For XS momentum:** No — many strategies with significant gross returns yield insignificant
net profits. The loser book short positions face particularly high costs (illiquid coins,
wider spreads, borrow costs).

**15bps realism check (ANALYSIS):** Binance maker fee ~10bps, taker fee ~20bps. For limit
orders, 15bps per trade is achievable. For market orders, 20bps per trade is more realistic.
The coin universe is filtered for minimum volume (471 coins vs 1,332 unfiltered), which
improves cost assumptions. However, even within the filtered universe, smaller coins may
face wider spreads. 15bps is a reasonable floor assumption for a large-cap subset but may
underestimate costs for the full 471-coin universe.

**Capacity:** No capacity discussion in accessible sources. With 471 coins and daily
rebalancing, capacity may be limited for institutional-scale trading.

---

## Backtest Evidence

**Status:** CLAIMED, UNVERIFIED

- SSRN 4675565 working paper (Han, Kang, Ryu, Dec 2023); Sungkyunkwan University
- Not peer-reviewed (SSRN working paper as of 2026-05-30; no confirmed journal publication found)
- No open code found
- No independent replication found
- Primary sources returned HTTP 403; evidence from search engine summaries

---

## Forward-Test Evidence

**Status:** NOT REPORTED — no forward test or live trading results found.

---

## Performance Metrics

**Reported for TS Momentum (28-day lookback, 5-day hold):**

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | 1.51 (TS strategy); 0.84 (market portfolio) | CLAIMED, UNVERIFIED | SSRN 4675565 / search summaries |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | — |
| CAGR | NOT REPORTED | NOT REPORTED | — |
| Annual Return | NOT REPORTED | NOT REPORTED | — |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | 5 days (holding period) | CLAIMED, UNVERIFIED | SSRN 4675565 / search summaries |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | NOT REPORTED | NOT REPORTED | — |

**XS Momentum:** Sharpe NOT REPORTED explicitly; stated to be weak / insignificant after
15bps costs in most configurations.

**Sharpe 1.51 < 3 trigger:** Does NOT apply. No mandatory scrutiny trigger.

**ANALYSIS — Sharpe comparison context:**
A Sharpe of 1.51 for a crypto TS momentum strategy is plausible if the edge primarily comes
from avoiding large downside periods (2018-2019 and 2022 crypto bear markets) while
participating in the 2020-2021 bull market. This is consistent with a defensive market-timing
rule, not exceptional alpha generation. The market Sharpe of 0.84 is notably high for a 6.5-year
window that includes the 2017-2021 crypto bull cycles, implying this is a period favorable to
crypto longs in general.

---

## Community Sentiment

- The paper has been cited by at least one follow-up paper ("Cryptocurrency Market Risk-Managed
  Momentum Strategies," published in Finance Research Letters 2025, ScienceDirect), suggesting
  it is treated as a credible contribution in the academic crypto literature
- No forum-level discussion of this specific paper found in accessible searches
- The broad conclusion (TS momentum > XS in crypto) is consistent with other academic crypto
  momentum papers and is not controversial

---

## Why This Might Be Nothing

1. **TS momentum is a bull market timer, not alpha generation:** The strategy is long-only
   and goes flat in negative momentum periods. Over the 2017-2023 window, which includes two
   massive crypto bull cycles, a rule that "stays long when things are going up" will trivially
   beat buy-and-hold on a Sharpe basis. The correct question is whether this rule would have
   the same edge in a prolonged sideways or bear market (e.g., if 2023-2025 data were included
   with no new bull cycle). The post-sample period (2023-2025) is not evaluated.

2. **Sharpe improvement may be entirely from drawdown avoidance:** The paper notes that
   "superior performance mainly results from reduced downside risk." This means the TS
   strategy's Sharpe gain over B&H comes from skipping crypto crashes — not from generating
   better returns. A simple "exit when Bitcoin drops below 200-day MA" would achieve similar
   results with fewer parameters and less overfitting risk.

3. **Parameter concentration:** The reported best parameters (28-day / 5-day) were selected
   from a grid of lookback/holding combinations (5 to 150 days). This is a parameter search
   on a single in-sample window — there is no confirmed OOS test. The sensitivity of the 1.51
   Sharpe across neighboring parameters (e.g., 25-day / 5-day, 30-day / 4-day) is not reported
   in accessible sources.

4. **15bps cost assumption may be optimistic for the full 471-coin universe:** Smaller coins
   in the filtered universe likely have wider bid-ask spreads than the 15bps assumption captures.
   For the XS short book specifically, borrowing costs for crypto are not mentioned but can
   be significant.

5. **Liquidation risk in XS:** The paper explicitly identifies liquidation risk as a concern
   for momentum portfolios (a coin can go to near-zero), but this risk is not fully captured
   by the 15bps cost assumption alone. For XS shorts, a coin that gaps up dramatically creates
   catastrophic loss risk not reflected in steady-state transaction cost modeling.

6. **Sample period ends August 2023:** Misses the 2023-2025 period including the BTC ETF
   launch and subsequent rally. The FTX collapse (Nov 2022) and subsequent recovery are
   included, but the full post-collapse structure is not evaluated.

7. **What would change my mind:** An OOS test on 2023-2025 data with the same 28/5 parameters,
   confirming that the TS strategy still outperforms buy-and-hold at 15bps costs. I do not have this.

---

## Strengths / Weaknesses

### Strengths
- **Costs explicitly modeled** — this is the primary contribution; the paper is designed as a
  corrective to prior studies that ignore costs
- Simple, replicable strategy (long market when 28-day momentum positive)
- Large and filtered coin universe (471 coins with volume filter = tradeable)
- XS failure after costs is a clean, honest negative result
- Consistent with broader crypto momentum literature (TS outperforms XS)
- 6.5-year primary analysis period is reasonable for crypto

### Weaknesses
- **No OOS / walk-forward period** — selected best parameters on full in-sample window
- Long-only TS in a crypto bull market epoch — regime-specific result
- No open code; no peer review; SSRN working paper only
- Maximum drawdown NOT REPORTED (critical risk metric)
- XS requires margin/shorting — not accessible to all traders
- 15bps cost assumption may understate actual costs for smaller coins

---

## Confidence Score

**Per-dimension breakdown:**

| Dimension | Weight | Score | Contribution |
|-----------|--------|-------|-------------|
| Logical / economic soundness (falsifiable) | 3 | 6 | 18 |
| Transparency & reproducibility | 2 | 7 | 14 |
| Evidence auditability | 2 | 4 | 8 |
| Risk management quality | 2 | 4 | 8 |
| Cost & capacity realism | 2 | 8 | 16 |
| Honest treatment of drawdowns / failure | 1.5 | 6 | 9 |
| Robustness evidence (OOS / walk-forward / multi-market) | 1.5 | 4 | 6 |
| Survived independent scrutiny | 1 | 4 | 4 |

**Weighted sum:** 83 / 150 max

**latent 55 (capped to 55 — Evidence auditability = 4 (≤4), cap = 59; latent below cap)**

**Confidence: 55 — EXPERIMENTAL**

*Notes on score:*
- **Cost/capacity = 8** is the notable strength — this paper explicitly models costs at
  15bps; the entire contribution is making cost-realistic claims
- **Evidence auditability = 4** because the methodology is clearly described and rigorous,
  but SSRN working paper without peer review and without open code keeps it below 5
- **Robustness = 4**: reasonable dataset length, but no OOS split; parameters selected in-sample

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Very low (TS) — compute J-day rolling return, binary on/off signal
- **Complexity:** Medium (XS) — rank 471 coins daily, maintain long-short book
- **Scalability:** TS scales to any size; XS capacity-constrained by illiquid coins
- **Automation:** Straightforward for TS; XS requires robust data feed for 471+ coins

---

## MQL5 / Python / Pine Feasibility

- **Python:** Easiest — `pandas`, `ccxt` for Binance data; 5-line implementation for TS signal
- **Pine Script:** TS strategy trivially implemented; XS requires multi-asset management (external tool)
- **MQL5:** MT5 + crypto broker required; TS logic is simple; XS multi-symbol portfolio management
  more complex but doable with MT5's multi-symbol framework

---

## Similar Strategies

- `catching-crypto-trends-donchian-ensemble` — Donchian channel trend following on BTC + alts;
  momentum-based but uses Donchian bands instead of rolling return signal; broader multi-coin approach
- `adaptivetrend-crypto` — 6-hour BTC trend following with rolling Sharpe asset selection;
  related but more complex with different entry mechanism
- `bitcoin-max-min-trendrev` — rolling MAX/MIN price signal on BTC; single-asset vs multi-coin;
  different mechanism but same broad momentum/trend theme

---

## Red Flags

- **No OOS test** — best parameters (28/5) selected on full in-sample window; overfitting risk
- **Long-only TS in crypto bull epoch** — Sharpe advantage may be regime-specific
- **XS momentum fails after costs** — the more interesting multi-coin strategy doesn't survive
- **Max drawdown NOT REPORTED** — critical risk metric for a long-only crypto strategy missing
- **SSRN working paper only** — no peer review; not independently replicated

---

## Source Links

| URL | Retrieved |
|-----|-----------|
| [SSRN 4675565](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4675565) | 2026-05-30 (HTTP 403) |
| [AUT institutional PDF](https://acfr.aut.ac.nz/__data/assets/pdf_file/0009/918729/Time_Series_and_Cross_Sectional_Momentum_in_the_Cryptocurrency_Market_with_IA.pdf) | 2026-05-30 (HTTP 403) |
| [Semantic Scholar entry](https://www.semanticscholar.org/paper/7ef80b7cc68aea58bb78206b00fbc1ef1da91a37) | 2026-05-30 (HTTP 403) |
| [ResearchGate](https://www.researchgate.net/publication/377457967_Time-Series_and_Cross_Sectional_Momentum_in_the_Cryptocurrency_Market_A_Comprehensive_Analysis_under_Realistic_Assumptions) | 2026-05-30 (HTTP 403) |

*Note: All primary sources returned HTTP 403. Evidence sourced entirely from search engine
summaries and snippets.*

---

## Analyst Notes

**FACTS (from sources):**
- Authors: Han, Kang, Ryu — Sungkyunkwan University, South Korea
- SSRN 4675565, December 2023; not confirmed peer-reviewed as of 2026-05-30
- Data: Binance, Jan 2014 – Aug 2023; primary analysis Jan 2017 – Aug 2023
- Coin universe: ~471 coins filtered by market cap + volume (1,332 from cap filter alone)
- Transaction costs: 15 bps per trade explicitly assumed
- Best TS momentum: 28-day lookback, 5-day holding period → Sharpe 1.51 (market: 0.84)
- TS outperformance mainly from reduced downside risk
- XS momentum: weak after costs; many strategies statistically significant gross but insignificant net
- Momentum concentrated in large winners; losers rebound (mean-reverse)

**ANALYSIS (my reasoning):**
- The TS strategy is a long-only market timer, not a stock-selection strategy. Its performance
  should be evaluated relative to a "hold BTC / exit on momentum signal" benchmark, not vs. the
  passive market portfolio
- The 28-day / 5-day parameter combination was searched across multiple combinations on one
  dataset — the parameter-searching risk is not negligible
- 15bps per trade is reasonable for the filtered, liquid subset of 471 coins; the annualized
  cost burden at 5-day rebalancing is ~15.6% which is meaningful; the fact that TS still shows
  Sharpe 1.51 after this cost is a genuine positive signal
- The XS failure is a high-information finding: the strategy that would generate alpha in
  traditional equities (long-short momentum) does NOT survive in crypto after costs

**ASSUMPTIONS (flagged):**
- ASSUMED the Sharpe of 1.51 is net of 15bps costs (consistent with paper's design);
  the paper's primary contribution is showing cost-adjusted results
- ASSUMED equal-weighted coin basket for TS (not confirmed from accessible summaries)

---

## Future Research Needed

1. **OOS test:** Carve out 2023-2025 as OOS; test whether 28-day/5-day TS momentum
   remains Sharpe > market (without re-optimizing parameters)
2. **Robustness check:** Test sensitivity of Sharpe to nearby parameter combinations
   (25/5, 30/5, 28/7) — if edge disappears with minor parameter variation, it's overfit
3. **Higher cost assumption:** Re-run with 20bps (market orders) and 30bps per trade;
   determine at what cost level the TS edge disappears
4. **2022 bear market isolation:** Is the edge driven entirely by going flat during
   2018-2019 and 2022 crashes? Strip these periods and measure standalone performance
5. **Long-short TS:** Test whether a short-side add-on (short market when 28-day
   momentum negative) improves or degrades Sharpe and drawdown
