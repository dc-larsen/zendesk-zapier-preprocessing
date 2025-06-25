# Zendesk Ticket Preprocessing with Zapier & OpenAI

This project provides a step-by-step guide for automating Zendesk ticket triage using Zapier, OpenAI (GPT-4), and Zendesk’s API.  
No custom code is required—just Zapier’s built-in actions and Webhooks.

---

## Features

- Classifies support type (Technical or Account & Billing)
- Sets ticket priority using AI and a playbook
- Detects potential broad issues and tags tickets
- Summarizes the ticket in a casual, concise way
- Assigns a category based on your Ticket Definitions (from Zapier Tables)
- Searches Zendesk Help Center for relevant articles
- Posts a single internal comment with all findings

---

## Prerequisites

- **Zapier account**
- **Zendesk account** (with API access)
- **OpenAI account** (with API key for GPT-4)
- **Ticket Definitions** in a Zapier Table

---

## Setup Instructions

### 1. **Create a New Zap**

#### **Step 1: Trigger**
- **App:** Zendesk
- **Event:** New Ticket
- **Configure:** Connect your Zendesk account and select the trigger options you want.

#### **Step 2: Webhooks by Zapier — OpenAI (Support Type)**
- **Action:** Webhooks by Zapier
- **Event:** Custom Request (POST)
- **URL:** `https://api.openai.com/v1/chat/completions`
- **Headers:**
  - `Authorization: Bearer YOUR_OPENAI_API_KEY`
  - `Content-Type: application/json`
- **Data (JSON):**
  ```json
  {
    "model": "gpt-4",
    "messages": [
      {"role": "user", "content": "Is this ticket about technical support or account & billing?\n\nTicket: {{Ticket Description}}\n\nRespond with 'Technical Support' or 'Account & Billing Support'."}
    ],
    "max_tokens": 10,
    "temperature": 0.3
  }
  ```
- **Map** the ticket description from the trigger.

#### **Step 3: Webhooks by Zapier — OpenAI (Priority)**
- **Repeat** as above, but use the priority prompt (see `prompts.md`).

#### **Step 4: Webhooks by Zapier — OpenAI (Broad Issue Detection)**
- **Repeat** as above, but use the broad issue prompt.

#### **Step 5: Webhooks by Zapier — OpenAI (Summary)**
- **Repeat** as above, but use the summary prompt.

#### **Step 6: Webhooks by Zapier — OpenAI (Category)**
- **Repeat** as above, but use the category prompt.  
  - **Tip:** Use a Formatter step to join your Ticket Definitions table into a comma-separated list for the prompt.

#### **Step 7: Zendesk Help Center Search**
- **Action:** Webhooks by Zapier (GET)
- **URL:** `https://{{subdomain}}.zendesk.com/api/v2/help_center/articles/search.json?query={{Ticket Subject}} {{Ticket Description}}`
- **Auth:** Basic Auth (`email/token` and API token)

#### **Step 8: Update Zendesk Ticket**
- **App:** Zendesk
- **Event:** Update Ticket
- **Map** the outputs from previous steps to:
  - Priority
  - Category (custom field)
  - Tags (add `potential_broad_issue` if needed)
  - Internal comment (combine summary, support type, priority, category, broad issue, and KB article)

---

## Example Prompts and Payloads

See [`prompts.md`](./prompts.md) for all OpenAI prompts and example JSON payloads.

---

## Example Ticket Definitions Table

| Category                | Description                                  |
|-------------------------|----------------------------------------------|
| Account & Login Issues  | Problems with login, password, or accounts   |
| Subscription & Billing  | Questions about billing, plans, or invoices  |
| Technical Bug           | Errors, crashes, or technical malfunctions   |
| Feature Request         | Suggestions for new features or improvements |
| Sponsorship Application | Requests for account sponsorship             |

---

## Notes

- All API keys should be stored securely in Zapier’s connection or environment.
- The internal comment is casual and concise, and includes all findings in one message.
- For custom fields (like category), use your Zendesk field ID.
- No custom code is required—just Zapier’s built-in actions and Webhooks.

---

## License

MIT License
