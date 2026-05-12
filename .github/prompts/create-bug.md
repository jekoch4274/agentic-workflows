---
description: "Create Bug Issues using the BUG template and label guidance"
---

# Create Bug Issue Prompt

Use `docs/templates/BUG_TEMPLATE.md` as the canonical bug report template when creating new bug issues.

## Your Task

1. When a bug is reported, create a GitHub Issue using the BUG_TEMPLATE.md structure.
2. Ensure the issue includes the `bug` label when creating via `gh`, e.g.:

```bash
gh issue create \
  --title "Bug: {short description}" \
  --body-file ./path/to/issue-body.md \
  --label "bug"
```

3. Update any `FEATURE.md` or story files that reference the bug with `> **Issue**: #<number>` and, where appropriate, add `> **Labels**: bug` to the story metadata.

## Notes

- The canonical template is located at `docs/templates/BUG_TEMPLATE.md`.
- Prefer branch pattern `bugfix/<short-description>` for fixes.
- If `gh` is not authenticated, instruct the user to run `gh auth login` first.
