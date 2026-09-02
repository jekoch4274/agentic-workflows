# Story 3: Integrate Model with Agent Instructions

**Title:** Update agents to reference and follow coordination model  
**Story:** As a template maintainer, I want each agent to reference the SKILL files and coordination model, so that new users understand how agents work together.

## Acceptance Criteria

- [ ] Each of 8 agent files (.github/agents/*.md) includes:
  - Tool scope section (what they can/cannot edit)
  - Consultative workflow (when to ask other specialists)
  - Handoff pattern (who they delegate to next)
  - Reference to relevant SKILL.md files
  - Example request/response for their role
  
- [ ] Agent handoff chain documented:
  ```
  PO decomposes problem
    ↓
  PO → Architect (feasibility review)
    ↓
  Architect → Security (threat modeling)
    ↓
  Security → Designer (component/UX design)
    ↓
  Designer → QE (test strategy)
    ↓
  QE → Developers (implementation, frozen spec)
    ↓
  PR Review (reverse: QE→Designer→Security→PO)
  ```

- [ ] Tool scoping prevents accidental edits:
  - PO: can edit docs/feat-*/, cannot edit code/tests/design/infra
  - Architect: can read .github/skills/, produces FEATURE.md section
  - Security: can read FEATURE.md, produces security considerations
  - Designer: can create Storybook files, produces design specs
  - QE: can create Playwright tests, produces testing section
  - Developers: can only edit code/tests per frozen spec

- [ ] Each agent includes example of:
  - "When to consult other agents" (when to use runSubagent tool)
  - "Response contract" (what output format is expected)
  - "Escalation path" (what to do if blocked)

## Implementation Notes

- Copy patterns from resume-app/.github/agents/ as reference
- Make agent instructions template-generic (usable for any project)
- Include inline comments showing where projects customize
- Emphasize that this is a template (users will fork and adapt)
