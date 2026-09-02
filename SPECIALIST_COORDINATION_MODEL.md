# Specialist Coordination Model

A **bidirectional workflow architecture** for AI agents that coordinates specialist input into frozen specifications, then reviews implementations against those specs.

The model works in three phases:
1. **Specification** — specialists coordinate to build a comprehensive, frozen feature spec
2. **Implementation** — developers code to the spec (variable, metered)
3. **Verification** — specialists review PRs to confirm implementation matches spec

---

## The Workflow: Bidirectional

### Phase 1: Specification (Forward)

```
User describes feature idea
        ↓
Specification Coordinator (Product Owner)
  • Decomposes intent into feature spec
  • Consults specialists (architect, designer, security, QE)
  • Freezes FEATURE.md spec
        ↓
Architect Agent
  • Reviews system design, data models
  • Updates FEATURE.md with Technical Considerations
  • Hands off to Security Agent
        ↓
Security Agent  
  • Threat models the feature
  • Defines auth, encryption, compliance requirements
  • Updates FEATURE.md with Security Considerations
  • Hands off to Designer Agent
        ↓
Designer Agent
  • Designs components, specs interactions
  • Creates Storybook stories (if applicable)
  • Updates FEATURE.md with Design Considerations
  • Hands off to QE Agent
        ↓
QE Agent
  • Maps acceptance criteria to tests
  • Defines test pyramid (unit %, integration %, E2E %)
  • Updates FEATURE.md with Testing section
  • Hands off to Developer Agents
```

### Phase 2: Implementation (Code)

```
Developer Agents
  • Implement code to pass tests
  • Follow specs in FEATURE.md
  • Write code, not specs
  • Create PR when ready
        ↓
```

### Phase 3: Verification (Reverse)

```
PR created (specs frozen, implementation is the variable)
        ↓
Specification Coordinator
  • Kicks off review cycle
  • Briefs each specialist: "Review the PR against FEATURE.md spec"
        ↓
QE Agent Review
  • Do tests pass?
  • Does implementation match testing spec?
  • Are edge cases handled?
  • ✅ Approve or ❌ Request changes
        ↓
Designer Agent Review
  • Does UI match design spec?
  • Is it responsive? Accessible?
  • ✅ Approve or ❌ Request changes
        ↓
Security Agent Review
  • Are threat models implemented correctly?
  • Is auth secure? Data encrypted?
  • ✅ Approve or ❌ Request changes
        ↓
Specification Coordinator Final Review
  • All AC met?
  • All specialist reviews approved?
  • Ready to ship?
  • ✅ Merge & ship or ❌ Escalate
```

**Key principle:** Specs are frozen during implementation. Reviews verify "did you implement what we specified?" not "should we change the spec?" If implementation can't meet the spec, escalate back to PO + specialists for decision (rare).

---

## Role-Based Tool Boundaries

Each agent's edit permissions are scoped to their domain:

| Agent | Can Edit | Cannot Edit |
|-------|----------|-------------|
| **Specification Coordinator** | `docs/feat-*/` specs | Code, design, tests, infra |
| **Architect** | `docs/feat-*/` tech section | Code |
| **Security** | `docs/feat-*/` security section | Code |
| **Designer** | Design specs, Storybook stories | Implementation code |
| **QE** | Tests, test specs | Implementation code |
| **Developer** | Implementation code | Specs, tests, design |
| **Platform Engineer** | Infrastructure code | App code |

**Why:** Constraints prevent agents from accidentally changing things outside their domain. Developers can't rewrite specs. Designers can't change app code. This ensures scope stays locked.

---

## Shared Knowledge (SKILL.md Files)

All agents reference shared domain knowledge. Create SKILL files in `.github/skills/`:

- **`architect-patterns/SKILL.md`** — System design patterns, data flows, tradeoffs, architectural decision templates
- **`testing-strategy/SKILL.md`** — Test pyramid, critical paths, test patterns, AC → test mapping
- **`security-checklist/SKILL.md`** — Threat modeling template, auth/encryption, compliance frameworks
- **`component-design/SKILL.md`** — Accessibility patterns, responsive design, component specs, Storybook conventions
- **`coordination-patterns/SKILL.md`** — When/how to delegate between agents, response contracts, delegation workflows

**Benefit:** One write, all agents reference. When knowledge updates, improve it once and all agents improve.

---

## Feature Spec Structure (FEATURE.md)

Each feature builds up gradually as it flows through the specialist chain:

```markdown
# Feature: [Title]

## Problem
What user problem does this solve?

## User Story
As a [type of user],  
I want to [perform an action],  
so that [achieve a benefit].

## Acceptance Criteria (Initial)
- User can...
- System must...
- Error handling...

---
## Technical Considerations (Architect adds)
- Data model
- API boundaries
- External services
- Tradeoffs
- Scaling concerns

---
## Security Considerations (Security adds)
- Threat model (assets, attack surface, scenarios)
- Auth flow
- Encryption (in transit, at rest)
- Compliance
- Security AC (testable requirements)

---
## Design Considerations (Designer adds)
- Component specs
- Storybook stories
- Design AC (visual, accessibility, responsive)
- Design tokens

---
## Testing Considerations (QE adds)
- Test pyramid (unit %, integration %, E2E %)
- Critical paths
- AC → Test mapping
- Test AC
- QE verification tests (tests QE owns that developers don't see)

---
## Engineering Tasks
1. [Architect-approved task]
2. [Security-approved task]
3. [Design-approved task]
4. [QE-approved task]
```

Each section is frozen before moving to the next agent. Developers implement the final, fully-specified `FEATURE.md`.

---

## Handoff Context

When one agent hands off to the next:

- **PO → Architect:** `FEATURE.md` (problem, user story, initial AC)
- **Architect → Security:** `FEATURE.md` + architecture decisions
- **Security → Designer:** `FEATURE.md` + auth/encryption requirements
- **Designer → QE:** `FEATURE.md` + component specs and design AC
- **QE → Developers:** Final `FEATURE.md` (all sections complete, tests specified)

Each agent reads, consults relevant SKILL.md, updates FEATURE.md, hands off.

---

## Cost Model (Specification Coordination Principle)

**Expensive model (you) spent once:**
- Decide what to build (feature idea)
- Approve spec transitions (each handoff)
- Review final implementation

**Cheap models (agents) metered:**
- Coordinator decomposes intent (used once per feature)
- Specialists (architect, security, designer, qe) add domain expertise (used once per feature)
- Developers implement and test (iterations until spec is met)

**Metric:** Correctness-per-dollar. If implementation requires fewer retries and spec changes, system is working.

---

## When Specs Change (Escalation)

If an agent discovers the spec is impossible or requires tradeoff:

```
Developer: "Testing spec requires 100% E2E coverage, but route is flaky"
     ↓
    PO: Escalates to QE: "Can we change E2E requirement to integration test?"
     ↓
    QE: "Yes, integration test sufficient for this route"
     ↓
    PO: Updates FEATURE.md testing section
     ↓
Developer: Re-implements to new spec
     ↓
Back to review cycle (QE → Design → Security → PO)
```

**Key:** Escalation is rare. Specs are frozen precisely to avoid surprises. If escalation happens:
1. Document the issue in FEATURE.md
2. PO + affected specialists decide
3. Update FEATURE.md
4. Developer re-implements and re-reviews

---

## Delegation Workflow

The Specification Coordinator uses the `agent` tool to delegate to specialists. Delegation happens in this order:

1. **Coordinator → Architect** — "Review for technical feasibility"
2. **Coordinator → Security** — "Threat-model this feature"
3. **Coordinator → Designer** — "Design the UX and component specs"
4. **Coordinator → QE** — "Design test strategy and AC → test mapping"
5. **Coordinator** — Consolidates feedback into FEATURE.md and hands to developers

Each delegation includes prior specialist feedback. Each specialist builds on prior work.

**Delegation principles:**
- **Frozen Spec Before Delegation to Implementation** — specification phase is thorough but cheap (metered requests)
- **Clear Handoff Context** — each delegation includes prior feedback
- **Predictable Response Format** — each specialist returns structured output (JSON or Markdown sections)
- **Escalation is the Release Valve** — if specialist can't decide, they escalate with a question; PO + team decides

Reference `.github/skills/coordination-patterns/SKILL.md` for detailed delegation patterns, response contracts, and examples.

---

## Summary

This architecture:
- ✅ **Freezes specs** before implementation (no scope creep)
- ✅ **Distributes intelligence** (specialist agents, each with bounded scope)
- ✅ **Reduces ambiguity** (SKILL.md and coordination patterns prevent drift)
- ✅ **Scales complexity** (handoffs document what each agent did, next agent knows why)
- ✅ **Constrains agents** (role-based tool boundaries prevent accidental changes)
- ✅ **Measures correctness** (AC → tests mapping, verification review cycle)

**The bet:** Once intent is encoded densely enough (in FEATURE.md + SKILL.md), developers need only follow the spec, not interpret it.

---

## Quickstart

1. **Copy `.github/skills/` to your repo** — get the five shared knowledge files
2. **Reference SKILL files in agents** — add skill references to your `product-owner.md`, `architect.md`, etc.
3. **Add Role-Based Tool Boundaries section to agents** — define what each agent can/cannot edit
4. **Add Handoff Pattern sections to agents** — show who calls whom and what context flows
5. **Create `.github/FEATURE.md` template** — use the structure above
6. **Invoke product-owner-agent to test** — define a feature and watch the workflow

See `docs/examples/` for agent customization examples.
