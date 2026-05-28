# MailBuddy PRD

## 1. Product Summary

MailBuddy is an AI-native inbox operating system that reduces cognitive overload by transforming Gmail messages into structured, actionable, semantically organized workflows.

MailBuddy is not a Gmail wrapper. Gmail stores messages. MailBuddy stores the meaning layer: tasks, updates, semantic threads, financial vigilance items, opportunities, digest items, noise, institutional notices, and operational memory.

The core thesis is that modern email is no longer just communication. It is a messy combination of tasks, receipts, updates, alerts, opportunities, newsletters, appointments, school/health/government notices, and long-term reference memory. Traditional inboxes organize messages. MailBuddy organizes intent.

## 2. Problem

Users experience the inbox as a high-volume stream where everything competes for attention. Unread counts flatten high-stakes notices, refunds, tasks, newsletters, receipts, and promotions into the same visual priority.

The main problems are:

- Important obligations are buried in low-value email.
- Users must manually infer what needs action.
- Delivery, refund, appointment, travel, and event states are scattered across messages.
- Search requires users to remember exact keywords instead of meaning.
- Newsletters and learning content create backlog guilt instead of useful digests.
- Notifications interrupt users before messages are understood.
- AI summaries alone are not trustworthy enough for calculations, state, or risky actions.

## 3. Target Users

The portfolio/demo MVP is designed for a single Gmail account owned by the project creator. The product direction still assumes broader future users.

Primary future users:

- Busy professionals managing async work, logistics, and personal operations.
- Household managers tracking deliveries, appointments, bills, school communication, and errands.
- AI-native productivity users who want semantic workflows instead of inbox hygiene.
- People with high email volume who need help knowing what matters today.

Non-target for MVP:

- Shared team inboxes.
- Enterprise compliance deployments.
- Multi-provider email support.
- Fully autonomous email agents.

## 4. Product Vision

MailBuddy should help users feel on top of their inbox instead of buried under it.

The ideal experience:

- The home screen answers "What needs my attention?"
- Tasks are extracted from emails with due dates, urgency, effort, and source links.
- Updates are presented as lifecycles, not scattered messages.
- Conversations are grouped by meaning and summarized by state.
- The assistant answers operational questions using structured data and deterministic tools.
- Voice provides a hands-free inbox briefing and quick review path.
- Noise is suppressed without hiding important institutional information.

## 5. Product Principles

- Structure before reasoning.
- Deterministic truth before AI phrasing.
- Semantic objects over raw email retrieval.
- Constrained tools over open-ended agents.
- Trustworthiness over maximum automation.
- Human approval for risky actions.
- AI should reduce anxiety, not create more.
- MailBuddy earns permission to interrupt.

LLMs should handle classification, summarization, interpretation, planning, explanation, and natural-language interaction.

Deterministic tools should handle retrieval, filtering, calculations, state transitions, totals, timelines, and notification policy enforcement.

LLMs should not be trusted for financial calculations, deterministic state, raw retrieval truth, or sending/deleting/unsubscribing without explicit approval.

## 6. Core Semantic Objects

MailBuddy should convert emails into semantic objects:

- `task`: obligation, request, approval, deadline, follow-up, form, review.
- `update`: delivery, refund, replacement, appointment, travel, event, lifecycle status.
- `semantic_thread`: related conversation grouped by meaning, not only Gmail thread ID.
- `financial_vigilance_item`: receipt, subscription, bank alert, invoice, payment issue, unusual activity.
- `opportunity`: recruiting, networking, event, grant, collaboration, professional opening.
- `digest_item`: newsletter, learning content, industry update, saved reading.
- `noise_item`: low-action marketing, repeated low-value notification, suppressible content.
- `institutional_item`: health, government, legal, school, immigration, insurance, high-stakes notice.
- `reference_item`: useful long-term memory that may not require action today.

Every object should preserve provenance to its source Gmail message or thread.

## 7. Feature Set

### Suggested To-Do List

