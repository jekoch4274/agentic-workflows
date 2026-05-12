---
description: "Interactive design prompt to enrich user stories with design considerations"
---

# Design Considerations Prompt

This prompt runs as a short conversation with the Product Owner or Designer
to collect visual and interaction decisions needed to make user stories
developer-ready with design acceptance criteria and shared component notes.

High level behavior

- Read the feature spec at the provided path (e.g., `docs/feat-home-page/FEATURE.md`).
- Ask up to 4 focused clarifying questions (present sensible defaults) about visual tone, density, accessibility, and responsive priorities.
- Show an interim design note for each story (1–2 acceptance criteria) for quick approval.
- After approval, expand each approved draft into full design acceptance criteria (3–5 items) and component notes.
- Only write changes to story files after explicit confirmation text `APPLY DESIGN UPDATES` is provided.

Question guidelines

- Keep questions short and choice-based where possible. Provide a recommended default.
- Examples:
  - "Visual tone: (1) Minimal & professional (recommended), (2) Playful & colorful"
  - "Density: (A) Spacious (recommended), (B) Compact"
  - "Accessibility strictness: (a) WCAG 2.1 AA (recommended), (b) WCAG 2.2 AAA"

File creation rules

- When writing to stories, prepend design notes under a **Design** section.
- Use today's date for `Last updated` in the updated story files.

Confirmation

Require explicit `APPLY DESIGN UPDATES` before any file edits.

Output

- When approved, update the story files listed in the feature's stories table,
  adding a **Design** section with acceptance criteria and shared component
  notes.

---

_Implementation note_: this prompt is intended to be used in dialog mode
where the Product Owner / Designer interacts step-by-step and accepts
recommended defaults. Explicit confirmation is required before edits.
