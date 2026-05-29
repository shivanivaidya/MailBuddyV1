# MailBuddyV1 Agent Entry Point

This root file exists so coding agents that auto-load `AGENTS.md` get the project rules immediately.

Before making changes, read:

1. `docs/AGENTS.md`
2. `docs/README.md`
3. the task-relevant plan docs listed in `docs/AGENTS.md`

Current state: this repo is still documentation-only. There is no app scaffold, package manager, test runner, or local dev command yet.

Default workflow:

1. Confirm the requested scope and affected docs/code areas.
2. Preserve original untracked planning docs in their own commit before applying skill-driven changes.
3. Keep skill-driven commits explicit, for example `Apply plan-devex-review skill to agent docs`.
4. Do not touch raw Gmail data, secrets, OAuth tokens, or private exports.
5. Update `docs/TASKS.md` and `docs/DECISIONS.md` when scope or architecture changes.

See `docs/AGENTS.md` for the full operating manual.
