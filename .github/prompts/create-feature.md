---
description: "Interactively create a feature spec (dialog mode)"
---

# Interactive Create Feature Prompt

This prompt guides a Spec Flow Agent to run a short, focused conversation with the Product Owner to produce a clear feature specification file. The agent must ask clarifying questions, propose sensible defaults, show a draft, and require explicit confirmation before writing the `FEATURE.md` file.

## High level behavior

- Run as a dialogue: read the Product Owner's description, ask clarifying questions until satisfied where needed, present a filled draft of the feature spec, and request explicit confirmation to create or update files.
- Prefer simple choices with recommended defaults. If the Product Owner accepts defaults, proceed; otherwise iterate.

## Task flow

1. Ask the user which feature folder to use: an existing `docs/feat-{title}/` (if they specify) or the next sequential feature number discovered in `docs/`.
2. Read `docs/templates/FEATURE_TEMPLATE.md` to understand required fields.
3. Ask clarifying questions as needed to fill in the template. Example questions (use as relevant):
	- "Who is the primary user for this feature?"
	- "What problem does this solve in one sentence?"
	- "List 2–3 measurable goals for the feature."
	- "Are there any clear non-goals or out-of-scope items?"
	- "Do you have an initial suggestion for Target Release or Target Sprint?"
4. After collecting answers, show a filled draft of `FEATURE.md` (Overview, Goals, Non-Goals, Personas, suggested stories, Technical Considerations, Open Questions) and ask: "Approve and write file? (Type: CREATE FEATURE NOW)".
5. If the Product Owner types the exact confirmation token, create the folder (if missing), ensure `stories/` exists, and write `docs/feat-{title}/FEATURE.md` with Status set to Draft and Owner set to Product Owner. Include `> **Related Issues**: TBD` in the metadata block and initialize the story table with an `Issue` column. If not confirmed, do not write files and continue iterating.

## Drafting rules

- Use clear, non-technical language in Overview and Goals.
- Suggest adequate user stories in the stories table but do NOT create story files.
- Include at least one Open Question to prompt follow‑up discussions.
- Use today's date for the "Last updated" field.

## Output

When the Product Owner confirms, the agent will:
1. Create or update the folder `docs/feat-{title}/` and ensure `docs/feat-{title}/stories/` exists.
2. Write `docs/feat-{title}/FEATURE.md` populated from the template, Status: Draft, Owner: Product Owner, Related Issues: TBD.
3. Return a concise summary of what was created and recommend running the interactive "Create Stories" prompt next.

If the Product Owner does not confirm, remain in dialog mode and allow edits until they approve.

_Implementation note:_ require the confirmation token `CREATE FEATURE NOW` to prevent accidental writes.
