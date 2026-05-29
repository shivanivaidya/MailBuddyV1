# MailBuddy Architecture

## 1. Architecture Summary

MailBuddy is a Gmail-only, AI-native inbox operating system. The MVP is a single-account portfolio/demo app that stores sanitized semantic objects derived from Gmail, not raw private mailbox data.

Recommended MVP stack:

- Frontend: Next.js mobile-first web app.
- Backend: Python FastAPI.
- Database: Postgres with pgvector.
- Email: Gmail API OAuth.
- AI: OpenAI text, embeddings, and voice APIs.
- Jobs: FastAPI background tasks or a simple Python worker for MVP.
- Deployment target: Vercel frontend, Render/Fly/Railway backend, Supabase/Neon Postgres.

## 2. System Layers

```txt
Next.js web/mobile UI
  -> FastAPI backend
    -> Gmail ingestion
    -> normalization pipeline
    -> demo sanitization pipeline
    -> semantic classification/extraction
    -> semantic object store
    -> deterministic tools
    -> assistant planner/executor/finalizer
    -> voice pipeline
    -> evaluation hooks
Postgres + pgvector
```

## 3. Gmail Ingestion

The Gmail ingestion layer connects to one Gmail account through OAuth and fetches recent messages.

MVP behavior:

- Initial sync fetches the last 90 days or last 1,000 messages.
- Manual `Sync now` endpoint starts an incremental sync.
- Background sync runs every 30 minutes while the backend is active.
- Gmail message IDs, thread IDs, timestamps, labels, and `historyId` are tracked to prevent duplicate processing.
- Raw bodies are processed transiently and should not be retained after sanitized semantic objects are created.

Production behavior:

- Gmail Pub/Sub notifies the backend of mailbox changes.
- Backend fetches changed messages using Gmail history API.
- Priority queue processes high-stakes messages before digests/noise.

## 4. Normalization Pipeline

Normalization converts Gmail API responses into a consistent internal format.

Normalized fields:

- Gmail message ID
- Gmail thread ID
- source URL
- sender display name
- sender email domain
- recipient metadata
- subject
- received timestamp
- labels
- snippet
- parsed plain text
- attachment metadata
- detected links

The normalized raw representation should be temporary for the MVP. It exists to support classification, extraction, and sanitization, then is discarded or minimized.

## 5. Demo Sanitization Pipeline

The MVP stores demo-safe data only.

Sanitization runs before durable storage of semantic objects.

Rules:

- Keep ordinary merchant names unless the merchant implies sensitive financial/health/legal/government context.
- Replace financial institutions with fabricated realistic names.
- Replace health providers with fabricated realistic names.
- Replace government/legal/immigration institutions with generalized or fabricated names.
- Replace people names, emails, phone numbers, addresses, account numbers, order IDs, and confirmation numbers.
- Preserve category, lifecycle state, relative dates, and product meaning.

Example transformations:

- `Chase` -> `Northstar Bank`
- `Kaiser Permanente` -> `Willow Health`
- `Dr. Jane Smith Dental` -> `Brightline Dental`
- `john.smith@gmail.com` -> `person_12@example.com`
- `Order #123-4567890` -> `Order #A-20491`

Sanitization should be deterministic enough that repeated references to the same entity remain consistent within a demo snapshot.

## 6. Semantic Classification

Classification routes each normalized email into one or more semantic categories:

- task
- update
- semantic thread
- financial vigilance
- opportunity
- digest
- noise
- institutional
- reference

Use prompt chaining for fixed pipelines:

```txt
normalize -> classify -> extract fields -> validate -> sanitize -> store
```

Use routing for specialized extraction:

- task extraction workflow
- update tracking workflow
- financial vigilance workflow
- digest workflow
- noise/unsubscribe workflow
- institutional item workflow

## 7. Semantic Object Layer

The semantic object layer is the core of MailBuddy.

Core entities:

- `semantic_objects`
- `tasks`
- `updates`
- `semantic_threads`
- `financial_items`
- `opportunities`
- `digest_items`
- `noise_items`
- `institutional_items`
- `reference_items`
- `source_refs`
- `redaction_rules`
- `assistant_queries`
- `evaluation_runs`

Every object should include:

- stable ID
- type
- title
- summary
- status
- priority/urgency
- confidence
- source reference
- extracted fields
- timestamps
- sanitized display fields
- embedding where useful

## 8. Deterministic Tool Layer

Assistant tools should be constrained and deterministic.

Tools should handle:

- retrieving semantic objects
- filtering by date/category/status
- calculating totals
- building timelines
- checking task due dates
- finding unresolved threads
- ranking items by urgency
- applying state transitions
- enforcing notification policy

LLMs should not directly compute totals, mutate state without tool calls, or claim facts that tools did not retrieve.

## 9. Assistant Planner / Executor / Finalizer

The assistant should use a structured flow:

1. Planner interprets the user request and chooses tools.
2. Executor runs deterministic tools against Postgres.
3. Finalizer explains results in natural language with citations/provenance.

The assistant should say when it does not have enough data. It should not infer refunds, payments, deadlines, or replies unless supported by semantic objects or source evidence.

## 10. Voice Pipeline

Voice is implemented through the Python backend and OpenAI voice APIs.

Flow:

```txt
User taps mic
-> browser records audio
-> Next.js uploads audio to FastAPI
-> FastAPI transcribes with OpenAI
-> assistant planner handles transcript
-> deterministic tools query Postgres
-> finalizer writes response
-> optional text-to-speech creates audio
-> frontend displays text and plays audio
```

The voice assistant should support:

- daily briefing
- task review
- update review
- "what needs attention"
- "did I get refunded"
- "which conversations need replies"

## 11. Notification Architecture

Notification decisions happen after semantic processing, not directly after Gmail arrival.

Production flow:

```txt
Gmail push
-> fetch changed messages
-> normalize
-> classify/extract
-> score importance
-> choose notification tier
```

Tiers:

- `critical`: immediate notification
- `timely`: next allowed notification window
- `digest`: daily/weekly summary
- `silent`: no interruption

MVP does not need production push notifications. It should include an in-app notification center and simulated notification examples.

## 12. Agentic Workflow Layer

Specialized conceptual agents may enrich semantic objects but should not take risky actions.

Potential modules:

- Email Triage Agent
- Task Extraction Agent
- Update Tracking Agent
- Thread Understanding Agent
- Financial Vigilance Agent
- Opportunity Agent
- Noise / Unsubscribe Agent
- Memory / Preference Agent
- Assistant Orchestrator Agent
- Evaluator / Guardrail Agent

Allowed:

- suggest unsubscribe
- draft reply
- flag urgency
- summarize thread
- create or enrich structured objects

Not allowed without explicit approval:

- send reply
- delete email
- unsubscribe
- log into external accounts
- make payments or purchases

## 13. Evaluation Layer

Evaluation should be a first-class backend capability.

Evaluate:

- extraction precision/recall
- classification accuracy
- redaction safety
- assistant groundedness
- deterministic tool correctness
- notification tier accuracy
- voice transcription and assistant response quality

Use sanitized fixtures with expected semantic outputs. Redaction tests should be hard gates before screenshots or portfolio demos.

## 14. Security And Privacy

Principles:

- Gmail OAuth only; never store Gmail passwords.
- Request the narrowest feasible Gmail scopes.
- Store tokens securely.
- Store sanitized semantic objects, not raw private mailbox content.
- Avoid long-term raw body storage.
- Log minimally.
- Do not expose raw emails in screenshots, assistant responses, or demo fixtures.
- Require explicit approval for risky actions.

## 15. Parallel Development Boundaries

Suggested parallel workstreams:

- backend Gmail ingestion
- backend semantic pipeline
- backend assistant API
- backend voice pipeline
- frontend dashboard
- frontend assistant/voice UI
- demo sanitization
- evaluation/tests
- docs

Each coding agent should own explicit files/modules and avoid modifying unrelated work.
