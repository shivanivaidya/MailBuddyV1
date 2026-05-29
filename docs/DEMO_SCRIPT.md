# MailBuddyV1 Five-Minute Demo Script

This script is the acceptance test for V1. The first implementation specs should build the smallest vertical slice that can run this demo end to end with sanitized snapshot data, then prove the same pipeline can ingest live Gmail readonly data.

## Demo Goal

Show that MailBuddy turns Gmail overwhelm into a small set of source-backed decisions within seconds.

The demo must prove:

- real-Gmail-inspired semantic objects, not synthetic-perfect emails
- Attention Today as the primary workspace
- source evidence behind every claim
- honest handling of link-limited updates
- semantic threads beyond Gmail thread IDs
- semantic search over meaning
- assistant answers grounded in objects and source refs
- redaction and evaluation as visible product trust

## Demo Setup

Mode:

- Primary walkthrough: sanitized snapshot mode.
- Technical proof: live Gmail readonly ingest status can be shown if available, but the demo must not depend on live Gmail succeeding in real time.

Required seeded object counts:

- 12-20 sanitized source emails.
- 3-5 tasks.
- 3-5 updates.
- 2 semantic threads.
- 2-3 financial items.
- 2 institutional/reference items.
- 4-8 noise/digest items.

Required seeded story objects:

1. A high-priority task with a due date and source evidence.
2. A merchant/order update with `details_behind_link`.
3. A semantic thread assembled from messages with different Gmail thread IDs.
4. A financial item with deterministic amount/status handling.
5. A reference item discoverable by semantic search.
6. A noise/digest group that shows MailBuddy did not surface low-value emails.
7. A Data Safety / Evaluation status record with passing redaction and assistant grounding checks.

## Minute-By-Minute Script

### 0:00-0:30 — Open Attention Today

Narration:

> This is MailBuddy. Instead of showing my inbox, it shows what actually needs attention today, with evidence for every claim.

User action:

- Open the app to Attention Today.

Screen must show:

- attention count
- top three source-backed decisions
- Needs Action
- Important Updates
- Financial Watch
- Incomplete / Needs Review
- Quieted Noise summary

Acceptance check:

- The first viewport does not look like a raw inbox.
- The top three items each show type, priority, confidence, reason, and evidence action.

### 0:30-1:10 — Open A Task And Inspect Evidence

Narration:

> This task was extracted from email, but MailBuddy does not ask me to trust the summary blindly.

Seed object:

- Type: `task`
- Example title: `Submit insurance reimbursement form`
- Due date: within the next 7 days
- Priority: high
- Confidence: medium or high
- Source refs: at least 1

User action:

- Open the task from Attention Today.
- Open the evidence drawer or mobile sheet.

Screen must show:

- task title
- due date
- reason it appears
- suggested next step
- source subject
- sender/domain
- evidence snippet
- support reason
- confidence

Acceptance check:

- The task can be verified from source evidence without exposing raw private email body content.

### 1:10-1:55 — Show A Link-Limited Update

Narration:

> Real Gmail is messy. Some emails say an update happened but hide the details behind a link. MailBuddy should say that instead of making up item-level facts.

Seed object:

- Type: `update`
- Example title: `Whole Foods order changed`
- Completeness state: `details_behind_link`
- Status: needs review
- Source refs: at least 1

User action:

- Open the update detail view.
- Inspect completeness state and evidence.

Screen must show:

- current state
- latest change
- completeness state
- what MailBuddy knows
- what MailBuddy cannot verify
- source evidence

Acceptance check:

- The UI does not claim specific replacement/refund item details unless those details are in the email.
- The app clearly labels the missing external-link dependency.

### 1:55-2:35 — Show A Semantic Thread

Narration:

> Gmail thread IDs are not enough. These messages were separate in Gmail, but they belong to the same real-world workflow.

Seed object:

- Type: `semantic_thread`
- Example title: `Apartment maintenance appointment`
- Sources: 2-4 emails
- At least two different Gmail thread IDs
- Related object: one task or update

User action:

- Open Semantic Threads.
- Open the seeded thread.

Screen must show:

- topic
- participants/domains
- current status
- unresolved questions
- who is waiting on whom
- timeline of related source emails
- related semantic objects
- ambiguity notes if any

Acceptance check:

