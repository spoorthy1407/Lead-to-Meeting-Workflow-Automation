# Automated Lead Qualification & Notification System

A no-code workflow automation built in **Make.com** that connects **Airtable**, **Gmail** and **Slack**. When a lead is marked as qualified, it instantly emails the lead a Calendly booking link and alerts the team in Slack, with no manual follow-up.

![Scenario overview](./screenshots/scenario.png)

---

## Problem

Sales teams lose leads when responses are slow or when nobody notices a new qualified enquiry. Manually checking a table, writing an email and telling the team is repetitive and error-prone.

## Solution

An event-driven scenario that watches a lead table and, for every qualified lead, runs two parallel actions:

1. **Responds to the lead** with a personalised email and a self-scheduling link.
2. **Notifies the team** in a Slack channel in real time.

---

## Workflow

```
Airtable (Watch Records)
        │
        ▼
     Router
     ├── Route 1 ──► Gmail (Send an email)
     └── Route 2 ──► Text Parser (Match pattern) ──► Slack (Send a message)
```

| # | Module | Purpose |
|---|--------|---------|
| 1 | **Airtable – Watch Records** | Monitors the `Lead Contacts` table in `Lead Base`. Triggers on the `Created on` field and filters with the formula `{Qualification} = "Qualified"` (max 10 records per run). |
| 3 | **Router** | Splits each qualified lead into two parallel branches. |
| 4 | **Gmail – Send an email** | Sends a personalised HTML email to the lead using their first name and company, with a Calendly link so the lead can book an intro call themselves. |
| 10 | **Text Parser – Match pattern** | Applies a regular expression to the lead's `Message` field to extract key text. |
| 9 | **Slack – Send a message** | Posts the lead's message to a public Slack channel to alert the team. |

---

## Airtable schema

The `Lead Contacts` table uses these fields:

`First Name`, `Last Name`, `Email`, `Phone`, `Company`, `Budget`, `Notes`, `Message`, `Qualification`, `Created on`, `Contacted on`, `Attachment Summary`

---

## Key concepts demonstrated

- Event-driven triggers and formula-based filtering
- Router branching and parallel execution
- Dynamic data mapping between apps (`{{1.Email}}`, `{{1.First Name}}`, `{{1.Company}}`)
- Regular expressions for text extraction
- OAuth/API connections to Airtable, Gmail and Slack
- Automated email outreach and self-service meeting scheduling

---

## Setup

### Prerequisites
- A [Make.com](https://www.make.com) account
- An Airtable base with a leads table (see schema above) and an API token
- A Gmail account
- A Slack workspace and channel
- A Calendly (or similar) booking link

### Steps
1. Create a new scenario in Make.com.
2. Click the three-dot menu → **Import Blueprint** and upload `Integration_Airtable_blueprint.json`.
3. Reconnect each module with your own connections (Airtable, Gmail, Slack).
4. In the Airtable module, select your own **Base** and **Table**.
5. In the Gmail module, replace the Calendly link with your own.
6. In the Slack module, choose your own channel.
7. Turn the scenario on and add a record with `Qualification = Qualified` to test it.

> **Note:** The blueprint contains placeholder IDs from the original setup. These will not work for your account, so reselect your own base, table, connections and channel after importing.

---

## How to test

1. Add a new record in Airtable with a valid email, a first name, a company and a message.
2. Set `Qualification` to **Qualified**.
3. Run the scenario once (or wait for the next scheduled run).
4. Check that the lead receives the email and the Slack channel shows the message.

---

## Results

- Instant response to every qualified lead
- Meeting booking handled by the lead through the Calendly link
- Team alerted in real time, so no lead is missed
- Zero manual steps after a lead is qualified

---

## Known limitations and future improvements

- The Slack message currently posts the full `Message` field; mapping the Text Parser output into Slack would send only the extracted value.
- The email body is sent as raw HTML but uses plain line breaks; switching to `<br>` tags would render it better.
- Add a step to write back to Airtable (for example, fill `Contacted on`) so leads are not contacted twice.
- Add AI-based lead scoring to set `Qualification` automatically.
- Sync qualified leads to a CRM such as HubSpot.
- Add error handling and notifications for failed runs.

---

## Tech stack

`Make.com` · `Airtable` · `Gmail` · `Slack` · `Regex` · `Calendly`

---

## Repository structure

```
├── README.md
├── Integration_Airtable_blueprint.json
└── screenshots/
    └── scenario.png
```

---

## Author

**Your Name**
[LinkedIn](https://linkedin.com/in/your-profile) · [GitHub](https://github.com/your-username) · your.email@example.com
