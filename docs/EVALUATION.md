# MailBuddyV1 Evaluation Strategy

## 1. Evaluation Goals

MailBuddyV1 must be useful, grounded, and demo-safe. Evaluation verifies that the system extracts the right semantic objects, avoids exposing sensitive data, answers from stored evidence, handles link-limited emails honestly, and uses deterministic tools for truth-sensitive work.

Evaluation is a V1 product requirement. It is part of the portfolio story and a hard gate before public screenshots or demo snapshots.

## 2. Evaluation Areas

Evaluate:

- secrets and private-path safety
- redaction and demo safety
- semantic classification
- task extraction
- update extraction and completeness-state detection
- financial item extraction
- institutional item classification
- semantic thread grouping
- noise/digest/reference classification
- semantic search relevance
- assistant groundedness
- deterministic tool correctness

Voice and production notification evaluation are future scope because full voice and push notifications are deferred from V1.

## 3. Fixture Strategy

Create sanitized fixtures that represent real Gmail patterns without private information.

Suggested fixture groups:

- `fixtures/gmail/source_emails.json`
- `fixtures/semantic/tasks.json`
- `fixtures/semantic/updates.json`
- `fixtures/semantic/financial_items.json`
- `fixtures/semantic/institutional_items.json`
- `fixtures/semantic/semantic_threads.json`
- `fixtures/semantic/noise_digest_reference.json`
- `fixtures/search/queries.json`
- `fixtures/assistant/questions.json`
- `fixtures/redaction/cases.json`

Each fixture should include:

- sanitized input email fields
- expected semantic object type
- expected extracted fields
- expected source refs
- expected redactions
- expected uncertainty flags
- expected assistant/search behavior when relevant

Fixtures should include both complete and incomplete evidence.

## 4. Redaction Hard Gate

Redaction tests are hard gates.

Verify:

- no raw email addresses remain
- no real phone numbers remain
- no real addresses remain
- real people names are replaced
- financial institution names are replaced
- health institution names are replaced
- government/legal/immigration/insurance names are replaced or generalized
- account numbers are altered
- order/confirmation numbers are altered
- raw email bodies are not stored long-term
- assistant responses only use sanitized display values
- search results only expose sanitized objects and source refs

Allow:

- ordinary non-sensitive merchant names when context is safe
- generalized categories
- realistic fabricated institution names
- relative semantic meaning

Failing redaction blocks:

- public screenshots
- curated demo snapshot publication
- hosted demo refresh

## 5. Secrets And Private-Path Safety

Secrets checks are hard gates before pushing implementation changes.

Verify:

- no `.env` files are committed
- no OAuth `credentials.json` is committed
- no OAuth `token.json` is committed
- no raw Gmail export is committed
- no local database file is committed
- no private snapshot is committed before redaction
- no OpenAI API key pattern appears in tracked files
- no Google OAuth client secret pattern appears in tracked files

Minimum ignored private paths:

- `data/private/`
- `data/raw/`
- `data/local/`
- `.env`
- `.env.*`
- `credentials.json`
- `token.json`
- `*.sqlite`
- `*.db`

These checks should run in CI once implementation starts.

## 6. Classification Quality

Track classification accuracy across:

- `task`
- `update`
- `financial_item`
- `institutional_item`
- `semantic_thread`
- `digest_item`
- `noise_item`
- `reference_item`

Important confusion cases:

- receipt vs financial alert
- newsletter vs reference item
- appointment confirmation vs task
- refund notice vs generic update
- school/government notice vs ordinary update
- marketing email vs digest candidate
- institutional notice vs suppressible noise

Metrics:

- object type accuracy
- multi-label accuracy where one email creates multiple objects
- false positive rate for Attention Today candidates
- false negative rate for high-priority objects

## 7. Task Extraction Quality

Measure whether MailBuddy correctly extracts:

- task title
- due date
- urgency
- status
- reason
- source refs
- evidence snippet
- uncertainty flags

Metrics:

- precision: extracted task is real
- recall: important task was not missed
- field accuracy: due date, urgency, and source
- confidence calibration: low confidence when evidence is weak

## 8. Update Quality

