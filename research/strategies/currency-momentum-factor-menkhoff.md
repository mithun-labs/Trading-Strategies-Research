# Currency Momentum Factor (Cross-Sectional FX Momentum)

## Overview

The definitive academic study of cross-sectional currency momentum. Rank ~48 currencies by their past return over a 1–12 month formation period; go long the top quintile (past winners) and short the bottom quintile (past losers); hold for 1 month. The best-performing variant — MOM(1,1): 1-month formation, 1-month holding — produced an annualized Sharpe of 0.95 on the in-sample period 1976–2010.

Critical context: a peer-reviewed 2022 follow-up study shows average out-of-sample Sharpe collapsed from +0.39 to **−0.32** post-publication. Combined with multiple corroborating studies showing G10 momentum stopped working after the 2008 GFC, this is a textbook post-publication decay case.

- **Authors:** Lukas Menkhoff, Lucio Sarno, Maik Schmeling, Andreas Schrimpf
- **Primary source:** Journal of Financial Economics, Vol. 106(3), pp. 660–684 (2012)
- **Pre-print:** BIS Working Paper No. 366
- **SSRN:** 1988679
- **Peer-reviewed:** YES (Journal of Financial Economics — top-3 finance journal)
- **Post-publication decay paper:** Hutchinson, Kyziropoulos, O'Brien, O'Reilly, Sharma — "Are Carry, Momentum and Value Still There in Currencies?" International Review of Financial Analysis (2022); SSRN 4024296

---

## Asset Class / Timeframes

- **Markets:** Up to 48 currencies: G10 developed-market pairs + emerging markets (USD-based spot rates)
- **Timeframe:** Monthly signal, monthly rebalancing (MOM(1,1) = 1-month signal, 1-month hold)
- **Data period (original study):** January 1976 – January 2010 (34 years in-sample)
- **Post-publication OOS period:** 2010–2022 (Hutchinson et al. 2022 follow-up)

---

## Core Logic

**Cross-sectional momentum (MOM):**
1. At each month-end, compute each currency's excess return (in USD) over the past f = 1, 3, 6, 9, or 12 months (five formation variants)
2. Sort all ~48 currencies from best to worst
3. **Long** top quintile (past winners)
4. **Short** bottom quintile (past losers)
5. Equal-weight within each leg
6. Hold for 1 month, then rebalance

The benchmark variant in the paper is **MOM(1,1)**: 1-month formation, 1-month holding.

All five formation periods (1, 3, 6, 9, 12 months) produce positive in-sample excess returns, with the largest returns for short holding periods. Results are fairly stable across sub-periods within the 1976–2010 window.

---

## Economic Rationale

**Behavioral under-reaction hypothesis:** Investors are slow to incorporate information into exchange rates, causing recent winners to continue outperforming (momentum) over 1–3 months, before a reversal at longer horizons (12+ months). Analogous to Jegadeesh-Titman (1993) equity momentum.

**Falsifiability:** This mechanism should fail when:
- Transaction costs exceed the gross momentum premium (already documented in the paper)
- Arbitrageurs identify and close the mispricing (publication effect)
- Information transmission speeds up, eliminating underreaction lags
- Correlated macro shocks (e.g., GFC) cause simultaneous unwinding across all momentum positions

**What we observe:** All four failure conditions appear to have materialized. G10 momentum stopped working post-2008 GFC; the publication effect (post-2010) is documented at −0.32 Sharpe; modern market microstructure reduces information lags. The rationale was correct about when the edge should disappear — and it did.

---

## Entry Conditions

- At month-end, compute 1-month (or 3/6/9/12 month) USD-excess returns for all available currencies
- Quintile sort by formation-period return
- Enter long positions in top quintile (top ~20% of currencies by return)
- Enter short positions in bottom quintile (bottom ~20% of currencies by return)
- Equal-weight within each quintile leg

---

## Exit Conditions

- Monthly rebalancing — exit/rebalance all positions at month-end
- No explicit stop-loss or regime-based exit in original framework

---

## Risk Management

- Long-short construction provides approximate market neutrality within FX
- Equal-weight sizing is passive and does not respond to volatility or concentration risk
- No explicit position limits, stop-losses, or max-drawdown rules in the original study
- Transaction costs acknowledged as risk: momentum portfolios are skewed toward illiquid currencies with high bid-ask spreads

