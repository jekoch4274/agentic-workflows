# Coordination Patterns — Agent Delegation SKILL

This skill teaches agents **when and how to delegate work to other specialist agents** using the `agent` tool. It ensures clear handoffs, predictable response contracts, and cost-effective coordination.

**Core principle:** Expensive agent (you) decides intent once. Cheap agents execute metered requests within frozen spec boundaries.

---

## When to Delegate (Specification Coordinator Workflow)

### Coordinator → Architect
**When:** Feature has technical complexity, data modeling, or infrastructure concerns

**Request:** "Review this feature spec for technical feasibility. Does the proposed architecture work? Any concerns?"

**Context:** FEATURE.md (problem, user story, acceptance criteria)

**Expect back:** Technical considerations (data model, API boundaries, tradeoffs, risks, technical AC)

### Coordinator → Security Agent
**When:** Feature touches user data, auth, secrets, or external APIs

**Request:** "Threat-model this feature. What are the risks and mitigations we should include?"

**Context:** FEATURE.md + architecture decisions

**Expect back:** Threat model (assets, attack surface, scenarios, mitigations, security AC)

### Coordinator → Designer Agent
**When:** Feature has user-facing UI/interactions or new components

**Request:** "Design the user experience and component specs. What do we need to build?"

**Context:** FEATURE.md + security/architecture decisions

**Expect back:** Design section (components, states, Storybook stories, design AC)

### Coordinator → QE Agent
**When:** Feature needs test strategy, AC clarification, or critical path definition

**Request:** "Map acceptance criteria to tests. What's the test pyramid for this feature?"

**Context:** FEATURE.md + all design/security/architecture decisions

**Expect back:** Testing section (test pyramid breakdown, AC→test mapping, critical paths, QE verification tests)

### Architect → Security Agent
**When:** Architect has designed infrastructure with secrets, encryption, or auth requirements

**Request:** "Validate the security model for this architecture. What are the threat vectors?"

**Context:** Architecture decisions

**Expect back:** Security considerations (encryption, auth flow, secret management, compliance AC)

### Designer → QE Agent
**When:** Designer has defined components, QE needs to verify testability

**Request:** "Can we test this design? Are the interactions observable?"

**Context:** Component specs, Storybook stories, design AC

**Expect back:** Confirmation that design is testable, E2E test scenarios

---

## Delegation Request Template

**Be specific.** Vague requests cause agents to guess.

```
Hey [agent-name],

I'm working on [feature name]. Here's the spec:

Problem: [user problem]
User Story: [As a... I want... so that...]
AC: [acceptance criteria]

[Previous specialist feedback, if any]

I need you to [specific task]:
- [Question 1]
- [Question 2]
- [Question 3]

Reference .github/skills/[relevant-skill]/SKILL.md for patterns and examples.

Thanks!
```

---

## Response Contracts

Each agent should return structured output. Define the contract to avoid ambiguity.

### Architect Response Contract

```json
{
  "status": "feasible | blocked | requires_decision",
  "data_model": {
    "primary_store": "database | cache | file",
    "schema": "<<exact definition>>",
    "access_patterns": ["<<how data is accessed>>"]
  },
  "api_pattern": "<<route handler | lambda | etc>>",
  "external_services": ["<<service names>>"],
  "tradeoffs": [
    {
      "choice": "async email",
      "pro": "user gets response fast",
      "con": "eventual consistency",
      "recommendation": "proceed"
    }
  ],
  "risks": ["<<architectural risks>>"],
  "technical_ac": ["<<testable requirements>>"],
  "escalation": "none | <<question for Coordinator>>"
}
```

### Security Response Contract

```json
{
  "status": "secure | mitigated | requires_decision",
  "threat_model": {
    "assets": ["<<what we protect>>"],
    "attack_surface": ["<<how attackers access>>"],
    "scenarios": [
      {
        "threat": "email injection",
        "impact": "attacker BCC'd",
        "likelihood": "medium",
        "mitigation": "validate email + use SES",
        "verification": "<<test or code pattern>>"
      }
    ]
  },
  "auth_flow": "<<diagram or prose>>",
  "encryption": {
    "in_transit": "TLS 1.3",
    "at_rest": "KMS encryption",
    "secrets": ["<<what secrets>>"]
  },
  "compliance": {
    "gdpr": "email deletable",
    "gdpr_ac": "implement deletion endpoint"
  },
  "security_ac": ["<<testable requirements>>"],
  "escalation": "none | <<question>>"
}
```

### Designer Response Contract

```json
{
  "status": "designable | blocked | needs_clarification",
  "components": [
    {
      "name": "SubscribeForm",
      "props": ["onSubmit", "isLoading", "error"],
      "states": ["default", "filled", "loading", "success", "error"],
      "accessibility": "<<WCAG notes>>"
    }
  ],
  "storybook_stories": [
    "Component/Default",
    "Component/Loading",
    "Component/Error",
    "Component/Mobile",
    "Component/A11y"
  ],
  "design_ac": [
    "Works at 200% zoom",
    "Keyboard accessible",
    "4.5:1 contrast minimum",
    "Responsive: 320px, 768px, 1024px"
  ],
  "escalation": "none | <<question>>"
}
```

