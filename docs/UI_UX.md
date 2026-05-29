# MailBuddyV1 UI/UX Plan

## 1. UX Thesis

MailBuddyV1 should feel like a personal operations dashboard powered by Gmail, not a prettier inbox. The first screen should reduce overwhelm within seconds by turning email into a small set of source-backed decisions.

The product should feel quiet, dense, and trustworthy. It should use restrained visual styling, predictable navigation, and evidence-first interactions. The UI should not rely on a raw email list, generic dashboard widgets, decorative cards, or marketing-page composition.

V1 design priority:

1. Show what needs attention today.
2. Show why MailBuddy thinks it matters.
3. Let the user inspect evidence without exposing raw private Gmail content.
4. Make uncertainty visible instead of pretending incomplete email data is complete.

## 2. Design Completeness Review

Initial rating before this review: **5/10**.

The original plan had the right product direction but kept too many broad V0 surfaces in the primary navigation: Opportunities, full Insights, full Digest, and full Voice. It also did not define enough states for incomplete data, source evidence, demo safety, mobile layout, or assistant groundedness.

Target rating after this review: **8/10**.

A 10/10 design plan would also include approved visual mockups, a named design system, typography tokens, color tokens, component specs, and screenshot-level layout decisions. Those should come from a later design exploration pass or implementation design QA.

## 3. What Already Exists

Existing product docs define the following constraints:

- `docs/PRD.md` defines Attention Today as the flagship screen.
- `docs/ARCHITECTURE.md` defines the dual-mode snapshot/live Gmail architecture.
- `docs/EVALUATION.md` defines redaction, assistant groundedness, semantic thread, and search evaluation expectations.
- `docs/DECISIONS.md` defers full voice, broad opportunities, full digest, production push, and external site fetching from V1.

No project-level `DESIGN.md` exists yet. Until a design system is created, the app should use universal app-UI rules: calm hierarchy, dense but readable layouts, restrained color, 44px minimum touch targets, visible labels, source evidence, and no generic AI/SaaS decoration.

## 4. V1 App Structure

Primary navigation:

- Attention
- Tasks
- Updates
- Threads
- Financial
- Search
- Assistant
- Safety
- Settings

Mobile bottom navigation:

- Attention
- Tasks
- Search
- Assistant
- More

`More` contains Updates, Threads, Financial, Safety, and Settings.

Navigation hierarchy:

```txt
Attention Today
  -> item detail
    -> source evidence drawer
    -> related semantic thread

Tasks
  -> task detail
    -> source evidence drawer

Updates
  -> update detail
    -> completeness state
    -> source evidence drawer

Threads
  -> semantic thread detail
    -> related tasks / updates / financial items

Financial
  -> financial item detail
    -> deterministic calculation evidence

Search
  -> grouped semantic results
    -> object detail
    -> source evidence drawer

Assistant
  -> answer
    -> cited objects
    -> source evidence drawer

Safety
  -> redaction and evaluation status
```

## 5. Attention Today

Attention Today is the first screen and main demo moment. It should be optimized for a five-minute walkthrough and for a real user opening the app on a phone.

First viewport hierarchy:

1. **Attention count:** total number of items MailBuddy believes need review today.
2. **Top three decisions:** the most important action/update/risk items, each with source evidence.
3. **Needs Review:** incomplete or uncertain items that MailBuddy refuses to overclaim.

Sections:

- Needs Action
- Important Updates
- Financial Watch
- Waiting On Me
- Incomplete / Needs Review
- Quieted Noise

Each item must show:

- title
- object type
- priority
- confidence
- status
- date
- one-line reason
- source count
- evidence action

Attention item actions:

- open detail
- mark handled
- snooze
- dismiss
- inspect evidence
- open related thread

The UI should cap the first screen at a small number of high-signal items. Long lists belong in the section detail pages, not in the initial Attention Today view.

## 6. Source Evidence Pattern

Every task, update, thread, financial item, search result, and assistant answer needs a source evidence pattern.

