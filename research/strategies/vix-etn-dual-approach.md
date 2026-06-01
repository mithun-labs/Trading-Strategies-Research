# VIX ETN Dual Approach (Volatility Risk Premium + Term Structure)

## Overview

Swiss Finance Institute Research Paper (June 2025, SSRN 5316487) by Zarattini, Aziz, and Mele.
Tests four progressively sophisticated strategies for trading VIX-linked ETNs (VXX/UVXY/SVXY),
from a constant short-volatility baseline to a dynamically sized strategy driven by two complementary
signals: the option-market volatility risk premium (VRP) and the VIX futures term structure slope.
After realistic transaction costs, the final strategy compounds at 16.3%/yr with Sharpe 1.0 and
~15% correlation to equity markets over 2008–2025. Authors: Carlo Zarattini (Concretum Group /
Swiss Finance Institute), Andrew Aziz, Antonio Mele.

---

## Asset Class / Timeframes

- **Markets:** US Volatility ETNs/ETFs — VXX (long vol), UVXY (1.5x long vol), SVXY/SVIX (inverse/short vol)
- **Frequency:** Daily rebalancing
- **Holding period:** Variable (position held while signals are active; no fixed duration)
- **Data period tested:** 2008–2025 (17 years); note: VXX launched January 2009, UVXY launched
  October 2011 — pre-inception data must use synthetic VIX futures proxies

---

## Core Logic

The paper tests four rule sets in order of increasing sophistication:

**Strategy 1 (Baseline):** Constant short-volatility allocation — sell short vol exposure (e.g.,
hold SVXY/SVIX, or short VXX) unconditionally. Captures the roll yield in contango (VIX futures
in contango ~84% of the time), but vulnerable to tail events.

**Strategy 4 (Final — main result):** Dynamic sizing driven by two signals simultaneously:
1. **Option-market volatility risk premium (VRP):** Compare VIX (30-day implied volatility) to
   short-term realized volatility. When VIX > realized vol → positive VRP → go short volatility.
   When realized vol > VIX → negative VRP → reduce/exit short volatility exposure.
2. **VIX term structure slope:** VIX futures in contango (M1 < M2) supports short-vol positioning;
   backwardation reverses the trade. A steep contango ≈ high roll yield for short-vol ETNs.

The strategy EXITS or reduces exposure when: (a) realized vol exceeds implied vol (VRP negative)
OR (b) term structure inverts into backwardation. Both conditions independently signal elevated
tail risk.

Position sizing is dynamic, scaling with the magnitude of the VRP and/or term structure signals
rather than binary on/off. Automation via standard broker API is described.

---

## Economic Rationale

**Mechanism:** Structural demand for volatility insurance creates a persistent volatility risk
premium: options and VIX futures are systematically priced above realized volatility. Hedgers
(institutional investors, portfolio managers) pay a systematic premium for downside insurance,
and the seller of this insurance earns that premium as compensation for bearing jump/tail risk.

**Why this edge exists:** The VRP is a well-documented risk premium across multiple asset classes
(equity, FX, commodity, fixed income). In equity markets, the VIX has averaged ~4–5 vol points
above subsequent realized vol over long horizons (Carr/Wu 2009; Bakshi/Kapadia 2003). The edge
is structural, not behavioral — it compensates for genuine tail risk.

**Falsifiability (edge should disappear when):**
1. VIX persistently underestimates realized volatility → negative VRP regime → no premium to harvest
2. VIX term structure inverts into sustained backwardation → no roll yield
3. Crowding in the short-vol trade reduces available premium (pre-Volmageddon 2018 regime)
4. ETN product structural changes eliminate the roll-yield mechanism (e.g., SVXY leverage cut
   from -1x to -0.5x after February 2018)

**Assessment:** The economic mechanism is genuine and well-documented. The dual-signal exit
rules give specific failure conditions. This PASSES the falsifiability test.

---

## Entry Conditions

- **Short-vol entry triggered when both:**
  - VIX (30-day implied vol) > short-term realized volatility (positive VRP)
  - VIX futures term structure in contango (M1 < M2, i.e., front-month < second-month contract)
- **Long-vol entry (defensive):** Implied by backwardation signal or negative VRP
- **Specific thresholds and lookback windows:** NOT RETRIEVED (paper behind HTTP 403)
- **Position sizing:** Dynamic, scaled to signal magnitude (specific sizing rule NOT RETRIEVED)

