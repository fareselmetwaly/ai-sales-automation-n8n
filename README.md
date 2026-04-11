# End-to-End AI Sales Automation Engine

> Most sales teams don't lose deals because of bad products — they lose them because of slow follow-ups, generic emails, and leads that fall through the cracks. This system fixes all three.

---

## The Problem

Every sales team running manual operations faces the same bottlenecks:

- Hours wasted researching each lead before writing a single email
- Cold emails that sound cold — because they are generic
- Follow-ups that depend on someone remembering to send them
- Proposals and contracts built from scratch after every meeting
- Deals lost simply because a lead went cold while waiting for a response

The real cost isn't just time — it's the deals that never close because the process was too slow.

---

## The Solution

A 9-workflow automation pipeline that eliminates every manual touchpoint in the sales funnel — from the moment a lead file lands in Google Drive to the moment the client receives their onboarding document.

Humans stay in the loop only where it matters: approving emails before they go out, reviewing proposals, and signing off on contracts. Everything else runs automatically.

### How the system works end-to-end:

**Stage 1 — Lead Generation & Filtration**
Leads are sourced via Apify and uploaded to Google Drive. The system picks up the file automatically, extracts company name, email, and website, and filters out incomplete records.

**Stage 2 — AI-Powered Email Drafting**
An AI agent visits each company's website, reads and summarizes their business using Gemini, then generates a hyper-personalized cold email for each lead. Every draft is saved to Google Sheets for human review.

**Stage 3 — CRM Integration & Sending**
Once a human approves an email in Google Sheets, the system creates a new record in Notion CRM, sends the email via Gmail, and updates the CRM status — automatically.

**Stage 4 — Email Reply Handling**
The system monitors Gmail for replies and uses AI to detect intent. Interested leads get a calendar booking link. Uninterested leads are moved to Drop status. The team is notified on Telegram either way.

**Stage 5 — Automated Follow-Up Sequence**
If a lead doesn't respond, the system triggers follow-ups automatically on Days 1, 3, and 7. After three attempts with no reply, the lead is marked as dropped — no manual tracking needed.

**Stage 6 — Meeting Booking & Confirmation**
When a lead books a meeting on Google Calendar, the system updates Notion, sends a confirmation to the team on Telegram, and schedules a reminder before the meeting.

**Stage 7 — AI Proposal Generation**
After the meeting, the team fills a form with the client's needs. The AI analyzes the data, writes a full professional proposal, downloads it, and creates a draft email — ready for human review in minutes.

**Stage 8 — Contract Automation**
Once the client accepts the proposal, the system generates a contract from the agreed terms, prepares the email draft, updates the CRM, and notifies the team — all without manual input.

**Stage 9 — Client Onboarding**
When the contract is signed, the system creates a dedicated folder in Google Drive, fills the onboarding document with the client's data, prepares the delivery email, and marks the lead as Launched in Notion.

---

## ROI / Impact

- **15–20 hours/week** of manual sales work eliminated across lead research, email writing, follow-ups, and document preparation
- **Zero leads fall through the gaps** — every prospect is tracked from first contact to signed contract inside Notion CRM
- **Proposal and contract generation** reduced from hours to minutes
- **Follow-up consistency** is no longer human-dependent — the sequence runs automatically regardless of workload
- **Sales team focuses entirely on calls and closing** — all surrounding admin work is handled by the system

---

## 🔄 Automation vs Human Responsibility

A key design principle of this system is **keeping humans in the loop** at critical decision points — not replacing human judgment, but eliminating all surrounding manual work.

| Stage | Automated ✅ | Human Required 👤 |
|-------|-------------|-------------------|
| Lead file processing | ✅ | ❌ |
| Website analysis & email drafting | ✅ | ❌ |
| Email review & approval | ❌ | ✅ Quality control |
| CRM creation & tracking | ✅ | ❌ |
| Email reply analysis | ✅ | ❌ |
| Meeting booking | ✅ | ❌ |
| Proposal writing | ✅ | ❌ |
| Proposal review & sending | ❌ | ✅ Final approval |
| Contract generation | ✅ | ❌ |
| Contract review & sending | ❌ | ✅ Sign-off |
| Client onboarding | ✅ | ❌ |

---

## Tech Stack

| Category | Tools |
|----------|-------|
| Automation | n8n (Self-Hosted) |
| AI Models | Gemini, Ollama, OpenRouter |
| Lead Sourcing | Apify |
| CRM | Notion API |
| Communication | Gmail API, Telegram API |
| Scheduling | Google Calendar API |
| Storage | Google Drive API, Google Sheets API |
| Infrastructure | Docker, VPS, Nginx, SSL |


---

## 🗺️ System Architecture Overview

![System Architecture](./architecture_overview.png)

The system runs across **9 automated workflows**, organized into 5 main stages:

```
Lead Generation → CRM Integration → Email Reply Handling
→ Meeting & Proposal → Contract & Onboarding
```

---
## ⚙️ System Workflows (9 Workflows)

---

### Workflow 1 — Lead Generation & AI Filtration

