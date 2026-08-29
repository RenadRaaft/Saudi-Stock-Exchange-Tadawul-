# Saudi Stock Exchange (Tadawul) Data Analysis

## 📊 Overview

This project focuses on analyzing data from the **Saudi Stock Exchange (Tadawul)** to explore stock market performance, trends, and patterns.

The analysis uses Python and data analysis techniques to clean, explore, and visualize the dataset, with the goal of extracting meaningful insights from Saudi stock market data.

## 🎯 Objectives

* Explore and understand the Tadawul dataset.
* Clean and prepare the data for analysis.
* Analyze stock price and trading trends.
* Compare the performance of different stocks.
* Identify patterns and trends in the Saudi stock market.
* Create visualizations to communicate key findings.
* Generate insights that can support a better understanding of market behavior.

## 🛠️ Technologies & Tools

* **Python**
* **Pandas** – Data manipulation and analysis
* **NumPy** – Numerical operations
* **Matplotlib** – Data visualization
* **Seaborn** – Statistical visualization
* **Jupyter Notebook** – Data analysis and visualization

## 📁 Project Structure

```text
Saudi-Stock-Exchange-Tadawul/
│
├── Data
│     └── Tadawul_stcks_23_4.xlsx
├── Notebook
│     └── DataCleaning.ipynb
├── Reports
│     └── Tadawul_Insights_Report.md
├── requirements.txt
└── README.md
```

## 🔍 Analysis

The project includes several stages of analysis:

### 1. Data Exploration

Understanding the dataset structure, variables, data types, and basic statistics. The cleaned dataset covers **200 companies across 11 sectors over 35 trading days** (April 2020).

### 2. Data Cleaning

Handling missing values, duplicates, inconsistent data, and preparing the dataset for analysis. This included fixing trailing whitespace in column names, correcting the misspelled `sectoer` → `sector` column, converting `date` to a proper datetime type, casting `no_trades` to integer, and imputing 162 missing `open`/`high`/`low` values.

### 3. Exploratory Data Analysis

Exploring questions such as:

* Which stocks have performed the best?
* How have stock prices changed over time?
* What are the major market trends?
* Which stocks experience higher volatility?
* How does trading activity vary over time?

### 4. Data Visualization

Creating charts and visualizations to identify trends and make comparisons between stocks and market indicators, including OHLC trend lines, top-10 bar charts, a sector/percentage-change breakdown, a correlation heatmap, and a volume–value–sector bubble chart.

## 📈 Key Insights

* **Missing data isn't random — it's a signal.** 162 rows were missing `open`/`high`/`low` simultaneously. Four tickers (`ATHEEB TELECOM`, `ALKHODARI`, `THIMAR`, `WAFA INSURANCE`) were missing for **all 35 days**, each with `change = 0`, indicating these stocks likely weren't actively trading rather than having a genuine data gap.

* **Sector representation is heavily imbalanced.** Financials alone accounts for ~24% of all rows (1,645 of 6,992), while Utilities and Information Technology are each represented by just 2 companies — meaning sector-level averages for the smaller sectors are far less statistically reliable.

* **Price level and trading activity barely relate.** `open`/`high`/`low`/`close` correlate at 0.99–1.00 with each other (expected), but correlate only around -0.06 to -0.07 with `volume_traded` — higher-priced stocks don't trade in higher volume.

* **Daily price movement is largely unpredictable from activity alone.** `perc_Change` shows almost no correlation (≤0.03) with price level, volume, value traded, or number of trades — none of these factors explain next-step direction in this sample.

* **Sector performance reflects the COVID-19 market shock.** Of all 11 sectors, only **Information Technology** posted a positive average daily % change (~+0.5%); every other sector was negative, with **Industrials** and **Consumer Discretionary** hit hardest — consistent with the dataset's April 2020 timeframe.

* **Liquidity is concentrated in a few giants.** The top companies by total value traded — **ALRAJHI, ALINMA, SAUDI ARAMCO, SABIC** — are dominated by major banks and Aramco, meaning market-wide averages are implicitly driven by a handful of large financial names.

> Full write-up with charts and methodology is available in [`Tadawul_Insights_Report.md`](./Tadawul_Insights_Report.md).

## 🚀 Getting Started

### Clone the Repository

```bash
git clone https://github.com/RenadRaaft/Saudi-Stock-Exchange-Tadawul.git
```

### Navigate to the Project

```bash
cd Saudi-Stock-Exchange-Tadawul
```

### Install Dependencies

```bash
pip install -r requirements.txt
```

### Run the Analysis

Open the Jupyter Notebook:

```bash
jupyter notebook
```

Then open the analysis notebook located in the `notebooks` folder.

## 📌 Future Improvements

* Add more recent Tadawul market data.
* Perform advanced statistical analysis.
* Investigate relationships between different market indicators.
* Add interactive visualizations.
* Develop predictive models for further analysis.
* Replace mean-imputation for non-trading stocks with an explicit `is_suspended` flag.
* Add a sector volatility index (standard deviation of `perc_Change`) alongside average performance.
* Build a liquidity-weighted (rather than simple-average) sector performance metric.

## 👩‍💻 Author

**Renad Yassin**

Computer Science Graduate | Data Analysis & AI Enthusiast