---

## Exit Conditions

- **Exit / reduce short-vol when:**
  - Realized vol > VIX (negative VRP → short-vol insurance cost exceeds premium)
  - VIX term structure inverts into backwardation
- **Trailing stop:** NOT REPORTED
- **Maximum holding period:** NOT REPORTED

---

## Risk Management

- **Volatility filter:** Realized vol vs. VIX (implied vol) comparison — primary risk manager.
  Cuts or reduces exposure when realized risk is elevated. This is the key defense against
  tail events, though effectiveness during sudden overnight/intraday spikes is uncertain.
- **Term structure signal:** Provides regime awareness; backwardation = elevated risk environment.
- **Dynamic sizing:** Reduces position when signals weaken.
- **Position sizing rule:** NOT REPORTED (specific formula not accessible)
- **Stop-loss / max-loss:** NOT REPORTED
- **Maximum drawdown limit:** NOT REPORTED

**Critical risk management gap:** For a short-volatility strategy, the maximum drawdown during
stress events (February 2018, March 2020) is the single most important metric. This is NOT
REPORTED in accessible summaries. The strategy's Sharpe 1.0 over 2008–2025 implies the risk
management worked, but cannot be independently verified.

---

## Indicators Used

- VIX (CBOE 30-day implied volatility index)
- VIX M1 futures (front month)
- VIX M2 futures (second month) — for term structure slope
- Short-term realized volatility (rolling window, specific period NOT REPORTED)
- VIX-linked ETNs: VXX, UVXY, SVXY (pre-Feb 2018) / SVIX (post-2021), or equivalent

---

## Transaction Costs & Capacity

**Transaction costs:**
- The paper states "realistic costs" are included; specific amounts (bid-ask spread, ETN creation/
  redemption fees, borrow cost for shorts) NOT RETRIEVED from accessible summaries.
- VXX/UVXY bid-ask spread in normal conditions: ~$0.01–0.05 per share (tight); during high-vol
  episodes (exactly when the filter exits), spreads widen substantially — this cost is hard to model.
- Daily rebalancing with dynamic sizing means potentially many partial position adjustments.
- ETN tracking error: VXX/UVXY lose ~4–9% monthly in persistent contango from roll costs —
  this is the SOURCE of the strategy's premium, not an additional cost.
- **Capacity:** Limited — SVXY/SVIX daily volume is ~$300–800M depending on vol regime; UVXY
  ~$100–400M. Institutional-scale is not feasible; retail scale (under $10M) is fine.
- **ASSUMPTION:** "Realistic costs" in the paper likely includes bid-ask for ETNs; it is unclear
  whether ETN tracking errors (beyond pure roll costs) are fully modeled.

---

## Backtest Evidence

- **Period:** 2008–2025 (17 years)
- **Pre-2009 data note:** VXX launched January 2009, UVXY October 2011. The 2008 portion of the
  backtest must use synthetic VIX futures or proxies — this introduces approximation error.
- **Regime coverage:** GFC 2008–2009, post-GFC calm 2010–2017, Volmageddon February 2018,
  COVID March 2020, inflation/rate-tightening 2022, normal regime 2023–2025.
- **Strategy variant count:** 4 variants tested — minor multiple-testing inflation.
- **Status:** CLAIMED, UNVERIFIED — Swiss Finance Institute working paper; not yet peer-reviewed
  in a journal; no independent replication found.

---

## Forward-Test Evidence

NOT REPORTED — no live trading or prospective forward test referenced.

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | 1.0 (final strategy, cost-adjusted) | CLAIMED, UNVERIFIED | SSRN 5316487 |
| Sortino Ratio | NOT REPORTED | | |
| Calmar Ratio | NOT REPORTED | | |
| Profit Factor | NOT REPORTED | | |
| Win Rate | NOT REPORTED | | |
| Expectancy per Trade | NOT REPORTED | | |
| Maximum Drawdown | NOT REPORTED | | |
| CAGR | 16.3%/yr (final strategy, cost-adjusted) | CLAIMED, UNVERIFIED | SSRN 5316487 |
| Annual Return | NOT REPORTED | | |
| Number of Trades | NOT REPORTED | | |
| Average Trade Duration | NOT REPORTED | | |
| Recovery Factor | NOT REPORTED | | |
| Risk-Reward Ratio | NOT REPORTED | | |
| Exposure Percentage | NOT REPORTED | | |
| Equity-market correlation | ~15% | CLAIMED, UNVERIFIED | SSRN 5316487 |

