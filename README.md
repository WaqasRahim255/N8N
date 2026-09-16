# N8N Workflows

A collection of n8n workflows I've built, covering AI-powered automation across chatbots, document processing, and vehicle damage estimation.

## Workflows

### 🚗 Main EST V2
An end-to-end AI vehicle damage estimation pipeline. Given a claim (VIN, images, damage description), it identifies point-of-impact areas with a YOLO model, uses GPT-5 to determine affected categories and generate line-item repair estimates, validates output via a Pydantic AWS Lambda, blends/merges related damage categories, and uploads the final structured estimate (JSON) to S3 — with Slack notifications and human-in-the-loop review steps along the way.

**Key tech:** OpenAI (GPT-5), AWS Lambda, AWS S3, Apify, Slack

---

### 🔍 AI Info Extractor
A vehicle-data enrichment sub-workflow. It checks a claim's chat-collected data for missing fields (VIN, mileage, license plate, color code), pulls the relevant vehicle photos from S3, and uses GPT-5 vision to extract the missing details from the images. It then cross-references the VIN against JD Power vehicle data (via Lambda) to determine paint type/color and enriches the record with normalized results.

**Key tech:** OpenAI (GPT-5 Vision), AWS Lambda, AWS S3, JD Power API

---

### 💬 ChatBot + Google Sheets
A simple conversational AI agent (Groq/Llama 3.3 as the LLM, with buffer memory) that acts as a sports journalist chatbot. Each exchange (user message, AI response, timestamp) is parsed from the agent's JSON-formatted reply and logged as a new row in a connected Google Sheet.

**Key tech:** Groq (Llama 3.3), Google Sheets

---

### 📋 PanicAppNotification
An automated CV/résumé screening and interview-scheduling workflow. It pulls PDF résumés from a Google Drive folder, uses an AI agent (Groq/Llama 3.3) to evaluate each candidate against senior C# developer criteria (structured JSON output), emails a hiring manager for approval via Gmail's send-and-wait, and — if approved — has a second AI agent find an open slot in Google Calendar next week and auto-schedule the interview.

**Key tech:** Groq (Llama 3.3), Gmail, Google Calendar, Google Drive

---

## ⚠️ Note on Credentials

These are workflow exports from n8n. Node-level credentials (API keys, OAuth tokens) are **not** included in the export files themselves — only credential *references* (IDs/names) are. However, some workflows may contain hardcoded values in node parameters (e.g. API tokens in HTTP request URLs); these should be rotated and redacted before treating any exported file as safe to share publicly.

## Requirements

To import and run these workflows, you'll need:
- An [n8n](https://n8n.io) instance (self-hosted or cloud)
- API credentials for the relevant services used in each workflow (OpenAI, Groq, AWS, Google Workspace, Apify, Slack, etc.)

## Usage

1. Import the desired `.json` file into your n8n instance via **Workflows → Import from File**.
2. Reconnect the credentials for each node (they won't carry over from the export).
3. Update any hardcoded IDs (S3 bucket names, spreadsheet IDs, calendar emails, Lambda ARNs, etc.) to match your own environment.

