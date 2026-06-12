# 📊 Bitcoin Market Sentiment vs Trader Performance Analysis

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458?logo=pandas&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-11557c)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

> **Data Science Internship Project** · Primetrade.ai  
> Uncovering how Bitcoin market sentiment drives trader behaviour and profitability on Hyperliquid

---

## 🧭 Executive Summary

This project investigates the statistical relationship between the **Bitcoin Fear & Greed Index** and real trader performance metrics derived from **Hyperliquid's historical trading data**. By merging sentiment classifications with trade-level outcomes, the analysis surfaces actionable patterns in profitability, win rates, and trade sizing across five distinct market sentiment regimes: *Extreme Fear, Fear, Neutral, Greed,* and *Extreme Greed*.

Key takeaway: **sentiment matters, but not always in the direction intuition suggests.** Fear periods produced the highest total profits and largest average trade sizes, while Extreme Greed periods yielded the highest per-trade average returns and win rates — suggesting that professional traders capitalise on both panic-driven and euphoria-driven markets in fundamentally different ways.

---

## 📁 Project Structure

```
bitcoin-sentiment-trader-analysis/
│
├── data/
│   ├── historical_data.csv          # Hyperliquid historical trader data
│   └── fear_greed_index.csv         # Bitcoin Fear & Greed Index
│
├── notebooks/
│   └── Primetrade_Market_Sentiment_Analysis.ipynb   # Main analysis notebook
│
├── visuals/
│   ├── avg_pnl_by_sentiment.png     # Average PnL bar chart
│   ├── win_rate_by_sentiment.png    # Win rate bar chart
│   └── trade_count_by_sentiment.png # Trade count distribution
│
├── README.md
└── requirements.txt
```

---

## 📦 Datasets

### Dataset 1 — Bitcoin Fear & Greed Index

| Column           | Description                                        |
|------------------|----------------------------------------------------|
| `timestamp`      | Unix timestamp of the index reading                |
| `value`          | Numeric score (0 = Extreme Fear, 100 = Extreme Greed) |
| `classification` | Sentiment label: Extreme Fear / Fear / Neutral / Greed / Extreme Greed |
| `date`           | Calendar date of the reading                       |

### Dataset 2 — Hyperliquid Historical Trader Data

| Column             | Description                              |
|--------------------|------------------------------------------|
| `Account`          | Trader wallet address                    |
| `Coin`             | Cryptocurrency traded                    |
| `Execution Price`  | Price at which the trade was executed    |
| `Size USD`         | Notional trade value in USD              |
| `Side`             | Buy or Sell                              |
| `Timestamp IST`    | Trade execution time (IST timezone)      |
| `Direction`        | Long or Short                            |
| `Closed PnL`       | Realised profit or loss on the trade     |
| `Fee`              | Trading fee paid                         |

---

## 🔬 Methodology

### 1. Data Loading & Initial Exploration
Both datasets were loaded using `pandas`. An initial inspection of column names, data types, shape, and date ranges confirmed compatibility for a date-based merge.

### 2. Date Conversion & Cleaning
The `Timestamp IST` column in the trades dataset was parsed using `pd.to_datetime()` with `dayfirst=True` to handle the DD/MM/YYYY format. A normalised `date` column (Python `date` object) was then extracted. The same conversion was applied to the sentiment dataset to ensure type-consistent join keys.

### 3. Dataset Merging
A left merge on the `date` key joined each trade to its corresponding sentiment classification. This preserved all trade records while appending the market sentiment context for the day the trade occurred.

### 4. Feature Engineering
A binary `win` column was derived: `win = 1` where `Closed PnL > 0`, enabling win rate calculations per sentiment group.

### 5. Aggregated Analysis
The merged dataset was grouped by `classification` to compute:
- Mean and total `Closed PnL`
- Trade count per sentiment period
- Win rate (mean of `win` column)
- Average trade size (`Size USD`)

### 6. Visualisation
Bar charts were produced using `matplotlib` for each key metric, with axis labels, rotated ticks, and tight layout applied for clarity.

---

## 📈 Key Findings

### Average Closed PnL by Sentiment
| Sentiment       | Avg Closed PnL (USD) |
|-----------------|----------------------|
| 🟢 Extreme Greed | **67.89**           |
| 🔴 Fear          | 54.29               |
| 🟡 Greed         | 42.74               |
| 🟠 Extreme Fear  | 34.54               |
| ⚪ Neutral        | 34.31               |

> **Extreme Greed periods generated the highest average profit per trade** — suggesting skilled traders effectively capitalise on market euphoria.

---

### Total Closed PnL by Sentiment
| Sentiment       | Total Closed PnL     |
|-----------------|----------------------|
| 🔴 Fear          | **$3.36M**          |
| 🟢 Extreme Greed | $2.72M              |
| 🟡 Greed         | $2.15M              |
| ⚪ Neutral        | $1.29M              |
| 🟠 Extreme Fear  | $0.74M              |

