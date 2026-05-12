# Feature: Open-Source README & Developer Onboarding

> **Feature ID**: feat-open-source-readme
> **Labels**: documentation, dx, onboarding
> **Status**: Done
> **Owner**: Product Owner
> **Target Release**: v1.0.0
> **Related Issues**: TBD

---

## Overview

The `agentic-workflows` repository contains a complete GitHub Copilot agent system — specialized agents, interactive prompt workflows, and planning templates — that any developer can drop into their project to get AI-driven product planning out of the box. Currently the repo has no README, no onboarding guide, and no explanation of what exists or how to use it. This feature makes the repository welcoming, discoverable, and immediately usable for any developer who lands on it.

## Goals

- A visitor to the repo can understand what it is, why it exists, and how it works within 60 seconds of reading the README.
- A developer can copy the `.github/` folder into any project and start using agents and prompts with zero ambiguity.
- Each agent and each prompt workflow is documented with its name, purpose, modes, and an example invocation.
- The onboarding path is linear: clone → copy → configure → invoke.

## Non-Goals (Out of Scope)

- Building a documentation site (GitHub Pages, Docusaurus, etc.)
- Writing tutorials or video walkthroughs
- CI automation for docs linting
- Translations or i18n
- Versioning or changelog management

## User Personas

| Persona | Description |
|---|---|
| **Solo Developer** | Building a side project alone; wants structured planning without overhead. Finds the repo via search or social. |
| **Team Lead** | Manages a small team and wants to standardize how features and stories are captured. Evaluating the repo for adoption. |
| **OSS Contributor** | Wants to understand the project well enough to submit a PR — fix a prompt, add an agent, or improve docs. |
| **AI Tooling Explorer** | Experimenting with GitHub Copilot agent customization; landed here looking for real-world examples. |

## User Stories

| Story | Title | Priority | Status | Issue |
|---|---|---|---|---|
| [story-1-readme-overview](stories/story-1-readme-overview.md) | Write README: what, why, how | P0 | Draft | TBD |
| [story-2-quickstart](stories/story-2-quickstart.md) | Add quickstart: clone, copy `.github/`, configure | P0 | Draft | TBD |
| [story-3-agent-reference](stories/story-3-agent-reference.md) | Document each agent: name, purpose, example invocation | P1 | Draft | TBD |
| [story-4-prompt-reference](stories/story-4-prompt-reference.md) | Document each prompt workflow: name, usage, confirmation tokens | P1 | Draft | TBD |

## UI / UX Notes

This feature is documentation only. All output is Markdown rendered on GitHub. Apply GitHub Markdown best practices:

- Use a top-level badge row (license, version, PRs welcome) below the title.
- Use collapsible `<details>` blocks for reference tables if length is a concern.
- Code blocks should always specify a language identifier for syntax highlighting.
- Every heading should be reachable via a Table of Contents anchor link.

## Technical Considerations

- All documentation lives in `README.md` at the repo root. No subdirectory site.
- The quickstart must be platform-agnostic (macOS, Linux, Windows WSL). Use `cp -r` with a note about Windows alternatives.
- Agent reference and prompt reference can live as sections in `README.md` or as linked `docs/` pages — the story author should decide based on length. If either section exceeds ~150 lines, extract to a linked file.
- The `.github/copilot-instructions.md` referenced in onboarding will be the generic version produced by `feat-generic-copilot-instructions`. These two features should be sequenced: `feat-generic-copilot-instructions` completes first, then `feat-open-source-readme` docs reference the generic file.
- Do not document the Progression App-specific content. All examples in README should use placeholder project names.

## Open Questions

- [ ] Should the repo include a `CONTRIBUTING.md`? Likely yes as a follow-on, but is it in scope for v1.0?
- [ ] Should the agent reference table live in README or in a dedicated `docs/agents.md`?
- [ ] Is a `LICENSE` file needed before publishing? (MIT is the likely choice — confirm with repo owner.)
- [ ] Should the quickstart include a `git init` path for brand-new projects as well as a "drop into existing repo" path?

---

_Last updated: 2026-05-11_
