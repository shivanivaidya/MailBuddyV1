# MailBuddy Decisions

## Decision 001: MailBuddy Is Not A Gmail Wrapper

Decision: MailBuddy will not center the product around a raw chronological inbox.

Reason: The product thesis is that email overload comes from treating every message as the main unit of work. MailBuddy should transform messages into semantic objects and workflows.

Implication: Gmail remains the source mailbox. MailBuddy becomes the semantic operating layer.

## Decision 002: Gmail-Only MVP

Decision: The MVP supports Gmail only.

Reason: Gmail OAuth and APIs are enough to prove the product thesis. Multi-provider support would add complexity before the demo validates semantic inbox workflows.

Implication: Architecture should not hard-code Gmail everywhere, but implementation can optimize for Gmail first.

## Decision 003: Single-Account Portfolio Demo

Decision: The MVP connects only to the creator's Gmail account.

Reason: The project is optimized for a portfolio/demo, not public SaaS onboarding.

Implication: Multi-user auth, billing, tenant isolation, and enterprise controls are future work.

## Decision 004: Demo-Mode-Only Storage

Decision: The MVP stores altered demo-safe semantic data, not a separate private mode.

Reason: The public artifact must be safe to demo while still reflecting real inbox complexity.

Implication: Sanitization is not cosmetic. It is part of the ingestion architecture.

## Decision 005: Keep Ordinary Merchant Names

Decision: Non-sensitive merchant names can remain visible.

Reason: Real merchant names make the demo more understandable and help preserve realistic workflows.

Implication: Merchants such as Amazon, Target, Whole Foods, Uber, and DoorDash may remain unless context makes them sensitive.

## Decision 006: Fabricate Sensitive Institution Names

Decision: Financial, health, legal, immigration, government, and insurance institutions should be replaced with realistic fabricated names.

Reason: These categories are high-sensitivity and should not be exposed in portfolio materials.

Implication: Redaction tests must check institution replacement.

## Decision 007: Semantic Objects Are Core

Decision: The primary stored product units are semantic objects such as tasks, updates, threads, financial vigilance items, opportunities, digests, noise items, institutional items, and reference items.

Reason: LLMs should reason over structured inbox intelligence rather than raw inbox chaos.

Implication: The database schema and assistant tools should be built around semantic objects.

## Decision 008: Deterministic Tools Are Required

Decision: Filtering, retrieval, totals, timelines, state transitions, and notification decisions must be handled by deterministic tools.

Reason: LLMs are useful for language and interpretation but should not be trusted for financial calculations or state truth.

Implication: Assistant answers must be grounded in tool outputs.

## Decision 009: Risky Actions Need Approval

Decision: MailBuddy may suggest but must not autonomously send, delete, unsubscribe, log into websites, or make purchases/payments.

Reason: Trust is central to the product. Automation errors in email can be costly.

Implication: Any risky future action requires explicit approval UI and audit logging.

## Decision 010: Python Backend

Decision: The backend will be Python FastAPI.

Reason: The project will use AI pipelines, OpenAI voice, evaluation scripts, Gmail ingestion, and semantic processing. Python is a strong fit for this intelligence layer.

Implication: Next.js remains the product surface; FastAPI owns Gmail, AI, semantic processing, and voice APIs.

## Decision 011: Next.js Frontend

Decision: The frontend will be a mobile-first Next.js web app.

Reason: The demo needs a polished web/mobile UI with microphone capture, assistant interaction, dashboards, and responsive layout.

Implication: Voice capture starts in the browser and routes audio to FastAPI.

## Decision 012: OpenAI Voice Pipeline

Decision: Voice support will use an OpenAI voice pipeline through the Python backend.

Reason: It creates a stronger AI-native portfolio demo than browser-only speech APIs and keeps transcription/assistant orchestration in one backend.

Implication: The app should support audio upload, transcription, assistant execution, and optional generated audio response.

## Decision 013: Gmail Fetch Cadence

Decision: MVP fetch behavior is initial sync of the last 90 days or 1,000 messages, manual sync, and 30-minute background sync while active.

Reason: This is sufficient for a demo without ingesting the entire mailbox.

Implication: Full Gmail Pub/Sub is future production scope.

## Decision 014: Notify Selectively In Production

Decision: Production should ingest new email immediately but notify only after semantic processing determines the item deserves attention.

Reason: Notifying for every new email recreates inbox overload.

