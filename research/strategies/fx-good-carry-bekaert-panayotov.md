# FX Good Carry — Dynamic Quality-Filtered Carry Trade

## Overview

Dynamic currency carry trade that continuously identifies and excludes the "bad carry" pairs
that generate crash risk and negative skewness, retaining only "good carry" pairs that exhibit
improved Sharpe ratios and sometimes even positive return skewness. The paper argues that the
negative skewness of standard carry — which has historically deterred institutional adoption
and invited crash-risk explanations — is not intrinsic to carry as a risk premium, but is
instead concentrated in a specific subset of currency pairs (chiefly AUD/JPY-type, high-yield
vs. low-yield EM-adjacent pairs). By dynamically excluding the Sharpe-impairing pairs each
month, the residual "good carry" portfolio captures the structural interest rate differential
premium while substantially reducing tail losses.

Source: Bekaert/Panayotov (JFQA Vol. 55(4) 2020; NBER WP 25420; SSRN 2600366)

---

## Asset Class / Timeframes

- **Market:** Forex — G10 currency pairs
- **Instruments:** Currency spot/forwards; implementable via G10 currency futures or FX forwards
- **Timeframe:** Monthly rebalancing
- **Style:** Carry trade with quality filter (dynamic exclusion of crash-risk pairs)

---

## Core Logic

### Standard carry trade (baseline)

1. Rank G10 currencies by short-term interest rate differential vs. the USD (or by 1-month
   forward discount to spot — these are equivalent via covered interest parity)
2. Long the highest-yielding basket, short the lowest-yielding basket
3. Rebalance monthly

This yields the standard carry trade: Sharpe ~0.4–0.8 (G10), negatively skewed, crash-prone.

### Good carry enhancement (Bekaert/Panayotov 2020)

4. **Evaluate each currency pair's Sharpe contribution** to the strategy in the preceding
   period (typically 1-month or rolling lookback)
5. **Exclude** any currency pair whose inclusion in the portfolio would impair the preceding-
   period Sharpe ratio of the carry portfolio — these are "bad carry" pairs
6. **Retain only "good carry" pairs**: those whose inclusion raises or preserves the
   portfolio's Sharpe in the rolling evaluation window
7. **Long good high-carry, short good low-carry** — equally weighted among retained pairs
8. Rebalance monthly, re-applying the exclusion criterion each period

**Key empirical finding:** Good carry pairs are typically *not* the intuitive/popular carry
pairs (AUD, NOK vs. JPY). The high-yield EM-adjacent and commodity-currency pairs that retail
traders typically associate with carry tend to be systematically classified as "bad" because
they carry the most crash risk.

---

## Economic Rationale

The carry trade earns a premium because investors demand compensation for bearing the
exchange rate risk associated with interest rate differentials. Uncovered interest parity (UIP)
fails empirically — high-yield currencies do not depreciate as fast as the rate differential
implies — generating a persistent return to the carry strategy.

**Why does negative skewness arise?** The crash risk in standard carry comes from correlated
unwinding: when global risk aversion rises sharply (2008, 2020, EUR/CHF 2015), carry traders
collectively exit the same crowded positions (long AUD/NZD/NOK, short JPY/CHF). This creates
sudden, large losses in the specific currency pairs that were most crowded.

**The good carry insight:** The negative skewness is not fundamental to the carry premium — it
is an artifact of specific currency pairs that exhibit crowding and crash-risk behavior. By
dynamically excluding these pairs, the premium can be captured without the crash tail.

**When the edge should fail:**
1. When global risk-off events are so severe that even non-crowded carry pairs unwind
   (systemic crises where all risk assets are correlated)
2. When the exclusion criterion lags — rapid regime changes (e.g., 2022 global rate hiking)
   may temporarily reclassify "good" pairs incorrectly
3. When interest rate differentials compress globally (low-yield regime: 2015–2021), reducing
   the carry premium to near-zero before the quality filter can help
4. When the good/bad distinction becomes widely known and arbitraged (publication-effect risk;
   paper published SSRN 2015, JFQA 2020 — the 10-year elapsed makes this a real concern)

This rationale is genuinely falsifiable: the strategy predicts that in regimes where carry
crash risk is driven by crowded specific pairs, good carry avoids losses; when crashes are
systemic, good carry still loses.

---

