---
description: "Generate a GitHub PR description from feature and story context"
---

# PR Description Generator Prompt

Use this prompt when a developer is ready to create a PR for a feature branch (e.g., `feat/blog`).

## Required inputs

- Feature folder path (e.g., `docs/feat-blog-content/FEATURE.md`)
- Story file paths implemented in this PR (e.g., `docs/feat-blog-content/stories/story-1-s3-ingest-blog.md`)
- Issue IDs linked to stories (e.g., `#123`, `#124`)
- Summary of change (1-2 sentences)

## Output format

Generate markdown that matches `.github/pull_request_template.md` with these sections:

- Summary
- Feature
  - Link to feature spec
- User Story
  - List implemented story files
- Related Issues
  - `Closes #<issue_number>` for each issue ID

## Behavior

- **Branch type validation** (CRITICAL):
  - Extract branch prefix from `headRefName`: `feat/` or `doc/`
  - If branch is `doc/*` but PR contains code changes: ⚠️ **"Wrong branch type! Your PR has code changes. Branch should be `feat/*`, not `doc/*`. This PR will fail CI."**
  - If branch is `feat/*` but PR only contains docs/no code: ⚠️ **"Wrong branch type! Your PR only has documentation changes. Branch should be `doc/*`, not `feat/*`. This PR will fail CI."**
- If `closes` IDs are provided, use exact keyword `Closes` preceding each tag.
- Keep output concise and copy/paste ready.
- Include a short checklist / status note if the PR is not fully complete (optional).
- Use `git diff main...HEAD` and/or `gh pr view --json body,files` to detect whether this branch has a documented feature.
- If no `docs/feat-*` `FEATURE.md` entry or `docs/feat-*/stories/*` entry exists, suggest creating feature docs and stories in `docs/`.
- **REQUIRED for `feat/*` branches**: If the PR contains code but no linked issue, **must recommend** creating issues using `gh issue create` for each story file. Feature branches require at least one `Closes #N` reference in the PR body for workflows to pass.
- **For `doc/*` branches**: Do not use `Closes` in the generated PR description; use `Relates #N` instead. If issue numbers are present in story files (`> **Issue**: #N`), reuse those; do not create new issues for already-linked stories.
- **For backfill PRs** (doc branches with existing story files that have `> **Issue**: #N`): Extract those issue numbers from story files and reference them in the PR description using `Relates #N` keyword.
- **For feature/bugfix/chore/hotfix branches**: use `Closes #N` and if the stories have no issue references, create new issues via `gh issue create`.


## CLI assistants

- Validate branch type:
  - `gh pr view --json headRefName` → extract `feat/` or `doc/` prefix
  - `git diff --name-only main...HEAD` → check for code files (not just `docs/`, `.github/`)
  - Compare branch type vs. change type; warn if mismatch
- Evaluate changed files:
  - `git diff --name-only main...HEAD`
  - `grep -E "docs/feat-[^/]+/FEATURE.md" -` in changed files
- Check for PR description up-to-date:
  - `gh pr view --json title,body,headRefName`.
  - `gh pr edit --body "$(cat pr-body.md)"` to update after generation.

## Example output

```
## Summary
Implemented blog ingestion pipeline, tag normalization, tagged listing, and subscriber email notification.

## Feature
- [Feature spec](docs/feat-blog-content/FEATURE.md)

## User Story
- [Story 1](docs/feat-blog-content/stories/story-1-s3-ingest-blog.md)
- [Story 2](docs/feat-blog-content/stories/story-2-tag-normalization.md)
- [Story 3](docs/feat-blog-content/stories/story-3-blog-filter-pagination.md)
- [Story 5](docs/feat-blog-content/stories/story-5-subscribe-notice.md)

## Related Issues
- Closes #123
- Closes #124
```
