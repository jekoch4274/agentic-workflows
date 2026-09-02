# Story 2: Create Shared SKILL Files for Domain Knowledge

**Title:** Build reusable domain knowledge library  
**Story:** As a template user, I want shared SKILL files that teach best practices in each domain (architect, security, testing, design, coordination), so I can bootstrap specialist agents without reinventing patterns.

## Acceptance Criteria

### architect-patterns SKILL.md
- [x] System design principles documented
- [x] Architectural Decision Tradeoff (ADT) template provided
- [x] Common patterns (server-fetch, route handlers, data modeling)
- [x] DynamoDB modeling example (no AWS specific, just relational concepts)
- [x] API boundary definitions
- [x] Validation checklist for architects
- [x] Anti-patterns section
- [x] No stack-specific code (framework-agnostic)

### security-checklist SKILL.md
- [x] Threat modeling 4-step template
- [x] Auth flow patterns (public, authenticated, service-to-service)
- [x] Encryption patterns (in-transit TLS, at-rest, secrets management)
- [x] Input validation examples
- [x] Compliance frameworks (GDPR template with requirements)
- [x] Security AC examples
- [x] OWASP Top 10 references
- [x] Anti-patterns section

### testing-strategy SKILL.md
- [x] Test pyramid (60% unit, 25% integration, 10% E2E) with justification
- [x] AC→Test mapping template and examples
- [x] Vitest unit test patterns (with generic examples, not framework-specific)
- [x] Integration test patterns
- [x] E2E/Playwright patterns
- [x] QE verification tests concept explained
- [x] QE verification tests examples
- [x] Anti-patterns section

### component-design SKILL.md
- [x] Accessibility patterns (WCAG 2.1 AA checklist)
- [x] Responsive design framework (breakpoints, mobile-first)
- [x] Responsive typography patterns
- [x] Component spec template
- [x] Storybook story patterns and examples
- [x] Design tokens/variables guidance
- [x] Common UI patterns (form fields, buttons, grids)
- [x] Anti-patterns section

### coordination-patterns SKILL.md
- [x] When to delegate (each handoff scenario)
- [x] Delegation request template
- [x] Response contracts (JSON structures for predictable output)
- [x] Full workflow example (feature end-to-end)
- [x] Cost tracking pattern
- [x] Escalation patterns
- [x] Anti-patterns section

## Implementation Notes

- Each SKILL is independent (can be used standalone)
- Examples use generic patterns, not framework-specific code
- Templates are copy-paste ready
- Include "when to use this skill" guidance
- Provide anti-patterns so users know what NOT to do
