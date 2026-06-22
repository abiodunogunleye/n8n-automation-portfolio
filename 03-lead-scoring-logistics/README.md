AI Lead Scoring System — Logistics / 3PL

An n8n automation that scores inbound sales enquiries from 1–10 using GPT-4o,
instantly alerts the sales team to hot leads, and logs every enquiry to a
Google Sheet with its score and reasoning.

Built for UK logistics and 3PL companies that receive a high volume of mixed-
quality enquiries and need to prioritise the ones worth chasing.


The Problem

A logistics company receives dozens of enquiries a week. They range from
serious prospects (recurring shipments, thousands of units, urgent timelines)
to low-value one-offs and price-only tyre-kickers.

Treated equally, the high-value leads get the same slow response as the rest —
and by the time someone follows up, the prospect has gone elsewhere.

The Solution

This workflow reads each enquiry with GPT-4o and assigns a score based on real
buying signals, then routes the lead accordingly:


Hot (8–10): instant email alert to the sales team for immediate follow-up
Warm (5–7): logged for standard follow-up
Cold (1–4): logged, no alert


Every lead — regardless of score — is recorded in a Google Sheet with its
score, the AI's reasoning, and all contact details.


How It Works

Enquiry Form
     │
     ▼
Extract Lead Data        (standardise form fields)
     │
     ▼
Score with GPT-4o        (HTTP request to OpenAI, returns score + reason)
     │
     ▼
Extract Score & Merge    (parse AI response, keep original lead data)
     │
     ▼
Route by Score (Switch)  ── Hot ──▶ Email Alert
     │                                    │
     └────────────────────────────────────▶ Log All Leads to Google Sheet

Scoring Logic

The GPT-4o prompt starts every lead at a base score of 5 and adjusts:

Positive signals


5,000+ units, recurring or ongoing contract → +2
Urgency ("ASAP", "urgent", tight timeline) → +2
Clear service need (fulfilment, warehousing, distribution) → +1
Enterprise or chain company → +1


Negative signals


Price-only enquiry → −2
One-off shipment → −1
Vague or unclear → −1


The model returns strict JSON: {"score": 8, "reason": "brief reason"}


Tech Stack


n8n (self-hosted) — workflow orchestration
GPT-4o (OpenAI API) — lead scoring and reasoning
Gmail — hot-lead alerts
Google Sheets — lead logging and audit trail
n8n Form Trigger — enquiry capture


Key Engineering Notes


The HTTP Request node overwrites input data with the API response, so a
dedicated function node re-merges the original lead fields with the parsed
score before routing.
Switch node comparisons are type-sensitive — scores are handled as numbers,
not strings, to avoid silent routing failures.
The scoring prompt enforces strict JSON output to keep downstream parsing
reliable.


Files


workflow.json — the complete n8n workflow (import-ready)
scoring-prompt.md — the GPT-4o system prompt


Setup


Import workflow.json into n8n
Add your OpenAI API key to the HTTP Request node
Connect Gmail and Google Sheets credentials
Point the Google Sheet node at your own sheet
Activate and submit a test enquiry



Part of a series of practical AI automations built for UK small businesses.

Live demos: demos.abiodunogunleye.store
Website: www.abiodunogunleye.store
