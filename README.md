# ⚡ Grid-Based Trading Bot (HyperLiquid + Streamlit)

A robust, simple **grid trading bot** built on the **HyperLiquid API** with a dedicated **Streamlit dashboard**. 

You can create, edit, start, stop, and monitor live logs for multiple bots—all from a clean web interface. The bot automatically places buy and sell orders at fixed price intervals and intelligently rebuilds the grid whenever orders fill.

---

## 📚 Table of Contents

1.  [🌟 Overview](#-overview)
2.  [🚀 Features](#-features)
3.  [💻 Getting Started](#-getting-started)
4.  [⚙️ How It Works](#-how-it-works)
5.  [💡 Bot Logic Overview](#-bot-logic-overview)
6.  [📂 Project Structure](#-project-structure)
7.  [🔒 Configuration Notes](#-configuration-notes)
8.  [✨ Future Improvements](#-future-improvements)
9.  [👨‍💻 Author](#-author)

---

## 🌟 Overview

This project provides a full grid-trading system using the HyperLiquid decentralized exchange.

The **Streamlit interface** allows you to:

* Create, save, and manage configurations for multiple bots.
* Start and stop bots individually.
* Monitor live trades and detailed action logs in real time.

All bot configurations are stored locally in a `bots.json` file, and the backend logic handles order placement, fills, and dynamic grid rebuilding.

---

## 🚀 Features

* **Streamlit Dashboard** — Create, edit, start, stop, and delete trading bots via a clean UI.
* **Live Logs** — See fills, HyperLiquid responses, and strategy actions in real time.
* **Persistent Storage** — Bot configurations are stored and loaded from `bots.json`.
* **Grid Trading Engine**
    * Builds grid levels between specified minimum and maximum prices.
    * Places initial buy/sell limit orders across the grid.
    * Dynamically adjusts the grid after partial or full order fills.
* **HyperLiquid API Integration** — Uses `hyperliquid-python-sdk` for reliable exchange interaction.
* **Multi-bot Support** — Run several independent trading strategies simultaneously.

---

## 💻 Getting Started

### Install Requirements

First, install the necessary libraries using the provided requirements file:

```bash
# Assuming you have a virtual environment set up (recommended)
# python -m venv venv
# source venv/bin/activate  # On macOS/Linux
# venv\Scripts\activate     # On Windows

pip install -r requirements.txt
```
Your requirements.txt must include the following dependencies:

- streamlit
- numpy
- eth-account
- hyperliquid-python-sdk
- hyperliquid-monitor

Run the App
Execute the main application file using Streamlit. This command is executed directly in the terminal (Bash or Command Prompt), but it launches the Python application:
streamlit run app.py
After starting, open the link that appears in your terminal (usually a local host address like http://localhost:8501).

⚙️ How It Works
You enter critical bot settings via the Streamlit form:

- token symbol (e.g., BTC, ETH)
- wallet address and private key
- min/max price (defining the grid boundaries)
- bin step (the interval between grid orders)
- order size (the size of each limit order)

The bot initialization process:

1. Cancels any existing open orders for the specified symbol.
2. Gets the last close price.
3. Builds a symmetrical buy/sell grid.
4. Places limit orders.
5. Starts a dedicated listener through HyperliquidMonitor

On a Fill Event:
- The bot removes the old order that was just filled.
- It immediately places the next order one grid step above (if a buy filled) or below (if a sell filled), effectively rebalancing the inventory.
- It ensures the new order remains within the defined min/max range.
Simple Example: If the price fills a buy order at 0.0420, the bot will immediately place a new sell order at 0.0420 + bin_step.

💡 Bot Logic Overview
The main trading logic resides in logic.py.
Key components and their roles:

ExchangeClient
- Handles connection and authentication to HyperLiquid.
- Places and cancels orders.
- Fetches current price and wallet data.

GridManager
- Builds the structured list of price levels.
- Calculates and separates them into distinct buy and sell zones.

RunningBot
- Manages the bot lifecycle: startup, order placement, fill detection, rebalancing, and eventual stopping/cleanup.

Global Functions
- start_bot(bot_name, config)
- stop_bot(bot_name)
- get_bot_logs(bot_name)

📂 Project Structure
GridBased_TradingBot/
│
├── app.py                  # Streamlit UI (Frontend)
├── logic.py                # Trading logic engine (Backend)
├── bots.json               # Local persistent storage for bot configs
├── requirements.txt        # Python dependencies
├── packages.txt            # Deployment support file (optional)
└── README.md               # Project documentation

🔒 Configuration Notes
- Your private key is required to sign and place orders on the decentralized exchange.
- The key is stored locally inside bots.json. Note: It is not currently encrypted.
- Prices and step sizes use Streamlit numeric inputs, accepting float values.
- The bot uses constants from the HyperLiquid SDK for the mainnet API URL.
- The grid updates are fully automated upon successful trade fills.

✨ Future Improvements
- Add a backtesting mode for historical analysis.
- Implement optional stop-loss or take-profit layers.
- Integrate websocket price charts directly into the Streamlit dashboard.
- Implement encryption for private keys stored in bots.json.
- Add external Telegram or Discord alerts for fills and errors.

👨‍💻 Author
- Ali : AI Student
- GitHub: https://github.com/alisahito17
