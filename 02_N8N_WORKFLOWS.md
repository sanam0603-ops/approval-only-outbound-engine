# Hosted n8n workflows — build specification

Build these as three separate workflows. Leave every workflow **inactive** until explicitly approved for testing.

## Workflow 1 — Find and draft

**Name:** `Sunnyworks 01 — Find and Draft`

**Purpose:** Find up to five potential agencies and prepare drafts. It never sends an external email.

```text
Manual Trigger
  → Apollo People Search (HTTP Request)
  → Split Out / Limit (maximum 5)
  → Set (keep company-level fields only)
  → OpenAI (fit score + public observation + email draft)
  → IF (fit score is 7 or higher)
  → Notion Create Page (Status = Draft ready)
```

### Apollo search rules

- Use the official People Search endpoint.
- Search for agency decision-makers, such as founder, owner, principal, managing director, client services director, or operations director.
- Filter for small companies in creative, digital, marketing, consulting, or AI-supported services.
- Start with `per_page=5`.
- Do not automatically enrich phone numbers.
- Do not use personal emails.

### AI input rule

Pass only the company name, company website, company description, visible services, employee range, and a generic role title. Do **not** pass the prospect's name, email address, or private notes to the model.

### Approval gate

Workflow 1 ends at `Draft ready`. It cannot reach Gmail.

## Workflow 2 — Send approved only

**Name:** `Sunnyworks 02 — Send Approved Only`

**Purpose:** Send only drafts explicitly approved in Notion.

```text
Schedule Trigger (weekday morning)
  → Notion query (Status = Approved; maximum 3)
  → Gmail Send Message
  → Notion Update Page (Status = Sent; First sent = today; Follow-up due = +5 business days)
```

### Required limits

- Maximum 3 messages per weekday during the first two weeks.
- No follow-up until an initial email has been sent.
- No email if `Do not contact`, `Bounced`, or `Not a fit`.
- The email must contain Sunnyworks identity, a permitted postal address, and a clear "reply no" opt-out instruction.

### Activation rule

Do not activate this workflow until separately, explicitly approved.

## Workflow 3 — Reply and suppression handler

**Name:** `Sunnyworks 03 — Replies and Suppressions`

**Purpose:** Protect people who say no and make replies easier to handle. It does not send replies automatically.

```text
Gmail Trigger or scheduled Gmail search
  → Match email thread to Notion record
  → OpenAI classify: Interested / Later / No / Unsubscribe / Unclear
  → Notion Update Page
  → IF Unsubscribe or No → Status = Do not contact
  → IF Interested → Status = Interested
```

### Rules

- An unsubscribe or "no" immediately ends all follow-up.
- "Interested" creates a Notion status only; a human reviews before replying.
- Do not feed attachments, contracts, customer information, or private client records into AI.
- Do not use n8n community nodes or unreviewed GitHub workflows for this.

## Testing order

1. Run Workflow 1 with one manually entered test company and verify the Notion record.
2. Run Workflow 2 with the founder's own email address as the only approved test.
3. Run Workflow 3 against that test reply.
4. Review the first ten drafts before any outreach begins.