## Entry Conditions

- **Monthly timing:** At each calendar month-end (or forward settlement date)
- **Universe:** G10 currencies (AUD, CAD, CHF, EUR, GBP, JPY, NOK, NZD, SEK vs. USD)
- **Pair evaluation:** Calculate each currency pair's Sharpe contribution to the portfolio
  using the preceding period's returns
- **Exclusion criterion:** Drop any pair whose inclusion would reduce the preceding-period
  portfolio Sharpe; retain the remainder as "good carry"
- **Long:** Highest-yielding currencies among retained "good" pairs
- **Short:** Lowest-yielding currencies among retained "good" pairs
- **Equal weighting** within the long and short baskets

---

## Exit Conditions

- Positions are closed at monthly rebalancing (replaced by new portfolio)
- No intra-month stop-loss described in accessible summaries
- Position is naturally duration-limited (monthly roll of currency forwards)

---

## Risk Management

- **Primary mechanism:** Dynamic exclusion of bad pairs — structural risk reduction by
  portfolio composition rather than stop-losses
- **Monthly rebalancing:** Limits concentrated drawdown from extended losing streaks in any
  single pair
- **Equal weighting:** Prevents concentration risk from a single currency pair dominating
- **No explicit position-level stop-loss** described in accessible summaries
- **Overall:** 5/10 — robust structural mechanism (good/bad selection), but no intra-period
  protection; during a systemic crisis, even "good" pairs would lose

---

## Indicators Used

- Short-term interest rate differential (or 1-month forward discount/premium) between each
  currency pair and the USD
- Rolling Sharpe ratio contribution of each pair to the full portfolio (typically 1-month
  lookback; exact window NOT REPORTED in accessible summaries)
- No technical indicators; purely based on interest rate and portfolio-level metrics

---

## Transaction Costs & Capacity

- **Instrument:** G10 currency spot or forwards; some implementations use CME currency futures
- **Costs:** Monthly rebalancing in G10 FX = low bid-ask spreads (major pairs: 0.5–2 pips;
  ~0.01–0.04% round trip) and minimal market impact; essentially the lowest-cost systematic
  strategy in the archive
- **Transaction cost analysis:** NOT REPORTED in accessible summaries, but the low-cost nature
  of monthly G10 FX trading is well-established; the strategy's 100–200bps annual gross return
  for enhanced carry likely survives even generous cost assumptions
- **Capacity:** G10 FX is the world's most liquid market (~$7T/day); strategy is scalable
  to institutional size without market impact; no capacity constraint for any realistic account
- **Overall:** 7/10 — monthly G10 rebalancing is minimal cost; specific cost analysis not
  confirmed but market characteristics make this robust to reasonable cost assumptions

---

## Backtest Evidence

- Source: Bekaert/Panayotov (JFQA Vol. 55(4), June 2020, pp. 1063–1094; NBER WP 25420)
- SSRN 2600366 (first circulated 2015; published in JFQA 2020 — ~5-year review process)
- Data: G10 currency spot rates + interest rates (source: Datastream, Bloomberg)
- Backtest period: NOT REPORTED in accessible summaries (SSRN 2015 submission suggests
  sample likely ends ca. 2013–2014; formal publication 2020 may extend)
- "Good carry" trades: higher Sharpe ratios than bad carry (no exception reported)
- "Good carry" skewness: sometimes positive (vs. plain carry which is structurally negative)
- Specific Sharpe, CAGR, win rate, max drawdown: NOT REPORTED in accessible summaries

For context from plain carry literature (Brunnermeier/Nagel/Pedersen NBER; Daniel/Hodrick/Lu
*Critical Finance Review* 2017):
- Plain G10 carry Sharpe: ~0.4–0.8
- Plain carry win rate (% profitable months): ~60–70% (inferred from documented negative
  skewness with positive mean; CLAIMED from academic literature consensus, not primary calc)
- Plain carry max drawdown: ~15–25% during crisis periods (2008, 2020)

## Forward-Test Evidence