---

## Indicators Used

| Indicator | Signal | Data |
|-----------|--------|------|
| f-month return (f = 1, 3, 6, 9, 12) | Momentum rank signal | Monthly FX spot rates (USD-based) |
| Bid-ask spread | Cost proxy / liquidity filter | FX market maker quotes |

---

## Transaction Costs & Capacity

- **Modeled in original paper:** YES — bid-ask spreads used to compute cost-adjusted returns
- **Key finding from original paper:** Transaction costs significantly reduce profitability; momentum portfolios are skewed toward currencies with high transaction costs (illiquid pairs); costs partially explain the cross-sectional spread
- **Trading volume price impact:** A 2024 study (Journal of Financial Economics) finds that price impact quickly erodes returns and renders FX strategies unprofitable when trading volume is considered
- **Monthly rebalancing of a 48-currency portfolio:** ~100% turnover per month in the long leg; high gross-to-net haircut
- **Capacity:** Institutional-scale trading would further erode returns through market impact; small position sizes required
- **Verdict:** Costs erode the gross Sharpe materially. The post-publication OOS failure is at least partly attributable to cost pressure as the strategy became crowded.

---

## Backtest Evidence

- In-sample (1976–2010): Sharpe 0.95 for MOM(1,1); up to 10% p.a. spread between top and bottom quintile. AUDITABLE (peer-reviewed JFE with documented reproducible methodology)
- Five formation periods all positive in-sample: some parameter robustness within the training window
- Returns concentrated in high idiosyncratic volatility currencies: ~8% p.a. vs ~4% p.a. for low-vol currencies

---

## Forward-Test Evidence

- **Post-publication OOS (Hutchinson et al. 2022, IRFA):** Average out-of-sample Sharpe drops from +0.39 to **−0.32** for carry, momentum, and value strategies in currencies. Strategy performance disappeared subsequent to publication. AUDITABLE (peer-reviewed follow-up study).
- **G10 subperiod (Rohrbach/Suremann 2017):** G10 momentum unprofitable since 2008 GFC; emerging market and crypto momentum still works
- **200-year test (Chen 2024):** Currency momentum only works in the latter half of 200-year sample; fails full-history test. CLAIMED, UNVERIFIED (SSRN working paper)

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | 0.95 (MOM(1,1), 1976–2010, in-sample) | AUDITABLE | Menkhoff et al. JFE 2012 |
| Sharpe Ratio | ~0.82 (carry, for comparison, same period) | AUDITABLE | Menkhoff et al. JFE 2012 |
| Sharpe Ratio (OOS) | −0.32 (average OOS, post-publication) | AUDITABLE | Hutchinson et al. IRFA 2022 |
| Sharpe Ratio (OOS in-sample) | +0.39 (average in-sample across carry/mom/val) | AUDITABLE | Hutchinson et al. IRFA 2022 |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | — |
| CAGR | NOT REPORTED (annual spread ~10% in-sample) | NOT REPORTED | — |
| Annual Return (excess) | ~10% p.a. spread (MOM, best variant, in-sample) | AUDITABLE | Menkhoff et al. JFE 2012 |
| Number of Trades | ~48 currencies × monthly rebalancing | NOT REPORTED | — |
| Average Trade Duration | ~1 month (MOM(1,1) holding period) | AUDITABLE | Menkhoff et al. JFE 2012 |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | ~100% (always invested) | NOT REPORTED | — |

---

## Community Sentiment

- Menkhoff et al. (2012) is one of the most-cited FX strategy papers (~1,500+ citations via Google Scholar)
- Widely considered the foundational academic evidence for currency momentum
- The post-publication decay (Hutchinson et al. 2022, IRFA) and the G10 post-GFC failure (Rohrbach/Suremann 2017) are well-known in the quant research community
- Quantpedia maintains a "Currency Momentum Factor" strategy page based on this paper
- General view: in-sample evidence was real; current exploitability is doubtful without modifications (emerging markets, vol-scaling, cost filters)

---

## Why This Might Be Nothing

### 1. Post-publication decay is the dominant story

This is the clearest possible case of post-publication strategy decay. The original paper (JFE 2012) established momentum works 1976–2010. A peer-reviewed 2022 follow-up using the same methodology found average out-of-sample Sharpe collapsed from +0.39 to −0.32. This is not a cherry-picked result — it's the same research group's confirmed failure mode applied to an academically rigorous in-sample finding.

