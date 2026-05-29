# MailBuddyV1 Architecture

## 1. Architecture Summary

MailBuddyV1 is a Gmail-only semantic intelligence layer. V1 uses a dual-mode architecture:

- **Sanitized snapshot mode:** the default polished demo path.
- **Live Gmail readonly mode:** a technical proof path that ingests the creator's Gmail.

Both modes feed the same semantic object store. The UI, search, assistant, and evaluation layers must not know whether an object came from a curated snapshot or live Gmail ingest.

V1 is single-user and demo-safe. It stores sanitized semantic objects, source references, and embeddings. It does not store raw private mailbox bodies long-term.

## 2. Recommended V1 Stack

- Frontend: Next.js mobile-first web app.
- Backend: Python FastAPI.
- Database: Postgres with pgvector.
- First database target: local Postgres for development, with Supabase or Neon as the hosted deployment target.
- Email: Gmail API OAuth readonly.
- AI: OpenAI text, embeddings, and optional later voice APIs.
- Jobs: simple Python worker or FastAPI background tasks for V1.
- Deployment target: Vercel frontend plus hosted FastAPI/Postgres, or a single full-stack host if engineering review later chooses one.

Voice is not a V1 architecture dependency. The architecture should not block future voice, but V1 should prove semantic objects, Attention Today, search, assistant, redaction, and evaluation first.

## 3. System Layers

```txt
Next.js UI
  -> FastAPI API
    -> snapshot import
    -> Gmail readonly ingest
    -> normalization
    -> raw transient processing boundary
    -> sanitization/redaction
    -> semantic extraction
    -> semantic object store
    -> deterministic tool layer
    -> assistant planner/finalizer
    -> evaluation runner
Postgres + pgvector
```

## 4. Core Data Flow

```txt
Snapshot JSON or Gmail API message
  -> source normalization
  -> transient raw content
  -> redaction + replacement maps
  -> sanitized source_email
  -> classification
  -> typed semantic objects
  -> source_refs
  -> embedding text
  -> embeddings
  -> Attention Today / Search / Assistant / Evaluation
```

Important boundary:

- Raw Gmail content may exist only in the ingestion process, local raw export files, or explicitly private development storage.
- Durable application storage should contain sanitized `source_email` records, semantic objects, source refs, redaction maps, eval records, and audit metadata.

## 5. Modes

### Snapshot Mode

Snapshot mode is the default demo mode.

Purpose:

- reliable walkthrough
- controlled redaction
- repeatable evaluation
- portfolio screenshots and recordings

Input:

- curated sanitized snapshot JSON generated from real-pattern examples

Required behavior:

- imports into the same tables as live Gmail objects
- preserves synthetic source IDs and source refs
- marks objects with `ingest_mode = snapshot`
- supports reset/reseed

### Live Gmail Readonly Mode

Live Gmail mode proves the pipeline works on real Gmail data.

Purpose:

- technical credibility
- local/dev validation
- future production path

Input:

- Gmail API readonly messages

Required behavior:

- uses readonly OAuth scope
- stores tokens outside committed files
- tracks Gmail message ID and thread ID
- marks objects with `ingest_mode = gmail_live`
- runs the same normalization, redaction, extraction, and evaluation checks as snapshot mode
- can be disabled for public demos

## 6. Database Model

### `source_emails`

Stores minimized, sanitized source-message records.

Core fields:

- `id`
- `ingest_mode`: `snapshot` or `gmail_live`
- `gmail_message_id`
- `gmail_thread_id`
- `sender_display`
- `sender_domain`
- `subject`
- `received_at`
- `labels`
- `snippet`
- `sanitized_excerpt`
- `detected_links`
- `attachment_filenames`
- `raw_retention_status`
- `created_at`

Do not store full raw body in this table.

### `semantic_objects`

Base table for extracted meaning.

Core fields:

- `id`
- `type`: `task`, `update`, `semantic_thread`, `financial_item`, `institutional_item`, `digest_item`, `noise_item`, `reference_item`
- `title`
- `summary`
- `status`
- `priority`
- `confidence`
- `object_date`
- `extracted_entities`
- `uncertainty_flags`
- `embedding_text`
- `embedding`
- `created_at`
- `updated_at`

Typed details can live in `details` JSONB for V1, with table extraction later if needed. This keeps the first implementation fast while preserving a clean base object model.

### `source_refs`

Links semantic objects to evidence.

Core fields:

- `id`
- `semantic_object_id`
- `source_email_id`
- `evidence_snippet`
- `support_reason`
- `confidence`
- `created_at`

Every semantic object shown in the UI or assistant must have at least one source ref unless it is an aggregate object whose source refs are inherited from children.

### `redaction_maps`

Stores deterministic replacement mappings.

Core fields:

- `id`
- `entity_type`
- `raw_hash`
- `replacement_value`
- `sensitivity_level`
- `created_at`

The raw value itself should not be stored. Use a stable hash for repeatability.