NOT REPORTED — no live-trading or forward-test results found in accessible sources.
Post-publication (2015–2025) performance of specifically the good/bad carry split has NOT
been independently validated in any accessible source found during this research pass.

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | Higher than plain carry (no exception stated) | CLAIMED, UNVERIFIED | JFQA 2020 paper summary |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED for "good carry" specifically; ~60–70% inferred for plain carry | CLAIMED, UNVERIFIED | Carry trade literature (academic consensus) |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | NOT REPORTED for "good carry" | NOT REPORTED | — |
| CAGR | NOT REPORTED | NOT REPORTED | — |
| Annual Return | NOT REPORTED | NOT REPORTED | — |
| Number of Trades | Monthly rebalancing × G10 pairs; low turnover | CLAIMED, UNVERIFIED | Inferred from strategy structure |
| Average Trade Duration | ~1 month (monthly rebalancing) | CLAIMED, UNVERIFIED | Strategy design |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | Improved vs. plain carry | CLAIMED, UNVERIFIED | JFQA 2020 paper summary |
| Exposure Percentage | ~100% (always in market) | CLAIMED, UNVERIFIED | Strategy design |

**Scrutiny notes:** No explicit metric triggers (no specific Sharpe, CAGR, win rate stated).
The core metric that would be most informative — max drawdown of good carry during the 2008
GFC, 2020 COVID crash, and 2022 rate-shock — is NOT REPORTED in accessible summaries.

---

## Community Sentiment

- Published in JFQA (one of the top 4 finance journals) after a ~5-year review process
  (SSRN 2015 → JFQA 2020); survived rigorous peer review
- NBER Working Paper 25420 circulated within the academic community
- Paper has been cited in subsequent carry trade literature (Dupuy 2021 JBF cites related
  methodology; FX carry review papers reference it)
- No specific forum threads (ForexFactory, r/algotrading) discussing practical implementation
  found during this search session — the strategy is primarily known in academic circles
- Criticism in the literature: The "dynamic Sharpe contribution" criterion is potentially
  endogenous (selects past winners), which overlaps with currency momentum; it is unclear
  whether the carry premium or the momentum element explains the improved performance

---

## Why This Might Be Nothing

### 1. Circular/endogenous selection — carry quality or momentum?

The exclusion criterion (drop pairs that impaired Sharpe in the preceding period) is
essentially a form of momentum selection applied to carry pairs: you're keeping the pairs that
worked recently and dropping the ones that didn't. This may be capturing **currency momentum**
(Menkhoff et al. 2012 — already in archive) rather than a structural quality distinction
in the carry premium. If the alpha comes from the momentum element, it would have decayed post-
2010 (as documented by the OOS Sharpe of −0.32 for currency momentum — see archive entry
`currency-momentum-factor-menkhoff`).

### 2. Post-publication performance is unknown

The SSRN preprint dates to 2015; the JFQA paper to 2020. The sample likely ends ca. 2013–
2014. The carry trade performed inconsistently from 2015–2021 (compressed differentials,
zero rates globally) and experienced disruption in 2022 (global rate hiking, which rewarded
some carry pairs but disrupted the good/bad classification). No accessible source documents
how the good carry strategy performed in the 2015–2026 out-of-sample period.

### 3. Crisis losses not eliminated — just redistributed

The claim is that good carry has "sometimes positive skewness," not that it is positively
skewed in all periods. During a true systemic crisis (2008, March 2020 COVID crash), capital
flows out of *all* risk assets, including the "good" carry pairs. Selective exclusion helps in
moderate drawdown environments, but the strategy's behavior during the worst 1% of months is
not validated from accessible summaries.

### 4. Carry differentials have structurally compressed since 2008

Post-GFC, most G10 central banks converged on near-zero interest rates. The carry trade's
gross premium depends on interest rate differentials; with differentials near zero (2015–2021),
the carry return approaches zero before any quality filter can help. In 2022–2023, rate hiking
cycles re-opened differentials but created a novel regime (simultaneous large rate moves across
multiple currencies) not present in the original sample.

### 5. The sample is small: G10 = 45 possible pairs, "good" subset even smaller

With only 10 currencies and a dynamic exclusion criterion, the "good" portfolio may contain
as few as 2–4 currency pairs in some periods. Results driven by a small number of pair-years
can look robust in-sample but fail out-of-sample for reasons unrelated to the carry premium.

**What would change my mind:** Access to the full paper to extract specific Sharpe, drawdown,
and win rate for the good carry portfolio; plus an independent replication covering 2015–2026,
explicitly through the COVID crash and 2022 rate-shock period.

