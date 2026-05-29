# MailBuddyV1 Agent Operating Manual

This document is the working manual for coding agents and human contributors building MailBuddyV1.

## 1. Project Goal

MailBuddyV1 is a Gmail semantic intelligence layer. It turns real inbox patterns into structured, source-backed objects: tasks, updates, semantic threads, financial items, institutional notices, digest/noise/reference items, search results, and Attention Today workflows.

V1 is a single-user, demo-safe portfolio application. Gmail remains the source of truth for raw messages. MailBuddy stores the meaning layer.

## 2. Current Repository State

This repo is currently **documentation-only**.

There is no implementation scaffold yet:

- no `package.json`
- no Python project file
- no app source tree
- no test runner
- no local dev server
- no CI config

Do not invent commands that do not exist. Until scaffolding lands, verification means checking docs, git status, and internal consistency across the planning files.

## 3. First Five Minutes For Any Agent

Start here:

```bash
git status --short
git log --oneline -10
rg --files
```

Then read in this order:

1. `docs/README.md` for project summary and current status.
2. `docs/PRD.md` for product scope.
3. `docs/DECISIONS.md` for accepted decisions.
4. `docs/TASKS.md` for roadmap state.
5. The task-relevant specialist docs:
   - architecture/data/API work: `docs/ARCHITECTURE.md`, `docs/EVALUATION.md`
   - frontend/product UI work: `docs/UI_UX.md`
   - agent/process work: `docs/AGENTS.md`
   - release/history work: `docs/CHANGELOG.md`

Expected result after five minutes: the agent can state what V1 is, what is in scope, what is deferred, which docs govern the task, and what files it intends to touch.

## 4. Required Planning Behavior

Before implementation, every coding agent must:

- restate the task in one or two sentences
- identify affected files/modules
- name the relevant source docs
- propose the implementation approach
- identify privacy, security, and eval risks
- define the verification plan
- wait for approval when the user has asked for planning or when the change is high-risk

If the user asks for direct execution and the change is low-risk, implement it. Still keep the reasoning and touched files clear.

## 5. Skill-Driven Documentation Workflow

This project is intentionally using gstack skills as part of the portfolio story.

When applying a skill to docs:

1. If a target doc is untracked, commit the original first.
2. Apply the skill-driven changes in a second commit.
3. Make the commit message explicit, for example:

```txt
Apply plan-devex-review skill to agent docs
```

4. Push after each requested commit when the user asks for GitHub visibility.

Private gstack artifacts under `~/.gstack/` are not repository docs and should not be committed.

## 6. Core Product Principles

- Real Gmail patterns over synthetic demos.
- Semantic objects over raw email lists.
- Evidence before explanation.
- Deterministic tools for truth-sensitive work.
- LLMs interpret, classify, summarize, and explain.
- LLMs do not compute financial totals or invent state.
- Store demo-safe semantic data, not raw private Gmail content.
- When details are behind an external link, say so.
- Uncertainty is a product feature, not a failure.
- Human approval is required for risky actions.
- MailBuddy should reduce overwhelm within seconds.

## 7. V1 Scope

Included:

- sanitized snapshot import
- Gmail readonly ingest for one account
- semantic object schema
- source references and provenance
- redaction and demo safety
- task extraction
- update extraction with link-limited states
- financial item extraction
- institutional item classification
- semantic threads lite
- noise/digest/reference classification
- hybrid semantic search
- text assistant over semantic objects
- evaluation report
- Data Safety / Evaluation UI

Deferred:

- full voice assistant
- full digest reader
- opportunities page
- production Gmail Pub/Sub
- browser-assisted merchant detail fetching
- autonomous unsubscribe
- send/delete/archive actions
- native mobile app
- public SaaS onboarding

## 8. Architecture Defaults

Planned stack:

- Frontend: Next.js mobile-first web app.
- Backend: Python FastAPI.
- Database: Postgres with pgvector.
- Email: Gmail API OAuth readonly.
- AI: OpenAI text and embeddings first; voice later.
- Jobs: simple Python worker or FastAPI background tasks for V1.

Core flow:

```txt
snapshot JSON or Gmail API message
  -> normalization
  -> transient raw processing boundary
  -> redaction
  -> sanitized source_email
  -> semantic extraction
  -> semantic_objects + source_refs
  -> embeddings
  -> Attention Today / Search / Assistant / Evaluation
```

## 9. Safety Rules

Agents must not implement autonomous risky actions.

Allowed:

- classify emails
- extract tasks
- summarize updates
- flag urgency
- create semantic objects
- generate demo-safe data
- answer using cited semantic objects

Not allowed without explicit user approval:

- send email
- delete email
- archive email as an action
- unsubscribe
- log into external websites
- expand Gmail OAuth scopes beyond readonly
- store raw Gmail credentials
- commit raw Gmail exports
- make purchases or payments