- The thread explains why messages were grouped.
- Ambiguous messages are shown as possible related messages, not silently merged.

### 2:35-3:10 — Search / Memory

Narration:

> Search works over meaning, not just subject lines.

Seed object:

- Type: `reference_item` or `financial_item`
- Example query: `Find my dental receipt`
- Source may use fabricated sensitive institution names.

User action:

- Open Search.
- Run `Find my dental receipt`.
- Open the top result.

Screen must show:

- grouped semantic results
- object type
- matched reason
- date
- sender/domain
- confidence
- source count
- evidence action

Acceptance check:

- The result can be found even if the exact query phrase does not appear in the subject.
- Source refs are available from the result.

### 3:10-4:00 — Ask The Assistant

Narration:

> The assistant is not reading raw email and improvising. It answers through structured objects and deterministic tools.

Prompt:

```txt
What needs my attention today?
```

Expected answer shape:

- 3-5 bullet items
- each item cites a semantic object
- each item includes source/evidence action
- incomplete/link-limited items are labeled
- financial claims are phrased from deterministic fields

Required answer content:

- the seeded high-priority task
- the link-limited merchant update
- one semantic-thread follow-up or waiting item
- one financial watch item, if present

Acceptance check:

- The assistant does not cite raw private email bodies.
- The assistant does not perform risky actions.
- The assistant says when information is partial or behind a link.

### 4:00-4:40 — Open Data Safety / Evaluation

Narration:

> Because this uses real inbox patterns, privacy and evaluation are part of the product, not a hidden dev detail.

User action:

- Open Safety.

Screen must show:

- demo-safe data active
- ingest mode
- last redaction run
- redaction test status
- raw retention status
- source emails processed
- semantic objects created
- evaluation run status
- assistant groundedness status
- prompt-injection fixture status

Acceptance check:

- No raw private values are visible.
- Redaction and evaluation status are understandable at a glance.
- Failed gates would visibly block demo readiness.

### 4:40-5:00 — Optional Live Gmail Proof

Narration:

> The walkthrough is stable because it uses a sanitized snapshot, but the same pipeline is designed to run against readonly Gmail.

User action:

- Show Gmail readonly connection or ingest status only if configured.

Screen must show, if available:

- readonly scope
- live ingest disabled/enabled status
- latest sync metadata
- no send/delete/archive scopes

Acceptance check:

- Live Gmail proof is clearly secondary.
- The demo does not expose private data or depend on real-time sync.

## Demo Data Requirements

### Source Email Fixture Requirements

Every seeded source email must include:

- stable fixture ID
- ingest mode: `snapshot`
- sender display or sender domain
- subject
- received timestamp
- snippet
- sanitized excerpt, if needed
- detected links
- raw retention status

Do not include raw private Gmail bodies in committed fixtures.

### Semantic Object Fixture Requirements

Every seeded semantic object must include:

- stable object ID
- type
- title
- summary
- status
- priority
- confidence
- object date
- details JSON
- source refs
- uncertainty flags
- embedding text

### Source Reference Requirements

Every visible object must have at least one source ref with:

- source email ID
- evidence snippet
- support reason
- confidence

## Demo Acceptance Checklist

- [ ] Attention Today loads from seeded semantic objects.
- [ ] Top three decisions are visible in the first viewport.
- [ ] Task detail opens and shows source evidence.
- [ ] Link-limited update shows `details_behind_link`.
- [ ] Semantic thread groups messages across Gmail thread IDs.
- [ ] Search returns grouped semantic object results.
- [ ] Assistant answers "What needs my attention today?" with citations.
- [ ] Safety screen shows redaction/evaluation status.
- [ ] No raw private data appears in UI, fixtures, logs, or screenshots.
- [ ] Demo can run without live Gmail.
- [ ] Live Gmail readonly proof path is secondary and optional.

## Spec Implications

The first implementation spec should not start with a broad app scaffold alone. It should build the smallest vertical slice that can satisfy this script:

1. scaffold app and API
2. define minimal source email, semantic object, and source ref models
3. import a sanitized snapshot
4. render Attention Today from seeded objects
5. open object detail and source evidence
6. show Safety status from static/eval metadata

Later specs can add live Gmail ingest, full extraction, search ranking, assistant tooling, and richer evaluation.