**No automatic scrutiny triggers fired:**
- Sharpe 1.0 < 3 threshold — within normal range for a risk-premium strategy.
- CAGR 16.3% < 50% threshold.

**Sanity check (ANALYSIS):** Sharpe 1.0 with CAGR 16.3% → implied annual vol ≈ 16.3%. This
is higher than typical equity vol (~15-16%) but consistent with a leveraged/dynamically-sized
VIX ETN strategy. If max drawdown ≈ 20-30% (typical for Sharpe-1 strategies with this vol),
the strategy is plausible on its face. However, without the actual max drawdown figure, the
risk profile cannot be fully assessed.

---

## Community Sentiment

- The broader VIX term structure strategy is well-studied in the quantitative community:
  Quantpedia lists "Exploiting Term Structure of VIX Futures" as a standalone strategy with
  documented academic support.
- Short-volatility strategies are a known and crowded strategy class, with significant academic
  and practitioner interest post-Volmageddon.
- This specific paper (SSRN 5316487) has no independent forum discussion found as of June 2026.
- Related academic work: Augustin, Cheng, Van den Bergen (SSRN 3819342) — "Volmageddon and the
  Failure of Short Volatility Products" — documents the systemic risks of this strategy class.
- Authors have established research credibility: Zarattini/Aziz previously co-authored the SPY
  intraday momentum paper (SSRN 4824172, score 50 in this database) and the VWAP day-trading
  paper (SSRN 4631351); Antonio Mele is a finance professor.
- **Community sentiment: cautious on short-vol strategies post-Volmageddon; no specific scrutiny
  of this paper found.**

---

## Why This Might Be Nothing

### 1. February 2018 (Volmageddon) — Critical Unknown

On February 5–6, 2018, the VIX more than doubled intraday (from ~16 to ~38). XIV (Credit Suisse
inverse VIX ETN) was redeemed at near-zero; early SVXY lost ~93% in two trading sessions. Any
short-vol strategy that claims to survive 2008–2025 MUST have handled this event. The paper's
dual filter (VRP + term structure) theoretically exits before crises, but:
- The February 2018 spike was primarily intraday and accelerated by ETN rebalancing mechanics —
  a DAILY realized-vol filter would not have triggered in time to exit before the damage
- The strategy's maximum drawdown during this event is NOT REPORTED, making independent
  verification impossible
- If the strategy avoided severe loss in February 2018, it's unclear whether the filter protected
  it or whether the backtest uses synthetic/adjusted data that smooths the event

This is the single most important missing piece of evidence for this strategy class.

### 2. SVXY Structural Break (February 2018)

Post-Volmageddon, ProShares changed SVXY from -1x to -0.5x leverage. Credit Suisse terminated XIV.
A 2008–2025 backtest spanning this structural break implicitly assumes: either (a) the paper uses
the new -0.5x SVXY for the full backtest (understating pre-2018 returns), OR (b) uses the pre-2018
-1x exposure (which is no longer available as a live instrument). The specific instrument choice
is NOT RETRIEVED. This structural break makes pre/post-2018 results non-comparable.

### 3. VRP Strategy Crowding and Decay Post-2017

Pre-2018, short-vol strategies attracted significant retail and institutional capital, compressing
the available premium. The VRP in equity options has been documented to vary cyclically. Whether
the post-2018 VRP (smaller SVXY leverage + diminished retail short-vol participation) delivers
the same historical premium is unknown.

### 4. Multiple Metrics NOT REPORTED

Max drawdown, win rate, profit factor, and number of trades are all NOT REPORTED in accessible
summaries. For a short-vol strategy, the drawdown during tail events IS the entire risk story.
Reporting only Sharpe and CAGR while omitting max drawdown creates an incomplete and potentially
misleading picture — specifically when the strategy is susceptible to catastrophic tail losses.

### 5. What Would Change My Mind