**How leads are sourced:**
Leads are collected via **Apify** (a professional web scraping platform), exported as a structured file, and uploaded to Google Drive. This approach was chosen over full automation for cost-efficiency — Apify provides $5 free monthly credits which covers the required lead volume without extra cost.

**What the automation does from here:**
- Picks up the leads file automatically from Google Drive
- Extracts company name, email, and website from the file
- Filters out incomplete or invalid records
- AI agent visits each company's website and reads its content
- Gemini AI summarizes the company and understands what they do
- Generates a hyper-personalized cold email draft for each lead
- Saves all drafted emails to Google Sheets for human review

> 👤 **Human step:** Sales rep reviews and approves emails before sending — ensuring quality control at this critical stage.

---

### Workflow 2 — CRM Integration & Email Sending


**What it does:**
- Detects when a human marks an email as approved in Google Sheets
- Creates a new lead record inside Notion CRM automatically
- Sends the approved email via Gmail
- Updates the CRM status to "Email Sent"
- Logs the CRM ID back into Google Sheets for full tracking


---

### Workflow 3 — Automated Email Reply Handler


**What it does:**
- Monitors Gmail inbox for replies from leads
- AI (using OpenRouter) analyzes the reply to detect intent
- If the lead is **interested** → updates Notion status and sends a calendar booking link automatically
- If **not interested** → moves the lead to "Drop" status in Notion
- Sends a Telegram notification to the team with the reply summary
- Creates a follow-up task in Notion for the team

---

### Workflow 4 — Automated Follow-Up System

**What it does:**
- Triggered by Notion when a lead's "Next Communication" date arrives
- Loops through all leads that need follow-up
- Checks the current CRM status of each lead
- If status is "Email Sent" → sends a follow-up email automatically
- If status is "Meeting Sent" → notifies the team to follow up personally
- If status is "Follow-Up Sent" → marks the lead as dropped in Notion

---

### Workflow 5 — Meeting Booking Handler

**What it does:**
- Triggered when a new event is created in Google Calendar
- Checks if the meeting email matches an existing lead in Notion CRM
- If matched → updates Notion status to "Booked Meeting"
- Sends a Telegram notification to the team with meeting details
- Reminds the team to fill in the meeting outcome form afterward

---

### Workflow 6 — AI Proposal Generator

**What it does:**
- Triggered when the team fills in the post-meeting form
- Verifies the lead exists in Notion CRM
- AI analyzes the meeting form data to understand client needs
- Gemini AI writes a full professional proposal document automatically
- Downloads the generated proposal file
- Creates a draft email with the proposal attached
- Updates Notion CRM status to "Proposal Ready"
- Stores the proposal file URL inside the CRM
- Sends a Telegram notification to the team that the proposal is ready

> 👤 **Human step:** Team reviews the AI-generated proposal and sends it to the client.

---

### Workflow 7 — Contract Automation

**What it does:**
- Triggered when the client accepts the proposal via a contract form
- Verifies the lead status in Notion CRM
- AI analyzes the contract form data
- Generates a professional contract document automatically
- Downloads the contract file
- Creates a draft email with the contract attached for the team
- Updates Notion CRM status to "Contract Accepted"
- Stores the contract file URL in the CRM
- Sends a Telegram notification confirming the contract is ready

> 👤 **Human step:** Team reviews and sends the contract to the client for signing.

---

### Workflow 8 — Client Onboarding Automation

**What it does:**
- Triggered when the onboarding form is submitted
- Verifies the client email exists in Notion CRM
- Confirms the contract status is "Sent or Signed"
- Creates a dedicated onboarding folder for the client in Google Drive
- Copies the standard onboarding file template into the folder
- Updates the onboarding file with the client's specific data
- Downloads the finalized onboarding document
- Creates a draft email with the onboarding file attached
- Updates Notion CRM status to "Launch"
- Sends a Telegram notification confirming onboarding is complete

---

### Workflow 9 — Telegram Orchestrator Agent

**What it does:**
- A central command interface via Telegram for the sales team
- AI analyzes any incoming message from the team
- Finds the relevant lead record in Notion CRM automatically
- Based on the current lead status, the AI decides the correct action:
  - **Proposal Sent** → reminds the team to update CRM before sending
  - **Proposal Ready** → prepares the email fields (To, Subject, Body)
  - **Contract Ready** → prepares the contract email fields
  - **Contract Sent** → confirms and moves to onboarding stage
- Updates CRM status automatically after each action
- Sends a confirmation message back to the team on Telegram


---
---


## 🔐 Note on Source Code

The full workflow logic, AI prompts, and automation scripts are kept private for confidentiality. This repository documents the system architecture, design decisions, and outcomes only.

---

## 📬 Contact

Interested in a similar system for your business?

- 📧 fares.m.elmetwaly@gmail.com
- 🔗 [LinkedIn](https://www.linkedin.com/in/fares-maaty/)
- 💼 [Upwork Profile](https://www.upwork.com/freelancers/~0119c06665ee55e643?mp_source=share)