AI extracts tasks from emails and presents them as editable action items.

Required fields:

- title
- due date when available
- urgency
- estimated effort
- status
- confidence
- source email link
- evidence snippet

Actions:

- complete
- snooze
- dismiss
- edit
- open source

### Updates Dashboard

Tracks operational lifecycles:

- deliveries
- refunds
- replacements
- appointments
- travel
- event updates
- reservations

The dashboard should show current state, timeline, latest change, and whether the user needs to act.

### Semantic Threads

Groups related conversations by meaning, not only Gmail thread ID.

Capabilities:

- summarize discussion state
- detect unresolved questions
- detect who is waiting on whom
- identify pending replies
- connect related messages across separate Gmail threads when appropriate

### AI Assistant

Natural-language interface over structured inbox intelligence.

Example questions:

- What needs my attention today?
- Did I get refunded?
- Which conversations need replies?
- What important emails did I miss this week?
- How much did I spend at Whole Foods this month?
- Find my dental receipt.

The assistant should call deterministic tools for retrieval, filtering, aggregation, and timelines, then use the LLM to explain results.

### Voice Assistant

Voice-first inbox interaction using an OpenAI voice pipeline.

Demo flow:

- Browser records audio.
- Next.js sends audio to FastAPI.
- FastAPI transcribes audio with OpenAI.
- Assistant planner interprets the request.
- Deterministic tools query Postgres.
- LLM finalizes the response.
- FastAPI returns text and optional generated audio.
- Frontend displays transcript and plays the response.

### Insights Widgets

Possible widgets:

- spending summaries
- subscription trends
- response delays
- busiest senders
- opportunity tracking
- cognitive overload sources
- muted/noise volume

### Digest Mode

Summarizes newsletters and learning content by topic. Initial digest groups may include AI, product management, career, local events, personal learning, and industry updates.

### Ignore / Mute System

Allows users to reduce low-value email pressure.

Capabilities:

- mute senders
- mute merchants
- hide low-value categories
- learn from dismiss/mute behavior
- keep high-stakes institutional items visible even if sender patterns are noisy

### Unsubscribe Assistant

Detects repeated low-value senders and suggests unsubscribe actions. The MVP should suggest only. Production unsubscribe actions require explicit approval.

### Smart Search / Operational Memory

Search by meaning, not just keywords.

Examples:

- Find my dental receipt.
- Show my immigration emails.
- When did I buy this?
- Find that refund email.
- Show emails about the school form.

### Connected Detail Fetching

Future scope for emails that do not contain full details, such as grocery orders or event sites.

Rules:

- OAuth preferred.
- Browser-assisted flows only with explicit user approval.
- Never store raw credentials.
- Never log into external accounts autonomously.

## 8. MVP Scope

The MVP is a portfolio/demo app, optimized to show the product thesis on real Gmail-derived patterns while protecting sensitive information.

Included:

- Gmail-only integration.
- Single Gmail account owned by the creator.
- Web and mobile-first app.
- Next.js frontend.
- Python FastAPI backend.
- Postgres with pgvector.
- OpenAI text and voice pipeline.
- Initial Gmail sync of recent mail.
- Manual sync.
- Periodic background sync while the backend is active.
- Email normalization.
- Semantic classification.
- Sanitized semantic object storage.
- Task extraction.
- Updates dashboard.
- Basic semantic threads.
- Assistant over semantic objects.
- Voice assistant demo.
- Demo-safe redaction and alteration.
- Evaluation fixtures and tests.

Not included:

- Multi-user production onboarding.
- Multi-provider email support.
- Full push notification delivery.
- Autonomous send/delete/unsubscribe.
- External account login.
- Raw private mailbox replication.
- Enterprise compliance.

## 9. Demo Data Policy

MailBuddy MVP is demo-mode-only. It connects to the creator's Gmail, processes real inbox patterns, and stores/displays altered, demo-safe semantic data.

Policy:

- Store sanitized semantic objects, not raw private inbox data.
- Raw email content may be held temporarily during ingestion and processing, then discarded.
- Keep non-sensitive merchant names when useful for realism, such as Amazon, Target, Whole Foods, Uber, DoorDash.
- Fabricate realistic names for financial, health, legal, immigration, government, insurance, and other sensitive institutions.
- Alter people names, email addresses, phone numbers, physical addresses, account numbers, order IDs, confirmation numbers, and exact sensitive notes.
- Preserve the product meaning: a dental appointment remains a dental appointment; a refund remains a refund; a bank alert remains a financial vigilance item.

## 10. Gmail Fetch Strategy

MVP defaults:

- Initial sync: last 90 days or last 1,000 messages, whichever comes first.
- Manual sync: user can trigger `Sync now`.
- Background sync: every 30 minutes while backend is active.
- Incremental sync: use Gmail message IDs and Gmail `historyId` when available to avoid reprocessing.
- Processing priority: high-stakes categories, tasks, updates, and financial vigilance before digests/noise.

Future production:

- Gmail Pub/Sub push notifications.
- Incremental sync using Gmail history API.
- User-controlled sync windows.
- Backfill older email only when requested.
- Category-specific processing priorities.

## 11. Notification Strategy

Production should ingest immediately but notify selectively.

New email flow:

1. Gmail push notification arrives.
2. Backend fetches changed messages.
3. Messages are normalized.
4. Semantic objects are extracted.
5. Importance and urgency are scored.
6. Notification decision engine chooses interrupt, queue, digest, or suppress.

Notification tiers:

- `critical`: notify immediately.
- `timely`: notify during an allowed notification window.
- `digest`: include in daily or weekly summary.
- `silent`: store and organize without interruption.

Immediate notifications should be limited to urgent tasks, appointment changes, travel changes, delivery problems, refund/payment problems, high-stakes institutional notices, and conversations where someone is waiting on the user.

For the MVP, production push notifications are documented but not required. The demo can show an in-app notification center, important update badge, and simulated notification examples.

## 12. Success Metrics

Product metrics:

- Fewer important emails missed.
- Less time spent scanning raw inbox.
- High user trust in extracted tasks and updates.
- High assistant answer correctness.
- Low redaction failure rate.
- Reduction in notification noise.
- Repeat use of dashboard and voice briefing.

AI/evaluation metrics:

- Task extraction precision and recall.
- Update lifecycle field accuracy.
- Classification accuracy by semantic category.
- Assistant groundedness.
- Redaction safety pass rate.
- Deterministic calculation correctness.

Portfolio metrics:

- Clear demo walkthrough.
- Credible architecture.
- Strong visual artifacts.
- GitHub repository with docs, tests, and decision log.
- Screenshots/GIFs that are demo-safe.

## 13. Risks

- Incorrect classification causes missed important items.
- Assistant fabricates facts not present in semantic objects.
- Redaction misses sensitive data.
- Gmail API limits or OAuth friction slow the demo.
- Voice pipeline adds latency or complexity.
- Storing too much raw email creates privacy risk.
- Over-notification recreates inbox anxiety.
- Agentic development branches conflict without ownership boundaries.

## 14. Differentiation

MailBuddy is different from:

- A Gmail wrapper: it does not center raw message browsing.
- Inbox zero tools: it does not treat processing email as the user's job.
- AI summarizers: it creates durable semantic objects, not just prose.
- Search tools: it maintains operational memory and lifecycle state.
- Autonomous agents: it uses constrained tools and approval gates.

The differentiated claim:

MailBuddy is a semantic operating layer for inbox intent.

## 15. Open Questions

- Which exact screens should be included in the first recorded demo?
- Should the MVP include generated audio responses or browser playback of text responses first?
- Which categories should be most visually prominent in the first dashboard?
- What redaction strictness is acceptable for public portfolio screenshots?
- Should production architecture target mobile push later through a wrapper app or web push first?
