Automated Invoice Chaser — Service Businesses

An n8n automation that tracks issued invoices and automatically sends polite,
professional payment reminders to clients when invoices fall due — so business
owners get paid without chasing anyone by hand.

Built for UK service businesses (trades, freelancers, agencies) that lose time
and cash flow to forgotten, unpaid invoices.


The Problem

A service business finishes a job, sends the invoice, and moves on. Weeks
later the invoice is still unpaid — not because the client refuses, but
because it slipped everyone's mind. Chasing it means an awkward email or phone
call the owner keeps putting off. The money sits uncollected.

The Solution

This workflow does the chasing automatically:


The owner logs an invoice through a simple form (client, email, amount, due
date)
The invoice is recorded and tracked in a Google Sheet
After a set period, if the invoice isn't marked paid, the automation sends
the client a polite, professional reminder email


Most overdue invoices are simply forgotten — a single automated reminder
recovers a large share of them, with zero awkwardness for the owner.


How It Works

Invoice Submission Form
        │
        ▼
Log to Google Sheet     (client, amount, due date, status = Unpaid)
        │
        ▼
Wait                    (e.g. 5 days)
        │
        ▼
Send Reminder Email     (polite payment reminder to the client)

Tech Stack


n8n (self-hosted) — workflow orchestration and scheduling
n8n Form Trigger — invoice logging
Google Sheets — invoice tracking and status
Gmail — automated reminder emails (HTML formatted)


Key Notes


The reminder email is addressed to the client (the party who owes
payment), sent on behalf of the business — the wording is deliberately
friendly and professional to preserve the client relationship.
The Wait node handles the delay between logging and reminding; the interval
is configurable (5, 10, 14 days, etc.).
A styled Google Sheet template keeps invoice records clean and readable
(dark header, currency-formatted amounts, date columns).


Roadmap


Escalating reminders (Day 5 → Day 10 → Day 15) with firmer wording each time
Automatic stop when an invoice is marked Paid
Optional Telegram/SMS alert to the owner when a reminder goes out


Files


workflow.json — the invoice chaser workflow (import-ready)
invoice-tracker-template.xlsx — pre-formatted tracking sheet


Setup


Import workflow.json into n8n
Connect Google Sheets and Gmail credentials
Point the Sheets node at your own invoice tracker
Adjust the Wait node interval to your reminder timing
Activate and log a test invoice



Part of a series of practical AI automations built for UK small businesses.

Live demos: demos.abiodunogunleye.store
Website: www.abiodunogunleye.store