Access to the full paper with: (a) maximum drawdown statistics (specifically Feb 2018 and Mar
2020 drawdowns), (b) the specific instrument used pre- and post-Volmageddon, and (c) the exact
lookback windows and thresholds for the VRP and term structure signals. If max drawdown is below
30% and the February 2018 handling is transparent, confidence would rise substantially.
Independent peer review would lift the evidence auditability score.

**Verdict:** The strategy class is economically sound (VRP is real) and the paper adds a
theoretically defensible dual-signal filter. The Sharpe 1.0 and CAGR 16.3% are credible values.
BUT: the absence of max drawdown data during Volmageddon and the unresolved SVXY structural break
make this CLAIMED, UNVERIFIED and incomplete. Not ready for implementation without the full paper.

---

## Strengths / Weaknesses

**Strengths:**
- Strong economic rationale: VRP is a documented and compensation-based risk premium
- Dual-signal exit rules provide specific, falsifiable failure conditions
- 17-year backtesting period including multiple stress regimes
- Credible performance numbers: Sharpe 1.0 and CAGR 16.3% are not extreme
- Low equity correlation (~15%) — genuine diversification value
- Institutional authorship (Swiss Finance Institute working paper)
- Automation feasibility explicitly addressed

**Weaknesses:**
- Maximum drawdown NOT REPORTED — critical gap for a short-vol strategy
- Volmageddon (February 2018) handling unclear; intraday daily filter may have been insufficient
- SVXY structural break (leverage change post-Feb 2018) makes pre/post-2018 data non-comparable
- Pre-2009 VXX/UVXY data is synthetic — 2008 GFC performance is backtested on proxies
- No independent replication; no peer-reviewed journal publication
- Specific rule parameters (thresholds, lookback periods, sizing formula) NOT RETRIEVED
- VIX ETNs are leveraged, path-dependent instruments — carry significant tracking error and
  decay in sustained high-vol environments

---

## Confidence Score

**Per-dimension scoring:**

| Dimension | Weight | Score | Weighted |
|-----------|--------|-------|---------|
| Logical/economic soundness (falsifiable) | 3 | 7/10 | 21.0 |
| Transparency & reproducibility | 2 | 5/10 | 10.0 |
| Evidence auditability | 2 | 4/10 | 8.0 |
| Risk management quality | 2 | 5/10 | 10.0 |
| Cost & capacity realism | 2 | 5/10 | 10.0 |
| Honest treatment of drawdowns/failure | 1.5 | 2/10 | 3.0 |
| Robustness evidence (OOS/walk-forward) | 1.5 | 5/10 | 7.5 |
| Survived independent scrutiny | 1 | 3/10 | 3.0 |
| **TOTAL** | **15** | | **72.5** |

`latent_score = round(72.5 / 150 × 100) = 48`

**Verification cap:** Evidence auditability = 4 (≤4) → cap at 59.
`confidence = min(48, 59) = 48`

**Final: latent 48 (capped to 48; cap irrelevant: latent < 59; institutional working paper with credible Sharpe, but critical max-drawdown data missing and Volmageddon handling unverified)**

**Band:** EXPERIMENTAL (40–59)

**Note:** The economic rationale is strong enough to warrant a second look if the full paper
(with max drawdown statistics) becomes accessible. The key gate is: does the strategy survive
February 2018 with a drawdown below 30%? If yes, confidence rises to ~58–62 pending peer review.

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Moderate — two signal calculations (VRP + term structure slope) plus dynamic sizing
- **Scalability:** Retail-scale only; SVXY/SVIX daily volume constrains position sizes to ~$5–20M
- **Automation:** Explicitly described as automatable via standard broker API; daily rebalancing is
  straightforward to implement in Python with CBOE VIX data + broker position management

---

## MQL5 / Python / Pine Feasibility

- **Python:** High feasibility — VIX data from CBOE (free), VIX futures from CME Group or data
  vendors; ETN data from Yahoo Finance; signals computable with pandas/numpy
- **MQL5:** Low — VIX data not natively available in MT4/5; external data feed required
- **Pine Script:** Not feasible for live trading (no broker integration); VIX data available
  for signal visualization only

---

## Similar Strategies

- `vix-cmf-ml-walkforward` (score 45) — same underlying (VIX futures) but daily ML prediction
  of VIX CMF returns; different signal generation; compare: that paper uses ML on term structure
  features, this uses rule-based VRP + slope filter