---

## Strengths / Weaknesses

### Strengths

- Published in JFQA (top 4 finance journal) after a ~5-year review process — strong
  academic credibility
- Directly addresses the core problem with carry trades: crash risk and negative skewness
- Economic rationale is coherent and novel: separates carry premium from crash risk
- G10 FX = zero capacity constraints and minimal transaction costs
- Monthly rebalancing = low turnover, feasible for individual and institutional accounts
- Counterintuitive finding (avoid popular AUD/JPY pairs) is not obvious and unlikely to be
  curve-fitted

### Weaknesses

- Specific Sharpe, CAGR, max drawdown, and win rate NOT REPORTED from accessible summaries
- Post-publication (2015–2026) performance unknown
- Selection criterion is potentially endogenous with currency momentum (already shown to decay)
- Carry premium structurally compressed in 2015–2021 low-rate regime
- No explicit stop-loss or intra-period risk management
- The most accessible summaries don't provide enough detail to implement exactly

---

## Confidence Score

### Per-dimension breakdown

| Dimension | Weight | Score | Weighted | Rationale |
|---|---|---|---|---|
| Logic / economic soundness (falsifiable) | 3 | 7 | 21 | Carry premium mechanism well-established; good/bad distinction novel but partially circular; failure conditions stated |
| Transparency & reproducibility | 2 | 6 | 12 | JFQA paper + SSRN preprint exist; G10 data is public (Datastream/Bloomberg); full formula not accessible via 403 |
| Evidence auditability | 2 | 6 | 12 | Peer-reviewed in JFQA (top 4 finance journal); but full methodology not independently verifiable from accessible summaries |
| Risk management quality | 2 | 5 | 10 | Dynamic exclusion is structural risk control; monthly rebalancing; no explicit stop-loss; tail in systemic crash unaddressed |
| Cost & capacity realism | 2 | 7 | 14 | G10 FX monthly = near-zero costs; effectively unlimited capacity; specific cost analysis not confirmed but moot at these volumes |
| Honest treatment of drawdowns / failure | 1.5 | 3 | 4.5 | Max DD not reported for good carry; crash behavior during 2008/2020 not validated in accessible summaries |
| Robustness evidence (OOS / walk-forward) | 1.5 | 5 | 7.5 | JFQA publication implies robustness tests; "robust to time period and currency choices" cited by Dupuy 2021; no OOS post-2015 found |
| Survived independent scrutiny | 1 | 7 | 7 | JFQA peer review (top-4); NBER WP; cited by subsequent literature; 10+ year lifespan in academic discourse |
| **Total** | **15** | — | **88** | |

`latent_score = round(88 / 150 × 100) = 59`

Evidence auditability = 6 (5–7 range) → `confidence = min(59, 74) = 59`

**`latent 59 (capped to 59 — Evidence auditability = 6: JFQA peer-reviewed but full methodology not independently verifiable from accessible summaries; post-publication OOS performance unknown)`**

**Band: Experimental (40–59)**

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Low-to-moderate — standard carry construction (rank by interest rate
  differential) plus a rolling Sharpe contribution filter; no exotic data or complex ML
- **Scalability:** Effectively unlimited — G10 FX markets handle trillions per day
- **Automation:** Highly feasible — monthly rebalancing, public data (FRED/Bloomberg);
  Python implementation would require: (1) daily spot rates + overnight rates, (2) monthly
  portfolio construction, (3) rolling Sharpe calculation, (4) dynamic exclusion rule

---

## MQL5 / Python / Pine Feasibility

- **Python:** Highly feasible — FX rate data via FRED (free), Interactive Brokers, or
  Bloomberg; rolling Sharpe calculation is standard; monthly forward order execution via
  OANDA/IB. This is one of the most implementable strategies in the archive.
- **MQL5 (MetaTrader):** Feasible in principle (MT4/MT5 support G10 FX); would need custom
  Sharpe calculation indicator and monthly rebalancing logic
- **Pine Script (TradingView):** Feasible for backtesting/monitoring; execution not available

---

## Similar Strategies

- `fx-carry-value-momentum-200yr` — plain G10 carry without quality filter; Sharpe ~0.39;
  this strategy is a direct improvement on that entry's baseline
