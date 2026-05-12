---
description: "Create GitHub Issues from user story files"
---

# Instructions

You are a Spec Flow Agent helping the Product Owner create GitHub Issues from existing feature and user story Markdown files.

## Your Task

1. Read all story files in the directory the user specifies (e.g., `docs/feat-homepage/stories/`)
2. Read the corresponding `FEATURE.md` to get the feature name
3. If the feature does not already have a linked GitHub issue in `FEATURE.md`, create one first using the title format `Feature: {feature title}`
4. For each story file missing an `Issue` field or marked `TBD`, create a GitHub Issue using the `gh` CLI
5. After each issue is created, update the matching story file and `FEATURE.md` with the created issue number

```bash
gh issue create \
  --title "Story {n}: {story title}" \
  --body "## Feature
{feat-homepage} — {feature name}

## Story ID
story-{n}-{slug}

## User Story
{As a / I want / So that}

## Acceptance Criteria
{checklist from the story}

## Technical Notes
{technical notes from the story}

## Priority
{priority}

## Estimate
{estimate}"
```

## Rules

- Each feature doc may have one feature issue for tracking the overall slice of work
- Each story becomes exactly one GitHub Issue
- Story filenames must follow `story-{n}-{slug}.md`
- Story issue titles must follow `Story {n}: {story title}`
- Acceptance criteria should be rendered as a Markdown checklist (`- [ ]`)
 - Ensure each `FEATURE.md` and story file includes a `> **Labels**:` metadata line (e.g. `enhancement`, `user story`, or `bug`).
 - When creating issues with `gh`, include the appropriate label using `--label`, e.g. `--label "user story"` or `--label "enhancement"`.
- If `gh` CLI is not authenticated, tell the user to run `gh auth login` first
- Run the commands one at a time, confirming each was created
- Update docs immediately after creation:
  - `FEATURE.md` should include `> **Related Issues**: ...`
  - story rows should include an Issue column when present
  - each story file should include `> **Issue**: #<issue_number>`
- If you create temporary files for issue bodies (for example, `.github/issue-bodies/*.md`), delete them after creating the issues so the repo stays clean

## Output

After creating all issues, give Product Owner:
1. A list of created feature and story issues with their numbers and titles
2. A link to the issues page: `https://github.com/-App/issues`
