---
description: "Interactively break a feature spec into user stories (dialog mode)"
---

# Interactive Create Stories Prompt

This prompt makes the Spec Flow Agent run as a short conversation with the Product Owner, asking focused clarifying questions and showing interim drafts before creating story files.

## High level behavior

- The agent MUST run as a conversation: read the feature, ask targeted clarifying questions (present sensible defaults), then produce story drafts. Do not produce final story files until the Product Owner confirms answers to clarifying questions.
- Prefer short, focused questions ( a few at a time). When the Product Owner accepts defaults, proceed.
- Keep the Product Owner in the loop: after each small set of questions, show the interim story draft(s) and ask for confirmation or edits.

## Task flow

1. Read the feature spec at the path the user provides (e.g., `docs/feat-homepage/FEATURE.md`).
2. Read the story template at `docs/templates/STORY_TEMPLATE.md`.
3. Inspect the feature's stories table. If rows exist, prepare drafts for those rows; if sparse, propose candidate stories derived from the Overview and Goals.
4. Enter interactive question mode to confirm details needed to make each story specific and testable. Example clarifying questions:
   - "Which single user action represents success for this story? (example: user clicks Publish, user receives an email)"
   - "Who performs this action?"
   - "Are there non-functional constraints (cache TTL, max latency, retention)?"
   - "What data fields must be present for testing (name, team id, content body)?"
   - Offer suggested defaults and present them clearly (e.g., "Default: path‑based cache invalidation — OK?").
5. After each small answer set, show a draft `As a / I want / So that` and 1–2 acceptance criteria for quick validation.
6. When the Product Owner approves a draft, expand it into a full story with 3–5 acceptance criteria and optional technical notes.
7. Once the Product Owner confirms all drafts, create the story files `docs/feat-{title}/stories/story-{n}-{slug}.md` and update the `FEATURE.md` stories table with links.

## Questioning strategy

- Ask the minimum set of questions needed for testable acceptance criteria.
- Provide choice-based options and a recommended default to speed decisions.
- When the Product Owner is unsure, provide a low‑cost default and explain tradeoffs in one sentence.

## File creation rules

- Follow `.github/copilot-instructions.md` conventions.
- Story numbers are sequential starting from 1.
- Story filenames must follow `story-{n}-{slug}.md`.
- Each story file must include metadata lines for `Story ID`, `Feature`, `Issue: TBD`, `Priority`, `Estimate`, and `Status`.
- `FEATURE.md` should use a story table with an Issue column so GitHub tasks can be backfilled later.
- Use today's date for "Last updated".
- Each acceptance criterion must be specific and testable and written in Given/When/Then format.
- Keep language non‑technical; where technical notes are present, explain implications plainly.

## Output

When approved, the agent will:
1. Create story files: `docs/feat-{title}/stories/story-{n}-{slug}.md`.
2. Update the `FEATURE.md` story table to link to each story file and include `TBD` in the Issue column.
3. Return a concise summary: story titles and number of acceptance criteria for each, and suggest the next step: "Create GitHub Issues".

If the Product Owner has not approved drafts, do NOT write files — remain in dialog mode.

---

_Implementation note:_ require explicit confirmation text (e.g., "CREATE STORIES NOW") from the Product Owner before any file writes.
