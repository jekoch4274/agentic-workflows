# Story 2: Create Shared SKILL Files for Domain Knowledge

**Title:** Build reusable domain knowledge library  
**Story:** As a template user, I want shared SKILL files that teach best practices in each domain (architect, security, testing, design, coordination), so I can bootstrap specialist agents without reinventing patterns.

## Acceptance Criteria

### architect-patterns SKILL.md
- [ ] System design principles documented
- [ ] Architectural Decision Tradeoff (ADT) template provided
- [ ] Common patterns (server-fetch, route handlers, data modeling)
- [ ] DynamoDB modeling example (no AWS specific, just relational concepts)
- [ ] API boundary definitions
- [ ] Validation checklist for architects
- [ ] Anti-patterns section
- [ ] No stack-specific code (framework-agnostic)

### security-checklist SKILL.md
- [ ] Threat modeling 4-step template
- [ ] Auth flow patterns (public, authenticated, service-to-service)
- [ ] Encryption patterns (in-transit TLS, at-rest, secrets management)
- [ ] Input validation examples
- [ ] Compliance frameworks (GDPR template with requirements)
- [ ] Security AC examples
- [ ] OWASP Top 10 references
- [ ] Anti-patterns section

### testing-strategy SKILL.md
- [ ] Test pyramid (60% unit, 25% integration, 10% E2E) with justification
- [ ] AC→Test mapping template and examples
- [ ] Vitest unit test patterns (with generic examples, not framework-specific)
- [ ] Integration test patterns
- [ ] E2E/Playwright patterns
- [ ] QE verification tests concept explained
- [ ] Hidden battery examples
- [ ] Anti-patterns section

### component-design SKILL.md
- [ ] Accessibility patterns (WCAG 2.1 AA checklist)
- [ ] Responsive design framework (breakpoints, mobile-first)
- [ ] Responsive typography patterns
- [ ] Component spec template
- [ ] Storybook story patterns and examples
- [ ] Design tokens/variables guidance
- [ ] Common UI patterns (form fields, buttons, grids)
- [ ] Anti-patterns section

### coordination-patterns SKILL.md
- [ ] When to delegate (each handoff scenario)
- [ ] Delegation request template
- [ ] Response contracts (JSON structures for predictable output)
- [ ] Full workflow example (feature end-to-end)
- [ ] Cost tracking pattern
- [ ] Escalation patterns
- [ ] Anti-patterns section

## Implementation Notes

- Each SKILL is independent (can be used standalone)
- Examples use generic patterns, not framework-specific code
- Templates are copy-paste ready
- Include "when to use this skill" guidance
- Provide anti-patterns so users know what NOT to do
