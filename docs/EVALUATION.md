# MailBuddy Evaluation Strategy

## 1. Evaluation Goals

MailBuddy's AI must be useful, grounded, and demo-safe. Evaluation should verify that the system extracts the right semantic objects, avoids exposing sensitive data, answers from stored evidence, and uses deterministic tools for truth-sensitive work.

## 2. Evaluation Areas

Evaluate:

- extraction quality
- classification quality
- assistant correctness
- redaction/demo safety
- deterministic tool correctness
- notification tier decisions
- voice pipeline quality

## 3. Fixture Strategy

Create sanitized fixtures in the repo. Fixtures should represent realistic email patterns without private information.

Suggested fixture groups:

- `fixtures/gmail/tasks.json`
- `fixtures/gmail/updates.json`
- `fixtures/gmail/financial.json`
- `fixtures/gmail/health.json`
- `fixtures/gmail/institutional.json`
- `fixtures/gmail/digest.json`
- `fixtures/gmail/noise.json`
- `fixtures/gmail/opportunities.json`
- `fixtures/gmail/threads.json`

Each fixture should include:

- input email fields
- expected semantic category
- expected extracted objects
- expected redactions
- expected source references
- expected assistant answers when relevant

## 4. Extraction Quality

Measure whether MailBuddy correctly extracts:

- tasks
- due dates
- urgency
- estimated effort
- updates
- refund status
- appointments
- travel/event changes
- financial vigilance items
- opportunities
- digest items

Metrics:

- precision: extracted object is correct
- recall: important object was not missed
- field accuracy: due date, amount, merchant, status, source link
- confidence calibration: low confidence when evidence is weak

## 5. Classification Quality

Track classification accuracy across:

- task
- update
- financial_vigilance
- health_institutional
- government/legal/institutional
- digest
- opportunity
- noise
- reference

Important confusion cases:

- receipt vs financial alert
- newsletter vs opportunity
- appointment confirmation vs task
- refund update vs generic update
- school/government notice vs ordinary update

## 6. Assistant Correctness

The assistant should answer from semantic objects and deterministic tools.

Test questions:

- What needs my attention today?
- Did I get refunded?
- Which conversations need replies?
- What appointments are coming up?
- How much did I spend at Whole Foods this month?
- What important emails did I miss this week?
- Find my dental receipt.

Evaluation criteria:

- answer is correct
- answer includes relevant sources
- answer does not invent facts
- answer says it does not know when data is missing
- calculations come from deterministic tools
- filters and timelines match stored data

## 7. Redaction And Demo Safety

Redaction tests are hard gates.

Verify:

- no raw email addresses remain
- no real phone numbers remain
- no real addresses remain
- real people names are replaced
- financial institution names are replaced
- health institution names are replaced
- government/legal/immigration names are replaced or generalized
- account numbers are altered
- order/confirmation numbers are altered
- raw email bodies are not stored long-term
- assistant responses only use sanitized display values

Allow:

- ordinary non-sensitive merchant names
- generalized categories
- realistic fabricated institution names
- relative semantic meaning

## 8. Deterministic Tool Tests

Test deterministic tools independently:

- date filtering
- due date ranking
- spending totals
- merchant/category aggregation
- update timeline ordering
- task state transitions
- unresolved thread detection
- notification tier assignment

LLM tests should not be used to verify deterministic logic.

## 9. Notification Evaluation

For production design, evaluate notification decisions with fixtures.

Expected tiers:

- flight gate change: critical
- doctor appointment rescheduled: critical or timely
- package delivered: timely or digest
- refund completed: timely or digest
- newsletter arrived: digest
- promotion: silent
- high-stakes bank alert: critical

Measure:

- interruption precision
- missed critical rate
- over-notification rate
- user preference compliance

## 10. Voice Evaluation

Voice tests should cover:

- transcription accuracy for common prompts
- assistant routing from transcript
- latency
- graceful failure
- answer provenance
- text fallback

Voice demo prompts:

- What needs my attention today?
- Read my task briefing.
- Did I get refunded?
- Which conversations need replies?
- What updates changed recently?

## 11. Evaluation Report

Create an evaluation report command later that outputs:

- fixture count
- classification accuracy
- extraction precision/recall
- redaction pass/fail
- assistant groundedness pass/fail
- deterministic tool test results
- known failure cases

This report should be included as a portfolio artifact.
