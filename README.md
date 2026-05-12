# agentic-workflows

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/yourusername/agentic-workflows/pulls)

> Drop a `.github/` folder into any project and get AI-driven product planning — features, user stories, GitHub Issues — powered by GitHub Copilot agents.

---

## Table of Contents

- [What is this?](#what-is-this)
- [Why use it?](#why-use-it)
- [What's included](#whats-included)
- [How it works](#how-it-works)
- [AI SDLC](#ai-sdlc)
- [Quickstart](#quickstart)
- [Context7](#context7)
- [Agent Reference](#agent-reference)
- [Prompt Reference](#prompt-reference)
- [License](#license)

---

## What is this?

`agentic-workflows` is a collection of **GitHub Copilot agent definitions**, **interactive prompt workflows**, and **planning document templates** that bring structured product planning into any codebase.

Instead of starting from scratch every time you design a feature, write a user story, or open a GitHub Issue — you invoke an agent in Copilot Chat and it guides you through the process, asks the right questions, and writes the files.

---

## Why use it?

- **Consistent planning artifacts** — every feature follows the same spec format; every story has acceptance criteria; every issue links back to a story.
- **AI-guided, human-confirmed** — agents ask clarifying questions and show drafts before writing anything. Write-guard confirmation tokens prevent accidental file creation.
- **Zero lock-in** — everything is plain Markdown in your repo. No external services, no databases, no subscriptions.
- **Works with any stack** — configure once in `copilot-instructions.md` and the agents adapt to your project context.

---

## What's included

```
.github/
  agents/
    product-owner.md             # Core: product strategy, features, stories, issues
    architect.md                 # Core: system design, ADRs, runbooks
    designer.md                  # Core: UI/UX specs, component design, a11y
    quality-engineer.md          # Core: test strategy, QA plans, E2E coverage
    security-engineer.md         # Core: OWASP, threat modeling, compliance
    developer.md                 # Template: fill in your framework (Next.js, Vue, Go…)
    platform-engineer.md         # Template: fill in your cloud (AWS, Azure, GCP…)
  prompts/                       # 8 interactive prompt workflows
    create-feature.md            #   Dialog → FEATURE.md
    create-stories.md            #   Dialog → story-{n}-{slug}.md files
    create-issues.md             #   Story files → GitHub Issues via gh CLI
    create-bug.md                #   Structured bug report
    design-considerations.md     #   Design review for a feature
    issue-sync.md                #   Sync story files with Issue numbers
    operations.md                #   Operational runbooks and incident response
    pr-description-generator.md  #   Generate PR description from context
  workflows/                      # CI/workflow examples, adapt for your stack
    pr-check.yml
    pr-merge.yml
  copilot-instructions.md        # Project context injected into every agent

docs/
  templates/
    FEATURE_TEMPLATE.md          # Feature spec template
    STORY_TEMPLATE.md            # User story template
    BUG_TEMPLATE.md              # Bug report template
  examples/
    agents/
      developer-nextjs.md        # Example: Next.js developer agent (copy & adapt)
      platform-aws.md            # Example: AWS platform agent (copy & adapt)
  feat-{title}/                  # One folder per feature (created by agents)
    FEATURE.md
    stories/
      story-{n}-{slug}.md
```

### Two types of agents

| Type | Files | Description |
|---|---|---|
| **Core** | `product-owner`, `architect`, `designer`, `quality-engineer`, `security-engineer` | Role-based, stack-agnostic. Use as-is. |
| **Customizable** | `developer`, `platform-engineer` | Templates. Fill in your stack. Rename and duplicate for multi-stack repos. |

**Adding a new stack agent** — copy an example from `docs/examples/agents/`, rename it (e.g., `developer-vue.md`, `platform-azure.md`), customize it for your framework/cloud, and drop it in `.github/agents/`. VS Code will pick it up automatically.

---

## How it works

```
You describe an idea in Copilot Chat
        ↓
Product Owner Agent asks clarifying questions
        ↓
You confirm → FEATURE.md is written to docs/feat-{title}/
        ↓
Stories Agent breaks the feature into user stories
        ↓
You confirm → story-{n}-{slug}.md files are written
        ↓
Issues Agent creates GitHub Issues from story files via gh CLI
        ↓
Developers implement; PR Description Agent writes the PR body
```

Each agent operates on plain Markdown files in your repo. The `docs/feat-{title}/` folder becomes your living product backlog.

## AI SDLC

This repo encodes an AI-enabled software delivery lifecycle. The process is:

1. **Solution Owner** gathers requirements and drafts the feature using `create-feature.md`.
2. **Design** reviews the feature and stories, then provides design feedback, UI/UX acceptance criteria, and design documentation.
3. **Architecture** and **Platform** add technical details, feasibility checks, and implementation constraints.
4. **Quality Engineering** reviews story testability, adds test questions, and validates acceptance criteria.
5. **Security** reviews the story for risks, compliance controls, and security-oriented test cases.
6. **Developer** and **Platform Engineer** work from the approved feature and story docs when there are no open architectural, design, quality, or security questions.
7. After implementation, **Solution Owner**, **Designer**, **Architect**, **Quality Engineering**, and **Security** review the finished feature and confirm the doc requirements are met.

### What each agent should do before coding

- `@product-owner-agent` / `create-feature.md` — define the feature, requirements, and story scope.
- `@designer-agent` / `design-considerations.md` — review UI/UX, accessibility, design assumptions, and visual requirements.
- `@architecture-agent` — review system design, constraints, and integration points.
- `@platform-engineer-agent` — review platform capabilities, deployment patterns, and infra dependencies.
- `@qe-agent` — review testability, acceptance criteria, edge cases, and failure behavior.
- `@security-agent` — review threat model, data flows, auth, and security test needs.

### Example agent invocations

- `@product-owner-agent feature: define the data export feature for project X`
- `@designer-agent review: validate the export UI and accessibility requirements`
- `@architecture-agent design: architecture for the export pipeline with retries`
- `@platform-engineer-agent design: storage and compute choices for export service`
- `@qe-agent audit: test strategy for export validation and failure modes`
- `@security-agent review: threat model for export data handling`
- `@developer-agent implement: build the export endpoint once docs are approved`

### Example questions to ask before implementation

- Is the acceptance criteria testable and unambiguous?
- Are the UX states and accessibility expectations documented?
- Is the design feasible with the chosen platform and architecture?
- Are the security boundaries and data protections explicit?
- Are there any open questions left for architecture, design, QE, or security?

## Approval before implementation

Before any developer starts building a feature, the feature spec and stories should be reviewed and approved by the team agents.

- Create the feature draft with `create-feature.md` and the story drafts with `create-stories.md`.
- Use `architect` to validate system design, dependencies, and feasibility.
- Use `developer` to validate implementation approach and stack fit.
- Use `quality-engineer` to review story testability and suggest acceptance criteria.
- Use `designer` to review and approve design-related documentation and UI/UX acceptance criteria.
- Use `security-agent` to review the stories and docs for risk, controls, and secure behavior.

Only after the spec and stories are approved by these agents should you open a `feat/*` branch to build the feature.

In practice:

- `doc/*` branches are for planning, review, and documentation only.
- `feat/*` branches are for implementation once the plan is approved.

This ensures planning, review, and approval happen before implementation, not after.

---

## Quickstart

### Prerequisites

- [GitHub Copilot](https://github.com/features/copilot) with agent mode enabled in VS Code
- [gh CLI](https://cli.github.com/) (only needed for `create-issues.md`)

### Steps

**1. Copy the `.github/` folder and templates into your project**

```bash
# Clone this repo
git clone https://github.com/jekoch4274/agentic-workflows.git

# Copy into your project
cp -r agentic-workflows/.github your-project/
cp -r agentic-workflows/docs/templates your-project/docs/templates
```

> **Windows (WSL):** Use the same `cp -r` commands inside a WSL terminal. Native PowerShell users can use `Copy-Item -Recurse`.

> **⚠️ Existing `.github/` folder?** Merge carefully — do not overwrite an existing `copilot-instructions.md` or workflow files without reviewing them first.

**2. Configure `copilot-instructions.md`**

Open `.github/copilot-instructions.md` in your project and fill in every `[PLACEHOLDER]` with your project's details. This should include your build, test, and lint commands if your CI/workflows depend on them. The file includes a `## How to use this file` section that lists every token to replace. Delete that section when done.

**3. (Optional) Enable the pre-push hook**

Blocks accidental direct pushes to `main`:

```bash
git config core.hooksPath .githooks
chmod +x .githooks/pre-push
```

**4. Get planning approval before building**

Use `create-feature.md` and `create-stories.md` to draft your feature and stories, then review them with the `architect` and `developer` agents before opening a `feat/*` branch.

**5. Invoke your first agent**

Open Copilot Chat in VS Code and try:

```
@product-owner-agent feature: add user authentication
```

The agent will ask clarifying questions, show a draft spec, and wait for you to type `CREATE FEATURE NOW` before writing any files.

---

## Context7

Several agents in this repo mention `use context7`. Here's what that means and how to set it up.

### What is context7?

[Context7](https://context7.com) is an **MCP (Model Context Protocol) server** that gives AI assistants real-time, version-accurate library documentation. Instead of relying on training data — which may reference outdated APIs — context7 fetches the actual current docs for the exact library version you're using.

When an agent says "use context7", it's telling Copilot to look up the live documentation rather than guess from memory. This matters most for fast-moving frameworks (React, Next.js, Vue, AWS SDK, etc.) where APIs change between major versions.

### Installing context7

Context7 runs as an MCP server in VS Code. Add it to your VS Code `settings.json`:

```json
{
  "mcp": {
    "servers": {
      "context7": {
        "command": "npx",
        "args": ["-y", "@upstash/context7-mcp"]
      }
    }
  }
}
```

Or follow the [official setup guide](https://context7.com/docs) for your editor.

### Using context7 with agents

Once installed, include `use context7` in your prompt to any agent:

```
@architect-agent use context7: design a background job queue using BullMQ
@developer-agent use context7: implement the Stripe webhook handler
```

The agent will fetch the current docs and ground its recommendations in accurate, up-to-date API usage.

> **context7 is optional.** Agents work without it — they simply rely on training data. Enable it when you need precise, version-specific answers.

---

## Agent Reference

Agents are invoked in GitHub Copilot Chat. VS Code auto-discovers all `.md` files in `.github/agents/`.

### Core agents (always applicable)

| Agent | Purpose | Example invocation |
|---|---|---|
| `product-owner` | Feature specs, user stories, GitHub Issues, workflow planning | `@product-owner-agent feature: add a dashboard page` |
| `architect` | System design, ADRs, runbooks, architecture review | `@architecture-agent design: storage layer for high-throughput uploads` |
| `designer` | UI/UX specs, component design, accessibility review | `@designer-agent component: spec an accessible modal with focus trap` |
| `quality-engineer` | Test strategy, unit and E2E test scaffolding, CI plans | `@qe-agent unit: write tests for the email validation module` |
| `security-engineer` | OWASP review, threat modeling, compliance | `@security-agent review: audit the auth endpoint against OWASP Top 10` |

### Customizable agents (fill in for your stack)

| Agent | What to customize | Example invocation after setup |
|---|---|---|
| `developer.md` | Framework, language, project layout, conventions | `@developer-agent implement story-2-user-profile` |
| `platform-engineer.md` | Cloud provider, IaC tool, core services, conventions | `@platform-engineer-agent design: managed database with read replicas` |

See `docs/examples/agents/` for fully worked examples (`developer-nextjs.md`, `platform-aws.md`).

> **Multi-stack / multi-cloud repos:** rename duplicates of the templates — e.g., `developer-nextjs.md` + `developer-vue.md`, or `platform-aws.md` + `platform-azure.md` — and keep them all in `.github/agents/`. Each gets its own agent in Copilot Chat.

---

## Prompt Reference

Prompts are interactive workflows invoked via Copilot Chat. Most enter a **dialog mode** — they ask questions, show drafts, and require an explicit confirmation token before writing files.

| Prompt | Purpose | How to invoke | Confirmation token |
|---|---|---|---|
| `create-feature.md` | Guided dialog → writes `FEATURE.md` | `use prompts/create-feature.md` | `CREATE FEATURE NOW` |
| `create-stories.md` | Guided dialog → writes `story-{n}-{slug}.md` files | `use prompts/create-stories.md for docs/feat-{title}/FEATURE.md` | `CREATE STORIES NOW` |
| `create-issues.md` | Reads story files → creates GitHub Issues via `gh` CLI | `use prompts/create-issues.md for docs/feat-{title}/` | _(runs `gh` commands one at a time)_ |
| `create-bug.md` | Structured bug report → GitHub Issue with `bug` label | `use prompts/create-bug.md` | _(none — uses `gh` directly)_ |
| `design-considerations.md` | Design review and UI/UX notes for a feature | `use prompts/design-considerations.md for docs/feat-{title}/FEATURE.md` | _(none)_ |
| `issue-sync.md` | Syncs Issue numbers back into story files after issues are created | `use prompts/issue-sync.md for docs/feat-{title}/` | _(none)_ |
| `operations.md` | Operational runbooks, incident response, deployment guides | `use prompts/operations.md` | _(none)_ |
| `pr-description-generator.md` | Generates PR description from feature/story context | `use prompts/pr-description-generator.md` | _(none)_ |

> **Write-guard tokens:** Prompts that write files require you to type the confirmation token exactly. This prevents accidental file creation during the dialog phase.

---

## License

MIT — see [LICENSE](LICENSE).

