# 🤖 End-to-End AI Sales Automation Engine

> A full-cycle intelligent sales system built with n8n that automates the entire sales funnel — from lead generation to client onboarding — while keeping humans in the loop for critical decisions.

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

A fully automated, AI-powered sales pipeline that handles every stage of the funnel automatically — with humans only stepping in at key decision points.

---

## 🗺️ System Architecture

![System Architecture](./architecture.png)

The system runs across **9 automated workflows**, organized into 5 main stages:

```
Lead Generation → CRM Integration → Email Reply Handling
→ Meeting & Proposal → Contract & Onboarding
```

---

## ⚙️ System Workflows (9 Workflows)

---

### Workflow 1 — Lead Generation & AI Filtration

**What it does:**
- Pulls a leads file from Google Drive
- Extracts company name, email, and website from the file
- Filters out leads that are missing key data
- AI agent visits each company's website and reads its content
- Gemini AI summarizes the company and understands what they do
- Generates a hyper-personalized cold email draft for each lead
- Saves the drafted emails to Google Sheets for human review

![Workflow 1](./screenshots/01_lead_generation.png)

---

### Workflow 2 — CRM Integration & Email Sending (Human-in-the-Loop)

**What it does:**
- Human reviews the drafted emails in Google Sheets
- Once marked as approved, the workflow triggers automatically
- Creates a new record for each lead inside Notion CRM
- Sends the approved email via Gmail
- Updates the CRM status to "Email Sent"
- Logs the CRM ID back into Google Sheets for tracking

![Workflow 2](./screenshots/02_crm_integration.png)

---

### Workflow 3 — Automated Email Reply Handler

**What it does:**
- Monitors Gmail inbox for replies from leads
- AI (using OpenRouter) analyzes the reply to detect intent
- If the lead is **interested** → updates Notion status and sends a calendar booking link
- If **not interested** → moves the lead to "Drop" status in Notion
- Sends a Telegram notification to the team with the reply summary
- Creates a follow-up task in Notion for the team

![Workflow 3](./screenshots/03_email_reply.png)

---

### Workflow 4 — Automated Follow-Up System

**What it does:**
- Triggered by Notion when a lead's "Next Communication" date arrives
- Loops through all leads that need follow-up
- Checks the current CRM status of each lead
- If status is "Email Sent" → sends a follow-up email automatically
- If status is "Meeting Sent" → notifies the team to follow up personally
- If status is "Follow-Up Sent" → marks the lead as dropped in Notion

![Workflow 4](./screenshots/04_followup.png)

---

### Workflow 5 — Meeting Booking Handler

**What it does:**
- Triggered when a new event is created in Google Calendar
- Checks if the meeting email matches an existing lead in Notion CRM
- If matched → updates Notion status to "Booked Meeting"
- Sends a Telegram notification to the team with the meeting details
- Reminds the team to fill in the meeting outcome form afterward

![Workflow 5](./screenshots/05_meeting_handling.png)

---

### Workflow 6 — AI Proposal Generator

**What it does:**
- Triggered when the team fills in the post-meeting form
- Verifies the lead exists in Notion CRM
- AI analyzes the meeting form data to understand client needs
- Gemini AI writes a full professional proposal document
- Downloads the generated proposal file
- Creates a draft email with the proposal attached
- Updates Notion CRM status to "Proposal Ready"
- Stores the proposal file URL inside the CRM
- Sends a Telegram notification to the team that the proposal is ready for review

![Workflow 6](./screenshots/06_proposal_automation.png)

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
- Sends a Telegram notification to the team confirming contract is ready

![Workflow 7](./screenshots/07_contract.png)

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

![Workflow 8](./screenshots/08_onboarding.png)

---

### Workflow 9 — Telegram Orchestrator Agent

**What it does:**
- A central command interface via Telegram for the sales team
- AI analyzes any incoming message from the team
- Finds the relevant lead record in Notion CRM automatically
- Based on the current lead status, the AI takes the correct action:
  - **Proposal Sent** → reminds the team to update CRM before sending
  - **Proposal Ready** → prepares the email fields (To, Subject, Body)
  - **Contract Ready** → prepares the contract email fields
  - **Contract Sent** → confirms and moves to onboarding stage
- Updates CRM status automatically after each action
- Sends a confirmation message back to the team on Telegram

![Workflow 9](./screenshots/09_telegram_orchestrator.png)

---

## 🛠️ Tech Stack

| Category | Tools |
|----------|-------|
| **Automation** | n8n (Self-Hosted) |
| **AI Models** | Gemini, Ollama, OpenRouter, GPT |
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
- ✅ Complete audit trail of every lead stored in Notion CRM

---

## 🔐 Note on Source Code

The full workflow logic, AI prompts, and automation scripts are kept private for client confidentiality. This repository documents the system architecture and outcomes only.

---

## 📬 Contact

Interested in a similar system for your business?

- 📧 fares.amr@gmail.com
- 🔗 [LinkedIn](https://linkedin.com/in/your-profile)
- 💼 [Upwork Profile](https://upwork.com/your-profile)
