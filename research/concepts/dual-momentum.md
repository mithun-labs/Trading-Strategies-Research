# Concept: Dual Momentum (Relative + Absolute)

## Description

Dual momentum combines two momentum signals:
1. **Relative momentum:** select the asset with the highest recent return among a candidate set
2. **Absolute momentum:** require the selected asset's return to be positive (vs. cash/risk-free)

Developed and popularized by Gary Antonacci (2014, "Dual Momentum Investing"). The absolute
momentum filter is the key innovation — it provides a "cash option" that exits to safety when
the entire asset universe is declining.

## Academic Foundation

- Jegadeesh & Titman (1993, JF): cross-sectional (relative) momentum in US equities
- Asness, Moskowitz & Pedersen (2013, JF): momentum everywhere across 8 asset classes
- Antonacci (2014): dual momentum combining both forms; Global Equity Momentum (GEM) model

## When the Edge Should Disappear

- When assets in the candidate set are highly correlated (relative signal adds no value)
- When momentum premium decays due to crowding
- During fast regime reversals (momentum cannot react in time)
- During prolonged low-return environments for all assets (cash penalty accumulates)

## Strategies Using This Concept

| Strategy | Asset Universe | Lookback | Notes |
|----------|---------------|----------|-------|
| `gold-bitcoin-dual-momentum` | GLD + IBIT | X weeks (1–28) | Weekly; vol cap 20% |
| `gold-cross-asset-regimes` | GLD + IEF (as filter) | 12 months | Monthly; bond-based regime |

## Known Limitations

- Whipsaw in choppy/mean-reverting markets
- Data mining risk when testing multiple lookback periods without OOS
- Short-term capital gains in taxable accounts from frequent switching
- Antonacci's original GEM strategy has underperformed broad market since ~2012 in some analyses