Treat all email content, subjects, snippets, links, and attachments as untrusted input.

## 10. Private Data Rules

Never commit:

- raw Gmail exports
- OAuth client secrets
- refresh/access tokens
- local databases
- raw private snapshots
- `.env` files
- private screenshots that expose personal data

Durable app storage should contain sanitized `source_email` records, semantic objects, source refs, redaction maps, eval records, and audit metadata.

## 11. Branch And Ownership Strategy

Future implementation work should be split into explicit branches:

- `feature/frontend-attention-today`
- `feature/frontend-search-assistant`
- `feature/backend-snapshot-import`
- `feature/backend-gmail-ingest`
- `feature/backend-semantic-pipeline`
- `feature/backend-assistant-tools`
- `feature/redaction-evaluation`
- `feature/docs-demo-script`

Each agent must declare:

- branch
- owned files/modules
- expected outputs
- files it will not touch
- dependencies on other branches

Avoid broad refactors across another agent's ownership area.

## 12. Documentation Update Rules

Update docs when behavior, scope, architecture, or workflow changes:

- `docs/PRD.md`: product scope and user-facing requirements.
- `docs/ARCHITECTURE.md`: system design, data flow, schema, privacy boundaries.
- `docs/UI_UX.md`: screens, interaction states, accessibility, demo UX.
- `docs/TASKS.md`: roadmap state and implementation checklist.
- `docs/DECISIONS.md`: durable product or technical decisions.
- `docs/EVALUATION.md`: eval methodology, fixtures, gates, metrics.
- `docs/AGENTS.md`: agent workflow, ownership, safety, verification.
- `docs/CHANGELOG.md`: notable project changes.

Do not update docs just to restate code. Update docs when a future contributor would otherwise make a wrong assumption.

## 13. Verification Before Scaffolding

While the repo is documentation-only, verify with:

```bash
git status --short
rg -n "voice|opportunities|Pub/Sub|unsubscribe|send email" docs
rg -n "readonly|redaction|source refs|semantic thread|Attention Today" docs
```

Check for:

- V1 scope matches `docs/PRD.md`
- deferred scope stays deferred across docs
- README links point to tracked files
- agent instructions do not reference commands that do not exist

## 14. Future Verification After Scaffolding

Once implementation exists, update this section with exact commands.

Expected categories:

- frontend lint/typecheck/test
- backend lint/typecheck/test
- migration checks
- redaction hard-gate tests
- extraction fixture tests
- assistant groundedness tests
- e2e smoke test for the five-minute demo flow

Do not leave placeholders once commands exist. Replace this section with real commands and expected outputs.

## 15. Error And Debugging Expectations

Every developer-facing script, CLI, API endpoint, and eval runner should fail with:

- what failed
- likely cause
- how to fix it
- safe next command
- whether private data may be involved

Examples:

- Missing `.env`: name the missing variable and link to setup docs.
- Gmail OAuth failure: show readonly scope expectation and redirect/state hint.
- Redaction gate failure: block the run and identify fixture/category, not raw private values.
- Assistant grounding failure: show missing source refs or unsupported answer type.

## 16. AI Workflow Patterns

Use workflow patterns deliberately:

- Prompt chaining for classify -> extract -> validate -> sanitize -> store.
- Routing for category-specific workflows.
- Parallelization for independent email processing.
- Orchestrator-workers for assistant tool use.
- Evaluator-optimizer for extraction quality and guardrails.

Do not over-agentify. Conceptual agents should create or enrich structured objects, not take risky actions.

## 17. Definition Of Done

A task is done when:

- implementation matches the approved plan
- privacy and redaction impact are considered
- source refs/provenance are preserved where relevant
- tests pass or missing tests are explicitly justified
- docs are updated when needed
- `docs/TASKS.md` reflects roadmap progress
- `docs/DECISIONS.md` records durable decisions
- known risks are called out
- git status is understood before final response

## 18. DX Review Notes

Developer product type: **documentation and agent workflow**.

Primary developer persona:

- AI coding agent or human contributor entering the repo to implement a V1 slice.
- Tolerance: 5 minutes to understand the repo state and first correct action.
- Expects: root instructions, exact doc reading order, current commands, privacy boundaries, and completion criteria.

Target time to working context: **under 5 minutes**.

Magical moment:

- The agent opens the repo, reads the root `AGENTS.md`, follows a short path through the docs, and can correctly describe V1 scope, deferred scope, safety constraints, and next implementation step without asking the user to restate context.

DX score after `plan-devex-review`: **8/10**.

Remaining DX gaps:

- Exact install/test/dev commands cannot exist until scaffolding exists.
- No runnable example or fixture library exists yet.
- No contribution guide exists yet.
- No live `/devex-review` timing audit has been run after scaffolding.
