# Story: Document Each Prompt Workflow — Name, Usage, Confirmation Tokens

> **Story ID**: story-4-prompt-reference
> **Feature**: feat-open-source-readme — Open-Source README & Developer Onboarding
> **Labels**: documentation, dx
> **Priority**: P1 — Should Have
> **Estimate**: 3
> **Status**: Done
> **Issue**: #10

---

## User Story

**As a** developer using `agentic-workflows`,  
**I want** a reference guide for each prompt workflow,  
**So that** I understand when to use each prompt, how to invoke it, and what confirmation token (if any) is required before the agent writes files.

## Acceptance Criteria

- [ ] **Given** a developer reads the prompt reference, **when** they look up any of the 8 prompts, **then** they find: prompt name, what it does, when to use it, how to invoke it in Copilot Chat, and any required confirmation token.
- [ ] **Given** a developer is about to use `create-stories.md` for the first time, **when** they consult the reference, **then** they learn that the agent will enter a dialog and require typing `CREATE STORIES NOW` before writing any files.
- [ ] **Given** a developer reads the prompt reference, **when** they scan all 8 prompts, **then** every prompt with a write-guard confirmation token has that token prominently called out in a `**Confirmation required**` callout.
- [ ] **Given** the prompt reference section is added to the README, **when** the total README length would exceed ~300 lines, **then** the prompt reference is extracted to `docs/prompts.md` and linked from the README Table of Contents.
- [ ] **Given** the prompt reference is reviewed, **when** it is checked for project-specific references, **then** no prompt example references the Progression App or any AWS-specific resource.

## Technical Notes

- Prompts to document (from `.github/prompts/`):
  1. `create-feature.md` — interactive dialog to produce `FEATURE.md`; confirmation token: `CREATE FEATURE NOW`
  2. `create-stories.md` — interactive dialog to produce story files; confirmation token: `CREATE STORIES NOW`
  3. `create-issues.md` — produces GitHub Issue bodies from stories via `gh` CLI
  4. `create-bug.md` — structured bug report using `BUG_TEMPLATE.md`
  5. `design-considerations.md` — design review and UI/UX considerations for a feature
  6. `issue-sync.md` — keeps story files in sync with GitHub Issue numbers after issues are created
  7. `operations.md` — operational runbooks, incident response, deployment guides
  8. `pr-description-generator.md` — generates PR description from diff/branch context
- Format: Markdown table with columns `Prompt`, `Purpose`, `How to invoke`, `Confirmation token`.
- Confirmation tokens must be quoted verbatim in monospace: `CREATE FEATURE NOW`, `CREATE STORIES NOW`.
- Read each `.github/prompts/*.md` file before writing the reference to confirm all confirmation tokens and invocation syntax.

## Dependencies

- [ ] Read each `.github/prompts/*.md` file to confirm confirmation tokens and invocation syntax before writing the reference

## Out of Scope

- Full reproduction of prompt file contents
- Writing new prompts
- Editing existing prompt files

---

_Last updated: 2026-05-11_