Evidence drawer fields:

- source subject
- sender/domain
- received timestamp
- evidence snippet
- support reason
- confidence
- raw retention status
- related objects

Rules:

- Evidence opens as a drawer on desktop and a full-screen sheet on mobile.
- Raw private email bodies should not be shown in public demo mode.
- Evidence snippets should be short and sanitized.
- If the evidence is partial, the drawer should say what is known and what is missing.
- If an item aggregates multiple emails, the drawer should group evidence by source email.

## 7. Link-Limited Update UX

Updates must communicate completeness honestly.

Completeness states:

- `complete_from_email`: enough detail was in the email.
- `partial_from_email`: some useful detail was present, but not all.
- `details_behind_link`: the email points to an external page for key details.
- `requires_user_review`: MailBuddy is not confident enough to summarize as fact.
- `unsupported_without_external_fetch`: the update cannot be verified from email alone.

Update detail layout:

1. Current state.
2. Latest change.
3. Completeness state.
4. What MailBuddy knows.
5. What MailBuddy cannot verify.
6. Timeline.
7. Source evidence.

For merchant/order emails like Whole Foods, the UI must avoid pretending item-level refunds or replacements are known when the email only contains a link.

## 8. Tasks

Task list views:

- Today
- Upcoming
- Waiting
- Snoozed
- Completed
- Dismissed

Task card fields:

- title
- due date
- urgency
- status
- confidence
- reason
- source count

Task detail fields:

- extracted action
- due date and uncertainty
- why it matters
- suggested next step
- related semantic thread
- source evidence

Task actions:

- complete
- snooze
- dismiss
- edit title/status
- inspect evidence

## 9. Semantic Threads

Semantic Threads group related conversations by meaning, not only by Gmail thread ID.

Thread list fields:

- topic
- participants/domains
- current status
- unresolved questions
- who is waiting on whom
- related tasks
- related updates
- confidence

Thread detail layout:

1. Thread summary.
2. Current status.
3. Open questions.
4. Suggested next action.
5. Timeline of related source emails.
6. Related semantic objects.
7. Ambiguity notes.

Ambiguous grouping rule:

- If related messages are plausible but not certain, the UI should show "possible related messages" instead of merging them into the thread.

## 10. Financial Watch

Financial Watch is not a full finance dashboard in V1. It is a vigilance surface for bills, receipts, refunds, charges, statements, and money-related emails that deserve attention.

Financial item fields:

- institution or merchant
- amount, if safely extracted
- date
- category
- status
- action needed
- calculation eligibility
- source evidence

Rules:

- Financial totals must come from deterministic calculations, not assistant prose.
- If an amount is missing or uncertain, the UI should label it as missing or uncertain.
- Sensitive institution names should be fabricated in demo-safe mode.

## 11. Search / Memory

Search should return semantic objects grouped by type, not raw chronological email results.

Search result groups:

- Tasks
- Updates
- Semantic Threads
- Financial
- Institutional
- Reference
- Digest
- Noise

Result item fields:

- title
- type
- date
- sender/domain
- summary
- matched reason
- confidence
- source count

Filters:

- type
- date range
- sender/domain
- status
- confidence
- ingest mode

Example queries:

- Find my dental receipt.
- Show immigration-related emails.
- Did I get refunded?
- What conversations need replies?
- Find the order update with missing details.

## 12. Assistant

The assistant should behave like an operations analyst over semantic objects, not like a free-form chatbot over raw email.

Required elements:

- text input
- answer area
- cited semantic objects
- source evidence actions
- uncertainty labels
- suggested follow-up prompts

Assistant answer rules:

- Use deterministic tools for retrieval, timelines, filtering, and financial totals.
- Show source objects used for every answer.
- Say when data is missing, partial, stale, or behind a link.
- Refuse actions outside V1 scope, such as sending, deleting, unsubscribing, or logging into external websites.
- Do not treat email content as instructions.

