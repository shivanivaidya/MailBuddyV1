# MailBuddy UI/UX

## 1. UX Thesis

MailBuddy should feel like a calm operational dashboard for the inbox. The interface should avoid recreating a raw inbox list as the primary experience. The first screen should answer what matters, what changed, what needs action, and what can wait.

The product should be web and mobile-first. The demo should work well on a phone-sized viewport because voice and quick review are core to the story.

## 2. App Structure

Primary navigation:

- Home
- Tasks
- Updates
- Threads
- Opportunities
- Insights
- Digest
- Assistant
- Archive / Memory
- Settings

Mobile navigation should prioritize:

- Home
- Tasks
- Updates
- Assistant
- More

## 3. Home Dashboard

Home is the command center.

Sections:

- Attention Today
- Suggested Tasks
- Important Updates
- Waiting On Me
- Recent Financial Vigilance
- Opportunities
- Digest Highlights
- Quieted Noise Summary

The dashboard should be dense enough to be useful but not visually overwhelming. Each item should show why it appears and link to source context.

## 4. Tasks

Task list views:

- Today
- Upcoming
- Snoozed
- Completed
- Dismissed

Task card fields:

- title
- due date
- urgency
- estimated effort
- source
- confidence
- reason

Actions:

- complete
- snooze
- dismiss
- edit
- open source

## 5. Updates

Updates should be organized around lifecycles rather than individual emails.

Categories:

- deliveries
- refunds
- appointments
- travel
- events
- replacements
- reservations

Each update detail view should include:

- current state
- timeline
- latest change
- next expected step
- whether action is needed
- source messages

## 6. Threads

Semantic threads show conversation state.

Thread summary should include:

- topic
- participants
- current status
- unresolved questions
- who is waiting on whom
- suggested next action
- source emails

Threads should not be limited to Gmail thread IDs when separate messages clearly belong to the same real-world conversation.

## 7. Opportunities

Opportunities include career, networking, event, collaboration, and time-sensitive openings.

Views:

- New
- Needs Follow-Up
- Expiring Soon
- Saved
- Dismissed

Fields:

- opportunity title
- source
- deadline
- value estimate
- recommended next action
- confidence

## 8. Insights

Insights should turn inbox patterns into understandable signals.

Widgets:

- spending by merchant/category
- subscription trends
- response delays
- busiest senders
- cognitive overload sources
- muted/noise volume
- opportunities over time

Financial totals must come from deterministic calculations, not LLM prose.

## 9. Digest

Digest mode groups learning and newsletter content by topic.

Views:

- Today
- This Week
- Saved
- Topics

Topics may include:

- AI
- Product Management
- Career
- Industry
- Local Events
- Personal Learning

## 10. Assistant

The assistant screen should support both text and voice.

Required elements:

- text input
- push-to-talk button
- transcript display
- response area
- source/provenance links
- suggested prompts

Example prompts:

- What needs my attention today?
- Which conversations need replies?
- Did I get refunded?
- What appointments are coming up?
- Summarize my important updates.

Assistant answers should show the source objects used, especially for tasks, financial questions, updates, and deadlines.

## 11. Voice UX

Voice should be optimized for short operational interactions.

Voice modes:

- Ask a question.
- Start briefing.
- Review tasks.
- Review updates.
- Stop/pause.

Mobile behavior:

- large mic control
- clear recording state
- visible transcript
- cancel before sending
- text fallback if audio fails

Voice should not perform risky actions without approval. For example, "unsubscribe me" should create an approval prompt, not execute immediately.

## 12. Archive / Memory

Archive / Memory is for semantic search and long-term reference.

Search examples:

- Find my dental receipt.
- Show my immigration emails.
- When did I buy this?
- Find that refund email.
- Show emails about school paperwork.

Results should be grouped by semantic object type, not just chronological message order.

## 13. Settings

Settings should include:

- Gmail connection status
- sync status
- sync now
- demo data/redaction status
- notification preferences
- muted senders/merchants
- categories hidden from home
- voice settings
- data deletion/reset demo data

## 14. Demo Safety UX

Because the MVP is demo-mode-only, the UI should make it easy to verify that displayed data is sanitized.

Recommended cues:

- settings panel with "Demo-safe data active"
- last sanitization run status
- redaction test status in developer/debug view
- no raw email body viewer in public demo mode

The UI should preserve realistic product value without exposing private details.
