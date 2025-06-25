# OpenAI Prompts and Payloads for Zapier Webhooks

## 1. Support Type

**Prompt:**

Is this ticket about technical support or account & billing?
Ticket: {{Ticket Description}}
Respond with 'Technical Support' or 'Account & Billing Support'.


**Payload:**
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

---

## 2. Priority

**Prompt:**

Review the following ticket and set the priority (low, normal, high, urgent) based on this playbook: https://github.com/dc-larsen/support-playbooks/blob/main/Ticket%20Priority/README.md
Ticket: {{Ticket Description}}
Respond with only the priority word.


**Payload:**  
*(see README for structure, just change the prompt)*

---

## 3. Broad Issue Detection

**Prompt:**

Does this ticket describe a problem that could affect many users (e.g., website down, major feature broken)?
Ticket: {{Ticket Description}}
Respond 'Yes' or 'No'.


---

## 4. Summary

**Prompt:**

Summarize this ticket for a support agent in a casual, concise way:
{{Ticket Description}}


---

## 5. Category

**Prompt:**

Given the following ticket description:
"""
{{Ticket Description}}
"""
And these possible categories:
{{List of Categories from Ticket Definitions Table}}
Pick the best matching category and return only the category name.


---

## 6. Knowledge Base Search

**Zendesk API Endpoint:**

GET https://{{subdomain}}.zendesk.com/api/v2/help_center/articles/search.json?query={{Ticket Subject}} {{Ticket Description}}


---

*Replace curly braces with Zapier variables as needed.*

.gitignore
# Ignore API keys and environment files
.env
*.env