Implication: Notification policy requires semantic categories, urgency scoring, user preferences, and tiers.

## Decision 015: Parallel Agent Workflow

Decision: The project should be built with parallel coding agents using explicit branch and module ownership.

Reason: The product has separable vertical slices: ingestion, semantic pipeline, assistant, voice, frontend, sanitization, and evaluation.

Implication: `AGENTS.md` defines plan mode, ownership, docs, and testing requirements.

## Decision 016: Rebuild Around Prior Real-Gmail Discovery

Decision: MailBuddyV1 is a spec-driven rebuild informed by the creator's prior real Gmail discovery after the synthetic v0 prototype.

Reason: The first prototype showed that synthetic demo emails were too clean. Real Gmail contains more noise, financial documents, receipts, institutional notices, incomplete merchant updates, link-limited details, and fragmented conversations than the original demo represented.

Implication: V1 should not optimize for idealized sample emails. It should model uncertainty, source evidence, link-limited states, and categories observed in real Gmail patterns.

## Decision 017: Choose Dual-Mode Semantic System

Decision: V1 will use a dual-mode architecture: a polished sanitized demo snapshot as the default walkthrough, plus live Gmail readonly ingest as technical proof. Both modes must feed the same semantic object layer.

Reason: The sanitized snapshot protects the demo from live-data fragility, while live ingest proves the system is not a static mock. A shared semantic object layer keeps the product honest and avoids two separate demo paths.

Implication: The main demo should never depend on a live Gmail sync succeeding in real time. Live Gmail ingest is proof of realism, not the primary walkthrough.

## Decision 018: Attention Today Is The Flagship

Decision: The first screen and primary demo moment is `Attention Today`.

Reason: The strongest product promise is not "AI email assistant." It is getting the user productive within seconds by turning Gmail overwhelm into a small number of source-backed decisions.

Implication: Tasks, updates, financial items, semantic threads, search, assistant, and evaluation should support the Attention Today story. Secondary features should not compete with it in V1.

## Decision 019: Defer Full Voice And Broad Extra Surfaces

Decision: Full voice assistant, full digest reader, opportunities, production push notifications, browser-assisted merchant fetching, autonomous unsubscribe, and native mobile are deferred from V1.

Reason: V1 must prove semantic Gmail intelligence, provenance, privacy, and evaluation in a two-week build window. These extra surfaces increase scope without improving the core demo enough.

Implication: Voice may be referenced as future scope, but it should not be a V1 success criterion. The portfolio artifact should be judged on the semantic object system and Attention Today workflow.

## Decision 020: Require Five-Minute Demo Script Before Implementation

Decision: Before implementation, write the exact five-minute demo script.

Reason: If the product story is not compelling on paper, adding features will not fix it. The script forces the team to identify the five Attention Today decisions, one task evidence drilldown, one link-limited update, one semantic thread, one search query, one assistant query, and one safety/eval proof point.

Implication: `TASKS.md` should treat the demo script as a pre-implementation gate.

## Decision 021: Build Snapshot Import Before Live Gmail Ingest

Decision: Implement sanitized snapshot import before live Gmail ingest.

Reason: Snapshot import lets UI, semantic objects, assistant tools, and evaluation develop without exposing private data or depending on OAuth readiness. Live Gmail ingest remains important, but it should prove the same pipeline after the safer path exists.

Implication: Engineering work should start with schema, snapshot import, redaction, and seeded semantic objects. Live Gmail readonly ingest should reuse that path rather than creating a separate demo pipeline.

## Decision 022: Use JSONB Typed Details For V1 Semantic Objects

Decision: V1 will use a shared `semantic_objects` table with typed `details` JSONB rather than separate fully normalized tables for every object subtype.

Reason: V1 needs speed and flexibility while object shapes are still being validated against real-pattern fixtures. A stable base object plus typed details keeps the schema coherent without over-normalizing too early.

Implication: Evaluation fixtures must validate each object type's required details. If a type stabilizes or needs heavy querying later, it can move to a dedicated table.

## Decision 023: Use Hybrid Search

Decision: Semantic search will use hybrid retrieval: vector similarity plus keyword/exact filters.

Reason: Email search needs both meaning and exactness. Embeddings help with concepts like "dental receipt," while keyword and structured filters are necessary for senders, dates, subjects, merchants, IDs, and categories.

Implication: Search should return semantic objects with source refs. Stateful operational questions should go through assistant tools rather than plain search.
