# n8n-ai-job-alert-telegram-bot
Personal Project
# AI Job Filter & Instant Alert Bot (n8n + Gemini + Sheets + Telegram)

> **Note:** This is a **Personal Project** built for learning, testing, and demonstrating practical n8n workflow automation architecture.

## Overview
An automated n8n workflow that fetches live tech job posts from Hacker News RSS, evaluates and scores each role using Google Gemini, safely parses the AI JSON response in JavaScript, deduplicates high-match jobs in Google Sheets, and pushes instant formatted alerts to Telegram.

## Workflow Architecture
![Workflow Diagram](./workflow-diagram.png)

## How It Works (Step-by-Step)
1. **Live RSS Feed Intake (`1. Fetch Live Job Feed` & `2. Limit to 3 Items`):** Pulls live listings from `https://hnrss.org/jobs` and limits the batch size for testing (easily swappable from Manual Trigger to Schedule Trigger).
2. **AI Scoring & Extraction (`3. AI Analyze & Score`):** Sends each job title and description to **Google Gemini** (`Gemini 3 Flash Preview`) to return a strict JSON object containing `score` (1–10), `summary`, `key_skills`, and a tailored `draft_hook`.
3. **Defensive JSON Parsing (`4. Parse & Clean AI JSON`):** Uses a JavaScript Code Node to strip markdown fences, safely parse JSON with `try/catch` fallbacks, and merge metadata from the source feed.
4. **Threshold Filtering (`5. Is High-Match?`):** Routes jobs with `score >= 6` forward and skips lower-scoring posts.
5. **Deduplicated Logging (`6. Save & Dedup in Sheets`):** Uses Google Sheets `appendOrUpdate` matched on the `Link` column to prevent duplicate entries.
6. **Instant Notification (`7. Send AI Alert to Telegram`):** Sends a formatted summary with score, extracted skills, draft outreach hook, and direct job link to Telegram.

## Tech Stack
- **Automation:** n8n
- **AI / LLM:** Google Gemini API (`models/gemini-3-flash-preview`)
- **Data Storage & Dedup:** Google Sheets API
- **Messaging:** Telegram Bot API
- **Custom Logic:** JavaScript (n8n Code Node) & RSS Feed Read

## Setup Instructions
1. Download the `.json` file from this repository and import it into n8n (**Workflows** -> **Import JSON**).
2. Connect your own credentials for **Google Gemini API**, **Google Sheets OAuth2**, and **Telegram Bot API**.
3. Create a Google Sheet with a tab named `AI_Filtered_Jobs` and headers: `Timestamp`, `Title`, `Link`, `Score`, `Summary`, `Key_Skills`, `Draft_Hook`.
4. Replace `YOUR_GOOGLE_SHEET_ID` (Node 6) and `YOUR_TELEGRAM_CHAT_ID` (Node 7) with your own values, then run the workflow.
