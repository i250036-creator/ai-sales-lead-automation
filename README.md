# 🤖 AI Sales Lead Automation System

> Fully automated lead qualification & CRM pipeline powered by **GPT-4o** and **n8n**

<img width="1894" height="892" alt="image" src="https://github.com/user-attachments/assets/fde093ed-7562-4f83-9c7b-e50cae1640b0" />


---

## 📌 Overview

This project is a **production-ready AI automation workflow** built with n8n that handles the entire sales lead lifecycle — from form submission to CRM entry — without any manual intervention.

When a lead submits a form, the system:
1. Validates the incoming data
2. Uses **GPT-4o** to score and qualify the lead (Hot / Warm / Cold)
3. Creates a contact in **HubSpot CRM** with AI-generated notes
4. Sends a **personalized welcome email** via Gmail
5. Posts a real-time **Slack alert** to the sales team
6. Logs everything to **Google Sheets** for reporting
7. Returns a structured JSON response to the form

---

## 🏗️ Workflow Architecture

```
[Webhook Trigger]  ──→  [Validate Input]
                              │
                    ┌─────────┴──────────┐
                 Valid                Invalid
                    │                   │
         [GPT-4o Qualify]        [Respond 400]
                    │
          [Format AI Output]
                    │
         ┌──────────┼───────────┬──────────────┐
         │          │           │              │
   [HubSpot]   [Gmail]      [Slack]    [Google Sheets]
         │          │           │              │
         └──────────┴───────────┴──────────────┘
                              │
                       [Respond 200]
```

---

## ⚙️ Tech Stack

| Tool | Purpose |
|------|---------|
| **n8n** | Workflow automation engine |
| **GPT-4o** | AI lead qualification & personalization |
| **HubSpot** | CRM contact management |
| **Gmail (OAuth2)** | Automated welcome emails |
| **Slack (OAuth2)** | Real-time sales team alerts |
| **Google Sheets** | Lead reporting & logging |
| **Webhook** | Form data ingestion |

---

## 🧠 AI Lead Qualification Logic

GPT-4o analyzes each lead and returns a structured JSON with:

```json
{
  "qualification": "Hot",
  "score": 9,
  "reason": "High budget, urgent need, and decision maker role.",
  "recommended_action": "call immediately",
  "personalized_email_opening": "We noticed you're looking to automate your entire pipeline urgently — that's exactly what we do best at Beyondles."
}
```

**Scoring Criteria:**

| Tier | Criteria | Action |
|------|----------|--------|
| 🔥 **Hot** | High budget + urgent need + decision maker | Call immediately |
| 🌡️ **Warm** | Some interest, not yet ready to buy | Nurture sequence |
| ❄️ **Cold** | Low budget or not a decision maker | Add to newsletter |

---

## 📋 Node Breakdown

### Node 1 — Webhook Trigger
Listens for `POST /lead-capture` requests. Accepts JSON payload with lead details.

### Node 2 — Validate Input (IF Node)
Checks that `name` and `email` fields are present. Returns `400` error if missing.

### Node 3 — GPT-4o Lead Qualify
Sends lead data to OpenAI with a strict JSON-only prompt. Temperature set to `0.2` for consistent, structured output.

### Node 4 — Format Output (Code Node)
Parses the AI response, handles JSON errors, and merges lead data with qualification results into a clean unified object.

### Node 5 — HubSpot Create Contact
Creates or updates the contact in HubSpot CRM with qualification status and AI-generated reason as notes.

### Node 6 — Gmail Welcome Email
Sends a personalized welcome email using the AI-generated opening line tailored to the lead's message.

### Node 7 — Slack Sales Alert
Posts a formatted alert to `#sales-leads` channel with lead details, score, and recommended action.

### Node 8 — Google Sheets Log
Appends a new row to the master leads sheet: Name, Email, Company, Budget, Qualification, Score, Action, Date.

### Node 9 — Respond to Webhook
Returns `200 OK` with qualification results, or `400` with error details.

---

## 🧪 Testing

Use this sample payload with **Postman** or **curl**:

```bash
curl -X POST https://your-n8n.com/webhook-test/lead-capture \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Ahmed Khan",
    "email": "ahmed@acmecorp.com",
    "company": "Acme Corp",
    "budget": "$5000",
    "message": "We need to automate our entire sales pipeline urgently. Looking to start immediately.",
    "source": "Website Contact Form"
  }'
```

**Expected Response:**
```json
{
  "status": "success",
  "message": "Lead received and processed",
  "qualification": "Hot",
  "score": 9
}
```

---

## 🔐 Credentials Required

| Service | How to Get |
|---------|-----------|
| OpenAI API Key | [platform.openai.com](https://platform.openai.com) |
| HubSpot OAuth | [app.hubspot.com](https://app.hubspot.com) — Free account works |
| Gmail OAuth2 | Google Cloud Console → Create OAuth credentials |
| Slack OAuth2 | [api.slack.com](https://api.slack.com) → Create App |
| Google Sheets OAuth2 | Same Google Cloud project as Gmail |

---

## 🚀 Setup Instructions

1. **Clone this repo** and import the `workflow.json` into your n8n instance
2. Set up all credentials listed above in n8n's credential manager
3. Update the **Google Sheets ID** in Node 8 with your sheet's ID
4. Update the **Slack channel** in Node 7 to match your workspace
5. Activate the workflow
6. Test using the Postman payload above

---

## 📊 What This Demonstrates

- ✅ Multi-service API integration (5 platforms in one workflow)
- ✅ AI-powered decision making inside automation pipelines
- ✅ Structured prompt engineering for reliable JSON output
- ✅ Error handling & input validation
- ✅ Real-world sales automation use case
- ✅ Clean, maintainable n8n workflow design

---

## 👨‍💻 Built By

**Abdullah** — AI Automation Specialist  
BS Artificial Intelligence — FAST NUCES Islamabad  
Stack: n8n · GPT-4o · LangGraph · RAG · Python · APIs  

🔗 [Upwork Profile](#) · [LinkedIn](#) · [Portfolio](#)

---

## 📁 Repository Structure

```
├── workflow.json          # n8n workflow export (import this)
├── README.md              # You are here
└── assets/
    └── workflow-screenshot.png   # Workflow visual
```

---

> 💡 **Portfolio Note:** This project is part of a series of 10-15 AI automation systems built to demonstrate real-world n8n + AI integration skills for freelance clients.
