# AI WhatsApp Chatbot — Retail / Food Store

An n8n automation that lets customers ask a shop about products, prices and
availability over WhatsApp, and get instant AI-generated replies based on the
store's live product list.

Built for a UK-based African & Continental food store that receives constant
WhatsApp enquiries the owner can't answer while running the shop.

---

## The Problem

Small retail and food businesses get a steady stream of WhatsApp messages —
"Do you have X?", "How much is Y?", "Are you open?". The owner is busy serving
customers in-store and can't reply in real time. Unanswered messages become
lost sales.

## The Solution

This workflow puts an AI assistant on the shop's WhatsApp line:

- Reads each incoming customer message
- Checks the store's product catalogue (held in a Google Sheet the owner
  controls)
- Replies naturally in plain, friendly English with product info and prices

The owner updates products through a simple form or the sheet directly — no
code, no developer needed. Prices and stock stay current automatically.

---

## How It Works

```
WhatsApp message in  (via Twilio webhook)
        │
        ▼
Extract Message Data     (sender, name, message text)
        │
        ▼
Read Products            (pull catalogue from Google Sheet)
        │
        ▼
GPT-4o Assistant         (compose a natural reply using the product list)
        │
        ▼
Send WhatsApp Reply      (back to the customer via Twilio)
```

## Product Management

Products live in a Google Sheet with columns for name, price, category and
description. A separate n8n admin form lets the owner add or update products
without touching the sheet directly — submissions append straight to the
catalogue, and the chatbot reads the latest data on every message.

---

## Tech Stack

- **n8n** (self-hosted) — workflow orchestration
- **GPT-4o** (OpenAI API) — natural language replies
- **Twilio** — WhatsApp messaging (inbound webhook + outbound reply)
- **Google Sheets** — product catalogue and order log
- **n8n Form Trigger** — no-code product management for the owner

## Key Engineering Notes

- WhatsApp message bodies contain line breaks, emojis and special characters
  that break naive JSON payloads. User text is injected into the OpenAI request
  with `JSON.stringify()` so it's always safely escaped — this is the single
  most important fix for reliable WhatsApp → LLM pipelines.
- The Google Sheets read returns one item per row; the workflow consolidates
  these into a single context block so the model receives the whole catalogue
  in one prompt rather than firing once per product.
- On self-hosted n8n, Twilio fields are referenced explicitly through the
  webhook body, and node references use the full `$('NodeName')` form.

## Roadmap

- Multi-turn ordering (cart, totals, delivery address capture)
- Order logging to a dedicated sheet
- Optional RAG layer for stores with hundreds of products (semantic search
  over the catalogue instead of a full-list prompt)

## Files

- `workflow.json` — the chatbot workflow (import-ready)
- `admin-form.json` — the no-code product management form

## Setup

1. Import `workflow.json` into n8n
2. Add your OpenAI API key to the HTTP Request node
3. Connect Twilio (WhatsApp) and Google Sheets credentials
4. Point the Google Sheet node at your own product catalogue
5. Set the Twilio sandbox webhook to the workflow's webhook URL
6. Message the sandbox number to test

---

Part of a series of practical AI automations built for UK small businesses.

**Live demos:** [demos.abiodunogunleye.store](https://demos.abiodunogunleye.store)
**Website:** [www.abiodunogunleye.store](https://www.abiodunogunleye.store)
