# 🤖 End-to-End AI Sales Automation Engine

## Overview
A full-cycle sales automation system built with n8n that handles 
the entire sales funnel — from lead generation to client onboarding — 
while keeping humans in the loop for critical decisions.

## Problem Solved
Sales teams waste 15-20 hours/week on manual tasks:
- Researching leads one by one
- Writing personalized emails manually  
- Tracking follow-ups in spreadsheets
- Preparing proposals from scratch

## Solution Architecture
![Architecture](./architecture.png)

The system is divided into 5 automated stages:

**Stage 1 — Lead Generation & Filtration**
- Pulls leads from Google Drive
- AI agent visits each company website
- Generates personalized cold emails using Gemini

**Stage 2 — Human-in-the-Loop Email Sending**
- Human reviews & approves emails before sending
- Automatically adds approved leads to Notion CRM

**Stage 3 — Email Reply Handling**
- AI detects if reply is interested or not
- Interested? → Books a calendar meeting automatically
- Not interested? → Moves to follow-up sequence

**Stage 4 — Meeting & Proposal Automation**
- After meeting form is filled → AI writes proposal
- Proposal sent to team for review via Telegram

**Stage 5 — Contract & Onboarding**
- Contract auto-generated from approved proposal
- Onboarding folder created automatically in Drive

## Tech Stack
- **Automation:** n8n (self-hosted)
- **AI Models:** Gemini, Ollama, OpenRouter
- **CRM:** Notion API
- **Communication:** Gmail API, Telegram API
- **Infrastructure:** Docker, VPS, SSL

## Key Results
- Eliminated ~15-20 hours/week of manual sales work
- Zero leads lost due to automated CRM tracking
- Proposal generation time reduced from hours to minutes

## Screenshots
![Workflow Overview](./screenshots/overview.png)