- `hmm-rl-regime-taa` (score 44) — VIX-based regime switching for SPY/TLT/GLD rotation; different
  approach but overlapping regime-detection logic

---

## Red Flags

- **Maximum drawdown NOT REPORTED:** For a short-vol strategy this is the critical missing metric;
  the strategy's Sharpe-1 profile is incomplete without it
- **Volmageddon 2018 handling unclear:** Daily realized-vol filter inadequate for intraday spikes
- **SVXY structural break:** -1x → -0.5x leverage change in Feb 2018 makes pre/post data non-comparable
- **Pre-2009 synthetic data:** VXX didn't exist in 2008; GFC performance uses proxies
- **VIX ETN liquidity during stress:** Bid-ask spreads widen substantially when exits are most needed
- **Strategy class is crowded:** Short-vol VIX term structure is a well-known retail strategy;
  crowding risk before the next Volmageddon-type event is real

---

## Source Links

| URL | Retrieval Date | Notes |
|-----|---------------|-------|
| [SSRN 5316487](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5316487) | 2026-06-01 | Primary paper (HTTP 403) |
| [ideas.repec.org rp2591](https://ideas.repec.org/p/chf/rpseri/rp2591.html) | 2026-06-01 | HTTP 403 |
| [Concretum Group blog](https://concretumgroup.com/the-volatility-edge-a-dual-approach-for-vix-etns-trading/) | 2026-06-01 | HTTP 403 |
| [Augustin/Cheng SSRN 3819342 — Volmageddon](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3819342) | 2026-06-01 | Context: Volmageddon failure modes |
| [Quantpedia: Exploiting VIX Term Structure](https://quantpedia.com/strategies/exploiting-term-structure-of-vix-futures) | 2026-06-01 | Strategy class context |

---

## Analyst Notes

**FACTS (from sources):**
- Paper: "The Volatility Edge: A Dual Approach For VIX ETNs Trading" — Zarattini, Aziz, Mele;
  SSRN 5316487; Swiss Finance Institute Research Paper No. 25-91; June 23, 2025
- Four strategies tested from constant short-vol to dual-signal dynamic sizing
- Final strategy: CAGR 16.3%/yr, Sharpe 1.0, equity correlation ~15% (2008–2025, after costs)
- Dual signals: VRP (implied vs. realized vol) + VIX term structure slope
- Realized-vol filter: exit when realized vol > VIX
- Authors: Zarattini (Concretum Group / Swiss Finance Institute), Aziz (academic researcher),
  Mele (finance professor; identified separately in prior research)
- Automation via broker API stated as feasible

**ANALYSIS:**
- The VRP strategy is economically well-grounded but the key unknown is max drawdown in crisis
  regimes. Sharpe 1.0 is credible for a risk-premium strategy; CAGR 16.3% implies moderate
  leverage or dynamic sizing with good risk control.
- The SVXY leverage change (post-Volmageddon) is a structural break that limits comparability
  across the full 2008–2025 window. Pre-2018 performance with -1x SVXY is substantially different
  from post-2018 performance with -0.5x SVXY.
- Zarattini/Aziz's track record: their SPY intraday momentum paper (score 50 in this database)
  showed credible methodology with honest reporting. This is a positive prior on the VIX ETN
  paper.

**ASSUMPTIONS (flagged):**
- ASSUMPTION: The 2008 GFC portion uses synthetic VIX futures (not actual VXX). If the synthetic
  reconstruction dramatically underestimates the crisis spike, the 2008 performance is overstated.
- ASSUMPTION: "Realistic costs" include ETN tracking error and bid-ask during stress; this is
  probable but not confirmed.

---

## Future Research Needed

1. **Access the full paper:** Obtain SSRN 5316487 to retrieve: max drawdown (especially Feb 2018
   and Mar 2020), specific signal parameters (lookback windows, thresholds, sizing formula),
   and pre-2009 data construction methodology.
2. **Independent replication:** Replicate with CBOE VIX data + VXX/SVXY price data; test whether
   the dual filter would have protected against Volmageddon.
3. **Journal peer review:** This working paper's institutional pedigree (Swiss Finance Institute)
   makes it a reasonable candidate for journal submission; watch for peer-reviewed publication.
4. **Post-2025 forward test:** Test the signal on 2025–2026 live data.
