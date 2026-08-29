# Tadawul Stocks — Insights Report
*Derived from `DataCleaning.ipynb` (200 companies, 35 trading days, April 2020)*

---

## 1. Dataset Snapshot

| Metric | Value |
|---|---|
| Rows | 6,992 |
| Companies (`trading_name`) | 200 |
| Trading days | 35 |
| Sectors | 11 |
| Period | ~March–April 2020 (COVID-19 crash window) |

The dataset arrived with three formatting issues — trailing whitespace in column names, a misspelled `sectoer` column, and `no_trades` stored as float instead of int — all fixed in the cleaning step. This matters because messy headers like `'trading_name '` silently break `groupby` and merge operations if left unnoticed.

---

## 2. The Missing Data Isn't Random — It's a Signal

162 rows were missing `open`, `high`, and `low` simultaneously. Looking closer:

- **4 tickers were missing for all 35 days**: `ATHEEB TELECOM`, `ALKHODARI`, `THIMAR`, `WAFA INSURANCE`
- These rows all had `change = 0` and `perc_Change = 0`

**Insight:** this isn't random noise — it's the signature of **suspended or non-trading stocks**. A stock with no open/high/low and zero change likely wasn't actively traded that day at all, rather than "missing" in the data-entry sense.

The notebook fills these with the column **mean** across the whole dataset. That's a reasonable quick fix, but worth flagging: for a stock that never traded, imputing the *average price of 200 other companies* isn't meaningful — it manufactures a price that never existed. A cleaner alternative would be a per-stock fill (e.g., last known `close`) or an explicit `is_suspended` flag, so this behavior doesn't quietly bias later analysis (like the correlation heatmap or sector averages).

---

## 3. Sector Representation Is Heavily Imbalanced

| Sector | Rows | Companies (approx.) |
|---|---|---|
| Financials | 1,645 | 47 |
| Materials | 1,470 | 42 |
| Real Estate | 980 | 28 |
| Consumer Discretionary | 840 | 24 |
| Industrials | 700 | 20 |
| Consumer Staples | 560 | 16 |
| Health Care | 273 | 8 |
| Communication Services | 210 | 6 |
| Energy | 175 | 5 |
| Utilities | 70 | 2 |
| Information Technology | 69 | ~2 |

**Insight:** Financials alone makes up ~24% of all rows, while Utilities and Information Technology are represented by just 2 companies each. Any sector-level average (like the % change chart built later) is far less statistically reliable for those thin sectors — one company's bad day can swing the entire sector's "average."

---

## 4. Correlation Heatmap — What Actually Moves Together

From the `open/high/low/close/volume_traded/value_traded/no_trades/perc_Change` heatmap:

- **Price fields are essentially one variable**: `open`, `high`, `low`, `close` correlate at 0.99–1.00 with each other — unsurprising, but confirms they shouldn't be treated as independent features in any model.
- **Price level barely relates to trading activity**: correlation between price and `volume_traded` is slightly *negative* (-0.06 to -0.07). Higher-priced stocks don't trade in higher volume — if anything, slightly the opposite, likely because fewer shares are needed to move the same amount of money.
- **Activity metrics move together**: `value_traded`, `volume_traded`, and `no_trades` are all strongly correlated (0.83–0.90) — expected, since they're all facets of the same trading activity.
- **The most important finding: `perc_Change` is almost uncorrelated with everything** (max correlation 0.03). Neither price level nor trading volume/activity explains next-day-style price direction in this dataset. This is a meaningful (if slightly disappointing) result for anyone hoping "high volume → big price move" holds here — at the daily level in this sample, it doesn't.

---

## 5. Sector Performance Tells a COVID-Crash Story

The average daily `perc_Change` by sector shows:

- **Only one sector was positive**: Information Technology (~+0.5% average daily change)
- **Every other sector was negative**, with **Industrials (~-0.52%)** and **Consumer Discretionary** hit hardest
- Financials, Health Care, and Materials were also solidly negative but less extreme

**Insight:** this lines up with the real-world timeline — April 2020 was deep in the COVID-19 market shock. Cyclical, in-person-dependent sectors (industrials, discretionary retail, real estate) were punished, while tech-related activity held up or even gained. That's a good example of how a single chart, tied to real-world context, becomes a genuine insight instead of just a plot.

---

## 6. Liquidity Is Concentrated in a Few Giants

Top 10 companies by total value traded were dominated by banks and Aramco:

**ALRAJHI, ALINMA, SAUDI ARAMCO, SABIC, SEERA, NCB, STC, SULAIMAN ALHABIB, BJAZ, BAHRI**

**Insight:** the top 2 companies (ALRAJHI, ALINMA — both banks) traded roughly as much value as the next 4–5 companies combined. This mirrors the sector-imbalance finding above: Financials isn't just the *most represented* sector by row count, it's also where most of the actual market liquidity sits. Any market-wide "average" statistic in this dataset is implicitly dominated by a handful of large financial names unless deliberately weighted otherwise.

---

## 💡 Innovation Ideas — Where This Analysis Could Go Next

1. **Replace mean-imputation with a "suspended stock" flag.** Instead of filling `open/high/low` with the dataset mean, add a boolean `is_suspended` column for tickers with no trading activity. This preserves the real story (a stock not trading) instead of inventing a synthetic price.

2. **Volatility index by sector.** Instead of just the *mean* `perc_Change` per sector, plot the *standard deviation* per sector — this identifies which sectors are riskiest/most volatile, not just which lost the most. Combine both into a risk/return scatter (sector risk vs. sector average return).

3. **Liquidity-weighted sector performance.** The current sector average treats every company equally. A value-traded-weighted average would better reflect how the "market" actually moved, since a few giant names dominate volume.

4. **A price-to-activity efficiency metric.** `value_traded / no_trades` gives an "average trade size" — useful for spotting institutional-heavy stocks (large, infrequent trades) vs. retail-heavy stocks (many small trades).

5. **Annotated crash-and-recovery timeline.** Since this window sits inside the COVID crash, overlaying key COVID-19 news dates (lockdown announcements, oil price crash, etc.) on the sector performance chart would turn a generic time series into a narrative-driven chart.

6. **Anomaly/outlier flagging.** With `perc_Change` ranging from -12.9% to +19.9%, a simple z-score or IQR-based outlier flag per stock-day would surface the single biggest single-day movers automatically, rather than relying on manual `nlargest`/`nsmallest` calls.

---

*Report generated from the cleaned dataset and charts already present in `DataCleaning.ipynb`.*
