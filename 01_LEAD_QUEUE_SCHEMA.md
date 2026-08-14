# Notion Lead Queue — schema

Create one private Notion database called **Sunnyworks Outbound Queue**.

This is not public and is not a CRM for customers. It exists only to control outreach safely.

| Property | Type | Purpose |
|---|---|---|
| Company | Title | Agency name |
| Website | URL | Public company website |
| Company description | Text | Public company-level description only |
| Contact name | Text | Kept outside AI prompts |
| Work email | Email | Kept outside AI prompts |
| Role | Text | Founder, owner, or client-service lead |
| Fit score | Number | 0–10, created from company-level information |
| Fit reason | Text | Why the company may fit |
| Public observation | Text | One factual, verifiable public observation |
| Draft subject | Text | AI-generated, reviewed by Sunny |
| Draft email | Text | AI-generated, reviewed by Sunny |
| Status | Select | `New`, `Draft ready`, `Approved`, `Sent`, `Interested`, `Later`, `Not a fit`, `Do not contact`, `Bounced` |
| First sent | Date | Date of first email |
| Follow-up due | Date | Five business days after first send |
| Follow-up sent | Checkbox | Stops repeat messages |
| Do-not-contact reason | Text | Unsubscribe, no, bounce, etc. |
| Source | Select | `Apollo`, `Manual`, `Referral` |
| Notes | Text | Human-only notes; never add confidential information |

## Required rules

- A record marked `Do not contact` is never eligible for any workflow.
- A record marked `Bounced` is never eligible for any workflow.
- One company gets one contact and one sequence only.
- The AI receives `Company`, `Website`, and `Company description`; it does **not** receive `Contact name` or `Work email`.
- The status must be `Approved` before an external email can be sent.
