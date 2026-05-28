# MailBuddy V1 PRD

## 1. Product Summary

MailBuddy is a semantic intelligence layer for Gmail. It turns real inbox data into structured, source-backed objects such as tasks, updates, semantic threads, financial items, institutional notices, digests, noise, and reference memory.

MailBuddy is not a Gmail replacement. Gmail remains the source of truth for raw email. MailBuddy stores the meaning layer.

V1 is a single-user, demo-safe portfolio application built against the creator's real Gmail patterns.

## 2. Core Thesis

Modern email is not just communication. It is a stream of obligations, receipts, financial records, newsletters, account notices, appointments, logistics, and long-term reference material.

Traditional inboxes organize messages. MailBuddy organizes meaning.

The first product promise:

> MailBuddy tells you what actually needs attention, what can wait, and what evidence supports each conclusion.

## 3. Prior Real-Data Discovery

Before V1, MailBuddy was tested against a real Gmail sample after an initial synthetic-data prototype.

Key findings:

- Synthetic demo emails overrepresented complete, clean task and update data.
- Real Gmail contains much more noise, receipts, financial documents, account notices, newsletters, and reference material.
- Many merchant and order emails do not contain full item, refund, or replacement details in the email body.
- Important details are often behind external links.
- Gmail thread IDs alone are insufficient for grouping real-world workflows.
- The product needs durable semantic objects rather than UI-local derived state.
- Search and retrieval need to work over meaning, exact terms, dates, senders, and object categories.
- Demo safety and redaction must be part of the architecture, not a final polish step.

V1 semantic categories are based on this prior real Gmail discovery.

## 4. V1 Goals

V1 should prove four things:

- MailBuddy can ingest real Gmail data safely.
- MailBuddy can convert messy email into durable semantic objects.
- MailBuddy can separate attention-worthy items from noise.
- MailBuddy can answer questions using structured objects and source evidence.

## 5. Target User

V1 is for one user: the project creator.

Future target users:

- Busy professionals managing async work, logistics, and personal operations.
- Household managers tracking deliveries, appointments, bills, school communication, and errands.
- AI-native productivity users who want semantic workflows instead of inbox hygiene.
- People with high email volume who need help knowing what matters today.

Non-target for V1:

- Shared team inboxes.
- Enterprise compliance deployments.
- Multi-provider email support.
- Fully autonomous email agents.
- Public SaaS onboarding.

## 6. Product Principles

- Real Gmail patterns over synthetic demos.
- Semantic objects over raw email lists.
- Evidence before explanation.
- Deterministic tools for truth-sensitive work.
- LLMs interpret, classify, summarize, and explain.
- LLMs do not compute financial totals or invent state.
- Store demo-safe data, not raw private inbox content.
- When details are behind an external link, say so.
- Uncertainty is a product feature, not a failure.
- Human approval is required for risky actions.
- MailBuddy earns permission to interrupt.

## 7. Core User Experience

The first screen is **Attention Today**.

It should answer:

- What needs action?
- What changed?
- What money-related items should I notice?
- Who may be waiting on me?
- What is noise?
- What evidence supports each item?

Primary screens:

- Attention Today
- Tasks
- Updates
- Semantic Threads
- Financial
- Search / Memory
- Assistant
- Data Safety / Evaluation

## 8. Core Semantic Objects

MailBuddy stores semantic objects derived from Gmail.

### `source_email`

Represents a Gmail message with minimized and sanitized fields.

Fields:

- id
- Gmail message id
- Gmail thread id
- sender name/domain
- subject
- received timestamp
- labels
- snippet
- sanitized body excerpt, if allowed
- attachment metadata
- detected links
- raw content retention status

### `semantic_object`

Base object for all extracted meaning.

Fields:

- id
- object type
- title
- summary
- status
- priority
- confidence
- object date
- source refs
- evidence snippets
- uncertainty flags
- extracted entities
- embedding text
- embedding vector
- created at
- updated at

Object types:

- `task`
- `update`
- `semantic_thread`
- `financial_item`
- `institutional_item`
- `digest_item`
- `noise_item`
- `reference_item`

## 9. Source References

Every semantic object must preserve provenance.

A source ref includes:

- source email id
- source subject
- sender/domain
- timestamp
- evidence snippet
- confidence
- reason the source supports the object

The UI should let users inspect the source behind every task, update, financial item, semantic thread, search result, and assistant answer.

## 10. Gmail Ingestion

V1 supports Gmail readonly ingestion for a single account.

Included:

