# Story: Add Quickstart — Clone, Copy `.github/`, Configure

> **Story ID**: story-2-quickstart
> **Feature**: feat-open-source-readme — Open-Source README & Developer Onboarding
> **Labels**: documentation, dx, onboarding
> **Priority**: P0 — Must Have
> **Estimate**: 2
> **Status**: Done
> **Issue**: TBD

---

## User Story

**As a** developer who has decided to adopt `agentic-workflows`,  
**I want** a step-by-step quickstart guide,  
**So that** I can go from zero to invoking my first agent in under 10 minutes without needing to ask questions.

## Acceptance Criteria

- [ ] **Given** a developer follows the quickstart from start to finish, **when** they complete all steps, **then** they have a working `.github/` folder in their target project with all agents and prompts available to GitHub Copilot.
- [ ] **Given** a developer reads step 3 (configure), **when** they open `.github/copilot-instructions.md`, **then** every placeholder (e.g., `[YOUR_PROJECT_NAME]`, `[YOUR_TECH_STACK]`, `[YOUR_BUILD_COMMAND]`, `[YOUR_TEST_COMMAND]`, `[YOUR_LINT_COMMAND]`) is clearly marked and documented with an example value.
- [ ] **Given** a developer uses Windows (WSL), **when** they read the copy command, **then** a WSL-compatible alternative command is provided alongside the macOS/Linux version.
- [ ] **Given** a developer already has a `.github/` folder in their target project, **when** they read the quickstart, **then** there is a callout warning them to merge carefully rather than overwrite.
- [ ] **Given** a developer completes the quickstart, **when** they open VS Code with GitHub Copilot Chat, **then** the README tells them exactly how to invoke their first agent (e.g., `@product-owner-agent feature: add user login`).

## Technical Notes

- Quickstart lives as a `## Quickstart` section in `README.md` unless the section exceeds 80 lines.
- Steps: (1) Clone or download the repo, (2) Copy `.github/` and `docs/templates/` into your project, (3) Edit `copilot-instructions.md` placeholders, (4) Enable pre-push hook (optional but recommended), (5) Open Copilot Chat and invoke an agent.
- Pre-push hook setup: `git config core.hooksPath .githooks && chmod +x .githooks/pre-push`. Document what the hook does (blocks direct pushes to `main`).
- Reference `feat-generic-copilot-instructions` placeholder conventions — this story depends on that feature being complete so placeholders are defined.
- Include a "First invocation" example showing the exact Copilot Chat syntax.

## Dependencies

- [ ] Depends on `feat-generic-copilot-instructions` story-2-placeholder-template (placeholders must be defined before the quickstart can reference them)

## Out of Scope

- CI/CD setup instructions
- Instructions for GitHub Actions workflows included in `.github/workflows/`
- Per-agent usage details (covered in story-3)

---

_Last updated: 2026-05-11_