### `assistant_queries`

Stores assistant audit records.

Core fields:

- `id`
- `query`
- `tool_calls`
- `source_object_ids`
- `answer`
- `answer_type`
- `created_at`

Assistant audit records should never include raw private email bodies.

### `evaluation_runs`

Stores evaluation reports.

Core fields:

- `id`
- `run_type`
- `fixture_count`
- `passed`
- `failed`
- `metrics`
- `created_at`

## 7. Semantic Object Details

V1 can use a `details` JSONB field on `semantic_objects` for typed fields.

Required detail shapes:

- `task`: due date, urgency, action status, reason, uncertainty flags.
- `update`: current state, latest change, timeline, action needed, completeness state.
- `financial_item`: institution/merchant, amount, due date, category, calculation eligibility.
- `institutional_item`: institution type, sensitivity level, action required.
- `semantic_thread`: participant entities, Gmail thread IDs, unresolved questions, waiting-on state, suggested next action, merge reasons.
- `noise_item`: sender/domain, reason quieted, suppression confidence.
- `digest_item`: topic, summary, saved-reading candidate.
- `reference_item`: topic, long-term retrieval tags.

## 8. Link-Limited Updates

Update extraction must classify evidence completeness.

Allowed completeness states:

- `complete_from_email`
- `partial_from_email`
- `details_behind_link`
- `requires_user_review`
- `unsupported_without_external_fetch`

Rules:

- Do not infer refund, replacement, delivery, or payment details when the email only contains a portal link.
- Store the fact that an update exists separately from the unverified details.
- Surface link-limited state in Attention Today, update detail views, and assistant answers.

## 9. Semantic Threads

Semantic threads should be built conservatively.

V1 algorithm:

1. Group by Gmail thread ID.
2. Normalize subject lines.
3. Extract participants and key entities.
4. Compare candidate groups by entity overlap, participant overlap, date proximity, shared identifiers, and embedding similarity.
5. Auto-merge only high-confidence matches.
6. Flag ambiguous matches as possible related messages.

The UI must explain auto-merges.

Example:

```txt
Grouped because both messages mention the same insurance claim,
share two participants, and occurred within 4 days.
```

## 10. Search Architecture

Search should be hybrid.

Use:

- vector similarity over `embedding_text`
- keyword/full-text matching for exact names, subjects, IDs, and terms
- structured filters for object type, date, status, sender/domain, category, and priority

Search returns semantic objects, not raw emails. Each result must include source refs.

Stateful questions such as "Did I get reimbursed?" should go through assistant tools, not plain search.

## 11. Deterministic Tool Layer

Assistant tools should be constrained and deterministic.

Required V1 tools:

- list Attention Today items
- search semantic objects
- get object details
- get source refs
- filter objects by type/date/status/category
- build update timelines
- identify unresolved semantic threads
- calculate financial totals only for eligible financial items

LLMs should not directly compute totals, mutate state without tool calls, or claim facts that tools did not retrieve.

## 12. Assistant Flow

```txt
User question
  -> planner selects deterministic tools
  -> tools query semantic objects and source refs
  -> finalizer writes concise answer
  -> answer cites source objects
  -> audit record stored
```

Assistant rules:

- Say when evidence is incomplete.
- Say when details are behind links.
- Do not invent refunds, payments, due dates, or reply state.
- Do not expose unsanitized private data.
- Do not perform risky actions.

## 13. Evaluation Hooks

Evaluation should run against the same pipeline as product data.

Required gates:

- redaction hard gate
- semantic classification checks
- task extraction checks
- update completeness-state checks
- semantic thread grouping checks
- financial extraction checks
- semantic search relevance checks
- assistant groundedness checks
- deterministic calculation checks

Portfolio screenshots and public demo snapshots should require redaction passing.

## 14. Security And Privacy

Principles:

- Gmail OAuth only; never store Gmail passwords.
- Request readonly Gmail scope for V1.
- Store OAuth tokens outside the repo.
- Keep raw exports under ignored private paths.
- Avoid durable raw body storage.
- Store sanitized semantic objects and source refs.
- Log minimally.
- Never expose raw emails in screenshots, assistant responses, or fixtures.
- Require explicit approval for any future risky action.

Security review should run before implementation touches live Gmail data.

## 15. Implementation Slices

Recommended vertical slices:

1. Scaffold app, backend, database, CI, private-data conventions.
2. Define schema and migrations.
3. Implement snapshot import and seed demo data.
4. Implement redaction and source refs.
5. Implement semantic extraction for tasks, updates, financial items, institutional items, noise, and reference items.
6. Implement semantic threads lite.
7. Implement Attention Today.
8. Implement semantic search.
9. Implement assistant deterministic tools and finalizer.
10. Implement evaluation report.
11. Add live Gmail readonly proof mode.
12. Polish portfolio demo and README walkthrough.

Snapshot import should land before live Gmail ingest so UI and evals can develop without private-data risk.