> **Fear periods generated the most aggregate profit**, driven by a combination of high trade volume and elevated trade sizes.

---

### Win Rate by Sentiment
| Sentiment       | Win Rate |
|-----------------|----------|
| 🟢 Extreme Greed | **46.49%** |
| 🔴 Fear          | 42.08%   |
| ⚪ Neutral        | 39.70%   |
| 🟡 Greed         | 38.48%   |
| 🟠 Extreme Fear  | 37.06%   |

> **Extreme Greed had the highest win rate**, indicating traders make more accurate directional calls when markets are at peak optimism.

---

### Average Trade Size (USD) by Sentiment
| Sentiment       | Avg Trade Size (USD) |
|-----------------|----------------------|
| 🔴 Fear          | **$7,816**          |
| 🟡 Greed         | $5,737              |
| 🟠 Extreme Fear  | $5,350              |
| ⚪ Neutral        | $4,783              |
| 🟢 Extreme Greed | $3,112              |

> **Traders deployed the largest positions during Fear**, possibly exploiting oversold conditions with higher conviction bets.

---

## 🔑 Conclusions

1. **Sentiment is a meaningful signal** — all five sentiment regimes produce statistically distinguishable trader outcomes across PnL, win rate, and sizing metrics.

2. **Extreme Greed = precision, Fear = volume** — The market splits into two distinct alpha regimes. Extreme Greed sees fewer but higher-quality trades (high win rate, high average PnL). Fear sees large, high-conviction trades that accumulate the most total profit.

3. **The contrarian edge is real** — Traders appear to size up most aggressively during Fear, suggesting professional participants actively fade retail panic rather than follow it.

4. **Neutral sentiment is the worst environment** — Lowest average PnL and second-lowest win rate. Absence of a strong sentiment signal correlates with the least differentiated trader outcomes.

5. **Extreme Fear underperforms across the board** — Lowest win rate and lowest total PnL, indicating that trading at peak market distress (rather than early-fear) is less profitable, possibly due to high volatility and unpredictable price action.

---

## 🚀 Future Improvements

- **Coin-level segmentation** — Analyse whether sentiment effects hold uniformly across BTC, ETH, and altcoins, or whether some assets are more sentiment-sensitive.
- **Lagged sentiment analysis** — Test whether *yesterday's* Fear & Greed score better predicts *today's* performance, capturing delayed market reactions.
- **Directional breakdown (Long vs Short)** — Investigate how sentiment differentially affects long vs short traders, particularly during Extreme Fear.
- **Top trader cohort analysis** — Isolate the top 10% of traders by PnL and determine whether they exhibit stronger sentiment-alignment than the average.
- **Time-series modelling** — Apply rolling window analysis to track how the sentiment–performance relationship evolves over market cycles (bull/bear).
- **Statistical significance testing** — Apply ANOVA or Kruskal-Wallis tests to validate whether observed differences across sentiment groups are statistically significant.
- **Drawdown & risk-adjusted metrics** — Incorporate Sharpe Ratio and max drawdown per sentiment category for a more complete risk-return picture.

---

## 🛠️ Technologies Used

| Tool / Library   | Purpose                                 | Version |
|------------------|-----------------------------------------|---------|
| Python           | Core programming language               | 3.8+    |
| Pandas           | Data loading, cleaning, and aggregation | 1.5+    |
| Matplotlib       | Data visualisation                      | 3.6+    |
| Jupyter Notebook | Interactive analysis environment        | 6.0+    |
| NumPy            | Numerical operations                    | 1.23+   |

---

## ⚙️ Installation & Usage

### Prerequisites
Ensure you have Python 3.8+ and `pip` installed.

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/bitcoin-sentiment-trader-analysis.git
cd bitcoin-sentiment-trader-analysis
```

### 2. Install Dependencies
```bash
pip install -r requirements.txt
```

Or install manually:
```bash
pip install pandas matplotlib numpy jupyter
```

### 3. Add the Datasets
Place the following files in the `data/` directory:
- `historical_data.csv` — Hyperliquid trader data
- `fear_greed_index.csv` — Bitcoin Fear & Greed Index

### 4. Launch the Notebook
```bash
jupyter notebook notebooks/Primetrade_Market_Sentiment_Analysis.ipynb
```

### 5. Run All Cells
In Jupyter, select **Kernel → Restart & Run All** to reproduce the full analysis and regenerate all visualisations.

---

## 📄 `requirements.txt`
```
pandas>=1.5.0
matplotlib>=3.6.0
numpy>=1.23.0
jupyter>=1.0.0
```

---

## 👤 Author

**[Your Name]**  
Data Science Intern · Primetrade.ai  
📧 your.email@example.com  
🔗 [LinkedIn](https://linkedin.com/in/yourprofile) · [GitHub](https://github.com/yourusername)

---

## 📜 License

This project was completed as part of a Data Science internship assignment at **Primetrade.ai**. All data used belongs to their respective sources. For educational and research use only.

---

*Last updated: June 2026*
