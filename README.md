# Approval-Only Outbound Engine

**Status: design specification — not connected to any account, not deployed, no workflow has run.**

This is a system design for a safety-gated outbound lead-generation workflow, built as a spec before any implementation. I'm sharing it as a work sample: it shows how I think about workflow architecture, data handling, and AI-safety guardrails when designing an automation, not just when auditing one after the fact.

## What it's designed to do

Find suitable small agencies, draft truthful outreach emails, keep a lead queue, and sort replies — without cold calls, without LinkedIn spam, and without an AI model ever seeing a prospect's name or email address.

## What it deliberately does not do

- Does not connect to any account.
- Does not send any email automatically.
- Does not put prospect names or email addresses into AI prompts.
- Does not use a voice bot or robocalls.

## Safety design: draft-only by default

The first version caps lead drafts at five at a time. Every draft requires human approval in Notion before an email can leave Gmail — nothing sends automatically. Only after a run of accurate, respectful approved drafts would a very small automatic-send limit be separately authorized.

## The three workflows

1. **Find and draft** — Apollo finds potential agencies; AI scores only company-level public information and writes a draft. Ends at `Draft ready`; cannot reach Gmail.
2. **Send approved only** — only Notion records explicitly marked `Approved` are eligible to send, capped at 3/weekday for the first two weeks.
3. **Sort replies and protect opt-outs** — classifies replies, immediately suppresses anyone who says no or unsubscribes, and never auto-sends a reply.

## Files

| File | What it is |
|---|---|
| [`01_LEAD_QUEUE_SCHEMA.md`](./01_LEAD_QUEUE_SCHEMA.md) | The Notion database schema — every field, its purpose, and the rules that gate what's eligible to send |
| [`02_N8N_WORKFLOWS.md`](./02_N8N_WORKFLOWS.md) | The exact three-workflow build specification, node by node |
| [`03_AI_PROMPTS.md`](./03_AI_PROMPTS.md) | The prompts used for fit-scoring and reply classification, including explicit no-fabrication and untrusted-input rules |
| [`04_CONNECTION_CHECKLIST.md`](./04_CONNECTION_CHECKLIST.md) | The scoped, minimum-privilege account connections this would require if built |

## Why design it this way

Most outbound tooling optimizes for volume. This spec optimizes for the opposite: the smallest blast radius if something goes wrong, a human in the loop before anything external happens, and a paper trail (the Notion queue) for every decision the system makes. That tradeoff — slower, smaller, reviewable — is a deliberate choice, not a limitation of the tooling.

---

*Design work by [Sunny Anwarali](https://linkedin.com/in/sanamanwarali), Sunnyworks.AI. Part of the same practice behind the [AI Governance Starter Kit](https://github.com/sunnyworks-ai/ai-governance-starter-kit) and the [n8n case study](https://sunnyworks.ai/n8n-case-study.html).*


---

**Want something like this built for your business?** → [sunnyworks.ai/contact](https://sunnyworks.ai/contact)
