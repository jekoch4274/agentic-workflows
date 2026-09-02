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

- [ ] SPECIALIST_COORDINATION_MODEL.md provides end-to-end workflow documentation
- [ ] Five SKILL.md files define shared domain knowledge (architect, security, testing, design, coordination)
- [ ] Each SKILL includes patterns, examples, templates, and anti-patterns
- [ ] Agent instructions reference appropriate SKILL files for their domain
- [ ] Workflow architecture supports bidirectional verification (spec→implement→review)
- [ ] Documentation is template-agnostic (usable for any tech stack)
- [ ] Handoff patterns are explicit (who→who, when, what context)
- [ ] Cost model is documented (expensive decisions once, cheap executions metered)

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

- [ ] Model does not introduce new attack surface (docs only)
- [ ] FEATURE.md templates include security section
- [ ] security-checklist SKILL defines threat modeling template
- [ ] Examples include OWASP Top 10 patterns
- [ ] Compliance frameworks (GDPR) documented in SKILL

---

## Design Considerations (Designer)

- [ ] component-design SKILL provides WCAG 2.1 AA accessibility patterns
- [ ] Storybook story templates included
- [ ] Responsive design breakpoints defined
- [ ] Component spec template provided for designers to use

---

## Testing Considerations (QE)

- [ ] testing-strategy SKILL defines test pyramid (60/25/10)
- [ ] AC→test mapping template provided
- [ ] QE verification tests concept defined (tests QE owns)
- [ ] Critical path definition pattern documented
- [ ] Examples show Vitest, integration, and Playwright patterns

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