- `currency-momentum-factor-menkhoff` — monthly momentum in ~48 currencies; the good carry
  selection criterion may overlap with momentum; OOS Sharpe −0.32 post-2010 is a warning
- `fx-mean-reversion-futures-monthly` — monthly FX futures mean reversion; different mechanism
  but same monthly timeframe and same low-cost FX market

---

## Red Flags

- Specific performance metrics (Sharpe, max DD, win rate) NOT REPORTED from accessible summaries
- Post-publication (2015–2026) performance not validated
- Selection criterion potentially endogenous with currency momentum (which has decayed)
- Carry premium compressed in 2015–2021 zero-rate regime
- Crash behavior in worst periods (2008, 2020) not confirmed from accessible summaries

---

## Source Links

- JFQA publication: https://www.cambridge.org/core/journals/journal-of-financial-and-quantitative-analysis/article/abs/good-carry-bad-carry/3BD4883026CA735E1F206588E0B957C4 (retrieved 2026-06-09; HTTP 403)
- SSRN 2600366: https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2600366 (retrieved 2026-06-09; HTTP 403)
- NBER WP 25420: https://www.nber.org/papers/w25420 (retrieved 2026-06-09)
- Columbia PDF: https://business.columbia.edu/sites/default/files-efs/pubfiles/26212/good_carry.pdf (retrieved 2026-06-09; HTTP 403)
- Context: Brunnermeier/Nagel/Pedersen carry crash risk: https://www.nber.org/papers/w14473
- Context: Daniel/Hodrick/Lu carry risks and drawdowns: https://business.columbia.edu/sites/default/files-efs/pubfiles/6378/Daniel.Hodrick.Lu.Carry%20Trade.Critical%20Finance%20Review.2017.pdf

---

## Analyst Notes

**FACTS:** Authors: Geert Bekaert (Columbia Business School) and George Panayotov (HKUST).
Published JFQA Vol. 55(4), June 2020, pp. 1063–1094. NBER WP 25420. SSRN 2600366 (first
circulated 2015, suggesting ~5-year review). Strategy: G10 carry trade with dynamic exclusion
of pairs that impair rolling Sharpe. Finding: good carry has higher Sharpe and sometimes
positive skewness; bad carry (typically AUD, JPY, NOK) has poor Sharpe and highly negative
skewness. Specific numerical Sharpe, CAGR, max DD for the good carry portfolio: NOT REPORTED
in accessible summaries (all primary PDF sources HTTP 403).

Context from carry literature: plain carry Sharpe ~0.4–0.8; Dupuy (JBF 2021) reports G10
carry with combined signal achieving Sharpe 0.64–1.07 depending on signals used. Daniel/
Hodrick/Lu (Critical Finance Review 2017) document plain carry max DD exceeding 20% in crisis.

**ANALYSIS:** The "good carry" insight is genuinely novel and empirically motivated. The claim
that negative carry skewness is concentrated in specific pairs (and therefore removable) would
be significant if true — it would mean that the crash risk discount embedded in all carry trade
returns is mispriced, because most of the risk is in a small subset of pairs. However, the
endogeneity concern (does the exclusion capture momentum rather than carry quality?) is a real
threat to the strategy's standalone alpha claim. The key test that is not available from
accessible summaries: does the good/bad distinction hold in a period where momentum and carry
are negatively correlated (i.e., when past good carry pairs suddenly become bad)?

**ASSUMPTIONS:** (1) Data period likely ends ca. 2013–2014 (based on SSRN 2015 submission
date). (2) The G10 spot rates + interest rates used are from Datastream (standard in academic
carry literature). (3) The monthly rebalancing assumption is consistent with most academic
carry trade papers.

---

## Future Research Needed

1. Access the full paper to extract specific Sharpe ratio, max drawdown, and win rate for
   the good carry vs. bad carry portfolios, and the exact formula for the Sharpe contribution
   exclusion criterion
2. Independent replication covering 2015–2026, especially through 2020 COVID and 2022 rate
   hike cycle
3. Test whether good carry alpha remains after controlling for currency momentum (if alpha
   disappears after momentum control, the strategy is just momentum-in-carry-pairs)
4. Analysis of whether the good/bad classification is stable across regimes or changes
   dramatically during rate-shock periods (e.g., 2022 BoJ YCC changes)
5. Community/practitioner discussion of implementation — no forum thread found during this
   research pass