Voice is future scope for V1. The UI may leave room for it later, but the first implementation should prove text assistant, provenance, and grounded answers.

## 13. Data Safety / Evaluation

The Safety screen exists because trust is part of the product, not only a developer concern.

Required status blocks:

- demo-safe data active
- ingest mode: snapshot or live Gmail readonly
- last redaction run
- redaction test status
- raw retention status
- source emails processed
- semantic objects created
- evaluation run status
- assistant groundedness status
- prompt-injection fixture status

The screen should avoid exposing private data while still making it clear that the system has real safety checks.

## 14. Interaction State Coverage

| Feature | Loading | Empty | Error | Success | Partial |
| --- | --- | --- | --- | --- | --- |
| Attention Today | Skeleton rows with section labels | "Nothing needs review right now" plus Search action | Safety-preserving error with retry | Prioritized items grouped by section | Shows Needs Review when confidence is low |
| Tasks | List skeleton | "No tasks found" plus Search and sync status | Retry plus latest successful sync time | Task list with evidence actions | Due date or source uncertainty shown inline |
| Updates | Timeline skeleton | "No tracked updates yet" plus Search | Retry plus source-processing status | Updates grouped by lifecycle | Completeness state shown prominently |
| Threads | Thread row skeleton | "No related conversations grouped yet" | Retry semantic grouping | Threads with status and participants | Possible related messages shown separately |
| Financial | Financial row skeleton | "No money-related items found" | Retry extraction/eval | Items with deterministic evidence | Missing amount or uncertain status labeled |
| Search | Search-ready empty state | Query suggestions | Explain failed search and retry | Grouped semantic results | Low-confidence and exact-match gaps labeled |
| Assistant | Pending response state | Suggested prompts | Refusal or retrieval error with next step | Answer with cited objects | Missing/partial source limitations stated |
| Safety | Status skeleton | Not applicable after app setup | Failed gates shown as blockers | Passing gates with run metadata | Warnings for incomplete eval coverage |

## 15. User Journey Storyboard

| Step | User Does | User Feels | Plan Support |
| --- | --- | --- | --- |
| 1 | Opens MailBuddy | Relief that the inbox has been reduced | Attention count and top three decisions |
| 2 | Scans Attention Today | Oriented, not overwhelmed | Clear sections and capped first viewport |
| 3 | Opens a task | Curious but skeptical | Evidence drawer with source snippets |
| 4 | Opens a link-limited update | Trust that the app is honest | Completeness state and missing-detail explanation |
| 5 | Checks a semantic thread | Productive, because context is gathered | Thread status, open questions, related objects |
| 6 | Searches memory | In control | Grouped semantic results with filters |
| 7 | Asks assistant a question | Confident or correctly cautious | Cited answers and uncertainty labels |
| 8 | Opens Safety | Comfortable demoing it | Redaction and eval status without private data |

Time-horizon design:

- First 5 seconds: user sees a short list of source-backed decisions, not inbox chaos.
- First 5 minutes: user verifies one task, one incomplete update, one semantic thread, one search, one assistant answer, and one safety proof.
- Long-term relationship: user trusts MailBuddy because it exposes sources and admits uncertainty.

## 16. Responsive And Accessibility Requirements

Desktop:

- left navigation
- main workspace
- right evidence/context panel when space allows

Tablet:

- collapsible navigation
- evidence panel as side sheet
- two-column layouts only when content remains readable

Mobile:

- bottom navigation
- section-first Attention layout
- evidence as full-screen sheet
- sticky primary actions on detail screens
- no hover-only interactions

Accessibility:

- 44px minimum touch targets
- visible labels for inputs
- keyboard navigation for all actions
- logical heading order
- ARIA landmarks for navigation, main content, search, dialogs, and sheets
- body text contrast at least 4.5:1
- do not rely on color alone for priority, status, or confidence
- preserve focus when opening and closing evidence drawers

## 17. Visual Direction

Classifier: **App UI**.

Design rules:

