# Story: Write README — What, Why, and How

> **Story ID**: story-1-readme-overview
> **Feature**: feat-open-source-readme — Open-Source README & Developer Onboarding
> **Labels**: documentation, dx
> **Priority**: P0 — Must Have
> **Estimate**: 2
> **Status**: Done
> **Issue**: TBD

---

## User Story

**As a** developer who discovers `agentic-workflows` for the first time,  
**I want** to read a clear README that explains what the repo is, why I'd use it, and how it works,  
**So that** I can decide within 60 seconds whether to adopt it and know where to start.

## Acceptance Criteria

- [ ] **Given** a visitor opens the repo on GitHub, **when** they read the README, **then** the first screen (above the fold) communicates: what the project is, the key value proposition, and what files are included.
- [ ] **Given** a visitor reads the README, **when** they reach the "How it works" section, **then** they understand the relationship between agents, prompts, templates, and `docs/feat-{title}/` planning artifacts.
- [ ] **Given** a visitor reads the README, **when** they scan the top-level badge row, **then** they see at minimum: license badge and a "PRs welcome" badge.
- [ ] **Given** a visitor views the README on a mobile browser, **when** the page renders, **then** all tables, code blocks, and headings are readable without horizontal scrolling.
- [ ] **Given** the README references any file in the repo, **when** that reference is a link, **then** the link resolves correctly on GitHub (relative path, not absolute URL).

## Technical Notes

- Replace the current single-line `# agentic-workflows` with the full README.
- Required top-level sections: **What is this?**, **Why use it?**, **What's included** (file tree or table), **How it works** (agent → prompt → docs flow), **Quickstart** (teaser with link to full steps), **License**.
- The "What's included" section should enumerate: `.github/agents/` (7 agents), `.github/prompts/` (8 prompts), `docs/templates/` (3 templates), and the `docs/feat-{title}/` convention.
- Write in second person ("you") and keep tone approachable but professional.
- No Progression App references anywhere in the README.

## Out of Scope

- Detailed per-agent or per-prompt documentation (covered in story-3 and story-4)
- Quickstart steps (covered in story-2)
- `CONTRIBUTING.md`

---

_Last updated: 2026-05-11_
