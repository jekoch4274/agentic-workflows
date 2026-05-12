# Copilot Instructions

## How to use this file

Fill in every `[PLACEHOLDER]` below with your project's details. Placeholders use `[ALL_CAPS_SNAKE_CASE]` format.

Tokens to replace:
- `[YOUR_PROJECT_NAME]` — the name of your project
- `[ONE_SENTENCE_PROJECT_DESCRIPTION]` — what the project does in one sentence
- `[YOUR_TECH_STACK]` — list your key technologies (framework, language, database, infra)
- `[YOUR_NAME]` — team member name for each role row
- `[YOUR_SOURCE_DIR]` — your app source directory (e.g., `src/`, `app/`, `web/`) — optional
- `[YOUR_INFRA_DIR]` — your infrastructure directory (e.g., `infra/`, `terraform/`) — optional
- `[YOUR_BUILD_COMMAND]` — the command used to build your app in CI <!-- example: pnpm build | npm run build -->
- `[YOUR_TEST_COMMAND]` — the command used to run tests in CI <!-- example: pnpm test | npm test -->
- `[YOUR_LINT_COMMAND]` — the command used to lint code in CI <!-- example: pnpm lint | npm run lint -->

**Delete this section after completing setup.**

---

## Project Context

This is **[YOUR_PROJECT_NAME]** — [ONE_SENTENCE_PROJECT_DESCRIPTION].

**Tech Stack:**
- [YOUR_TECH_STACK] <!-- example: frontend, backend, database, infrastructure -->

## Team Roles

| Role | Name | Focus |
|---|---|---|
| Product Owner | [YOUR_NAME] | Specs, stories, acceptance criteria, smoke testing |
| Developer | [YOUR_NAME] | Implementation, testing, deployment |
| Designer | [YOUR_NAME] <!-- [REMOVE IF NOT APPLICABLE] --> | UI/UX, component design |

## Repository Layout

- `.github/agents/` — Copilot agent definitions
- `.github/prompts/` — Interactive prompt workflows
- `docs/feat-{title}/FEATURE.md` — Feature specification
- `docs/feat-{title}/stories/story-{n}-{slug}.md` — Individual user stories
- `[YOUR_SOURCE_DIR]/` — Application source <!-- optional: remove if not applicable -->
- `[YOUR_INFRA_DIR]/` — Infrastructure code <!-- optional: remove if not applicable -->

## Conventions

- Feature folders: `feat-{title}` (e.g., `feat-user-auth`, `feat-dashboard`)
- Story files: `story-{n}-{slug}.md` (sequential within a feature)
- Branch names: `feat/short-description` or `docs/feat-{title}`
- Commit messages: concise and descriptive <!-- Recommended: feat: | fix: | docs: | chore: -->
- All specs use Markdown with consistent heading structure

## CI / Build & Test

- `build` command: `[YOUR_BUILD_COMMAND]` <!-- example: pnpm build | npm run build -->
- `test` command: `[YOUR_TEST_COMMAND]` <!-- example: pnpm test | npm test -->
- `lint` command: `[YOUR_LINT_COMMAND]` <!-- example: pnpm lint | npm run lint -->
- Workflow files live in `.github/workflows/` and should be adapted to your stack.

## Agent Rules

- **Never push directly to `main`.** Always use a feature branch.
- **Always recommend creating a feature branch** before committing docs or code.
- **Preferred git sequence:**

  ```bash
  git checkout -b feat/your-feature-name
  git add .
  git commit -m "feat: short description"
  git push -u origin feat/your-feature-name
  ```

- **Enable the pre-push hook** to block accidental pushes to `main`:

  ```bash
  git config core.hooksPath .githooks
  chmod +x .githooks/pre-push
  ```

## Data Architecture <!-- [REMOVE IF NOT APPLICABLE] -->

> Fill in your project's data conventions here, or remove this section.

- [DESCRIBE YOUR PRIMARY DATA STORE] <!-- example: PostgreSQL via Prisma ORM -->
- [DESCRIBE YOUR CONTENT STRATEGY] <!-- example: CMS-driven via Contentful, database-backed -->