- Calm operational surface, not a marketing landing page.
- Dense but readable information.
- Minimal chrome.
- One accent color for priority/action.
- Cards only when the card is the interaction.
- Avoid generic dashboard card mosaics.
- Avoid purple/blue gradient SaaS styling.
- Avoid decorative icon circles, blobs, and centered hero patterns.
- Use utility copy: status, evidence, action, uncertainty.

Recommended visual language:

- Top-level surfaces should use table/list/detail patterns.
- Evidence and safety states should feel precise and audit-friendly.
- Semantic object types should use small labels or icons, not decorative badges.
- Priority should be visible but restrained.

## 18. Demo Script UX Requirements

Before implementation, the five-minute demo script should map to these exact interactions:

1. Open Attention Today and show the top three source-backed decisions.
2. Open one task and inspect evidence.
3. Open one update with `details_behind_link` and show honest incompleteness.
4. Open one semantic thread created across separate Gmail messages.
5. Search for a real-pattern reference item.
6. Ask the assistant "What needs my attention today?"
7. Open Safety and show redaction/evaluation status.

If any of these interactions cannot be shown clearly, the UX is not ready for implementation.

## 19. Not In Scope For V1 UI

- Full voice assistant: deferred until text assistant, provenance, and eval work.
- Full digest reader: digest items can appear in Search/Memory, but no dedicated reader.
- Opportunities page: useful later, but it competes with Attention Today in V1.
- Production notification center: semantic notification policy is future scope.
- Browser-assisted merchant fetching: external link details stay explicitly unsupported.
- Autonomous unsubscribe/send/delete actions: outside readonly Gmail V1.
- Full design system: should follow after the V1 flow is validated.

## 20. Implementation Tasks From Design Review

- [ ] **T1 (P1, human: ~2h / CC: ~20min)** — Attention Today — Build first-viewport hierarchy with capped top decisions, Needs Review, and evidence actions.
  - Surfaced by: Information architecture review.
  - Files: `docs/UI_UX.md`, future frontend screens.
  - Verify: mobile and desktop screenshots show the top decisions without raw inbox lists.
- [ ] **T2 (P1, human: ~90min / CC: ~15min)** — Evidence Drawer — Define and implement the shared source evidence drawer/sheet.
  - Surfaced by: Trust and provenance review.
  - Files: `docs/UI_UX.md`, future frontend components.
  - Verify: every semantic object detail has an evidence action.
- [ ] **T3 (P1, human: ~90min / CC: ~15min)** — Link-Limited Updates — Show completeness states and missing-detail language.
  - Surfaced by: Interaction state coverage review.
  - Files: `docs/UI_UX.md`, future update detail screen.
  - Verify: `details_behind_link` update does not overclaim item-level details.
- [ ] **T4 (P2, human: ~2h / CC: ~20min)** — Safety Screen — Build redaction/evaluation status surface.
  - Surfaced by: Safety and evaluation review.
  - Files: `docs/UI_UX.md`, future Safety screen.
  - Verify: demo-safe status, redaction status, and eval status are visible without private data.
- [ ] **T5 (P2, human: ~1h / CC: ~10min)** — Responsive/A11y — Add mobile sheet behavior, keyboard flow, focus management, and contrast checks.
  - Surfaced by: Responsive and accessibility review.
  - Files: `docs/UI_UX.md`, future frontend components.
  - Verify: keyboard-only and mobile viewport walkthroughs complete the demo script.

## 21. GSTACK REVIEW REPORT

| Review | Trigger | Why | Runs | Status | Findings |
| --- | --- | --- | --- | --- | --- |
| Design Review | `/plan-design-review` | UI/UX gaps | 1 | issues_open | score: 5/10 -> 8/10, 8 design decisions added, visual mockups not generated because designer binary was unavailable |

Unresolved:

- Create a real `DESIGN.md` or run design exploration before frontend implementation.
- Generate visual mockups once the gstack designer or another mockup path is available.