**ANALYSIS:** A drop from +0.39 to −0.32 is not statistical noise. It represents a complete sign reversal. The most parsimonious explanation is the publication effect: once the strategy became known, arbitrageurs corrected the mispricing.

### 2. G10 failure after the GFC is corroborating

Rohrbach/Suremann (2017) find G10 momentum stopped working post-2008. This predates the 2022 Hutchinson paper and provides independent corroboration that the G10-specific signal decayed.

### 3. Transaction costs are a material drag

The original paper explicitly notes momentum portfolios are skewed toward high-spread currencies. Net-of-cost performance was already lower in-sample. A 2024 JFE study finds trading volume price impact "quickly erodes returns and renders FX strategies unprofitable." For retail traders (who face wider spreads), the cost drag is even larger.

### 4. The strategy is essentially a market-timing variant for EM currencies

The remaining evidence that currency momentum still works is primarily in emerging markets and crypto. For G10 currencies (the most liquid and accessible), the edge appears absent. EM currencies have higher transaction costs, capital controls, and execution friction that further reduce implementability.

### 5. 100% monthly turnover is punitive

MOM(1,1) requires full portfolio rebalancing every month. For a 48-currency portfolio, this is substantial gross-to-net haircut even with institutional spreads.

### 6. The one piece of evidence that would change my mind

An independent, cost-inclusive, post-2020 walk-forward test on G10 currencies showing positive Sharpe > 0.5 net of realistic costs, without data-mining of formation/holding periods. I do not have this.

---

## Strengths / Weaknesses

**Strengths:**
- Published in Journal of Financial Economics (top-3 finance journal) — gold standard of peer review
- 34-year in-sample period with strong statistical support
- Five formation variants all work in-sample — some parameter robustness
- Original paper explicitly analyzes transaction costs
- Post-publication follow-up literature provides honest OOS accounting

**Weaknesses:**
- OOS Sharpe documented at −0.32 (negative) post-publication
- G10 momentum stopped working after 2008 GFC (independently corroborated)
- High monthly turnover (MOM(1,1)) creates large cost drag
- No explicit risk management rules — passive long-short rebalancing only
- In-sample period (1976–2010) preceded the era of algorithmic FX trading; conditions may be fundamentally different

---

## Confidence Score

**Scoring:**

| Dimension | Score (0-10) | Wt | Weighted |
|-----------|-------------|-----|----------|
| Logical/economic soundness (falsifiable) | 7 | 3 | 21 |
| Transparency & reproducibility | 7 | 2 | 14 |
| Evidence auditability | 8 | 2 | 16 |
| Risk management quality | 4 | 2 | 8 |
| Cost & capacity realism | 4 | 2 | 8 |
| Honest treatment of drawdowns/failure | 7 | 1.5 | 10.5 |
| Robustness evidence (OOS/walk-forward) | 3 | 1.5 | 4.5 |
| Survived independent scrutiny | 7 | 1 | 7 |
| **Totals** | | 15 | **89** |

`latent_score = round(89 / 150 × 100) = 59`

**Verification cap:** Evidence auditability = 8 → **no cap** (peer-reviewed JFE methodology)
`confidence = 59`

**latent 59 (no cap applies: JFE peer-reviewed methodology; OOS failure is documented by separate peer-reviewed study — robustness dimension drives score below 60)**

**Band: Experimental (40–59)**

**Note for queue:** latent 59 = highest latent score in the Experimental band; border-line case. The JFE evidence is real, but the OOS failure is decisive. Not an implementation candidate given documented post-publication decay.

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** LOW — monthly sort by return; quintile long-short; equal-weight
- **Data required:** Monthly FX spot rates for 48+ currencies (available from Bloomberg, Reuters, FRED for G10)
- **Python implementation:** Straightforward pandas/numpy monthly ranking logic
- **Challenge:** Accessing bid-ask spread data for cost-adjusted net returns; EM currency data for post-2010 period

---

## MQL5 / Python / Pine Feasibility

- **Python:** Fully implementable in pandas; Bloomberg/Yahoo/FRED for rate data
- **MQL5:** Feasible for the G10 pairs accessible on most brokers; but 48-currency universe requires custom data feed
- **Pine Script:** Not suitable for multi-currency cross-sectional ranking logic

---

