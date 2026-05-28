# MailBuddy Tasks

## Phase 0: Foundation Documentation

- [x] Create PRD.
- [x] Create architecture document.
- [x] Create UI/UX document.
- [x] Create coding agent instructions.
- [x] Create implementation roadmap.
- [x] Create decision log.
- [x] Create evaluation strategy.
- [x] Create README and changelog.

## Phase 1: Project Scaffolding

- [ ] Create monorepo structure.
- [ ] Scaffold Next.js frontend.
- [ ] Scaffold FastAPI backend.
- [ ] Configure shared environment documentation.
- [ ] Add formatting/linting.
- [ ] Add basic test runners.
- [ ] Add initial CI if using GitHub.
- [ ] Create local development setup instructions.

## Phase 2: Data Model And Storage

- [ ] Choose Supabase or Neon Postgres.
- [ ] Define semantic object schema.
- [ ] Define task/update/thread tables.
- [ ] Add pgvector support.
- [ ] Add migration tooling.
- [ ] Add source reference model.
- [ ] Add assistant query/audit model.
- [ ] Add redaction rules model.

## Phase 3: Gmail Ingestion

- [ ] Configure Gmail OAuth app.
- [ ] Implement OAuth callback in FastAPI.
- [ ] Store OAuth tokens securely.
- [ ] Fetch message metadata.
- [ ] Fetch recent messages for initial sync.
- [ ] Track Gmail message ID, thread ID, and `historyId`.
- [ ] Implement manual `Sync now`.
- [ ] Implement 30-minute background sync.
- [ ] Add ingestion status endpoint.

## Phase 4: Demo Sanitization

- [ ] Build entity detection for names, emails, phones, addresses, IDs, and sensitive institutions.
- [ ] Create deterministic replacement maps.
- [ ] Keep non-sensitive merchant names.
- [ ] Fabricate financial institution names.
- [ ] Fabricate health institution names.
- [ ] Fabricate government/legal/immigration names when needed.
- [ ] Add redaction tests.
- [ ] Add demo safety report endpoint.

## Phase 5: Semantic Pipeline

- [ ] Implement normalized email model.
- [ ] Implement classification prompt chain.
- [ ] Implement task extraction.
- [ ] Implement update extraction.
- [ ] Implement financial vigilance extraction.
- [ ] Implement digest/noise classification.
- [ ] Implement semantic thread grouping.
- [ ] Store sanitized semantic objects.
- [ ] Generate embeddings for semantic search.
- [ ] Add fixture-based extraction tests.

## Phase 6: Assistant API

- [ ] Define assistant request/response contract.
- [ ] Implement planner.
- [ ] Implement deterministic retrieval tools.
- [ ] Implement deterministic calculation tools.
- [ ] Implement timeline tools.
- [ ] Implement finalizer with provenance.
- [ ] Add assistant answer tests.
- [ ] Add refusal/unknown behavior for missing data.

## Phase 7: Voice Pipeline

- [ ] Add audio upload endpoint.
- [ ] Integrate OpenAI transcription.
- [ ] Route transcript into assistant API.
- [ ] Add optional text-to-speech response.
- [ ] Return transcript, answer, sources, and audio URL/blob.
- [ ] Add voice latency/error handling.
- [ ] Add voice demo fixtures where practical.

## Phase 8: Frontend MVP

- [ ] Build mobile-first app shell.
- [ ] Build Home dashboard.
- [ ] Build Tasks screen.
- [ ] Build Updates screen.
- [ ] Build Threads screen.
- [ ] Build Assistant screen.
- [ ] Build push-to-talk voice UI.
- [ ] Build Settings with sync status.
- [ ] Add demo safety indicator.
- [ ] Add responsive polish.

## Phase 9: Evaluation

- [ ] Create sanitized fixture library.
- [ ] Add expected outputs for each fixture.
- [ ] Add classification accuracy checks.
- [ ] Add extraction precision/recall checks.
- [ ] Add redaction hard-gate tests.
- [ ] Add assistant groundedness tests.
- [ ] Add deterministic calculation tests.
- [ ] Add notification tier evaluation fixtures.
- [ ] Add evaluation report script.

## Phase 10: Portfolio Demo

- [ ] Write demo script.
- [ ] Capture mobile screenshots.
- [ ] Capture desktop screenshots.
- [ ] Capture assistant/voice GIF.
- [ ] Create architecture diagram.
- [ ] Create product case study.
- [ ] Create short product deck.
- [ ] Publish GitHub repo.
- [ ] Add README walkthrough.

## Phase 2+ Future Features

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