- Gmail OAuth readonly scope.
- Initial sync of recent mail.
- Local/private raw export option.
- Message metadata fetch.
- Plain text extraction where available.
- Detected links.
- Attachment filenames.
- Gmail message/thread IDs.
- Ingestion status.
- Manual refresh.

Not required in V1:

- Production Pub/Sub.
- Multi-account auth.
- Sending email.
- Deleting email.
- Unsubscribe execution.
- External site login.

## 11. Demo Safety And Redaction

V1 stores and displays demo-safe semantic data.

Rules:

- Do not commit raw Gmail exports.
- Do not store raw private email bodies in the application database long-term.
- Replace personal names, emails, phone numbers, addresses, account numbers, confirmation numbers, and sensitive notes.
- Fabricate financial, health, legal, immigration, insurance, and government institutions.
- Keep ordinary merchant names only when safe.
- Preserve semantic meaning.
- Keep deterministic replacement maps so repeated entities stay consistent.
- Add redaction tests as hard gates.

The UI should include a Data Safety screen showing:

- demo-safe data active
- last redaction run
- redaction test status
- raw data retention status
- number of source emails processed
- number of semantic objects created

## 12. Link-Limited Updates

Many real emails do not contain full details. They link to merchant, bank, healthcare, government, or other portals.

MailBuddy must represent this honestly.

Update detail states:

- `complete_from_email`
- `partial_from_email`
- `details_behind_link`
- `requires_user_review`
- `unsupported_without_external_fetch`

Example:

> Whole Foods sent an order update, but item-level replacement details are behind an external link. MailBuddy can track that an update occurred, but cannot verify replacements from this email alone.

External fetching is future scope.

## 13. Tasks

MailBuddy extracts tasks from emails.

Task fields:

- title
- due date
- urgency
- status
- confidence
- reason
- source refs
- evidence snippet
- uncertainty flags

Actions:

- complete
- snooze
- dismiss
- edit
- open source

V1 task categories:

- forms
- bills
- appointment prep
- school/family tasks
- work follow-ups
- reply-needed messages
- registration/deadline tasks

## 14. Updates

MailBuddy tracks operational updates.

V1 update categories:

- deliveries
- refunds
- appointments
- reservations
- travel/events
- account or policy changes

Update fields:

- current state
- latest change
- timeline
- action needed
- completeness state
- source refs
- external link limitation, if any

## 15. Semantic Threads

MailBuddy should group related email conversations by meaning, not only Gmail thread ID.

Gmail thread ID is useful but insufficient because real-world conversations often split across:

- changed subject lines
- forwarded messages
- separate replies from different people
- calendar/order/system follow-ups
- repeated institutional notices
- multiple emails about the same form, claim, refund, appointment, or trip

### `semantic_thread`

A `semantic_thread` represents one real-world topic or unresolved workflow.

Fields:

- id
- title
- summary
- status
- priority
- confidence
- participant entities
- related source email ids
- Gmail thread ids
- topic embeddings
- unresolved questions
- who is waiting on whom
- latest meaningful change
- suggested next action
- evidence snippets
- created at
- updated at

### Thread Creation

MailBuddy can create semantic threads from:

- Gmail thread ID
- normalized subject similarity
- sender/recipient overlap
- entity overlap
- date proximity
- shared links/order numbers/claim numbers
- embedding similarity
- explicit semantic category match

### V1 Thread Scope

V1 should support semantic threads lite:

- group by Gmail thread ID first
- merge obvious related threads when confidence is high
- show confidence and source evidence
- avoid merging ambiguous threads
- flag possible related messages for review instead of silently merging

### Thread UI

The Threads screen should show:

- topic
- participants
- latest message
- current state
- unresolved questions
- who is waiting on whom
- suggested next action
- related source emails
- possible related emails, if confidence is not high enough to auto-merge

### Assistant Support

The assistant should answer:

- Which conversations need replies?
- Who is waiting on me?
- What is the status of the school form conversation?
- Did Maya respond about the trip?
- What is unresolved in the budget thread?
- Show related emails about the insurance claim.

If MailBuddy merges messages across Gmail thread IDs, the UI should explain why.

Example:

> Grouped because the messages mention the same insurance claim, share participants, and occurred within 4 days.

## 16. Financial Items

Financial items are first-class semantic objects.

Categories:

- receipts
- subscriptions
- invoices
- payment reminders
- refund notices
- bank/card alerts
- tax/insurance/benefit documents
- unusual financial notices

Financial item fields:

- merchant/institution
- amount, if available
- due date, if available
- status
- category
- confidence
- source refs
- sensitivity level
- calculation eligibility

Financial totals must come from deterministic code, not LLM prose.

## 17. Institutional Items

Institutional items are high-sensitivity or high-consequence notices.

Categories:

- health
- government
- legal
- school
- immigration
- insurance
- benefits
- housing
- taxes

Institutional items should not be suppressed as ordinary noise even when sender patterns are repetitive.

## 18. Noise And Digest

MailBuddy should identify low-action email.

Noise examples:

- promotions
- repeated merchant marketing
- routine newsletters
- generic product announcements
- low-value notifications

Digest items:

- newsletters
- learning content
- product/industry updates
- saved reading candidates

V1 does not need a full Digest page, but it should track:

- quieted noise count
- top noisy senders
- digest candidates
- items suppressed from Attention Today

## 19. Semantic Search

V1 includes basic semantic object search.

User examples:

- Find my dental receipt.
- Show school forms.
- Find refund emails.
- Show immigration-related notices.
- Find tax documents.
- Show subscription renewals.

Implementation:

- create embedding text per semantic object
- store embeddings
- vector search by query
- combine with keyword search for exact values
- filter by type/date/status/sender/category
- return source-backed results

Search should not answer stateful questions by itself. Stateful answers should go through deterministic tools and assistant reasoning.

## 20. Assistant

The assistant answers questions over semantic objects.

Supported V1 questions:

- What needs my attention today?
- What tasks are due soon?
- Which conversations need replies?
- Did I get a refund notice?
- Find my dental receipt.
- What financial items should I review?
- Show school-related tasks.
- What important emails did I miss this week?

Assistant rules:

- use deterministic tools for retrieval, filtering, totals, and timelines
- cite source objects
- say when evidence is incomplete
- say when details are behind links
- do not invent refunds, payments, due dates, or statuses
- do not take risky actions

## 21. Attention Today

This is the flagship screen.

Sections:

- Needs Action
- Important Updates
- Financial Watch
- Waiting On Me
- Quieted Noise
- Incomplete / Needs Review

Each item shows:

- title
- type
- priority
- confidence
- why it appears
- source evidence
- action buttons

This screen matters more than every secondary feature.

## 22. Evaluation

Evaluation is a product requirement, not an afterthought.

V1 evaluation areas:

- classification accuracy
- task extraction quality
- update completeness detection
- semantic thread grouping quality
- financial item extraction
- redaction safety
- semantic search relevance
- assistant groundedness
- deterministic calculation correctness

The app should include an evaluation report command or page showing:

- fixture count
- semantic object counts
- redaction pass/fail
- known failure cases
- unsupported patterns
- examples of link-limited emails

## 23. V1 Scope

Included:

- single-user Gmail readonly ingestion
- prior-discovery-informed semantic categories
- redaction/sanitization pipeline
- semantic object schema
- source references
- task extraction
- update extraction with link-limited states
- semantic threads lite
- financial item extraction
- institutional item classification
- noise/digest classification
- semantic object search
- Attention Today dashboard
- Assistant over semantic objects
- Data Safety / Evaluation screen
- curated demo snapshot generated from sanitized real patterns

Not included:

- multi-user auth
- production push notifications
- external account login
- browser-assisted merchant detail fetching
- autonomous unsubscribe
- send/delete/archive actions
- full voice assistant
- full digest reader
- opportunities page
- native mobile app
- billing or SaaS onboarding

## 24. Success Criteria

V1 is successful if:

- real Gmail data can be processed safely
- raw private data is not exposed in the demo
- the app creates useful semantic objects from messy inbox patterns
- Attention Today feels meaningfully better than a Gmail unread list
- semantic threads group real-world workflows better than Gmail thread IDs alone
- assistant answers are source-backed and honest about uncertainty
- the portfolio can show architecture, real-data discovery learnings, evals, and UI
- the product story clearly demonstrates spec-driven, agent-assisted development

## 25. Portfolio Story

The portfolio should explain:

1. Initial prototype exposed unrealistic synthetic-data assumptions.
2. Real Gmail discovery revealed noise, financial docs, receipts, link-limited updates, institutional notices, and semantic thread needs.
3. Product was redesigned around semantic objects, provenance, privacy, and uncertainty.
4. Implementation used spec-driven development and coding agents.
5. Evaluation and redaction were treated as first-class product features.

The intended story:

> I rebuilt an AI inbox prototype into a real-data semantic email system with provenance, privacy, and evaluation.
