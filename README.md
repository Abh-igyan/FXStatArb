# 📈 Quantitative FX Engine  
## Statistical Arbitrage & Market-Neutral Trading  

> Arbitrage Arena – Stage 2 Submission  
> High-Frequency FX Statistical Arbitrage using 5-second OHLCV data  

---

## 🚀 Overview

This project implements a systematic FX statistical arbitrage engine designed to exploit cross-currency dependencies while maintaining strict market neutrality.

The strategy operates exclusively on 5-second OHLCV data, uses only historical information (causal execution), and generates robust risk-adjusted returns under realistic constraints.

---

## 🧠 Strategy Summary

### Core Idea

Foreign exchange pairs exhibit strong structural relationships due to:

- Triangular parity  
- Shared macro drivers  
- Liquidity transmission  
- Order flow imbalances  

Temporary breakdowns in these relationships create mean-reverting opportunities exploitable via statistical arbitrage.

This engine builds a:

- **β-Neutral synthetic spread**
- Cointegration-driven mean reversion model
- Volatility-adjusted position sizing system

---

## 📊 Performance (20-Year Backtest)

| Metric | Result |
|--------|--------|
| **Sharpe Ratio** | **3.02** |
| **Cumulative Log Return** | **2.5** |
| **Maximum Drawdown** | **-5.76%** |
| **Avg Net Exposure** | **0.045** |
| **Market Neutrality** | Enforced |

✔ Robust across multiple FX regimes  
✔ Strictly causal  
✔ Transaction costs handled externally  

---

## 🏗 Strategy Architecture

### 1️⃣ Statistical Signal Generation

- Cointegration test between EUR/USD and GBP/USD  
- Engle-Granger framework  
- p-value: **0.0087**  
- Construct β-hedged synthetic spread  

Spread definition:

\[
Spread_t = EURUSD_t - \beta \cdot GBPUSD_t
\]

Z-score based entry/exit rules:

- Long spread if Z < -Z_threshold  
- Short spread if Z > +Z_threshold  
- Exit when |Z| < exit_threshold  

---

### 2️⃣ Market Neutrality

Implemented via:

- β-hedging  
- Dollar neutrality  
- Volatility-adjusted exposure  
- Bounded position sizing [-1, 1]  

Average net exposure across backtest: **0.045**

---

### 3️⃣ Risk Controls

- Volatility-normalized position sizing  
- Max position cap  
- Spread stop logic  
- Turnover-aware design  
- No regime labeling  
- No look-ahead bias  

---

### 4️⃣ Data Engineering

Built using:

- **Polars**
- LazyFrames
- Streaming execution

Optimized to:

- Handle high-frequency data efficiently  
- Operate under 10GB RAM constraint  
- Avoid unnecessary materialization  

---

## 📂 Dataset Structure
fx_data.zip
###├── EUR_USD.csv
###├── USD_JPY.csv
###├── GBP_USD.csv
###├── USD_CNH.csv


### CSV Schema

| Column | Description |
|--------|------------|
| utc | Timestamp (UTC) |
| open | Open price |
| close | Close price |
| high | High price |
| low | Low price |
| volumn | Volume (intentionally spelled) |

All data is sampled at fixed 5-second intervals.

---

## 📤 Output Format

The strategy produces:
strategy_output.csv

### Schema

| Column | Description |
|--------|------------|
| utc | Timestamp (UTC) |
| pair | Currency pair identifier (e.g., EURUSD) |
| position | Target position bounded between -1 and 1 |
| pnl | Incremental PnL for the bar |

---

## ⚙️ Execution Assumptions

- Positions are applied at bar close  
- PnL is computed using next bar close  
- Multiple market regimes are tested  
- Transaction cost modeling handled internally  

---

## 🔒 Compliance Checklist

- No external datasets  
- No future data usage  
- No manual regime labeling  
- Causal signals only  
- Market neutrality maintained  
- Valid bounded positions  
- No NaNs in output  

---

## 📈 Quantitative Framework

### Signal Stack

- Cointegration test  
- Rolling beta estimation  
- Spread construction  
- Z-score normalization  
- Volatility scaling  

### Position Logic

```python
if zscore > entry_threshold:
    short_spread()
elif zscore < -entry_threshold:
    long_spread()
else:
    flatten_positions()
```
🛠 Technology Stack

- Python
- Polars
- NumPy
- Statsmodels

Jupyter Notebook (StatArb.ipynb)

- ├── StatArb.ipynb
- ├── strategy_output.csv
- ├── fx_data.zip
- └── README.md

👨‍💻 Author

Abhigyan Tiwari
B.Tech CSE (2nd Year) | Quant Finance Enthusiast | Competitive Programming | ML/DL
