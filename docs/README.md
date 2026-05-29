# MailBuddyV1

MailBuddyV1 is a semantic intelligence layer for Gmail. It turns real inbox patterns into source-backed tasks, updates, semantic threads, financial items, institutional notices, search results, and attention workflows.

MailBuddy is not a Gmail replacement. Gmail stores raw email. MailBuddy stores the meaning layer.

The V1 product promise:

> Get productive within seconds by turning Gmail overwhelm into a small set of source-backed decisions.

## Product Direction

V1 is a portfolio/demo rebuild informed by a prior real-Gmail discovery pass after an initial synthetic prototype.

The important discovery was that real Gmail is messier than synthetic demos:

- many messages are noise, receipts, account notices, newsletters, or reference material
- many merchant and order emails contain partial information and link out for details
- financial and institutional messages need stronger privacy handling
- Gmail thread IDs are not enough to model real-world workflows
- useful answers require durable semantic objects, source evidence, and uncertainty handling

## V1 Strategy

MailBuddyV1 uses a dual-mode semantic system:

- **Sanitized demo snapshot:** the default polished walkthrough.
- **Live Gmail readonly ingest:** technical proof that the pipeline works on real data.

Both modes feed the same semantic object layer.

## Core Experience

The flagship screen is **Attention Today**.

It should answer:

- what needs action now
- what changed
- what money-related items deserve review
- who may be waiting on the user
- what was quieted as noise
- what needs review because evidence is incomplete or behind a link

Primary V1 surfaces:

- Attention Today
- Tasks
- Updates
- Semantic Threads
- Financial
- Search / Memory
- Assistant
- Data Safety / Evaluation

## Scope

Included in V1:

- Gmail readonly ingestion for one account
- sanitized snapshot import
- semantic object schema
- source references and provenance
- redaction and demo safety
- task extraction
- update extraction with link-limited states
- financial item extraction
- semantic threads lite
- noise/digest classification for attention filtering
- semantic object search
- assistant over semantic objects
- evaluation report

Deferred:

- full voice assistant
- full digest reader
- opportunities page
- production push notifications
- browser-assisted merchant detail fetching
- autonomous unsubscribe
- send/delete/archive actions
- native mobile app
- public SaaS onboarding

## Documentation

- [PRD.md](./PRD.md): product requirements and V1 scope.
- [ARCHITECTURE.md](./ARCHITECTURE.md): system design, ingestion, semantic object layer, assistant, privacy, and evaluation.
- [UI_UX.md](./UI_UX.md): product surfaces and interaction behavior.
- [DEMO_SCRIPT.md](./DEMO_SCRIPT.md): five-minute demo script, required seeded objects, and acceptance checks.
- [AGENTS.md](./AGENTS.md): instructions for coding agents working on this project.
- [TASKS.md](./TASKS.md): implementation roadmap.
- [DECISIONS.md](./DECISIONS.md): product and architecture decision log.
- [EVALUATION.md](./EVALUATION.md): AI quality, redaction, assistant, semantic search, and grounding evaluation.
- [CHANGELOG.md](./CHANGELOG.md): project history.

## Current Status

Foundation docs are being updated through gstack review skills before implementation starts. The five-minute demo script is now the acceptance test for the first implementation specs.

## Agent Quick Start

This repo is currently documentation-only. There is no app scaffold, package manager, test runner, or local dev server yet.

For coding agents and contributors:

```bash
git status --short
git log --oneline -10
rg --files
```

Then read:

1. [Root AGENTS.md](../AGENTS.md): root agent entry point.
2. [docs/AGENTS.md](./AGENTS.md): full agent operating manual.
3. [TASKS.md](./TASKS.md): current roadmap.
4. The task-relevant specialist docs.

Target time to working context: under 5 minutes.
