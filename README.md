# AIStockTrader

An autonomous stock trading system powered by AI agents. Multiple virtual traders with distinct investment strategies research markets, execute trades, and manage portfolios in real-time.

## Overview

AIStockTrader demonstrates how agentic AI frameworks can manage stock trading portfolios autonomously. Each trader operates as an independent AI agent with its own investment philosophy, using real market data to make trading decisions.

## Features

- **Autonomous Trading** - AI agents independently research, analyze, and execute trades
- **Multiple Trading Strategies** - Four distinct traders modeled after famous investors
- **Real-time Dashboard** - Live portfolio monitoring with performance charts
- **Market Integration** - Real stock prices via Polygon.io API
- **Persistent Memory** - Traders retain knowledge across sessions
- **Push Notifications** - Trade alerts via Pushover API
- **Multi-model Support** - Works with OpenAI, DeepSeek, Gemini, and Grok

## The Traders

| Trader | Strategy | Approach |
|--------|----------|----------|
| **Warren** | Value Investing | Buy quality companies below intrinsic value, long-term hold |
| **George** | Macro Trading | Exploit large economic mispricings with aggressive timing |
| **Ray** | Risk Parity | Systematic diversification across asset classes |
| **Cathie** | Innovation/Crypto | Aggressive positions in disruptive tech and crypto ETFs |

## Architecture

```
┌─────────────────────────────────────────┐
│         Gradio Web Dashboard            │
│      Real-time Portfolio Monitoring     │
└─────────────────────────────────────────┘
                    │
┌─────────────────────────────────────────┐
│        Trading Floor Scheduler          │
│         Runs every N minutes            │
└─────────────────────────────────────────┘
                    │
┌─────────────────────────────────────────┐
│          Trader AI Agents               │
│   Independent strategies & decisions    │
└─────────────────────────────────────────┘
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼
   ┌─────────┐ ┌─────────┐ ┌─────────┐
   │Accounts │ │ Market  │ │Research │
   │ Server  │ │ Server  │ │ Tools   │
   └─────────┘ └─────────┘ └─────────┘
```

## Project Structure

```
AIStockTrader/
├── app.py              # Web dashboard (Gradio UI)
├── trading_floor.py    # Scheduler for autonomous trading
├── traders.py          # Trader agent implementation
├── accounts.py         # Portfolio management
├── market.py           # Market data integration
├── database.py         # SQLite persistence
├── templates.py        # LLM instruction prompts
├── tracers.py          # Logging infrastructure
├── mcp_params.py       # MCP server configurations
├── reset.py            # Initialize trader strategies
├── accounts_server.py  # MCP server for account operations
├── accounts_client.py  # Client for accounts server
├── market_server.py    # MCP server for market data
├── push_server.py      # MCP server for notifications
├── util.py             # UI utilities
├── accounts.db         # SQLite database
└── memory/             # Per-trader persistent memory
```

## Setup

### Prerequisites

- Python 3.10+
- API keys for market data and LLM services

### Installation

1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd AIStockTrader
   ```

2. Install dependencies:
   ```bash
   pip install gradio pandas plotly python-dotenv polygon openai requests mcp pydantic
   ```

3. Configure environment variables in `.env`:
   ```env
   # Market Data (Polygon.io)
   POLYGON_API_KEY=your_polygon_key
   POLYGON_PLAN=free  # free, paid, or realtime

   # LLM API Keys
   OPENAI_API_KEY=your_openai_key
   DEEPSEEK_API_KEY=your_deepseek_key
   GROK_API_KEY=your_grok_key
   GOOGLE_API_KEY=your_google_key

   # Notifications (Pushover)
   PUSHOVER_USER=your_user_id
   PUSHOVER_TOKEN=your_token

   # Web Search (Brave)
   BRAVE_API_KEY=your_brave_key

   # Scheduler Settings
   RUN_EVERY_N_MINUTES=60
   RUN_EVEN_WHEN_MARKET_IS_CLOSED=false
   USE_MANY_MODELS=false
   ```

4. Initialize trader accounts:
   ```bash
   python reset.py
   ```

## Usage

### Launch the Dashboard

```bash
python app.py
```

Opens a web UI showing all traders with:
- Portfolio value and P&L
- Holdings breakdown
- Transaction history
- Real-time activity logs

### Start Autonomous Trading

```bash
python trading_floor.py
```

Runs the scheduler that executes all traders every N minutes (configurable). Each cycle:
1. Checks if market is open
2. Traders research market conditions
3. Execute trades based on strategy
4. Send notification summaries

## How It Works

### Trading Cycle

Each trader alternates between two modes:

- **TRADE Mode** - Search for new investment opportunities
- **REBALANCE Mode** - Adjust existing portfolio based on strategy

### Agent Capabilities

Traders have access to:
- **Account Operations** - Buy/sell shares, check balance, view holdings
- **Market Data** - Real-time stock prices
- **Research Tools** - Web search, page fetching, persistent memory
- **Notifications** - Send trade summaries

### Trade Execution

- 0.2% bid-ask spread applied to all trades
- Starting balance: $10,000 per trader
- All transactions validated and logged

## Tech Stack

- **Agent Framework** - Anthropic Agents SDK
- **Tool Protocol** - Model Context Protocol (MCP)
- **Web UI** - Gradio
- **Market Data** - Polygon.io
- **Database** - SQLite
- **Charts** - Plotly
- **Notifications** - Pushover

## Configuration Options

| Setting | Description | Default |
|---------|-------------|---------|
| `RUN_EVERY_N_MINUTES` | Trading frequency | 60 |
| `RUN_EVEN_WHEN_MARKET_IS_CLOSED` | Trade outside market hours | false |
| `USE_MANY_MODELS` | Use different LLMs per trader | false |
| `POLYGON_PLAN` | Market data tier (free/paid/realtime) | free |
