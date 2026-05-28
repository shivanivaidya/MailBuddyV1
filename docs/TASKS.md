# MailBuddyV1 Tasks

## Phase 0: Foundation Documentation

- [x] Create initial PRD.
- [x] Run CEO review and update PRD around V1 semantic-object scope.
- [x] Run office-hours review and choose dual-mode semantic system.
- [x] Record dual-mode, Attention Today, and scope decisions.
- [ ] Write exact five-minute demo script.
- [ ] Run engineering review on architecture, data flow, schema, edge cases, and tests.
- [ ] Run security review before touching real Gmail data.
- [ ] Run design review for Attention Today, semantic threads, assistant, and Data Safety.
- [ ] Split implementation into executable specs.

## Phase 1: Project Scaffolding

- [ ] Create monorepo structure.
- [ ] Scaffold Next.js frontend.
- [ ] Scaffold FastAPI backend.
- [ ] Configure shared environment documentation.
- [ ] Add formatting/linting.
- [ ] Add basic test runners.
- [ ] Add initial CI.
- [ ] Add local development setup instructions.
- [ ] Add private-data directory conventions and `.gitignore` coverage.

## Phase 2: Data Model And Storage

- [ ] Choose Supabase, Neon, or local Postgres first path.
- [ ] Define `source_email` schema.
- [ ] Define base `semantic_object` schema.
- [ ] Define `source_ref` schema.
- [ ] Define task object fields.
- [ ] Define update object fields, including link-limited states.
- [ ] Define financial item fields.
- [ ] Define institutional item fields.
- [ ] Define semantic thread fields.
- [ ] Define noise/digest/reference item fields.
- [ ] Add pgvector or equivalent embedding support.
- [ ] Add migration tooling.
- [ ] Add assistant query/audit model.
- [ ] Add redaction rules and replacement-map model.

## Phase 3: Gmail Ingestion And Snapshot Import

- [ ] Configure Gmail OAuth readonly app.
- [ ] Implement local/dev Gmail OAuth flow.
- [ ] Store OAuth tokens securely outside committed files.
- [ ] Fetch message metadata.
- [ ] Fetch recent message bodies where available.
- [ ] Capture detected links and attachment filenames.
- [ ] Track Gmail message ID, thread ID, labels, and timestamps.
- [ ] Implement local raw export path with explicit privacy warnings.
- [ ] Implement sanitized snapshot import path.
- [ ] Add ingestion status endpoint.
- [ ] Add manual refresh path for live Gmail proof mode.

## Phase 4: Redaction And Demo Safety

- [ ] Build entity detection for names, emails, phones, addresses, IDs, sensitive institutions, and sensitive notes.
- [ ] Create deterministic replacement maps.
- [ ] Keep ordinary merchant names only when safe.
- [ ] Fabricate financial institution names.
- [ ] Fabricate health institution names.
- [ ] Fabricate government/legal/immigration/insurance names when needed.
- [ ] Preserve semantic meaning while altering private details.
- [ ] Prevent durable raw body storage in demo mode.
- [ ] Add redaction tests as hard gates.
- [ ] Add Data Safety report endpoint.
- [ ] Add Data Safety / Evaluation UI surface.

## Phase 5: Semantic Pipeline

- [ ] Implement normalized email model.
- [ ] Implement classify -> extract -> validate -> sanitize -> store pipeline.
- [ ] Implement task extraction.
- [ ] Implement update extraction.
- [ ] Detect `complete_from_email`, `partial_from_email`, `details_behind_link`, `requires_user_review`, and `unsupported_without_external_fetch`.
- [ ] Implement financial item extraction.
- [ ] Implement institutional item classification.
- [ ] Implement digest/noise/reference classification.
- [ ] Implement semantic threads lite.
- [ ] Flag possible related messages instead of merging ambiguous threads.
- [ ] Generate embedding text for semantic objects.
- [ ] Generate embeddings for semantic search.
- [ ] Add fixture-based extraction tests.

## Phase 6: Attention Today And Core UI

- [ ] Build mobile-first app shell.
- [ ] Build Attention Today dashboard.
- [ ] Show Needs Action.
- [ ] Show Important Updates.
- [ ] Show Financial Watch.
- [ ] Show Waiting On Me.
- [ ] Show Quieted Noise.
- [ ] Show Incomplete / Needs Review.
- [ ] Build task evidence drilldown.
- [ ] Build link-limited update drilldown.
- [ ] Build semantic thread detail view.
- [ ] Build financial item detail view.
- [ ] Build source evidence preview.
- [ ] Add responsive polish.

## Phase 7: Search And Assistant

- [ ] Implement semantic object search.
- [ ] Combine vector search with keyword/exact filters.
- [ ] Filter by type, date, status, sender, and category.
- [ ] Define assistant request/response contract.
- [ ] Implement deterministic retrieval tools.
- [ ] Implement deterministic timeline tools.
- [ ] Implement deterministic calculation tools for financial totals.
- [ ] Implement assistant planner over allowed tools.
- [ ] Implement finalizer with provenance.
- [ ] Add refusal/unknown behavior for missing data.
- [ ] Add assistant answer tests.

## Phase 8: Evaluation

- [ ] Create sanitized fixture library from real-pattern examples.
- [ ] Add expected outputs for each fixture.
- [ ] Add classification accuracy checks.
- [ ] Add task extraction checks.
- [ ] Add update completeness-state checks.
- [ ] Add semantic thread grouping checks.
- [ ] Add financial item extraction checks.
- [ ] Add redaction hard-gate tests.
- [ ] Add semantic search relevance checks.
- [ ] Add assistant groundedness tests.
- [ ] Add deterministic calculation tests.
- [ ] Add evaluation report script.
- [ ] Surface evaluation status in the app.

## Phase 9: Portfolio Demo

- [ ] Finalize five-minute demo script.
- [ ] Seed curated sanitized snapshot.
- [ ] Verify live Gmail ingest proof path.
- [ ] Capture mobile screenshots.
- [ ] Capture desktop screenshots.
- [ ] Capture assistant interaction media.
- [ ] Create architecture diagram.
- [ ] Create product case study.
- [ ] Add README walkthrough.
- [ ] Publish hosted demo.

## Future Features

- [ ] Full voice assistant.
- [ ] Full digest reader.
- [ ] Opportunities page.
- [ ] Production Gmail Pub/Sub.
- [ ] Web push notifications.
- [ ] Notification preference center.
- [ ] Calendar integration.
- [ ] Reply drafting approval flow.
- [ ] Unsubscribe approval flow.
- [ ] Connected detail fetching with OAuth.
- [ ] Browser-assisted fetching with explicit approval.
- [ ] Multi-account support.
- [ ] Native mobile wrapper.
