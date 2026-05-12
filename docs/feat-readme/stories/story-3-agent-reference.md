# Story: Document Each Agent — Name, Purpose, Example Invocation

> **Story ID**: story-3-agent-reference
> **Feature**: feat-open-source-readme — Open-Source README & Developer Onboarding
> **Labels**: documentation, dx
> **Priority**: P1 — Should Have
> **Estimate**: 3
> **Status**: Done
> **Issue**: TBD

---

## User Story

**As a** developer who has set up `agentic-workflows` in their project,  
**I want** a reference guide for each available agent,  
**So that** I know exactly what each agent does, when to use it, and how to invoke it in Copilot Chat.

## Acceptance Criteria

- [ ] **Given** a developer reads the agent reference, **when** they look up any of the 7 agents, **then** they find: the agent name, a one-sentence description, the modes it supports, and at least one concrete example invocation.
- [ ] **Given** a developer wants to understand which agent to use for a specific task, **when** they scan the agent table, **then** the descriptions are distinct enough that the right choice is unambiguous.
- [ ] **Given** a developer reads an example invocation, **when** they copy it into Copilot Chat and substitute their own context, **then** the syntax is correct and produces a useful response.
- [ ] **Given** the reference section is added to the README, **when** the total README length would exceed ~300 lines, **then** the agent reference is extracted to `docs/agents.md` and linked from the README Table of Contents.
- [ ] **Given** the agent reference is written, **when** it is reviewed, **then** no agent example references the Progression App, Next.js, AWS, or DynamoDB — all examples use placeholder or generic project names.

## Technical Notes

- Agents to document (from `.github/agents/`):
  1. `product-owner.md` — product strategy, features, stories, issues, workflow planning
  2. `architect.md` — system design, ADRs, runbooks, architecture review
  3. `designer.md` — UI/UX specs, component design, Storybook
  4. `next-js-developer.md` — full-stack implementation (stack-specific example; note users should replace with their own stack agent)
  5. `platform-engineer.md` — infrastructure, CI/CD, environments
  6. `quality-engineer.md` — test strategy, QA plans, coverage
  7. `security-engineer.md` — security review, OWASP, threat modeling
- Format: Markdown table with columns `Agent`, `Purpose`, `Modes`, `Example Invocation`.
- All example invocations must use generic project context, e.g., `@product-owner-agent feature: add user authentication`.
- The `next-js-developer.md` agent should be noted as a stack-specific example that users should rename or replace.

## Out of Scope

- Full reproduction of each agent's internal instructions
- Editing the agent `.md` files themselves (covered in `feat-generic-copilot-instructions`)
- Per-prompt documentation (covered in story-4)

---

_Last updated: 2026-05-11_
