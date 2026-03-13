# 🤖 End-to-End AI Sales Automation Engine

> A full-cycle intelligent sales system built with n8n that automates the entire sales funnel — from lead generation to client onboarding — with human oversight at critical decision points.

---

## 📌 The Problem

Sales teams manually waste **15–20 hours per week** on repetitive tasks:

- Researching hundreds of leads one by one
- Writing personalized emails from scratch for each prospect
- Tracking follow-ups manually in spreadsheets
- Scheduling meetings and sending reminders
- Preparing proposals and contracts manually
- Handling client onboarding paperwork

---

## ✅ The Solution

A fully automated, AI-powered sales pipeline that handles every stage of the funnel automatically — with humans stepping in only at key decision points to ensure quality and control.

---

## 🗺️ System Architecture Overview

![System Architecture](./architecture_overview.png)

The system runs across **9 automated workflows**, organized into 5 main stages:

```
Lead Generation → CRM Integration → Email Reply Handling
→ Meeting & Proposal → Contract & Onboarding
```

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

## ⚙️ System Workflows (9 Workflows)

---

### Workflow 1 — Lead Generation & AI Filtration

![Workflow 1 Architecture](./diagram_a.png)

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

![Workflow 1 Screenshot](./screenshots/01_lead_generation.png)

---

### Workflow 2 — CRM Integration & Email Sending

![Workflow 2 Architecture](./diagrams/diagram_b.png)

**What it does:**
- Detects when a human marks an email as approved in Google Sheets
- Creates a new lead record inside Notion CRM automatically
- Sends the approved email via Gmail
- Updates the CRM status to "Email Sent"
- Logs the CRM ID back into Google Sheets for full tracking

![Workflow 2 Screenshot](./screenshots/02_crm_integration.png)

---

### Workflow 3 — Automated Email Reply Handler

![Workflow 3 Architecture](./diagrams/diagram_c.png)

**What it does:**
- Monitors Gmail inbox for replies from leads
- AI (using OpenRouter) analyzes the reply to detect intent
- If the lead is **interested** → updates Notion status and sends a calendar booking link automatically
- If **not interested** → moves the lead to "Drop" status in Notion
- Sends a Telegram notification to the team with the reply summary
- Creates a follow-up task in Notion for the team

![Workflow 3 Screenshot](./screenshots/03_email_reply.png)

---

### Workflow 4 — Automated Follow-Up System

![Workflow 4 Architecture](./diagrams/diagram_d.png)

**What it does:**
- Triggered by Notion when a lead's "Next Communication" date arrives
- Loops through all leads that need follow-up
- Checks the current CRM status of each lead
- If status is "Email Sent" → sends a follow-up email automatically
- If status is "Meeting Sent" → notifies the team to follow up personally
- If status is "Follow-Up Sent" → marks the lead as dropped in Notion

![Workflow 4 Screenshot](./screenshots/04_followup.png)

---

### Workflow 5 — Meeting Booking Handler

![Workflow 5 Architecture](./diagrams/diagram_e.png)

**What it does:**
- Triggered when a new event is created in Google Calendar
- Checks if the meeting email matches an existing lead in Notion CRM
- If matched → updates Notion status to "Booked Meeting"
- Sends a Telegram notification to the team with meeting details
- Reminds the team to fill in the meeting outcome form afterward

![Workflow 5 Screenshot](./screenshots/05_meeting_handling.png)

---

### Workflow 6 — AI Proposal Generator

![Workflow 6 Architecture](./diagrams/diagram_f.png)

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

![Workflow 6 Screenshot](./screenshots/06_proposal_automation.png)

---

### Workflow 7 — Contract Automation

![Workflow 7 Architecture](./diagrams/diagram_g.png)

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

![Workflow 7 Screenshot](./screenshots/07_contract.png)

---

### Workflow 8 — Client Onboarding Automation

![Workflow 8 Architecture](./diagrams/diagram_h.png)

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

![Workflow 8 Screenshot](./screenshots/08_onboarding.png)

---

### Workflow 9 — Telegram Orchestrator Agent

![Workflow 9 Architecture](./diagrams/diagram_m.png)

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

![Workflow 9 Screenshot](./screenshots/09_telegram_orchestrator.png)

---

## 🛠️ Tech Stack

| Category | Tools |
|----------|-------|
| **Automation** | n8n (Self-Hosted) |
| **AI Models** | Gemini, Ollama, OpenRouter, GPT |
| **Lead Sourcing** | Apify |
| **CRM** | Notion API |
| **Communication** | Gmail API, Telegram API |
| **Storage** | Google Drive API, Google Sheets API |
| **Infrastructure** | Docker, VPS, SSL, Nginx |

---

## 📊 Key Results

- ✅ Eliminated **15–20 hours/week** of manual sales work
- ✅ **Zero leads lost** due to automated CRM status tracking
- ✅ Proposal generation reduced from **hours → minutes**
- ✅ Full sales funnel runs **without human intervention** except at decision points
- ✅ **Cost-efficient** lead sourcing via Apify ($5/month free credits)
- ✅ Complete audit trail of every lead stored in Notion CRM

---

## 📁 Repository Structure

```
📦 ai-sales-automation-n8n
 ┣ 📂 screenshots/               ← n8n workflow screenshots
 ┃ ┣ 01_lead_generation.png
 ┃ ┣ 02_crm_integration.png
 ┃ ┣ 03_email_reply.png
 ┃ ┣ 04_followup.png
 ┃ ┣ 05_meeting_handling.png
 ┃ ┣ 06_proposal_automation.png
 ┃ ┣ 07_contract.png
 ┃ ┣ 08_onboarding.png
 ┃ ┗ 09_telegram_orchestrator.png
 ┣ 📂 diagrams/                  ← detailed architecture diagrams
 ┃ ┣ diagram_a.png
 ┃ ┣ diagram_b.png
 ┃ ┣ diagram_c.png
 ┃ ┣ diagram_d.png
 ┃ ┣ diagram_e.png
 ┃ ┣ diagram_f.png
 ┃ ┣ diagram_g.png
 ┃ ┣ diagram_h.png
 ┃ ┗ diagram_m.png
 ┣ 📄 architecture_overview.png  ← full system overview diagram
 ┗ 📄 README.md
```

---

## 🔐 Note on Source Code

The full workflow logic, AI prompts, and automation scripts are kept private for confidentiality. This repository documents the system architecture, design decisions, and outcomes only.

---

## 📬 Contact

Interested in a similar system for your business?

- 📧 fares.m.elmetwaly@gmail.com
- 🔗 [LinkedIn]([https://www.linkedin.com/in/fares-maaty/])
- 💼 [Upwork Profile](https://upwork.com/your-profile)
