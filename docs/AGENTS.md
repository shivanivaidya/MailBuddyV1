# AGENTS.md

This file is the universal instruction document for coding agents working on MailBuddy.

## 1. Project Goal

MailBuddy is an AI-native inbox operating system that turns Gmail into structured semantic workflows. The MVP is a single-account portfolio/demo app that connects to the creator's Gmail, extracts semantic objects, stores only demo-safe altered data, and provides a mobile-first dashboard plus text/voice assistant.

## 2. Required Plan Mode

Before implementation, every coding agent must enter plan mode.

Plan mode requires:

- read `AGENTS.md`
- read the relevant docs for the task
- restate the task
- identify affected files/modules
- propose architecture or implementation approach
- identify risks
- define test plan
- wait for approval before implementation

Do not begin code changes until implementation is approved.

## 3. Core Architecture Principles

- Structure before reasoning.
- Deterministic truth before AI phrasing.
- Semantic objects over raw email retrieval.
- Constrained tools over open-ended agents.
- Trustworthiness over maximum automation.
- Human approval for risky actions.
- AI should reduce anxiety, not create more.
- Gmail stores messages; MailBuddy stores the meaning layer.

## 4. Stack

MVP stack:

- Frontend: Next.js mobile-first web app.
- Backend: Python FastAPI.
- Database: Postgres with pgvector.
- Email: Gmail API OAuth.
- AI: OpenAI text, embeddings, and voice APIs.
- Jobs: simple Python worker or FastAPI background tasks at first.

## 5. Demo Scope

The MVP is demo-mode-only:

- Gmail-only.
- Single Gmail account owned by the project creator.
- Store sanitized semantic objects, not raw private mailbox data.
- Keep ordinary merchant names when non-sensitive.
- Fabricate financial, health, legal, immigration, government, and insurance institution names.
- Redact or alter people names, emails, phone numbers, addresses, account numbers, order IDs, confirmation numbers, and sensitive notes.

## 6. Safety Rules

Agents must not implement autonomous risky actions.

Allowed:

- classify emails
- extract tasks
- summarize updates
- draft replies
- suggest unsubscribe
- flag urgency
- create semantic objects
- generate demo-safe data

Not allowed without explicit user approval:

- send email
- delete email
- archive email as an action
- unsubscribe
- log into external websites
- store raw Gmail credentials
- make purchases or payments

## 7. Branch And Parallel Agent Strategy

Future work should be split into explicit parallel branches:

- `feature/backend-gmail-ingestion`
- `feature/backend-semantic-pipeline`
- `feature/backend-assistant-api`
- `feature/backend-voice-pipeline`
- `feature/frontend-dashboard`
- `feature/frontend-tasks-updates`
- `feature/frontend-assistant-voice`
- `feature/demo-sanitization`
- `feature/evaluation-tests`
- `feature/docs`

Each agent must declare:

- branch
- owned files/modules
- expected outputs
- files it will not touch
- dependencies on other branches

Agents should avoid broad refactors across another agent's ownership area.

## 8. Documentation Update Rules

Update docs when behavior or architecture changes:

- `PRD.md` for product scope and user-facing requirements.
- `ARCHITECTURE.md` for system design and data flow.
- `UI_UX.md` for screens and interaction behavior.
- `TASKS.md` for roadmap state.
- `DECISIONS.md` for major product or technical decisions.
- `EVALUATION.md` for eval methodology or gates.
- `CHANGELOG.md` for notable project changes.

## 9. Testing Requirements

Every feature should include tests appropriate to risk.

Required test categories:

- backend unit tests for deterministic tools
- schema/model tests
- redaction tests
- extraction fixture tests
- assistant groundedness tests
- API contract tests
- frontend component tests where practical
- end-to-end smoke tests for key demo flows

Redaction tests are hard gates before portfolio screenshots or public demos.

## 10. AI Workflow Patterns

Use workflow patterns deliberately:

- Prompt chaining for classify -> extract -> validate -> sanitize -> store.
- Routing for category-specific workflows.
- Parallelization for independent email processing.
- Orchestrator-workers for assistant tool use.
- Evaluator-optimizer for extraction quality and guardrails.

Do not over-agentify. Conceptual agents should create or enrich structured objects, not take risky actions.

## 11. Gmail Fetch Policy

MVP:

- initial sync: last 90 days or last 1,000 messages
- manual sync: available in UI
- background sync: every 30 minutes while backend is active
- incremental sync: track Gmail IDs and `historyId`

Production:

- Gmail Pub/Sub
- history API
- notification decision engine after semantic processing

## 12. Code Quality Expectations

- Prefer clear, boring architecture.
- Keep modules small and owned.
- Use typed schemas for API contracts and semantic objects.
- Keep LLM prompts versioned and testable.
- Do not hide deterministic logic inside prompts.
- Preserve provenance from every AI-created object to source email metadata.
- Keep secrets out of the repo.

## 13. Definition Of Done

A task is done when:

- implementation matches the approved plan
- tests pass or documented blockers are explained
- redaction impact is considered
- docs are updated when needed
- affected files are summarized
- known risks are called out