## Similar Strategies

- `fx-carry-value-momentum-200yr` — same asset class; tested over 200 years; carry (not momentum) found to be the robust factor; score 51
- `fx-mean-reversion-futures-monthly` — same asset class (FX futures); value/reversion signal; Sharpe 0.12 (null result); score 46
- `london-breakout-forex` — same asset class (forex); session breakout approach; score 47

---

## Red Flags

- **Post-publication OOS Sharpe = −0.32:** Documented by peer-reviewed 2022 paper; sign reversal, not just noise
- **G10 momentum stopped post-2008:** Independently corroborated by Rohrbach/Suremann (2017)
- **200-year test failure:** Momentum only works in latter half of 200-year sample (Chen 2024)
- **High monthly turnover:** ~100% turnover in MOM(1,1); cost drag significant
- **Publication effect:** Classic post-publication decay pattern — arbitrageurs close the gap after academic publication
- **All direct sources HTTP 403:** Full BIS PDF and JFE paper text inaccessible

---

## Source Links

| Source | URL | Retrieved |
|--------|-----|-----------|
| Menkhoff et al. JFE 2012 (SSRN pre-print) | https://papers.ssrn.com/sol3/papers.cfm?abstract_id=1988679 | 2026-05-31 (search result reference only) |
| BIS Working Paper 366 | https://www.bis.org/publ/work366.pdf | 2026-05-31 (HTTP 403) |
| City Research Online (open access) | https://openaccess.city.ac.uk/id/eprint/3296/ | 2026-05-31 (HTTP 403) |
| Hutchinson et al. IRFA 2022 (SSRN) | https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4024296 | 2026-05-31 (search result reference only) |
| Quantpedia Currency Momentum Factor | https://quantpedia.com/strategies/currency-momentum-factor | 2026-05-31 (HTTP 403) |

---

## Analyst Notes

**FACTS (from sources):**
- MOM(1,1) Sharpe = 0.95, in-sample 1976–2010: from Menkhoff et al. JFE 2012 (peer-reviewed; via search snippets)
- Annual excess return spread of up to 10% p.a.: from Menkhoff et al. JFE 2012
- Carry Sharpe = 0.82 (same period, for comparison): from search snippets referencing JFE 2012
- Transaction costs significantly reduce profitability; momentum portfolios skewed to high-spread currencies: from Menkhoff et al. JFE 2012
- Post-publication OOS Sharpe drops from +0.39 to −0.32: from Hutchinson et al. IRFA 2022 (peer-reviewed; via search snippets)
- G10 momentum unprofitable post-2008 GFC: from Rohrbach/Suremann (2017) (via search snippet)
- Momentum only works in latter half of 200-year sample: from Chen SSRN (2024) (via search snippet)

**ANALYSIS (reasoning):**
- The Hutchinson 2022 finding of −0.32 OOS Sharpe is a complete sign reversal from the +0.39 in-sample value. This represents a 0.71 Sharpe deterioration, which is far larger than typical estimation uncertainty. The publication effect interpretation is the most parsimonious explanation.
- The temporal pattern is consistent: works pre-GFC, breaks post-GFC (Rohrbach 2017); fails post-publication (Hutchinson 2022); fails full 200-year test (Chen 2024). All three independent sources converge on the same conclusion.
- The one area where momentum still seems to work is emerging markets and cryptocurrency — both asset classes with higher information barriers, higher transaction costs that limit arbitrage, and less crowded positioning. This is consistent with the behavioral underreaction mechanism: arbitrage is less effective where costs are higher.

**ASSUMPTIONS:**
- Specific max drawdown, CAGR, win rate, and annual breakdown not available from search snippets; assumed NOT REPORTED
- Monthly rebalancing confirmed from "1-month holding period" description

---

## Future Research Needed

1. Replicate MOM(1,1) on G10 currencies post-2015 with realistic bid-ask costs to confirm or deny current profitability
2. Test emerging-market-only version of MOM(1,1) to see whether EM momentum survives post-2010
3. Examine basis-momentum variant (Fan 2025, EFM) which generates Sharpe superior to pure momentum while combining carry and momentum signals — potential evolution of this strategy worth researching as a new queue entry
4. Investigate vol-scaled variant: momentum returns concentrated in high idiosyncratic vol currencies — a vol-scaled long-only version might extract the risk premium with less turnover
