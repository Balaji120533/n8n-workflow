# Telegram AI Command Manager

An AI-powered Telegram automation built using n8n that allows users to add and retrieve categorized data using simple text commands.

## Features
- Supports `/add` and `/get` commands
- Uses Google Gemini (LLM) to structure unstructured text
- Splits multiple items using JavaScript
- Stores and retrieves data from Google Sheets
- Sends real-time responses via Telegram
- Deployed on Railway

## Commands

### /add

#### /add groceries milk, meat
#### /add study CA Unit 1, Math Unit 2
#### /add project Euler find resources, execute

Adds categorized items to Google Sheets with automatic counting.

### /get
/get groceries
Fetches stored items by category and date and returns them via Telegram.

##  Tech Stack
- n8n
- Telegram Bot API
- Google Gemini
- JavaScript
- Google Sheets
- Railway