### QE Response Contract

```json
{
  "status": "testable | blocked | needs_clarification",
  "test_pyramid": {
    "unit_percent": 60,
    "integration_percent": 25,
    "e2e_percent": 10,
    "justification": "<<why this ratio>>"
  },
  "ac_to_tests": [
    {
      "ac": "User can enter email",
      "unit_test": "isValidEmail() test",
      "integration_test": "N/A",
      "e2e_test": "N/A"
    }
  ],
  "critical_paths": ["happy path", "error path"],
  "hidden_battery": [
    "Unconfirmed emails never sent",
    "Rate limit enforced",
    "XSS prevention"
  ],
  "testing_ac": [
    "Each test has business-rule assertion",
    "No flaky tests",
    "All critical paths E2E"
  ],
  "escalation": "none | <<question>>"
}
```

---

## Full Delegation Workflow Example

### Step 1: Coordinator Creates Initial Spec
```
Problem: Readers want blog notifications
User Story: As a reader, I want to subscribe, so I'm notified of posts
AC: User can enter email, receives confirmation link
```

### Step 2: Coordinator → Architect
```
"Review for technical feasibility.
Proposal: DynamoDB subscribers table, S3 email templates, Lambda for delivery.
Any concerns?"
```
Architect returns: data model, API pattern, tradeoffs, risks, technical AC

### Step 3: Coordinator → Security (with Architect feedback)
```
"Threat-model the subscription feature.
Architecture: [architect output]
How do we secure email + confirmation token?"
```
Security returns: threat model, auth flow, encryption, security AC

### Step 4: Coordinator → Designer (with Arch + Security feedback)
```
"Design the subscribe UX.
Architecture: [arch output]
Security: email verification required
What components and states do we need?"
```
Designer returns: components, Storybook stories, design AC

### Step 5: Coordinator → QE (with all feedback)
```
"Design test strategy.
Full spec: [combined feedback above]
Map AC to tests. What's the pyramid?"
```
QE returns: test pyramid, AC→test mapping, critical paths, testing AC

### Step 6: Coordinator Freezes FEATURE.md
Combines all feedback into final spec:
- Problem
- User Story
- AC
- Technical Considerations (from Architect)
- Security Considerations (from Security)
- Design Considerations (from Designer)
- Testing Considerations (from QE)
- Engineering Tasks

### Step 7: Developers Implement
Implement to frozen spec. If questions arise, escalate to Coordinator.

### Step 8: PR Review Cycle (Reverse)
```
Coordinator → QE: "Review PR against testing spec. Tests pass?"
Coordinator → Designer: "Review PR against design spec. Responsive? Accessible?"
Coordinator → Security: "Review PR against security spec. Mitigations implemented?"
Coordinator Final: "All AC met? Ready to ship?"
```

---

## Delegation Principles

1. **Frozen Spec Before Implementation**
   - Specification phase (Coordinator ↔ specialists) is thorough but cheap
   - Implementation phase is variable but metered
   - Once spec is frozen, developers should not need specialist input (escalate if rare)

2. **Clear Handoff Context**
   - Each delegation includes prior specialist feedback
   - Next specialist builds on prior work, not from scratch
   - FEATURE.md accumulates as it flows through chain

3. **Predictable Response Format**
   - Each specialist returns structured output (JSON or sections)
   - Developers, reviewers, PO know what to expect
   - Ambiguity is minimal

4. **Escalation is the Release Valve**
   - If specialist can't decide, escalate with clear question
   - Coordinator + affected specialists decide
   - FEATURE.md is updated, specialist continues
   - Rare (sign that spec was well-designed if rare)

5. **Cost-Aware Metering**
   - Expensive agent (you) decides intent once (expensive)
   - Cheap agents (specialists) execute metered requests
   - Track cost to verify model is working

---

## Anti-Patterns (What NOT to Do)

❌ **Vague Delegation Requests**
```
"What should we build?" ← Too open-ended
```
✅ **Better:**
```
"Review this spec for security risks. Threat-model the auth flow."
```

❌ **Delegating Out of Order**
```
Ask Designer AND Security at same time about different aspects
```
✅ **Better:**
```
Ask Architect first → Security → Designer → QE (order matters)
```

❌ **Ignoring Escalation Responses**
```
Security returns: "clarify: should token expire in 24h or 7 days?"
You ignore it and proceed.
```
✅ **Better:**
```
Make decision, update FEATURE.md, ask Security to continue.
```

❌ **Loose Response Contracts**
```
Security returns prose with no clear requirements
```
✅ **Better:**
```
Security returns JSON with explicit security_ac: [testable requirements]
```

❌ **Delegating During Implementation**
```
Developer finds bug, asks Architect to re-design
```
✅ **Better:**
```
Developer escalates to Coordinator, who decides with Architect.
Spec is updated, developer re-implements.
```

---

## Summary

Use this skill to:
- Know when to delegate vs execute yourself
- Structure delegation requests for clarity
- Expect predictable response contracts
- Avoid common delegation anti-patterns
- Keep specs frozen and implementation metered

**Remember:** The goal is fast, high-confidence implementation. Frozen specs (built through structured delegation) achieve this.
