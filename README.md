# Polymarket BTC 15m Assistant [Immediate Open Entry]
### Stage 8: Immediate Open Entry (+ Anti-Exhaustion <12 bp, Cheap Odds 45c-58c)

A real-time, high-performance automated trading bot for Polymarket **"Bitcoin Up or Down" 15-minute** binary options markets, executing via **Good 'Til Date (GTD)** limit orders powered by the **#1 Overall Champion Strategy** from the 7,678-market optimization report.

---

## 🏆 Empirical Backtest Reference (#1 Champion)

Evaluated across **7,678 historical Polymarket BTC 15-minute markets**:
- **Rank**: **#1 Overall Champion** (out of 30 simulated strategy configurations)
- **Win Rate**: **70.25%** (2,137 Wins / 905 Losses across 3,042 executed trades)
- **Market Coverage**: **39.6%** of all available 15m markets
- **Profit Factor**: **1.90**
- **Net Profit**: Exponential capital compounding
- **Directional Symmetry**:
  - **UP Trades**: 1,548 trades | **68.2% Win Rate**
  - **DOWN Trades**: 1,494 trades | **72.4% Win Rate**

---

## 🧠 Hardcoded Quantitative Strategy Architecture

### 1. Alpha & Entry Rules (`decide_open_entry`)
- **Immediate Open Timing Window ($0 \le t \le 45\text{s}$)**: Executes within the first 45 seconds of market open before Polymarket CLOB book odds adjust to Binance spot movement.
- **15m Structural Heikin-Ashi Directional Edge**:
  - Enters **UP** only if 15m HA has a **flat bottom** (institutional bullish expansion).
  - Enters **DOWN** only if 15m HA has a **flat top** (institutional bearish expansion).
  - Stands down on neutral/chop candles.
- **Cheap Odds Optimization ($0.40 \le \text{ask} \le 0.58$)**: Limits entries strictly to contract prices between 40¢ and 58¢, locking in asymmetric risk-reward payouts (average +100% on risk per win).
- **Anti-Exhaustion Gate ($< 12.0\text{ bp}$)**: Bypasses the market if the preceding completed 15m candle had an outsized body $\ge 12.0\text{ bp}$ ($0.12\%$), preventing exhaustion wick traps.

### 2. Drawdown & Defense Filters (`entry_block_reason`)
- **Consecutive Loss Circuit Breaker**: If 2 consecutive losses occur, the bot pauses all new entries for **60 minutes (4 consecutive markets)** to clear sideways consolidation chop. Win counter resets to 0 upon any win.
- **Realized Volatility / ATR Gate**: Automatically halts trading whenever trailing 1-hour realized volatility exceeds **$45.0\text{ bp}$**, protecting against news flushes (CPI, FOMC, SEC).
- **Transition Skip Hours**: Skips trading during **18:00 UTC** (US equity close rebalance) and **23:00 UTC** (daily UTC candle close / crypto funding roll).
- **Micro-Vetoes DISABLED (`use_vetoes: false`)**: The 1m RSI / HA exhaustion vetoes are turned off, allowing institutional breakout trades to execute cleanly.

---

## ⚡ GTD Low-Latency Execution Engine

- **Order Type**: Good 'Til Date (`OrderType.GTD`) limit orders with a **5-minute expiration timestamp**, preventing stale fills across candle transitions.
- **Signing Speed**: Sub-0.1ms C-accelerated EIP-712 signing via `coincurve`.
- **Pre-Warmed Token Caches**: Token IDs are pre-fetched ahead of candle roll, eliminating REST API lookup delays.
- **Reactive L2 WebSocket Feed**: Instant event-driven trade evaluation on Binance price ticks and CLOB book changes.
- **Default Port**: **`8084`**.

---

## 🏃 Running the Bot

```powershell
python main.py
```

Dashboard access:
👉 **`http://localhost:8084`**