Measure whether MailBuddy correctly extracts operational updates.

Categories:

- deliveries
- refunds
- appointments
- reservations
- travel/events
- account or policy changes

Required evaluation:

- current state
- latest change
- timeline ordering
- action needed
- source refs
- completeness state

Completeness states:

- `complete_from_email`
- `partial_from_email`
- `details_behind_link`
- `requires_user_review`
- `unsupported_without_external_fetch`

Hard rule:

If an email only links to a merchant or institutional portal for details, the system must not fabricate hidden details.

## 9. Financial Item Quality

Evaluate:

- receipts
- subscriptions
- invoices
- payment reminders
- refund notices
- bank/card alerts
- tax/insurance/benefit documents
- unusual financial notices

Metrics:

- amount accuracy when present
- due date accuracy when present
- merchant/institution extraction
- calculation eligibility
- source refs
- sensitivity classification

Financial totals must be tested through deterministic tools, not LLM output.

## 10. Semantic Thread Quality

Evaluate semantic thread grouping separately from Gmail thread IDs.

Test cases:

- simple Gmail thread remains grouped
- changed subject line still groups when evidence is strong
- related institutional notices group by claim/form/topic
- unrelated messages with similar words do not merge
- ambiguous messages are flagged as possible related, not silently merged

Metrics:

- thread grouping precision
- thread grouping recall
- false merge rate
- missed merge rate
- merge explanation presence
- unresolved question detection
- waiting-on state accuracy

False merges are higher risk than missed merges in V1. Prefer conservative grouping.

## 11. Semantic Search Relevance

Evaluate search over semantic objects.

Test questions:

- Find my dental receipt.
- Show school forms.
- Find refund emails.
- Show immigration-related notices.
- Find tax documents.
- Show subscription renewals.

Criteria:

- top result relevance
- source ref presence
- correct object type
- exact-match behavior for merchant/domain/date-like terms
- no raw private data exposure

Search should use hybrid retrieval: vector similarity plus keyword/exact filters.

## 12. Assistant Groundedness

The assistant should answer from semantic objects and deterministic tools.

Test questions:

- What needs my attention today?
- What tasks are due soon?
- Which conversations need replies?
- Did I get a refund notice?
- Find my dental receipt.
- What financial items should I review?
- Show school-related tasks.
- What important emails did I miss this week?

Evaluation criteria:

- answer is correct
- answer includes relevant source objects
- answer does not invent facts
- answer says it does not know when data is missing
- answer identifies link-limited evidence
- calculations come from deterministic tools
- filters and timelines match stored data
- prompt-injection text inside emails is ignored as untrusted content
- assistant never follows email-body instructions that conflict with system or developer instructions

## 13. LLM Payload Safety

Evaluate what is sent to LLM APIs.

Verify:

- planner receives sanitized context only
- finalizer receives sanitized tool results only
- raw Gmail body text is not sent after the redaction boundary
- source snippets in LLM payloads are sanitized
- assistant audit records do not contain raw private email bodies
- tool schemas are allowlisted

## 14. Deterministic Tool Tests

Test deterministic tools independently:

- date filtering
- priority ranking
- Attention Today item selection
- financial totals
- merchant/category aggregation
- update timeline ordering
- task state transitions
- unresolved semantic thread detection
- source ref lookup
- search filters

LLM tests should not verify deterministic logic.

## 15. Evaluation Report

Create an evaluation report command that outputs:

- fixture count
- secrets/private-path pass/fail
- semantic object count
- classification accuracy
- task extraction precision/recall
- update completeness-state accuracy
- financial extraction checks
- semantic thread grouping checks
- semantic search relevance checks
- redaction pass/fail
- assistant groundedness pass/fail
- deterministic tool test results
- known failure cases
- examples of link-limited emails

The app should also surface evaluation status in the Data Safety / Evaluation screen.

## 16. Known Failure Case Registry

Track failure cases explicitly.

Each failure case should include:

- fixture id or source pattern
- category
- expected behavior
- actual behavior
- severity
- fix status
- linked test, when available

Known failure cases are acceptable in the portfolio if they are visible, bounded, and honest.
