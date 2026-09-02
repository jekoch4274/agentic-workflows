# Feature: Specialist Coordination Model for Agentic Workflows

**Status:** In Progress  
**Target Release:** v1.0  
**Owner:** James Koch

---

## Problem

Agentic workflow projects need a **proven coordination architecture** that:
- Decouples specification (expensive, intent-setting) from implementation (cheap, execution)
- Routes work through specialist agents with clear handoffs
- Provides shared domain knowledge (SKILLs) that scale across projects
- Enables bidirectional verification (spec→implement→review cycle)

## User Story

As a **developer on an agentic workflow project**, I want **a complete coordination model with documentation and examples**, so that **I can build features with predictable handoffs, frozen specs, and specialist input at the right time**.

## Acceptance Criteria

- [x] SPECIALIST_COORDINATION_MODEL.md provides end-to-end workflow documentation
- [x] Five SKILL.md files define shared domain knowledge (architect, security, testing, design, coordination)
- [x] Each SKILL includes patterns, examples, templates, and anti-patterns
- [x] Agent instructions reference appropriate SKILL files for their domain
- [x] Workflow architecture supports bidirectional verification (spec→implement→review)
- [x] Documentation is template-agnostic (usable for any tech stack)
- [x] Handoff patterns are explicit (who→who, when, what context)
- [x] Cost model is documented (expensive decisions once, cheap executions metered)

---

## Technical Considerations (Architect)

The model defines:
- **Specification phase:** PO coordinates specialists (Architect→Security→Designer→QE) to build frozen FEATURE.md
- **Implementation phase:** Developers implement to spec, with escalation path if spec unclear
- **Verification phase:** PR flows reverse (QE→Designer→Security→PO) to verify spec compliance
- **Handoff structure:** Each agent knows what to read (prior output) and what to produce (their section)
- **Tool scoping:** Each agent has bounded edit scope (docs/ vs code/ vs tests/ vs infra/)

**Tradeoffs:**
- ✅ Frozen specs reduce scope creep and ambiguity
- ✅ Specialist input early prevents rework
- ✅ Shared SKILLs prevent knowledge drift
- ❌ Front-loading complexity analysis (not ideal for rapid iteration)
- ❌ Requires discipline (if specialists skip steps, model breaks)

---

## Security Considerations (Security)

- [x] Model does not introduce new attack surface (docs only)
- [x] FEATURE.md templates include security section
- [x] security-checklist SKILL defines threat modeling template
- [x] Examples include OWASP Top 10 patterns
- [x] Compliance frameworks (GDPR) documented in SKILL

---

## Design Considerations (Designer)

- [x] component-design SKILL provides WCAG 2.1 AA accessibility patterns
- [x] Storybook story templates included
- [x] Responsive design breakpoints defined
- [x] Component spec template provided for designers to use

---

## Testing Considerations (QE)

- [x] testing-strategy SKILL defines test pyramid (60/25/10)
- [x] AC→test mapping template provided
- [x] QE verification tests concept defined (tests QE owns)
- [x] Critical path definition pattern documented
- [x] Examples show Vitest, integration, and Playwright patterns

---

## Engineering Tasks

1. Provide SPECIALIST_COORDINATION_MODEL.md as main entry point
2. Create .github/skills/ directory with 5 SKILL.md files:
   - architect-patterns: data modeling, API boundaries, tradeoffs
   - testing-strategy: test pyramid, AC mapping, verification tests
   - security-checklist: threat modeling, auth, encryption, compliance
   - component-design: accessibility, responsive, Storybook
   - coordination-patterns: delegation workflows, response contracts
3. Update agent instructions to reference SKILL files
4. Document handoff patterns and tool scoping per agent
5. Verify documentation is template-agnostic (no framework assumptions)
