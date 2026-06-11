# 📈 Stock Market Telegram Bot — n8n Workflow

An automated stock market assistant that delivers AI-powered daily briefings and responds to on-demand stock analysis commands via Telegram. Designed for Indian retail investors tracking NSE/BSE listed stocks.

---

## 🚀 Just Want to Use the Bot?

**No setup needed.** Simply open Telegram and start a chat with the bot:

👉 **[t.me/stock_details_bot](https://t.me/@stock_details_bot)**

Type `/add` or `/analyze` and you're good to go. That's it.

---

## ✨ Features

- **Daily Briefing** — Every day, the bot automatically fetches all stocks on your watchlist and sends you an AI-generated analysis on Telegram.
- **`/add` Command** — Add any NSE/BSE stock to your personal watchlist directly from Telegram.
- **`/analyze` Command** — On-demand deep analysis of any stock, combining live price data with the latest Google News headlines.
- **AI Analysis** — Powered by Google Gemini, producing concise Bloomberg-style summaries.

---

## 💬 How to Use

### Add a Stock to Your Daily Watchlist
```
/add RELIANCE.NSE
/add TCS.BSE
/add INFY.NSE
```
Once added, you'll receive an analysis for this stock every day automatically.

Supported exchange suffixes: `.NSE` (NSE listed) and `.BSE` (BSE listed). If you omit the suffix, NSE is assumed by default.

### On-Demand Stock Analysis
```
/analyze RELIANCE.NSE
/analyze HDFCBANK.NSE
```
Returns live price data + top 3 breaking news headlines, analyzed instantly by Gemini.

---

## 📊 Sample Output

```
RELIANCE.NS — 🟢 Bullish

• Current Price: ₹2,987 | +1.24% from prev. close (₹2,950)
• Day Range: ₹2,961 – ₹3,002 (tight range, consolidation near highs)
• Volume: 4.2M — above average, confirms buying interest
• 📰 Catalyst / News: Reliance's new energy venture attracted positive
  coverage, with analysts noting strong FII inflows this session.
• Watch tomorrow: Hold above ₹2,975 for continuation toward ₹3,050.
```

---

## 🏗️ How It Works (For Developers)

The bot is built on [n8n](https://n8n.io) and runs two parallel workflows:

### Flow 1 — Scheduled Daily Briefing

```
Schedule Trigger (daily)
  → Google Sheets (read watchlist)
  → Yahoo Finance API (fetch live price data)
  → JavaScript (parse & carry chat_id)
  → Google Gemini LLM (generate analysis)
  → Telegram (send to each user)
```

### Flow 2 — Telegram Command Handler
```
Telegram Trigger (on message)
  → JavaScript (parse command & ticker)
  → Switch
      ├── /add  → Append to Google Sheet → Confirm to user
      └── /analyze → Yahoo Finance API
                       → JavaScript (parse price data)
                       → Google News RSS (top 3 headlines)
                       → JavaScript (merge data + news)
                       → Google Gemini LLM (generate analysis)
                       → Telegram (send to user)
```

---

## 🛠️ Self-Hosting Instructions

Want to run your own version? Follow the steps below.

### Prerequisites
- [n8n](https://n8n.io) instance (self-hosted or cloud)
- A Telegram Bot token (create one via [@BotFather](https://t.me/BotFather))
- A Google account with Google Sheets access
- A Google Gemini (PaLM) API key

### 1. Create a Telegram Bot
1. Open Telegram and message [@BotFather](https://t.me/BotFather).
2. Send `/newbot` and follow the prompts.
3. Copy the **Bot Token** — you'll need it for n8n credentials.

### 2. Prepare the Google Sheet
Create a new Google Sheet with the following columns in **Sheet1**:

| User | Ticker | Chat_ID |
|------|--------|---------|
| (auto-filled) | (auto-filled) | (auto-filled) |

Copy the **Spreadsheet ID** from the sheet URL:
```
https://docs.google.com/spreadsheets/d/<SPREADSHEET_ID>/edit
```

### 3. Import the Workflow into n8n
1. In n8n, go to **Workflows → Import from File**.
2. Upload `Stock_market_telegram_bot.json`.

### 4. Configure Credentials
Set up the following credentials in n8n (**Settings → Credentials**):

| Credential | Used For |
|---|---|
| `Telegram API` | Telegram Trigger + Send nodes |
| `Google Sheets OAuth2` | Read & write watchlist |
| `Google Gemini (PaLM) API` | AI analysis (both LLM chains) |

### 5. Update the Workflow
After importing, update these values:

- **Get row(s) in sheet** & **Append row in sheet** — point both to your Spreadsheet ID.
- **Send a text message** (daily briefing node) — set `chatId` to `={{ $json.chat_id }}`.
- **Schedule Trigger** — set your preferred time. `15:45 IST` (after NSE/BSE close) is recommended. Confirm your n8n instance timezone is set to `Asia/Kolkata`.

### 6. Activate the Workflow
Toggle the workflow to **Active**. The Telegram Trigger will begin listening immediately.

---

> Built with [n8n](https://n8n.io) · Powered by [Google Gemini](https://ai.google.dev) · Data from [Yahoo Finance](https://finance.yahoo.com)
