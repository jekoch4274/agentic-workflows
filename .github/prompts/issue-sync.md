---
description: "Sync story files and FEATURE.md with GitHub Issue numbers"
---

# Issue Sync Prompt

This prompt helps a Spec Flow Agent keep documentation and GitHub issue metadata aligned without creating or modifying issues.

## High level behavior

- Read `docs/PROCESS_TRACKER.md` and all feature docs under `docs/feat-*/`.
- Update the tracker rows to reflect current story counts, feature issue metadata, and story issue coverage.
 - Parse and report `> **Labels**:` metadata on `FEATURE.md` and story files; do not modify labels here.
- Use existing issue references in docs only. Do not create new GitHub issues.
- Optionally verify issue numbers if `gh` CLI is available, but do not modify GitHub state.

## Task flow

1. Read each feature folder under `docs/feat-*/`.
2. For each feature:
   - Parse `docs/feat-{name}/FEATURE.md` to get the feature title and any `> **Related Issues**:` metadata.
   - Count story files in `docs/feat-{name}/stories/`.
   - Count story files with an existing issue reference of the form `> **Issue**: #<number>`.
3. Update `docs/PROCESS_TRACKER.md`:
   - Keep `Story count` current.
   - Set `Related issues` to `Listed` when the feature doc has real issue references, otherwise `Missing`.
   - Set `Story issues` to `x/y` where `x` is the number of stories with issue references and `y` is the total story count.
4. Preserve existing tracker notes wherever possible.
5. Do not add or create issue references in docs that are not already present.

## Rules

- Do not create or close GitHub issues.
- Do not add new `> **Issue**:` lines to story files.
- Only update the tracker and existing metadata based on current docs.
- If a story file has `TBD` for issue metadata, treat it as not tracked.

## Output

- A concise summary of which tracker rows changed and what the new issue sync state is.
- No GitHub issue creation commands.
- If `gh` is available, it may be used only to verify issue number existence.

## Script support

Use `scripts/issue-sync.py` to automate tracker updates from the command line.
